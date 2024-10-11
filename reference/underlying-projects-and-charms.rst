Projects and charms
===================

MicroStack can be organised in terms of a number of underlying projects
as well as in terms of the individual charmed operators it leverages.

Projects
--------

There are core, dependency, and extended dependency projects.

Core
~~~~

.. list-table::
  :class: names
  :header-rows: 1

  * - Project
    - Source Code
    - Bug Report
  * - MicroCeph Charm
    - `Source <https://github.com/canonical/charm-microceph/>`__
    - `Bugs <https://bugs.launchpad.net/charm-microceph/>`__
  * - OpenStack Snap
    - `Source <https://github.com/canonical/snap-openstack.git>`__
    - `Bugs <https://bugs.launchpad.net/charm-rabbitmq-k8s>`__
  * - Openstack Hypervisor Snap
    - `Source <https://github.com/canonical/snap-openstack-hypervisor.git>`__
    - `Bugs <https://bugs.launchpad.net/snap-openstack-hypervisor>`__
  * - RabbitMQ Charm
    - `Source <https://github.com/openstack-charmers/charm-rabbitmq-k8s.git>`__
    - `Bugs <https://bugs.launchpad.net/charm-rabbitmq-k8s>`__
  * - Sunbeam Charms
    - `Source <https://opendev.org/openstack/sunbeam-charms.git>`__
    - `Bugs <https://bugs.launchpad.net/sunbeam-charms>`__
  * - Sunbeam Terraform
    - `Source <https://github.com/canonical/sunbeam-terraform.git>`__
    - `Bugs <https://launchpad.net/sunbeam-terraform>`__
  * - Ubuntu OpenStack Rocks
    - `Source <https://github.com/canonical/ubuntu-openstack-rocks.git>`__
    - `Bugs <https://launchpad.net/ubuntu-openstack-rocks>`__

Dependencies
~~~~~~~~~~~~

.. list-table::
  :class: names
  :header-rows: 1

  * - Project
    - Source Code
    - Bug Report
  * - Juju
    - `Source <https://github.com/juju/juju.git>`__
    - `Bugs <https://bugs.launchpad.net/juju>`__
  * - MicroCeph
    - `Source <https://github.com/canonical/microceph.git>`__
    - `Bugs <https://github.com/canonical/microceph/issues>`__
  * - MicroK8s
    - `Source <https://github.com/canonical/microk8s.git>`__
    - `Bugs <https://github.com/canonical/microk8s/issues>`__
  * - MicroK8s Operator
    - `Source <https://github.com/canonical/charm-microk8s/tree/legacy>`__
    - `Bugs <https://github.com/canonical/charm-microk8s/issues>`__
  * - MySQL Kubernetes Operator
    - `Source <https://github.com/canonical/mysql-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/mysql-k8s-operator/issues>`__
  * - MySQL Router Kubernetes Operator
    - `Source <https://github.com/canonical/mysql-router-k8s-operator>`__
    - `Bugs <https://github.com/canonical/mysql-router-k8s-operator/issues>`__
  * - Self-Signed Certificates Operator
    - `Source <https://github.com/canonical/self-signed-certificates-operator>`__
    - `Bugs <https://github.com/canonical/self-signed-certificates-operator/issues>`__
  * - TLS Certificates Operator
    - `Source <https://github.com/canonical/tls-certificates-operator>`__
    - `Bugs <https://github.com/canonical/tls-certificates-operator/issues>`__
  * - Traefik Kubernetes Operator
    - `Source <https://github.com/canonical/traefik-k8s-operator>`__
    - `Bugs <https://github.com/canonical/traefik-k8s-operator/issues>`__


Extended dependencies
~~~~~~~~~~~~~~~~~~~~~

