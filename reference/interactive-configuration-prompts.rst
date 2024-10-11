.. _Interactive configuration prompts:

Interactive configuration prompts
=================================

The ``sunbeam cluster bootstrap``, ``sunbeam configure``, and
``sunbeam cluster join`` commands may involve interactive prompts. This
allows the user to specify hardware and networking options particular to
their environment. This reference page provides a detailed description
of each prompt (question).

[note type=“note”] **Note:** Some of these questions’ values can be
provided by means of a manifest file passed to the
``sunbeam cluster bootstrap`` and ``sunbeam configure`` commands. See
the `Deployment manifest </t/42672>`__ page. [/note]

Below are all of the questions that can potentially be asked. What
questions get asked are sometimes dependent upon the response given in a
preceding question.

+-----------------------------------+-----------------------------------+
| Questions                         | Meaning                           |
+===================================+===================================+
| **Use proxy to access external    | Whether to enable usage of a      |
| network resources?**              | network proxy that may constrain  |
|                                   | network traffic. If ‘Yes’, the    |
|                                   | default values for the ensuing    |
|                                   | questions will be populated from  |
|                                   | the settings that should reside   |
|                                   | in the local ``/etc/environment`` |
|                                   | file.                             |
+-----------------------------------+-----------------------------------+
| **Enter value for http_proxy**    | *Question will appear if proxy    |
|                                   | usage is enabled.*\ Example       |
|                                   | value:                            |
|                                   | ``http://squid.proxy:3128``       |
+-----------------------------------+-----------------------------------+
| **Enter value for https_proxy**   | *Question will appear if proxy    |
|                                   | usage is enabled.*\ Example       |
|                                   | value:                            |
|                                   | ``http://squid.proxy:3128``       |
+-----------------------------------+-----------------------------------+
| **Enter value for no_proxy**      | *Question will appear if proxy    |
|                                   | usage is enabled.*\ Example       |
|                                   | value:                            |
|                                   | ``localhost,127.0.0.1,localhos    |
|                                   | t,10.121.193.0/24,10.20.21.0/27`` |
+-----------------------------------+-----------------------------------+
| **Management networks shared by   | The network addresses of the      |
| the hosts (CIDRs, separated by    | subnets used for management       |
| comma)**                          | traffic. This is typically the    |
|                                   | same as the network used by the   |
|                                   | machines supporting the           |
|                                   | deployment. Multiple CIDRs can be |
|                                   | specified, separated by commas.   |
+-----------------------------------+-----------------------------------+
| **MetalLB address allocation      | The range of IP addresses to be   |
| range (supports multiple ranges,  | allocated by MetalLB for access   |
| comma separated)**                | to the control plane (OpenStack   |
|                                   | APIs). This range should contain  |
|                                   | at least ten addresses and must   |
|                                   | not overlap with the external     |
|                                   | network CIDR. This configuration  |
|                                   | supports IP ranges and network    |
|                                   | CIDRs. To access the APIs from a  |
|                                   | remote host, the range must       |
|                                   | reside within the subnet that the |
|                                   | primary network interface is on.  |
+-----------------------------------+-----------------------------------+
| **Local or remote access to VMs** | If ‘local’ is selected then VMs   |
|                                   | will **only** be accessible from  |
|                                   | the local host, whereas if        |
|                                   | ‘remote’ is selected then VMs     |
|                                   | will **only** be accessible from  |
|                                   | remote hosts.For the remote case, |
|                                   | you will subsequently be asked to |
|                                   | specify what network interface to |
|                                   | dedicate to VM access traffic.    |
|                                   | The intended remote hosts must    |
|                                   | have connectivity to this         |
|                                   | interface.                        |
+-----------------------------------+-----------------------------------+
| **CIDR of OpenStack external      | *Question will appear for local   |
| network - arbitrary but must not  | access only.*\ CIDR of network    |
| be in use**                       | for assigning addresses to VMs    |
|                                   | intended to be accessed from the  |
|                                   | local host. It is arbitrary but   |
|                                   | should not conflict with another  |
|                                   | network known to the host.        |
+-----------------------------------+-----------------------------------+
| **CIDR of network to use for      | *Question will appear for remote  |
| external networking**             | access only.*\ CIDR of network    |
|                                   | for assigning addresses to VMs    |
|                                   | intended to be accessed from      |
|                                   | remote hosts. It represents a     |
|                                   | portion of the physical network   |
|                                   | (outside of the OpenStack         |
|                                   | installation) whose IP addresses  |
|                                   | are dedicated for this            |
|                                   | purpose.\ **Caution:** If you     |
|                                   | opted to access OpenStack APIs    |
|                                   | externally in the ``bootstrap``   |
|                                   | step **and** you opted to access  |
|                                   | OpenStack VMs externally in the   |
|                                   | current ``configure`` step,       |
|                                   | ensure that these ranges do not   |
|                                   | overlap.                          |
+-----------------------------------+-----------------------------------+
| **IP address of default gateway   | *Question will appear for remote  |
| for external network**            | access only.*\ IP address of      |
|                                   | existing default gateway for      |
|                                   | external network.                 |
+-----------------------------------+-----------------------------------+
| **Populate OpenStack cloud with   | Whether the cloud is to be        |
| demo user, default images,        | pre-populated with common         |
| flavours, etc.**                  | OpenStack artefacts. In most      |
|                                   | cases this is desirable.          |
+-----------------------------------+-----------------------------------+
| **Username to use for access to   | The username of the demo user.    |
| OpenStack**                       |                                   |
+-----------------------------------+-----------------------------------+
| **Password to use for access to   | The password for the demo user.   |
| OpenStack**                       | The default password is randomly  |
|                                   | generated.                        |
+-----------------------------------+-----------------------------------+
| **Network range to use for        | CIDR of the private network for   |
| project network**                 | the demo user’s project. This is  |
|                                   | typically a network that is not   |
|                                   | routable (e.g. RFC 1918) network  |
|                                   | such as 192.168.122.0/24.         |
+-----------------------------------+-----------------------------------+
| **Enable ping and SSH access to   | Whether security group rules are  |
| instances**                       | to be added that allow ICMP and   |
|                                   | SSH traffic to reach VMs. In most |
|                                   | cases this is desirable.          |
+-----------------------------------+-----------------------------------+
| **Start of IP allocation range    | A portion of the IP addresses in  |
| for external network**            | the external network specified    |
|                                   | earlier will be reserved for      |
|                                   | allocation to VMs. This is the IP |
|                                   | address at the **start** of that  |
|                                   | range.                            |
+-----------------------------------+-----------------------------------+
| **End of IP allocation range for  | A portion of the IP addresses in  |
| external network**                | the external network specified    |
|                                   | earlier will be reserved for      |
|                                   | allocation to VMs. This is the IP |
|                                   | address at the **end** of that    |
|                                   | range.                            |
+-----------------------------------+-----------------------------------+
| **List of nameservers guests      | A list of DNS server IP addresses |
| should use for DNS resolution**   | (comma separated) that should be  |
|                                   | used for external DNS resolution  |
|                                   | from cloud instances.             |
+-----------------------------------+-----------------------------------+
| **Network type for access to      | Network type for access to        |
| external network**                | external network - ‘flat’         |
|                                   | (untagged) or ‘vlan’ (tagged).    |
+-----------------------------------+-----------------------------------+
| **VLAN ID to use for external     | *Question will appear for VLAN    |
| network**                         | network type only.*\ VLAN ID to   |
|                                   | use for the external network.     |
+-----------------------------------+-----------------------------------+
| **Free network interface that     | *Question will appear for remote  |
| will be configured for external   | access only.*\ The network        |
| traffic**                         | interface used for external       |
|                                   | access to VMs. The interface      |
|                                   | should be connected to an         |
|                                   | appropriate physical network.     |
|                                   | Detected unconfigured (free)      |
|                                   | interfaces will be listed as      |
|                                   | acceptable values. However, an    |
|                                   | interface not appearing in the    |
|                                   | list can still be entered.Remote  |
|                                   | hosts intending to access VMs     |
|                                   | must be able to contact this      |
|                                   | interface.                        |
+-----------------------------------+-----------------------------------+
