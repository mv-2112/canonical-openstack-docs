Architecture
============

Topology
--------

MicroStack can be deployed as a single-node cloud or can span across
multiple nodes to provide capacity and resilience in a multi-node
solution.

Single-node
~~~~~~~~~~~

In a single-node deployment, all of the components of the deployed
OpenStack Cloud reside on the same node; networking is typically
configured so that access to the OpenStack APIs, Dashboard and
Instances is local and can only be access directly from the node being
used.

.. figure:: sunbeam-single-node.png
   :alt: single node architecture

In a single-node deployment the node will assume all roles - control,
compute and storage. Note that the use of the storage role currently
requires pristine, un-partitioned block devices on the node being used.

It is possible to deploy a single-node cloud with remote access to
control plane services and instances - this is the ‘remote’ option for
instance networking and requires a range of IP addresses for K8s
load-balancer use on the network upon which the node being used resides -
see the guided deployment tutorial for examples on how to do this.

Multi-node
~~~~~~~~~~

Multi-node deployments all start off as single-node deployments;
additional nodes are added to the cloud to expand the capacity and
resilience of the control plane and add additional capacity and
resilience to the compute and storage components.

.. figure:: sunbeam-multi-node.png
   :alt: multi-node architecture

[note] **Important:** High availability, for both the control plane and
the data plane, is only achievable with a minimum of three nodes.
[/note]

Node roles
----------

Each node in a deployment can assume one or more roles; this allows for
different topologies to be created where CPU, memory, and storage
resources are either shared between roles or dedicated to specific
roles.

Juju Controller
~~~~~~~~~~~~~~~

The juju-controller role encapsulates the function the juju controller
serves in the deployment. It is responsible for creating and managing
models as well as the deployment and scaling of charmed applications.
In a multi-node deployment the Juju controller is deployed in a separate
environment to ensure a separation between control plane and workload
tasks.

Sunbeam
~~~~~~~

The sunbeam role provides the operational framework and integration
layer for OpenStack in the deployment. It serves as the mechanism to
deploy, coordinate, and manage OpenStack components across the
underlying infrastructure.

Control
~~~~~~~

The control role encapsulates the control plane of the cloud;
specifically this includes Kubernetes (deployed using MicroK8s) and the
Charmed K8s Operators that are used for the OpenStack control plane
components - API and RPC services, Database and Messaging.

Compute
~~~~~~~

The compute role encapsulates the hypervisor component on the cloud;
specifically this includes the Nova Compute service, the Libvirt/QEMU
virtualisation stack, OVN (Open Virtual Network) and OVS (Open vSwitch)
services for software defined networking and a Neutron service for
provision of metadata to instances.

Nodes with the compute role should be hardware virtualisation (KVM)
capable otherwise user-space emulation is used for instances which has a
significant performance impact.

Storage
~~~~~~~

The storage role encapsulates the software defined storage component of
the cloud; this is provided by Ceph which provides a massively scalable
storage solution using commodity hardware and is deployed in the form
MicroCeph.

Nodes with the storage role must have free, un-partitioned disks for use
by Ceph.

MySQL configuration
-------------------

In order to support the scalability and resilience needs of the
OpenStack control plane the database component of the control plane can
be deployed in two different configurations. This is automatically
selected during the bootstrap of the initial node in the cluster but can
be overridden if required.

Single instance
~~~~~~~~~~~~~~~

For smaller, single node deployments a single MySQL service is deployed
to support all OpenStack services. As this configuration limits the
scalability of the control plane it is only recommended for small
single-node deployments and is automatically selected when bootstrapping
an initial node with less than 32GiB of RAM.

Multiple instances
~~~~~~~~~~~~~~~~~~

For larger, multi-node deployments a MySQL service is deployed for each
OpenStack service; each MySQL service will also be automatically scaled
for high-availability as the cluster is resized during deployment. This
configuration provides a more scalable approach to deployment of the
control plan and is automatically selected when bootstrapping an initial
node with more than 32GiB of RAM.
