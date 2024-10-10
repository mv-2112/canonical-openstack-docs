Scaling down the cluster
========================

Scaling down the cluster refers to the removal of cluster members.

These instructions show how to remove a node with the exception of the
primary (bootstrap) node. For instructions on the latter, see page
`Removing the primary node </t/39001>`__. Note that, in terms of
removing nodes, the primary node is always the last to be removed.

Remove the node
---------------

On the primary node:

::

   sunbeam cluster remove --name <node FQDN>

[note type=“note”] **Note:** A current software issue (fixed in the
``edge`` risk level) causes the ``cluster remove`` command to fail with
a message like:

``ERROR removing machine failed: machine 1 has unit "sunbeam-machine/1"  assigned``.

To work around this manually remove the unit in the error message by
issuing:

::

   juju remove-unit sunbeam-machine/1

The ``cluster remove`` command can then be reissued.

Repeat the workaround as needed. [/note]

Remove components from the node
-------------------------------

Software components now need to removed from the target node. Perform
all the below steps on the target node.

Remove the Juju agent:

::

   sudo /sbin/remove-juju-services

Remove the **juju** snap:

::

   sudo snap remove --purge juju

Remove Juju configuration:

::

   rm -rf ~/.local/share/juju

Remove the **openstack-hypervisor** and **openstack** snaps:

::

   sudo snap remove --purge openstack-hypervisor
   sudo snap remove --purge openstack

Remove openstack snap configuration:

::

   rm -rf ~/.local/share/openstack

Remove the **microk8s** snap:

::

   sudo microk8s leave
   sudo snap remove --purge microk8s

The above steps can take a few minutes to complete.

Remove the disk(s) used by **microceph** on this node:

::

   sudo microceph disk list
   sudo microceph disk remove <OSD on this node>

Remove the **microceph** snap:

::

   sudo snap remove --purge microceph

If required clean the disk(s) identified in the earlier command:

::

   sudo dd if=/dev/zero of=<DISK PATH> bs=4M count=10

[note type=“caution”] **Caution:** The ``dd`` command will result in the
permanent erasure of data. It is vital that you have specified the
correct disk path to avoid unintended data loss. [/note]

Clear the remaining network configuration with a reboot:

::

   sudo reboot
