Multi-node with MAAS
====================

This tutorial shows how to install a multi-node MicroStack cluster using
MAAS as machine provider. It will deploy an **OpenStack 2024.1**
(Caracal) cloud.

Some steps provide estimated completion times. They are based on an
average internet connection.

Prerequisite knowledge
----------------------

A MAAS cluster is needed so familiarity with the
`MAAS <https://maas.io>`__ machine provisioning system is a necessity.
You should also be acquainted with these MAAS concepts:

-  `machine tags <https://maas.io/docs/how-to-use-machine-tags>`__
-  `storage tags <https://maas.io/docs/how-to-use-storage-tags>`__
-  `network tags <https://maas.io/docs/how-to-use-network-tags>`__
-  `Reserved IP Ranges <https://maas.io/docs/how-to-customise-maas-networks>`__

Some knowledge of OpenStack networking will be helpful.

Hardware requirements
---------------------

You will need a total of six machines. One to act as a client host and
five to make up the MAAS cluster.

Client
~~~~~~

The client machine will act as an administrative host. It does not
require significant resources and it need not be dedicated to this task.

**All the commands in this tutorial are run on the client.**

.. tip::
   For environments constrained by a proxy server, the client machine must first be configured
   accordingly. See section **Configure for the proxy at the OS level** on the
   :ref:`Manage a proxied environment` page before proceeding.

MAAS cluster
~~~~~~~~~~~~

The MAAS cluster will consist of five machines (the MAAS nodes): one
will manage software orchestration, one will manage internal components
like clusterd and three will host the actual cloud (the cloud nodes).
MAAS needs to be set up in advance.

Orchestration node
^^^^^^^^^^^^^^^^^^

The requirements for the orchestration node are:

-  physical or virtual machine
-  a dual-core processor
-  a minimum of 4 GiB of free memory
-  128 GiB of storage available on the root disk
-  one network interface

Infra node
^^^^^^^^^^

The requirements for the infra node are:

-  physical or virtual machine
-  a dual-core processor
-  a minimum of 4 GiB of free memory
-  128 GiB of storage available on the root disk
-  one network interface

Cloud nodes
^^^^^^^^^^^

The requirements for each of the three cloud nodes are:

-  physical machine running
-  a 16+ core amd64 processor
-  a minimum of 32 GiB of free memory
-  500 GiB of SSD storage available on the root disk
-  a least one un-partitioned disk of at least 200 GiB in size
-  two network interfaces

   -  primary: for access to the OpenStack control plane
   -  secondary: for remote access to cloud VMs

Summary of MAAS nodes
^^^^^^^^^^^^^^^^^^^^^

In this tutorial, the five MAAS nodes that comprise the MAAS cluster are
described in this way:

========= ===================== ============== ==================
Machine   FQDN                  Storage device Purpose
========= ===================== ============== ==================
sunbeam00 sunbeam00.example.com                orchestration node
sunbeam01 sunbeam01.example.com                infra node
sunbeam02 sunbeam02.example.com /dev/sdb       cloud node
sunbeam03 sunbeam03.example.com /dev/sdb       cloud node
sunbeam04 sunbeam04.example.com /dev/sdb       cloud node
========= ===================== ============== ==================

Ensure that your MAAS cluster is built before proceeding.

Prepare cloud networking
------------------------

Some planning in your environment and a corresponding configuration in
MAAS is needed. This affects the primary and secondary network
interfaces on the three cloud nodes.

Primary interface
~~~~~~~~~~~~~~~~~

In this tutorial, a single subnet will be used for the primary network
interface on all five MAAS nodes: ``10.5.0.0/16``.

Ensure you have a subnet connected to the primary interface.

.. note::
   The :ref:`Network traffic isolation with MAAS` page contains background information
   on the purpose of using multiple subnets.

Secondary interface
~~~~~~~~~~~~~~~~~~~

The secondary network interface must be set as :code:`Unconfigured` in MAAS
and be connected to a subnet that has unused (available) IP addresses.
This requirement permits the VMs to be contacted by remote hosts and
comprises the “external networking” of the cloud. This interface must be
tagged with a network tag ``neutron:physnet1``.

In this tutorial, the following values are used:

============================= =========================
External networking parameter Value
============================= =========================
CIDR                          172.16.2.0/24
default gateway               172.16.2.1
address range                 172.16.2.2 - 172.16.2.254
============================= =========================

The number of addresses needed is dependent upon the number of VMs you
wish to be remotely contactable (simultaneously). When used in this way,
the addresses are known as “floating IP addresses”.