.. list-table::
  :class: names
  :header-rows: 1

  * - Project
    - Source Code
    - Bug Report
  * - Alertmanager Kubernetes Operator
    - `Source <https://github.com/canonical/alertmanager-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/alertmanager-k8s-operator/issues>`__
  * - BIND 9 Rock
    - `Source <https://git.launchpad.net/~ubuntu-docker-images/ubuntu-docker-images/+git/bind9>`__
    - `Bugs <https://bugs.launchpad.net/ubuntu-docker-images/+oci/bind9/+bugs>`__
  * - Catalogue Kubernetes Operator
    - `Source <https://github.com/canonical/catalogue-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/catalogue-k8s-operator/issues>`__
  * - Grafana Kubernetes Operator
    - `Source <https://github.com/canonical/grafana-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/grafana-k8s-operator/issues>`__
  * - Loki Kubernetes Operator
    - `Source <https://github.com/canonical/loki-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/loki-k8s-operator/issues>`__
  * - Prometheus Kubernetes Operator
    - `Source <https://github.com/canonical/prometheus-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/prometheus-k8s-operator/issues>`__
  * - Vault Kubernetes Operator
    - `Source <https://github.com/canonical/vault-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/vault-k8s-operator/issues>`__


Charms
------

Both Kubernetes charms and machine charms are available.

Configuration options are useful when a deployment manifest is in use.
See the `Deployment manifest </t/42672>`__ page.

Kubernetes charms
~~~~~~~~~~~~~~~~~

