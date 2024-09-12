:relatedlinks: [Diátaxis](https://diataxis.fr/)

.. _home:

Canonical OpenStack documentation
==================================

MicroStack is a modern cloud solution that uses snaps, Juju, and Kubernetes to deploy and manage
OpenStack.

Snaps are used to deploy and perform major cluster operations where Juju charmed operators are
leveraged internally to manage individual cloud services. Traditional charms oversee the cloud
data plane and Kubernetes charms govern the cloud control plane.

Deploying and managing an OpenStack cloud is generally considered to be a challenging endeavour.
MicroStack reduces the complexity traditionally imposed upon cloud administrators by automating
cloud operations where possible and reaping the benefits of Kubernetes-based API services.

MicroStack is designed from the ground up to accommodate users of varying skill levels. It is
appropriate for public, regional, and private clouds, and can satisfy a wide range of use cases:
from small single-node development environments through to large multi-node enterprise-grade
solutions.

---------

.. rubric:: :h2:`In this documentation`

..  grid:: 1 1 2 2

   ..  grid-item:: :doc:`Tutorial <tutorial/index>`

       **Start here**: a hands-on introduction to Example Product for new users

   ..  grid-item:: :doc:`How-to guides <how-to/index>`

      **Step-by-step guides** covering key operations and common tasks

.. grid:: 1 1 2 2
   :reverse:

   .. grid-item:: :doc:`Reference <reference/index>`

      **Technical information** - specifications, APIs, architecture

   .. grid-item:: :doc:`Explanation <explanation/index>`

      **Discussion and clarification** of key topics

---------

Project and community
---------------------

MicroStack is an Open Source project that welcomes usage discussion, project feedback, and
especially contributions!

* Join the user forum or the chat group on Matrix
* We abide by the Ubuntu Code of Conduct
* Get involved in improving the software or the documentation

Don’t hesitate to reach out if you have questions about integrating MicroStack into your own cloud
project.

.. toctree::
   :hidden:
   :maxdepth: 2

   tutorial/index
   how-to/index
   reference/index
   explanation/index
