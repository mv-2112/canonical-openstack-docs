Bootstrap LXD based Juju controller on single node
##################################################

Canonical OpenStack clouds deployed using the manual bare metal provider requires a local
LXD Juju controller. This controller will be used during the bootstrap process to setup
machine models and k8s. Once k8s is deployed, new Juju controller will be bootstrapped on
k8s and the necessary models will be migrated from LXD Juju controller.

This how-to guide provides all necessary information on how to perform aforementioned actions.

Set up LXD
++++++++++

Bootstrap LXD
-------------

To bootstrap LXD, execute the ``lxd init`` command on the node:

.. code-block :: text

   sudo lxd init

When prompted, answer some interactive questions. Below is a sample output from the *cloud-1* machine from the example configuration:

.. code-block :: text

   Would you like to use LXD clustering? (yes/no) [default=no]: no
   Do you want to configure a new storage pool? (yes/no) [default=yes]: yes
   Name of the new storage pool [default=default]: default
   Name of the storage backend to use (ceph, dir, lvm, powerflex, zfs, btrfs) [default=zfs]: zfs
   Create a new ZFS pool? (yes/no) [default=yes]: yes
   Would you like to use an existing empty block device (e.g. a disk or partition)? (yes/no) [default=no]: no
   Size in GiB of the new loop device (1GiB minimum) [default=15GiB]: 15GiB
   Would you like to connect to a MAAS server? (yes/no) [default=no]: no
   Would you like to create a new local network bridge? (yes/no) [default=yes]: yes
   What should the new bridge be called? [default=lxdbr0]: br0
   What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: auto
   What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: auto
   Would you like the LXD server to be available over the network? (yes/no) [default=no]: no
   Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: yes
   Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: no

Refer to the `LXD documentation <https://documentation.ubuntu.com/lxd/en/latest/>`_ for detailed description of each of those questions and some examples.

.. note::
   Change the bridge name, the default lxdbr0 fail to add current node to juju machine model.

LXD can be bootstrapped with a minimal preseed file:

.. code-block :: text

   cat <<EOF | sudo lxd init --preseed
   networks:
   - config:
       ipv4.address: auto
       ipv6.address: none
     name: br0
     project: default
   storage_pools:
   - name: default
     driver: dir
    profiles:
    - devices:
        eth0:
          name: eth0
          network: br0
          type: nic
        root:
          path: /
          pool: default
          type: disk
      name: default
   EOF

Bootstrap Juju controller
+++++++++++++++++++++++++

In the next step, bootstrap `Juju controller <https://juju.is/>`_ on the node.

Install the snap
----------------

Then, install the ``juju`` snap:

.. code-block :: text

   sudo snap install juju

Bootstrap Juju controller
-------------------------

Then, bootstrap the juju controller with name *mycloud*:

.. code-block :: text

   juju bootstrap localhost mycloud
