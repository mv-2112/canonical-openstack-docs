Removing the primary node
=========================

Removing the primary node refers to the removal of the initial
(bootstrap) node.

[note type=“warning”] **Warning:** Removing the primary node will
destroy the entire MicroStack deployment. [/note]

If the deployment consists of multiple nodes then remove all non-primary
nodes **before** removing the primary node. See page :doc:`Scaling down the
cluster </how-to/misc/scaling-the-cluster-in>` for instructions on doing that.

Remove components from the node
-------------------------------

Software components need to removed from the node. Perform all the below
steps on the primary node.

Remove the Juju model:

::

   juju destroy-model --destroy-storage --no-prompt --force --no-wait openstack

Remove the Juju controller:

::

   juju destroy-controller --no-prompt --destroy-storage  --force --no-wait sunbeam-controller

Remove the Juju agent:

::

   sudo /sbin/remove-juju-services

Remove the Juju snap:

::

   sudo snap remove --purge juju

Remove Juju configuration:

::

   rm -rf ~/.local/share/juju
   sudo rm -rf /var/lib/juju/dqlite
   sudo rm -rf /var/lib/juju/system-identity
   sudo rm -rf /var/lib/juju/bootstrap-params

Remove the OpenStack hypervisor and OpenStack snaps:

::

   sudo snap remove --purge openstack-hypervisor
   sudo snap remove --purge openstack

Remove openstack snap configuration:

::

   rm -rf ~/.local/share/openstack

Remove MicroK8s snap:

::

   sudo microk8s leave
   sudo snap remove --purge microk8s

The above steps can take a few minutes to complete.

Remove the MicroCeph snap:

::

   sudo microceph disk list
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
