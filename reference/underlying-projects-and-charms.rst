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
  :header-rows: 1

  * - Project
    - Source Code
    - Bug Report
  * - charm-microceph
    - `Source <https://github.com/canonical/charm-microceph/>`__
    - `Bugs <https://bugs.launchpad.net/charm-microceph/>`__
  * - charm-rabbitmq-k8s
    - `Source <https://github.com/openstack-charmers/charm-rabbitmq-k8s.git>`__
    - `Bugs <https://bugs.launchpad.net/charm-rabbitmq-k8s>`__
  * - snap-openstack
    - `Source <https://github.com/canonical/snap-openstack.git>`__
    - `Bugs <https://bugs.launchpad.net/charm-rabbitmq-k8s>`__
  * - snap-openstack-hypervisor
    - `Source <https://github.com/canonical/snap-openstack-hypervisor.git>`__
    - `Bugs <https://bugs.launchpad.net/snap-openstack-hypervisor>`__
  * - sunbeam-charms
    - `Source <https://opendev.org/openstack/sunbeam-charms.git>`__
    - `Bugs <https://bugs.launchpad.net/sunbeam-charms>`__
  * - sunbeam-terraform
    - `Source <https://github.com/canonical/sunbeam-terraform.git>`__
    - `Bugs <https://launchpad.net/sunbeam-terraform>`__
  * - ubuntu-openstack-rocks
    - `Source <https://github.com/canonical/ubuntu-openstack-rocks.git>`__
    - `Bugs <https://launchpad.net/ubuntu-openstack-rocks>`__

Dependencies
~~~~~~~~~~~~

.. list-table::
  :header-rows: 1

  * - Project
    - Source Code
    - Bug Report
  * - charm-microk8s
    - `Source <https://github.com/canonical/charm-microk8s/tree/legacy>`__
    - `Bugs <https://github.com/canonical/charm-microk8s/issues>`__
  * - Juju
    - `Source <https://github.com/juju/juju.git>`__
    - `Bugs <https://bugs.launchpad.net/juju>`__
  * - microceph
    - `Source <https://github.com/canonical/microceph.git>`__
    - `Bugs <https://github.com/canonical/microceph/issues>`__
  * - microk8s
    - `Source <https://github.com/canonical/microk8s.git>`__
    - `Bugs <https://github.com/canonical/microk8s/issues>`__
  * - mysql-k8s-operator
    - `Source <https://github.com/canonical/mysql-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/mysql-k8s-operator/issues>`__
  * - mysql-router-k8s
    - `Source <https://github.com/canonical/mysql-router-k8s-operator>`__
    - `Bugs <https://github.com/canonical/mysql-router-k8s-operator/issues>`__
  * - self-signed-certificates-operator
    - `Source <https://github.com/canonical/self-signed-certificates-operator>`__
    - `Bugs <https://github.com/canonical/self-signed-certificates-operator/issues>`__
  * - tls-certificates-operator
    - `Source <https://github.com/canonical/tls-certificates-operator>`__
    - `Bugs <https://github.com/canonical/tls-certificates-operator/issues>`__
  * - traefik-k8s-operator
    - `Source <https://github.com/canonical/traefik-k8s-operator>`__
    - `Bugs <https://github.com/canonical/traefik-k8s-operator/issues>`__


Extended dependencies
~~~~~~~~~~~~~~~~~~~~~

