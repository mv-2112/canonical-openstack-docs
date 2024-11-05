Observability
=============

This feature integrates MicroStack with (and optionally deploys) the
`Canonical Observability Stack
(COS) <https://charmhub.io/topics/canonical-observability-stack>`__.
MicroStack will automatically propagate default metrics and dashboards,
enabling you to effortlessly monitor the status of your single-node or
multi-node deployment of Sunbeam without the need for any additional
setup.

This feature provides ability to connect to an existing external COS or
deploy COS within the cloud.

Connect to an existing COS
--------------------------

Enabling Observability
~~~~~~~~~~~~~~~~~~~~~~

:doc:`Register the external controller </how-to/misc/manage-external-juju-controllers>`
hosting COS in MicroStack.

Ensure the Juju user has consume permissions granted on the
observability related offers.

To enable MicroStack observability integration, run the following
command:

::

   sunbeam enable observability external CONTROLLER GRAFANA_DASHBOARD_OFFER_URL PROMETHEUS_RECEIVE_REMOTE_WRITE_OFFER_URL LOKI_LOGGING_OFFER_URL

``CONTROLLER`` is the name of external Juju controller that hosts COS.

``GRAFANA_DASHBOARD_OFFER_URL`` is the remote Offer URL for Grafana
Dashboard.

``PROMETHEUS_RECEIVE_REMOTE_WRITE_OFFER_URL`` is the remote Offer URL
for Prometheus.

``LOKI_LOGGING_OFFER_URL`` is the remote Offer URL for Loki.

Disabling Observability
~~~~~~~~~~~~~~~~~~~~~~~

To disable Observability, run the following command:

::

   sunbeam disable observability external

Deploy COS in MicroStack
------------------------

.. _enabling-observability-1:

Enabling Observability
~~~~~~~~~~~~~~~~~~~~~~

To enable Observability, run the following command:

::

   sunbeam enable observability internal

.. _disabling-observability-1:

Disabling Observability
~~~~~~~~~~~~~~~~~~~~~~~

To disable Observability, run the following command:

::

   sunbeam disable observability internal

Retrieve Grafana dashboard URL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To get the URL of the dashboard use the ``dashboard-url`` command:

::

   sunbeam observability dashboard-url

Sample output:

::

   http://10.20.21.13/observability-grafana

This URL points to the Grafana login page. The credentials to use can be
retrieved using this command:

::

   juju run --model observability grafana/leader get-admin-password

Sample output:

.. code:: text

   Running operation 1 with 1 task
     - task 2 on unit-grafana-0

   Waiting for task 2...
   admin-password: ******
   url: http://10.20.21.13/observability-grafana

[note type=“note” status=“Information”] Only the initial admin password
is displayed in the above action. If the admin password is changed using
the Grafana UI, a message
``Admin password has been changed by an administrator`` will be
displayed. [/note]

Login Grafana dashboard
-----------------------

Once the COS model is deployed, you can use the Grafana dashboard to
view the metrics and alerts configured. The login page asks for the
following information:

**Email or username:** admin **Password:** \*****\*

The login page looks like this:

.. figure:: https://lh5.googleusercontent.com/O8QceGdUptYfyKOIk5oAUj4ElkpbC5BuXOVzgvd_G1DlNQnVuNT19H3Wm6g2eGdHudVOmUIa-6x30if4p1iUDB6bNmNcSkRRVX6VCf3rZv8yXmysteFwXNCVXIl3YRCfIynQOvpmubvaVgePC-fRzzo
   :alt: \|624x335

   \|624x335

After a successful login, you should see the landing page:

.. figure:: https://lh6.googleusercontent.com/WL_kptpJHJm4DwOr7K_wuckTFOz761hdYfhHRPkfxE6wxehsjoGco1dC2t-DmsU_iLg9v6Pjrk51Gizv_NbmZsgCmbMwbOwuhbo10Rr23qhPJ3AURIc9UPQBlIZV5mzutB0Qr45ckA-xvg1kDEqizOQ
   :alt: \|624x335

   \|624x335

You can now look at the different dashboards configured.

