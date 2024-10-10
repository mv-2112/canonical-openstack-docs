Proxy ACL access
================

For a network that is constrained by a proxy server, efforts will be
needed to ensure that MicroStack works as intended. See the `Manage a
proxied environment </t/43946>`__ page for guidance.

The proxy server itself must have ACL rules that permit all MicroStack
nodes to access the resources listed below.

================================ ==========================
Resource                         Description
================================ ==========================
streams.canonical.com            Juju agent packages
archive.ubuntu.com               Ubuntu archive packages
security.ubuntu.com              Ubuntu security packages
cloud-images.ubuntu.com          Cloud images
api.charmhub.io                  Juju charms
docker.io                        Container images
production.cloudflare.docker.com Container images
quay.io                          Container images
registry.k8s.io                  Container images
pkg.dev                          Container images
amazonaws.com                    Container images
registry.jujucharms.com          Container images
api.snapcraft.io                 Snaps
snapcraftcontent.com             Snaps
builds.coreos.fedoraproject.org  VM Image for Fedora CoreOS
download.cirros-cloud.net        VM Image for CirrOS
maas.io                          MAAS images [1]
contracts.canonical.com          Ubuntu Pro
================================ ==========================

[1] Only needed for deployments based on `MAAS <https://maas.io>`__.
