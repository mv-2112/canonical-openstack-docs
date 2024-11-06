Multi-node installation
=======================

Requirements
------------

You will need **three** machines, each of which has the following
requirements:

-  physical machine running Ubuntu 22.04 LTS
-  a multi-core amd64 processor (ideally with 4+ cores)
-  a minimum of 32 GiB of free memory
-  200 GiB of SSD storage available on the root disk
-  a least one un-partitioned disk of at least 200 GiB in size
-  two network interfaces

   -  primary: for access to the OpenStack control plane
   -  secondary: for remote access to cloud VMs

.. warning::
   Any change in IP address of any machine will be detrimental to the deployment. Dedicated
   physical machines with fixed IP address allocations are therefore required.

.. note::
   For environments constrained by a proxy server, the intended MicroStack nodes must first be
   configured accordingly. See section **Configure for the proxy at the OS level** on
   the :ref:`Manage a proxied environment` page before proceeding.

Machine names
~~~~~~~~~~~~~

For the purpose of this tutorial, the following machine names are used:

========= ===================== ===================
Machine   FQDN                  Un-partitioned disk
========= ===================== ===================
sunbeam01 sunbeam01.example.com /dev/sdb
sunbeam02 sunbeam02.example.com /dev/sdb
sunbeam03 sunbeam03.example.com /dev/sdb
========= ===================== ===================

Control plane networking
~~~~~~~~~~~~~~~~~~~~~~~~

The network associated with the primary network interface requires a
range of approximately ten IP addresses that will be used for API
service endpoints.

For the purposes of this tutorial, the following configuration is in
place:

========================= =========================
Network component         Value
========================= =========================
CIDR                      172.16.1.0/24
Gateway                   172.16.1.1
Address range             172.16.1.201-172.16.1.220
Interface name on machine eno1
========================= =========================

External networking
~~~~~~~~~~~~~~~~~~~

The network associated with the secondary network interface requires a
range of IP addresses that will be sufficient for allocating floating IP
addresses to VMs. This will, in turn, allow them to be contacted by
remote hosts.

For the purposes of this tutorial, the following configuration is in
place:

========================= =======================
Network component         Value
========================= =======================
CIDR                      172.16.2.0/24
Gateway                   172.16.2.1
Address range             172.16.2.2-172.16.2.254
Interface name on machine eno2
========================= =======================

Bootstrap the first machine
---------------------------

Commands in this section are performed on ``sunbeam01``.

.. note::
   During the deployment process you will be asked to input information in order to configure
   your new cloud. These questions are explained in more detail on the
   :ref:`Interactive configuration prompts` page.

Install the openstack snap
~~~~~~~~~~~~~~~~~~~~~~~~~~

Begin by installing the **openstack** snap:

::

   sudo snap install openstack --channel 2024.1/beta

.. tip::
   It is highly recommended to use the ``--channel 2024.1/beta`` switch which includes all the
   latest bug fixes and updates before the next stable release coming in Q4 2024.

Prepare the machine
~~~~~~~~~~~~~~~~~~~

Sunbeam can generate a script to ensure that the machine has all of the
required dependencies installed and is configured correctly for use in
OpenStack - you can review this script using:

::

   sunbeam prepare-node-script

or the script can be directly executed in this way:

::

   sunbeam prepare-node-script | bash -x && newgrp snap_daemon

The script will ensure some software requirements are satisfied on the
host. In particular, it will:

-  install ``openssh-server`` if it is not found
-  configure password-less sudo for all commands for the current user
   (``NOPASSWD:ALL``)

Bootstrap the cloud
~~~~~~~~~~~~~~~~~~~

Deploy the OpenStack cloud using the ``cluster bootstrap`` command:

::

   sunbeam cluster bootstrap --role control,compute,storage

This first node will therefore be a control node, a compute node, and a
storage node.

**On snap channel ``2024.1/beta``**, you will first be prompted whether
or not to enable network proxy usage. If ‘Yes’, several sub-questions
will be asked.

::

   Use proxy to access external network resources? [y/n] (y):
   Enter value for http_proxy: ():
   Enter value for https_proxy: ():
   Enter value for no_proxy: ():

Note that proxy settings can also be supplied by using a manifest (see
:doc:`Deployment manifest </reference/concepts/deployment-manifest>`).

When prompted, enter the CIDR and the address range for the control
plane networking and the full path to the un-partitioned disk. Here we
use the values given earlier:

::

   Management networks shared by hosts (CIDRs, separated by comma) (10.20.20.0/24): 172.16.1.0/24
   MetalLB address allocation range (supports multiple ranges, comma separated) (10.20.20.10-10.20.20.20): 172.16.1.201-172.16.1.220
   Disks to attach to MicroCeph: /dev/sdb

.. caution::
   The address range used for the control plane must be addressable by all nodes in the deployment.

The un-partitioned disk(s) will be detected and allocated for cloud
storage (Ceph).

Add the second machine
----------------------

To add second machine ``sunbeam02``, some commands are performed on the
first machine (``sunbeam01``) and some are performed on the new machine
(``sunbeam02``) itself.

Create a registration token
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam01``.

A registration token is needed before adding a new member. Run the
``cluster add`` command against the FQDN of the new node:

::

   sunbeam cluster add sunbeam02.example.com --output sunbeam02.asc

.. caution::
   Clustering does not support base hostnames. A node is only known by their FQDN.

Sample output (token):

::

   YmRlODViYjYtMGFlNy00MmFjLWE4NzMtNjI0ODg4YmUzZTM0Cg==

