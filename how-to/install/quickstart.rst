Single-node quick start
=======================

This quick start guide shows how to install OpenStack (based on project Sunbeam) in the simplest
way possible. It will deploy an **OpenStack 2024.1** (Caracal) cloud.

Requirements
------------

You will need a single machine whose requirements are:

* physical or virtual machine running Ubuntu 22.04 LTS
* a multi-core amd64 processor ideally with 4+ cores
* a minimum of 16 GiB of free memory
* 50 GiB of SSD storage available on the root disk
* A static IP address for the for the node installing

.. warning::

   Any change in IP address of the local host machine will be detrimental to the deployment. If
   installing on a machine using non-statically assigned DHCP (e.g. a laptop roaming on WIFI),
   it is recommended to use a virtual machine for testing purposes.

.. note::
  If you are using a virtual machine with `Multipass <https://multipass.run>`__, you will
  need to ensure it has a fully qualified domain name set in the `/etc/hosts` file.
  See bug `Github #3277 <https://github.com/canonical/multipass/issues/3277>`__ for more details.

Deploy the cloud
----------------

Install the openstack snap
~~~~~~~~~~~~~~~~~~~~~~~~~~

Begin by installing the openstack snap:

.. code:: bash

    sudo snap install openstack --channel 2024.1/beta

.. tip::
   It is highly recommended to use the 2024.1 beta channel (e.g. `--channel 2024.1/beta`) which
   includes all the latest bug fixes and updates before the next stable release coming in Q4 2024.

Prepare the machine
~~~~~~~~~~~~~~~~~~~

Sunbeam can generate a script to ensure that the machine has all of the required dependencies installed and is configured correctly for use in OpenStack - you can review this script using:

.. code:: bash

    sunbeam prepare-node-script --bootstrap

or the script can be directly executed in this way:

.. code:: bash

    sunbeam prepare-node-script --bootstrap | bash -x && newgrp snap_daemon

The script will ensure some software requirements are satisfied on the host. In particular, it will:

* install `openssh-server` if it is not found
* configure password-less sudo for all commands for the current user (`NOPASSWD:ALL`)

Bootstrap the cloud
~~~~~~~~~~~~~~~~~~~

Deploy the OpenStack cloud using the `cluster bootstrap` command and accept software defaults:

.. code:: bash

    sunbeam cluster bootstrap --accept-defaults

Configure the cloud and obtain credentials
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now configure the deployed cloud using the `configure` command:

.. code:: bash

    sunbeam configure --accept-defaults --openrc demo-openrc

The `--openrc` option specifies a regular user (non-admin) cloud init file (`demo-openrc` here).

Launch a VM
-----------

Verify the cloud by launching a VM called 'test' based on the 'ubuntu' image (Ubuntu 22.04 LTS). The `launch` command is used:

.. code:: bash

    sunbeam launch ubuntu --name test

Sample output:

.. terminal::
   :user: ubuntu
   :host: sunbeam01
   :dir: /home/ubuntu/
   :input: sunbeam launch ubuntu --name test

    Launching an OpenStack instance ...
    Access instance with `ssh -i /home/ubuntu/.config/openstack/sunbeam ubuntu@10.20.20.200`


Connect to the VM over SSH using the provided command.

Related how-to guides
---------------------

Now that OpenStack is set up, be sure to check out the following how-to guides:

* :ref:`Accessing the OpenStack Dashboard`
* :ref:`Using the OpenStack CLI`