.. figure:: https://lh7-us.googleusercontent.com/RPatT1lEIZA9jEXk2wG9DvPLtYRGsZCXNXScmWmAAXSdLdgiVXxAf1NT8HJHms7LngYcNijAhcgZDvfOVYxPgJOBz9L4AVuqSo_DwHy_3EZiqUlOt-8M8X1nfZGKp3FCSVlEypiW09V6IoA8cMHhLlY
   :alt: \|624x343

   \|624x343

Dashboard
---------

[note type=“info”] **Note:** Dashboard is currently only supported in
channel ``2023.2`` of the **:spelling:ignore:`openstack`** snap. [/note]

OpenStack Service Overview dashboard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a dashboard providing an overview of the OpenStack services and
stats.

.. figure:: upload://oYViUcJhxOorEZMO3KPxeidLMsR.jpeg
   :alt: 1713510393|800x386

   1713510393|800x386

OpenStack Cloud Usage dashboard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a dashboard providing information on the usage of the OpenStack
cloud (for example, projects and virtual machines), using metrics mostly from
`openstack-exporter <https://github.com/openstack-exporter/openstack-exporter>`__.

.. figure:: upload://qXotBvFlYbwssrcVTzH8EcvQYr7.png
   :alt: 1715301156|800x406

   1715301156|800x406

OpenStack Compute Overview dashboard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a dashboard more detailed information on the compute nodes,
using metrics mostly from the Libvirt exporter.

.. figure:: upload://84ANGD2FYcEnmyli0ZU3PEBk53B.png
   :alt: 1713510810|800x337

   1713510810|800x337

Capacity Dashboard
~~~~~~~~~~~~~~~~~~

**Capacity Dashboard** displays the overall capacity (storage, memory,
and CPU) of the MicroStack cluster, as well as the capacity of
individual MicroStack node.

.. figure:: upload://vLKlBFnI4L3Y8r4LKfLUIemnTYW.png
   :alt: capacity_dashboard-\|800x368

   capacity_dashboard-\|800x368

Days until storage / memory / CPU reaches threshold
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

“Days until storage / memory / CPU reaches 90%” shows the estimated days
until these resources reach 90% of their total capacity. This is a
linear projection based on the average usage over the past 360 days. If
the average usage is zero or negative, the panel will show “Stable”
because it’s not possible to estimate when they will be depleted. For
the overall capacity, this estimation is chosen to be the minimum value
across all MicroStack nodes. For example, if the projected days it will
take for storage consumption to reach 90% is about 80 days for node 1,
90 for node 2,, and “Stable” (i.e. not expected to run out given the
current trend) for node 3, then the panel will show “80” since node 1
will be the first one to exhaust its storage.

The node-specific panels estimate resource consumption only within the
given node.

.. figure:: upload://lIkSFG9DpRQUgvj4Y6O4v99hxPQ.png
   :alt: capacity_single_node|800x174

   capacity_single_node|800x174

[note type=“note” status=“Note”] You can filter the nodes using the
multi-select dropdown menu: **Hostname**. [/note]

[note type=“note” status=“Note”] The 90% threshold and the 360 days of
estimation can also be changed using the dropdown menu: **Resource Usage
Threshold** and **Days of Estimation**. [/note]

Disk usage
^^^^^^^^^^

“Disk usage (total size: …GB)” shows the usage of filesystems mounted on
the nodes. For the overall capacity, “Disk usage” shows the total usage
of all mounted filesystems for each node. The individual disk usage
capacity panel shows disk usage of each mounted filesystem on a
particular node.

Memory usage
^^^^^^^^^^^^

“Memory usage (total memory: …GB)” shows the total memory usage, memory
assigned to huge pages, and used huge pages memory. For the overall
capacity, “Memory usage” is summed over all MicroStack nodes. The
individual memory capacity panel shows the memory usage of a particular
node.

CPU usage
^^^^^^^^^

“CPU usage (total number of cores: …)” shows the CPU usage on the nodes.
For overall capacity, “CPU usage” shows the CPU usage of each node as
separate series. The individual CPU capacity panel shows the CPU usage
of a particular node.

OpenStack Project Overview dashboard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a dashboard that provides detailed information about a single
project, including limits and a table of virtual machines. It uses
metrics from openstack-exporter.

.. figure:: upload://jbnbbbK7zGm6J35zzBDlfUBwp0u.png
   :alt: 1717662978|800x450

   1717662978|800x450