Keep the token in a safe place. It will be used in a future step.

.. _install-the-openstack-snap-1:

Install the openstack snap
~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the below command on `sunbeam02`.

Install the openstack snap:

::

   sudo snap install openstack --channel 2024.1/beta

.. tip::
   It is highly recommended to use the `--channel 2024.1/beta` switch which includes all the
   latest bug fixes and updates before the next stable release coming in Q4 2024.

.. _prepare-the-machine-1:

Prepare the machine
~~~~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam02``.

Sunbeam can generate a script to ensure that the machine has all of the
required dependencies installed and is configured correctly for use in
MicroStack - you can review this script using:

::

   sunbeam prepare-node-script

or the script can be directly executed in this way:

::

   sunbeam prepare-node-script | bash -x && newgrp snap_daemon

The script will ensure some software requirements are satisfied on the
host. In particular, it will:

-  install ``openssh-server`` if it is not found
-  configure password-less sudo for all commands for the current user
   (``NOPASSWD:ALL``)

Add the new node
~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam02``.

Add the machine as a new cluster member by using the ``cluster join``
command. Refer to the registration token obtained earlier:

::

   cat sunbeam02.asc | sunbeam cluster join --role control,compute,storage -

[note type=“caution”] **Caution**: The ending dash is mandatory when
passing the token in standard input [/note]

The final part of the join process will prompt for a free network
interface to use for external networking. When prompted, enter the
interface name for the external networking. Here we use the values given
earlier:

::

   Free network interface that will be configured for external traffic [eno1/eno2] (eno1): eno2

Any remote hosts intending to connect to VMs on this node must have
connectivity with the interface selected for external traffic.

Add the third machine
---------------------

To add the third machine, we repeat the steps that were taken to add the
second machine.

.. _create-a-registration-token-1:

Create a registration token
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam01``.

A registration token is needed before adding a new member. Run the
``cluster add`` command against the FQDN of the new node:

::

   sunbeam cluster add sunbeam03.example.com --output sunbeam03.asc

Sample output (token):

::

   NGI0Mzg2NzktODA5OC00ZTRmLWIyZWEtNmU2NmQ2MjgxZmU1Cg==

.. _install-the-openstack-snap-2:

Install the openstack snap
~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam03``.

Install the openstack snap:

::

   sudo snap install openstack --channel 2024.1/beta

.. tip::
   It is highly recommended to use the ``--channel 2024.1/beta`` switch which includes all the
   latest bug fixes and updates before the next stable release coming in Q4 2024.

.. _prepare-the-machine-2:

Prepare the machine
~~~~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam03``.

::

   sunbeam prepare-node-script | bash -x && newgrp snap_daemon

Join the machine to the cluster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the below command on ``sunbeam03``.

Join the machine to the cluster by using the ``cluster join`` command.
Refer to the registration token obtained earlier:

::

   sunbeam cluster join --role control,compute,storage NGI0Mzg2NzktODA5OC00ZTRmLWIyZWEtNmU2NmQ2MjgxZmU1Cg==

The final part of the join process will prompt for a free network
interface to use for external networking. When prompted, enter the
interface name for the external networking. Here we use the values given
earlier:

::

   Free network interface that will be configured for external traffic [eno1/eno2] (eno1): eno2

Any remote hosts intending to connect to VMs on this node must have
connectivity with the interface selected for external traffic.

Resize the control plane
------------------------

Run the below command on either of the three nodes.

Finally the control plane of the cloud must be resized to make use of
the second and third nodes, providing resilience and expanded capacity:

::

   sunbeam cluster resize

Configure the cloud
-------------------

Now configure the deployed cloud using the ``configure`` command on the
bootstrap node:

::

   sunbeam configure --openrc demo-openrc

The ``--openrc`` option specifies a regular user (non-admin) cloud init
file (``demo-openrc`` here).

A series of questions will now be asked. Below is a sample output of an
entire interactive session. The values in square brackets, when present,
provide acceptable values. A value in parentheses is the default value.
Here we use the values given earlier:

[note type=“positive”] The first question relates to local or remote VM
access. For a multi-node cloud such as this one, remote access is a
necessity. [/note]

.. code:: text

   Local or remote access to VMs [local/remote] (local): remote
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
   Free network interface that will be configured for external traffic [eno1/eno2] (eno1): eno2

Any remote hosts intending to connect to VMs on this node must have
connectivity with the interface selected for external traffic (last
question above).

These questions are explained in more detail on the :ref:`Interactive configuration prompts`.

Launch a VM
-----------

Run the below command on either of the three nodes.

Verify the cloud by launching a VM called ‘test’ based on the ‘ubuntu’
image (Ubuntu 22.04 LTS). The `launch` command is used:

::

   sunbeam launch ubuntu --name test

Sample output:

.. code:: text

   Launching an OpenStack instance ...
   Access instance with `ssh -i /home/ubuntu/.config/openstack/sunbeam ubuntu@172.16.2.200`

.. note::
   Since “remote” access to VMs has been configured, you won’t be able to SSH into them from any
   of the nodes in the cluster. Copy the private key given in the above output from the
   launching node to an external machine with an access to the 172.16.2.0/24 network. Note that
   the VM will not be ready instantaneously; waiting time is mostly determined by the cloud’s
   available resources.

Related how-to guides
---------------------

Now that OpenStack is set up, be sure to check out the following how-to
guides:

-  :ref:`Accessing the OpenStack dashboard`
-  :ref:`Using the OpenStack CLI`
