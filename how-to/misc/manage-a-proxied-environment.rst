.. _Manage a proxied environment:

Manage a proxied environment
============================

This page shows how to configure proxy settings for MicroStack. This is
required for an environment that has network egress traffic restrictions
placed upon it. These restrictions are typically implemented via a
corporate proxy server that is separate from the MicroStack deployment.

The proxy server itself must permit access to certain external
(internet) resources in order for MicroStack to deploy (and operate)
correctly. These resources are listed on the `Proxy ACL
access </t/43948>`__ reference page.

[note type=“note”] **Note:** A proxied environment is currently only
supported in channel ``2023.2/candidate`` and later of the **openstack**
snap. [/note]

Configure for the proxy at the OS level
---------------------------------------

The steps given in the following two sub-sections will allow a network
host to “talk” to your local proxy server.

[note type=“note”] **Important:** Perform the instructions on all of
your MicroStack nodes. Do this before installing your cluster (as
described on either the `Multi-node </t/35727>`__ or `Single-node
guided </t/35765>`__ pages). [/note]

Provide the initial settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set proxy values in the ``/etc/environment`` file via well-known
environment variables. Ensure to set the management CIDR and
metallb/loadbalancer CIDR in the NO_PROXY variable.

Below are example commands for providing these initial proxy settings:

::

   echo "HTTP_PROXY=http://squid.proxy:3128" | sudo tee -a /etc/environment
   echo "HTTPS_PROXY=http://squid.proxy:3128" | sudo tee -a /etc/environment
   echo "NO_PROXY=localhost,127.0.0.1,localhost,10.121.193.0/24,10.20.21.0/27" | sudo tee -a /etc/environment

Restart snapd
~~~~~~~~~~~~~

Restart ``snapd`` so that it becomes aware of the new settings in
``/etc/environment``:

::

   sudo snap restart snapd

This will allow snaps to be installed on the configured nodes.

Show proxy settings
-------------------

Run the following command to view the proxy settings:

::

   sunbeam proxy show

Here is sample output from the above command:

+------------+---------------------------------------------------------+
| Proxy      | Value                                                   |
| variable   |                                                         |
+============+=========================================================+
| HTTP_PROXY | http://10.121.193.112:3128                              |
+------------+---------------------------------------------------------+
| H          | http://10.121.193.112:3128                              |
| TTPS_PROXY |                                                         |
+------------+---------------------------------------------------------+
| NO_PROXY   | loca                                                    |
|            | lhost,127.0.0.1,localhost,10.121.193.0/24,10.20.21.0/27 |
+------------+---------------------------------------------------------+

Update proxy settings
---------------------

User can update the proxy settings at later point of time after
bootstrap is completed. To update the proxy settings, run the command

::

   sunbeam proxy set --http-proxy <> --https-proxy <> --no-proxy <> 

Clear proxy settings
--------------------

To clear the proxy settings, run the following command

::

   sunbeam proxy clear

The above command will clear the proxy settings in /etc/environment and
model-configs for sunbeam created models.