.. list-table::
  :header-rows: 1

  * - Charm
    - Configuration options
  * - `Alert Manager <https://charmhub.io/alertmanager-k8s>`__
    - `options <https://charmhub.io/alertmanager-k8s/configure>`__
  * - `Aodh <https://charmhub.io/aodh-k8s>`__
    - `options <https://charmhub.io/aodh-k8s/configure>`__
  * - `Barbican <https://charmhub.io/barbican-k8s>`__
    - `options <https://charmhub.io/barbican-k8s/configure>`__
  * - `Catalogue <https://charmhub.io/catalogue-k8s>`__
    - `options <https://charmhub.io/catalogue-k8s/configure>`__
  * - `Ceilometer <https://charmhub.io/ceilometer-k8s>`__
    - `options <https://charmhub.io/ceilometer-k8s/configure>`__
  * - `Cinder <https://charmhub.io/cinder-k8s>`__
    - `options <https://charmhub.io/cinder-k8s/configure>`__
  * - `Cinder-Ceph <https//charmhub.io/cinder-ceph-k8s>`__
    - `options <https://charmhub.io/cinder-ceph-k8s/configure>`__
  * - `Designate <https://charmhub.io/designate-k8s>`__
    - `options <https://charmhub.io/designate-k8s/configure>`__
  * - `Designate-BIND <https://charmhub.io/designate-bind-k8s>`__
    - `options <https://charmhub.io/designate-bind-k8s/configure>`__
  * - `Glance <https://charmhub.io/glance-k8s>`__
    - `options <https://charmhub.io/glance-k8s/configure>`__
  * - `Gnocchi <https://charmhub.io/gnocchi-k8s>`__
    - `options <https://charmhub.io/gnocchi-k8s/configure>`__
  * - `Grafana Agent <httpscharmhub.io/grafana-agent-k8s>`__
    - `options <https://charmhub.io/grafana-agent-k8s/configure>`__
  * - `Grafana <https://charmhub.io/grafana-k8s>`__
    - `options <https://charmhub.io/grafana-k8s/configure>`__
  * - `Heat <https://charmhub.io/heat-k8s>`__
    - `options <https://charmhub.io/heat-k8s/configure>`__
  * - `Horizon <https://charmhub.io/horizon-k8s>`__
    - `options <https://charmhub.io/horizon-k8s/configure>`__
  * - `Keystone <https://charmhub.io/keystone-k8s>`__
    - `options <https://charmhub.io/keystone-k8s/configure>`__
  * - `Keystone LDAP <https://charmhub.io/keystone-ldap-k8s>`__
    - `options <https://charmhub.io/keystone-ldap-k8s/configure>`__
  * - `Loki <https://charmhub.io/loki-k8s>`__
    - `options <https://charmhub.io/loki-k8s/configure>`__
  * - `Manual TLS Certificates <https://charmhub.io/manual-tls-certificates>`__
    - `options <https://charmhub.io/manual-tls-certificates/configure>`__
  * - `Magnum <https://charmhub.io/magnum-k8s>`__
    - `options <https://charmhub.io/magnum-k8s/configure>`__
  * - `MySQL <https://charmhub.io/mysql-k8s>`__
    - `options <https://charmhub.io/mysql-k8s/configure>`__
  * - `MySQL Router <https:///charmhub.io/mysql-router-k8s>`__
    - `options <https://charmhub.io/mysql-router-k8s/configure>`__
  * - `Neutron <https://charmhub.io/neutron-k8s>`__
    - `options <https://charmhub.io/neutron-k8s/configure>`__
  * - `Nova <https://charmhub.io/nova-k8s>`__
    - `options <https://charmhub.io/nova-k8s/configure>`__
  * - `Octavia <https://charmhub.io/octavia-k8s>`__
    - `options <https://charmhub.io/octavia-k8s/configure>`__
  * - `OpenStack Exporter <https://charmhub.io/openstack-exporter-k8s>`__
    - `options <https://charmhub.io/openstack-exporter-k8s/configure>`__
  * - `OVN Central <https://charmhub.io/ovn-central-k8s>`__
    - `options <https://charmhub.io/ovn-central-k8s/configure>`__
  * - `OVN Relay <https://charmhub.io/ovn-relay-k8s>`__
    - `options <https://charmhub.io/ovn-relay-k8s/configure>`__
  * - `Placement <https://charmhub.io/placement-k8s>`__
    - `options <https://charmhub.io/placement-k8s/configure>`__
  * - `Prometheus <https://charmhub.io/prometheus-k8s>`__
    - `options <https://charmhub.io/prometheus-k8s/configure>`__
  * - `RabbitMQ <https://charmhub.io/rabbitmq-k8s>`__
    - `options <https://charmhub.io/rabbitmq-k8s/configure>`__
  * - `Self-signed Certificates <https://charmhub.io/self-signed-certificates>`__
    - `options <https://charmhub.io/self-signed-certificates/configure>`__
  * - `Tempest <https://charmhub.io/tempest-k8s>`__
    - `options <https://charmhub.io/tempest-k8s/configure>`__
  * - `Traefik <https://charmhub.io/traefik-k8s>`__
    - `options <https://charmhub.io/traefik-k8s/configure>`__
  * - `Vault <https://charmhub.io/vault-k8s>`__
    - `options <https://charmhub.io/vault-k8s/configure>`__

Machine charms
~~~~~~~~~~~~~~

.. list-table::
  :header-rows: 1

  * - Charm
    - Configuration options
  * - `Grafana Agent <https://charmhub.io/grafana-agent>`__
    - `options <https://charmhub.io/grafana-agent/configure>`__
  * - `MicroCeph <https://charmhub.io/microceph>`__
    - `options <https://charmhub.io/microceph/configure>`__
  * - `MicroK8s <https://charmhub.io/microk8s>`__
    - `options <https://charmhub.io/microk8s/configure>`__
  * - `OpenStack Hypervisor <https://charmhub.io/openstack-hypervisor>`__
    - `options <https://charmhub.io/openstack-hypervisor/configure>`__
  * - `Sunbeam Clusterd <https://charmhub.io/sunbeam-clusterd>`__
    - `options <https://charmhub.io/sunbeam-clusterd/configure>`__
  * - `Sunbeam Machine <https://charmhub.io/sunbeam-machine>`__
    - `options <https://charmhub.io/sunbeam-machine/configure>`__