You will be asked at a later step, via an interactive prompt, what
addressing to use for external networking.

Configure MAAS
--------------

Several aspects of MAAS need to be configured. Perform the steps in the
following sections within the MAAS web UI.

Configure MAAS Reserved IP Ranges
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Two particular cloud networks need to be assigned their own (labelled)
Reserved IP Range with a minimum number of available IP addresses. A
label is created by using the Comment field for the range. Its name is
based upon the chosen deployment name, which for this tutorial is
``mycloud`` (created later).

This is what is needed:

1. a range for network ``internal`` (minimum of **five** addresses) and
   with label ``mycloud-internal-api``
2. a range for network ``public`` (minimum of **ten** addresses) and
   with label ``mycloud-public-api``

In this tutorial, the ranges are defined in this way:

============= ======================= =======================
Cloud network Reserved IP Range label IP range
============= ======================= =======================
internal      mycloud-internal-api    10.5.0.100 - 10.5.0.104
public        mycloud-public-api      10.5.0.105 - 10.5.0.114
============= ======================= =======================

Configure your ranges now.

.. raw:: html

   <!-- INSERT MAAS SCREENSHOT WITH MENTION OF TUTORIAL'S NETWORK ADDRESSING -->

Create network space
~~~~~~~~~~~~~~~~~~~~

Create the network space that all the MAAS nodes will use.

In this tutorial, a single space is used, called ``myspace``.

.. raw:: html

   <!-- INSERT MAAS SCREENSHOT -->

The space will be mapped to cloud networks in a later step.

.. note::
   While MAAS supports `_` in space names, Sunbeam and Juju do not.
   Avoid using `_` in space names.

Choosing the MAAS resource tag
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sunbeam will look for machine bearing the “resource tag”. This tag is
used to identify the machines that will be used for the deployment. This
is built from the deployment name: ``openstack-<deployment name>``.

In this tutorial, the resource tag is called ``openstack-mycloud``.

.. raw:: html

   <!-- INSERT MAAS SCREENSHOT -->

Prepare the client
------------------

Prepare the client host by installing and configuring software.

Begin by installing the **openstack** snap:

::

   sudo snap install openstack --channel 2024.1/beta

.. tip::
   It is highly recommended to use the ``--channel 2024.1/beta`` switch which
   includes all the latest bug fixes and updates before the next stable
   release coming in Q4 2024.

MicroStack can generate a script to ensure that the client has all of
the required dependencies installed and is configured correctly for use
in MicroStack - you can review this script using:

::

   sunbeam prepare-node-script --client

or the script can be directly executed in this way:

::

   sunbeam prepare-node-script --client | bash -x

The script will ensure some software requirements are satisfied on the
host. In particular, it will:

-  install orchestration software (i.e. `Juju`)
-  create any necessary data directories

Prepare the MAAS nodes
----------------------

In this tutorial, the five MAAS nodes look like this:

+-------------+-------------+-------------+-------------+-------------+
| Machine     | Machine tag | Storage     | Storage tag | Network tag |
|             |             | device      |             |             |
+=============+=============+=============+=============+=============+
| sunbeam00   | ``openstack |             |             |             |
|             | -mycloud``, |             |             |             |
|             | ``juju-c    |             |             |             |
|             | ontroller`` |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| sunbeam01   | ``openstack |             |             |             |
|             | -mycloud``, |             |             |             |
|             | ``infra``   |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| sunbeam02   | ``openstack | /dev/sdb    | ``ceph``    | ``neutron   |
|             | -mycloud``, |             |             | :physnet1`` |
|             | `           |             |             |             |
|             | `control``, |             |             |             |
|             | `           |             |             |             |
|             | `compute``, |             |             |             |
|             | ``storage`` |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| sunbeam03   | ``openstack | /dev/sdb    | ``ceph``    | ``neutron   |
|             | -mycloud``, |             |             | :physnet1`` |
|             | `           |             |             |             |
|             | `control``, |             |             |             |
|             | `           |             |             |             |
|             | `compute``, |             |             |             |
|             | ``storage`` |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| sunbeam04   | ``openstack | /dev/sdb    | ``ceph``    | ``neutron   |
|             | -mycloud``, |             |             | :physnet1`` |
|             | `           |             |             |             |
|             | `control``, |             |             |             |
|             | `           |             |             |             |
|             | `compute``, |             |             |             |
|             | ``storage`` |             |             |             |
+-------------+-------------+-------------+-------------+-------------+