.. list-table::
  :header-rows: 1

  * - Project
    - Source Code
    - Bug Report
  * - alertmanager-k8s-operator
    - `Source <https://github.com/canonical/alertmanager-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/alertmanager-k8s-operator/issues>`__
  * - bind9-rock
    - `Source <https://git.launchpad.net/~ubuntu-docker-images/ubuntu-docker-images/+git/bind9>`__
    - `Bugs <https://bugs.launchpad.net/ubuntu-docker-images/+oci/bind9/+bugs>`__
  * - catalogue-k8s-operator
    - `Source <https://github.com/canonical/catalogue-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/catalogue-k8s-operator/issues>`__
  * - grafana-k8s-operator
    - `Source <https://github.com/canonical/grafana-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/grafana-k8s-operator/issues>`__
  * - loki-k8s-operator
    - `Source <https://github.com/canonical/loki-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/loki-k8s-operator/issues>`__
  * - prometheus-k8s-operator
    - `Source <https://github.com/canonical/prometheus-k8s-operator.git>`__
    - `Bugs <https://github.com/canonical/prometheus-k8s-operator/issues>`__
  * - vault-k8s-operator
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
  * - `alertmanager-k8s <https://charmhub.io/alertmanager-k8s>`__
    - `options <https://charmhub.io/alertmanager-k8s/configure>`__
  * - `aodh-k8s <https://charmhub.io/aodh-k8s>`__
    - `options <https://charmhub.io/aodh-k8s/configure>`__
  * - `barbican-k8s <https://charmhub.io/barbican-k8s>`__
    - `options <https://charmhub.io/barbican-k8s/configure>`__
  * - `catalogue-k8s <https://charmhub.io/catalogue-k8s>`__
    - `options <https://charmhub.io/catalogue-k8s/configure>`__
  * - `ceilometer-k8s <https://charmhub.io/ceilometer-k8s>`__
    - `options <https://charmhub.io/ceilometer-k8s/configure>`__
  * - `cinder-k8s <https://charmhub.io/cinder-k8s>`__
    - `options <https://charmhub.io/cinder-k8s/configure>`__
  * - `cinder-ceph-k8s <https//charmhub.io/cinder-ceph-k8s>`__
    - `options <https://charmhub.io/cinder-ceph-k8s/configure>`__
  * - `designate-k8s <https://charmhub.io/designate-k8s>`__
    - `options <https://charmhub.io/designate-k8s/configure>`__
  * - `designate-bind-k8s <https://charmhub.io/designate-bind-k8s>`__
    - `options <https://charmhub.io/designate-bind-k8s/configure>`__
  * - `glance-k8s <https://charmhub.io/glance-k8s>`__
    - `options <https://charmhub.io/glance-k8s/configure>`__
  * - `gnocchi-k8s <https://charmhub.io/gnocchi-k8s>`__
    - `options <https://charmhub.io/gnocchi-k8s/configure>`__
  * - `grafana-agent-k8s <httpscharmhub.io/grafana-agent-k8s>`__
    - `options <https://charmhub.io/grafana-agent-k8s/configure>`__
  * - `grafana-k8s <https://charmhub.io/grafana-k8s>`__
    - `options <https://charmhub.io/grafana-k8s/configure>`__
  * - `heat-k8s <https://charmhub.io/heat-k8s>`__
    - `options <https://charmhub.io/heat-k8s/configure>`__
  * - `horizon-k8s <https://charmhub.io/horizon-k8s>`__
    - `options <https://charmhub.io/horizon-k8s/configure>`__
  * - `keystone-k8s <https://charmhub.io/keystone-k8s>`__
    - `options <https://charmhub.io/keystone-k8s/configure>`__
  * - `keystone-ldap-k8s <https://charmhub.io/keystone-ldap-k8s>`__
    - `options <https://charmhub.io/keystone-ldap-k8s/configure>`__
  * - `loki-k8s <https://charmhub.io/loki-k8s>`__
    - `options <https://charmhub.io/loki-k8s/configure>`__
  * - `manual-tls-certificates <https://charmhub.io/manual-tls-certificates>`__
    - `options <https://charmhub.io/manual-tls-certificates/configure>`__
  * - `magnum-k8s <https://charmhub.io/magnum-k8s>`__
    - `options <https://charmhub.io/magnum-k8s/configure>`__
  * - `mysql-k8s <https://charmhub.io/mysql-k8s>`__
    - `options <https://charmhub.io/mysql-k8s/configure>`__
  * - `mysql-router-k8s <https:///charmhub.io/mysql-router-k8s>`__
    - `options <https://charmhub.io/mysql-router-k8s/configure>`__
  * - `neutron-k8s <https://charmhub.io/neutron-k8s>`__
    - `options <https://charmhub.io/neutron-k8s/configure>`__
  * - `nova-k8s <https://charmhub.io/nova-k8s>`__
    - `options <https://charmhub.io/nova-k8s/configure>`__
  * - `octavia-k8s <https://charmhub.io/octavia-k8s>`__
    - `options <https://charmhub.io/octavia-k8s/configure>`__
  * - `openstack-exporter-k8s <https://charmhub.io/openstack-exporter-k8s>`__
    - `options <https://charmhub.io/openstack-exporter-k8s/configure>`__
  * - `ovn-central-k8s <https://charmhub.io/ovn-central-k8s>`__
    - `options <https://charmhub.io/ovn-central-k8s/configure>`__
  * - `ovn-relay-k8s <https://charmhub.io/ovn-relay-k8s>`__
    - `options <https://charmhub.io/ovn-relay-k8s/configure>`__
  * - `placement-k8s <https://charmhub.io/placement-k8s>`__
    - `options <https://charmhub.io/placement-k8s/configure>`__
  * - `prometheus-k8s <https://charmhub.io/prometheus-k8s>`__
    - `options <https://charmhub.io/prometheus-k8s/configure>`__
  * - `rabbitmqk8s <https://charmhub.io/rabbitmq-k8s>`__
    - `options <https://charmhub.io/rabbitmq-k8s/configure>`__
  * - `self-signed-certificates <https://charmhub.io/self-signed-certificates>`__
    - `options <https://charmhub.io/self-signed-certificates/configure>`__
  * - `tempest-k8s <https://charmhub.io/tempest-k8s>`__
    - `options <https://charmhub.io/tempest-k8s/configure>`__
  * - `traefik-k8s <https://charmhub.io/traefik-k8s>`__
    - `options <https://charmhub.io/traefik-k8s/configure>`__
  * - `vault-k8s <https://charmhub.io/vault-k8s>`__
    - `options <https://charmhub.io/vault-k8s/configure>`__

Machine charms
~~~~~~~~~~~~~~

.. list-table::
  :header-rows: 1

  * - `grafana-agent <https://charmhub.io/grafana-agent>`__
    - `options <https://charmhub.io/grafana-agent/configure>`__
  * - `microceph <https://charmhub.io/microceph>`__
    - `options <https://charmhub.io/microceph/configure>`__
  * - `microk8s <https://charmhub.io/microk8s>`__
    - `options <https://charmhub.io/microk8s/configure>`__
  * - `openstack-hypervisor <https://charmhub.io/openstack-hypervisor>`__
    - `options <https://charmhub.io/openstack-hypervisor/configure>`__
  * - `sunbeam-clusterd <https://charmhub.io/sunbeam-clusterd>`__
    - `options <https://charmhub.io/sunbeam-clusterd/configure>`__
  * - `sunbeam-machine <https://charmhub.io/sunbeam-machine>`__
    - `options <https://charmhub.io/sunbeam-machine/configure>`__