To prepare the MAAS nodes for the deployment, use the above information
to perform the following:

1. assign the machine tags (to the machines)
2. assign the storage tags (to the storage devices)
3. assign the network tags (to the secondary interfaces)

.. raw:: html

   <!-- INSERT MAAS SCREENSHOT? -->

The tags must be named as per the above table.

Add the MAAS deployment
-----------------------

Adding the MAAS deployment informs the orchestration node about the MAAS
cluster.

To do this you will need to pass options that describe the deployment.
They are:

-  ``name``: an arbitrary name (e.g. ``mycloud``)
-  ``token``: a `MAAS API
   key <https://maas.io/docs/how-to-manage-user-access>`__
   (e.g. ``z6sbVdQTuKWPFCFvPF:WkRdtsJnwXu38aRHUz:77SqG9DmaugFRHNT4SFtyGqubmLawNBJ``)
-  ``url``: the MAAS URL (e.g. ``http://10.236.110.5:5240/MAAS``)

Add the MAAS deployment now:

::

   sunbeam deployment add maas mycloud <token> <maas url>

The above command will check for the following:

-  working authentication
-  uniqueness of the deployment name

.. raw:: html

   <!-- REMOVE THIS ENTIRE SECTION FOR NOW - SHOULD BE MOVED TO ANOTHER DOC OR REFACTORED IN THIS DOC
   ### Explore the added deployment

   Explore the added deployment by issuing the below commands.

   To list registered MicroStack deployments:

       sunbeam deployment list
       ┏━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━┓
       ┃ Deployment ┃ Endpoint                       ┃ Type ┃
       ┡━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━┩
       │ sbc01*     │ http://10.246.112.3:5240/MAAS/ │ maas │
       └────────────┴────────────────────────────────┴──────┘

   To list registered machines:

       sunbeam deployment machine list
       ┏━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━┳━━━━━━━━┓
       ┃ Machine       ┃ Roles                     ┃ Zone  ┃ Status ┃
       ┡━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━╇━━━━━━━━┩
       │ sunbeam00     │ juju-controller           │ alpha │ Ready  │
       │ sunbeam01     │ infra                              │ alpha │ Ready  │
       │ sunbeam02     │ storage, control, compute │ alpha │ Ready  │
       │ sunbeam03     │ storage, control, compute │ alpha │ Ready  │
       │ sunbeam04     │ storage, control, compute │ alpha │ Ready  │
       └───────────────┴───────────────────────────┴───────┴────────┘

   To list available spaces:

       sunbeam deployment space list
       ┏━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
       ┃ Space              ┃ Subnets                                                                     ┃
       ┡━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩
       │ public-space       │ 10.246.112.0/21, fd42:fd18:a780:590::/64                                    │
       │ admin-space        │ 10.6.4.0/22                                                                 │
       │ internal-space     │ 10.6.8.0/22                                                                 │
       │ overlay-space      │ 10.6.12.0/22                                                                │
       │ ceph-access-space  │ 10.6.16.0/22                                                                │
       │ ceph-replica-space │ 10.6.20.0/22                                                                │
       │ undefined          │ 192.168.122.0/24, 192.168.123.0/24, 10.201.243.0/24, fd42:78:62d4:12cb::/64 │
       └────────────────────┴─────────────────────────────────────────────────────────────────────────────┘

   To list your network mapping:

       sunbeam deployment network list

       ┏━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━┓
       ┃ Network         ┃ Network space ┃
       ┡━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━┩
       │ public          │ <unmapped> │
       │ storage         │ <unmapped> │
       │ storage-cluster │ <unmapped> │
       │ internal        │ <unmapped> │
       │ data            │ <unmapped> │
       │ management      │ <unmapped> │
       └─────────────────┴────────────┘
   -->

Map network spaces to cloud networks
------------------------------------

Certain machines need access to certain cloud networks.

In this tutorial, because we’re using a single subnet (and space) for
the primary network interface on each cloud node, map the same space
(``myspace``) to each supported cloud network:

::

   sunbeam deployment space map myspace data
   sunbeam deployment space map myspace internal
   sunbeam deployment space map myspace management
   sunbeam deployment space map myspace public
   sunbeam deployment space map myspace storage
   sunbeam deployment space map myspace storage-cluster

These mappings tell MicroStack where to route certain types of cloud
traffic.

.. raw:: html

   <!-- REMOVE THIS FOR NOW

   Here's an example of a complete mapping:

       sunbeam deployment network list
       ┏━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━┓
       ┃ Network         ┃ MAAS Space         ┃
       ┡━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━┩
       │ data            │ internal-space     │
       │ internal        │ internal-space     │
       │ management      │ admin-space        │
       │ public          │ public-space       │
       │ storage         │ ceph-access-space  │
       │ storage-cluster │ ceph-replica-space │
       └─────────────────┴────────────────────┘

   -->

Validate the added deployment
-----------------------------

MicroStack expects a correctly configured MAAS, which includes adequate
networking.

To check whether your environment is ready, use the
``deployment validate`` command:

::

   sunbeam deployment validate

Example output:

::

   Checking machines, roles, networks and storage... OK
   Checking zone distribution... OK
   Checking networking... OK
   Report saved to '/home/ubuntu/snap/openstack/common/reports/validate-deployment-mycloud-<...>.yaml'

A report will be generated under ``$HOME/snap/openstack/common/reports``
if a failure is detected. A sample failure looks like this:

::

   - diagnostics: A machine root disk needs to be at least 500GB to be a part of an openstack
       deployment.
     machine: sunbeam02
     message: root disk is too small
     name: Root disk check
     passed: false

.. note::
   A validation error will lessen the chances of a successful deployment but it will not block
   an attempted deployment.

Initialise the orchestration and infra layer
--------------------------------------------

Set up the orchestration and infra layer using the ``cluster bootstrap``
command. This will provision the MAAS nodes that are assigned the
``juju-controller`` tag and ``infra`` tag:

::

   sunbeam cluster bootstrap

You will first be prompted whether or not to enable network proxy usage.
If ‘Yes’, several sub-questions will be asked.

::

   Use proxy to access external network resources? [y/n] (y):
   Enter value for http_proxy: ():
   Enter value for https_proxy: ():
   Enter value for no_proxy: ():

Note that proxy settings can also be supplied by using a manifest (see
:doc:`Deployment manifest </reference/concepts/deployment-manifest>`).

Deploy the cloud
----------------

Deploy the cloud using the ``cluster deploy`` command. This will
provision the remaining three MAAS nodes:

::

   sunbeam cluster deploy

Configure the cloud
-------------------

Configure the deployed cloud using the ``configure`` command:

::

   sunbeam configure --openrc demo-openrc

The ``--openrc`` option specifies a regular user (non-admin) cloud init
file (``demo-openrc`` here).

A series of questions will now be asked interactively. Below is a sample
session. The values in square brackets, when present, provide acceptable
values. A value in parentheses is the default value. We use the values
for “external networking” given earlier:

.. code:: text

   CIDR of network to use for external networking (10.20.20.0/24): 172.16.2.0/24
   IP address of default gateway for external network (10.20.20.1): 172.16.2.1
   Populate OpenStack cloud with demo user, default images, flavors etc [y/n] (y):
   Username to use for access to OpenStack (demo):
   Password to use for access to OpenStack (mt********):
   Network range to use for project network (192.168.122.0/24):
   Enable ping and SSH access to instances? [y/n] (y):
   Start of IP allocation range for external network (10.20.20.2): 172.16.2.2
   End of IP allocation range for external network (10.20.20.254): 172.16.2.254
   Network type for access to external network [flat/vlan] (flat):
   Writing openrc to demo-openrc ... done

The network range for the initial project defaults to
``192.168.122.0/24``. This is for OpenStack internal purposes (“private
networking”) and should suffice for most clouds.

These questions are explained in more detail on the :doc:`Interactive
configuration prompts </reference/interactive-configuration-prompts>` page in
the reference section.

Launch a VM
-----------

Verify the cloud by launching a VM called ‘test’ based on the ‘ubuntu’
image (Ubuntu 22.04 LTS). The ``launch`` command is used:

::

   sunbeam launch ubuntu --name test

Sample output:

::

   Launching an OpenStack instance ...
   Access instance with `ssh -i /home/ubuntu/.config/openstack/sunbeam ubuntu@172.16.2.200`

Connect to the VM over SSH using the provided command.

Related how-to guides
---------------------

Now that OpenStack is set up, be sure to check out the following how-to
guides:

-  :doc:`Accessing the OpenStack dashboard </how-to/misc/accessing-the-openstack-dashboard>`
-  :doc:`Using the OpenStack CLI </how-to/misc/using-the-openstack-cli>`

.. raw:: html

   <!-- LINKS -->