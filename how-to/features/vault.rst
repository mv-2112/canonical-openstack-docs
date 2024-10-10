Vault
=====

This feature deploys Vault, a tool for securely managing secrets used in
modern computing (e.g. passwords, certificates, API keys).

Enabling Vault
--------------

To enable Vault, simply run the following command:

::

   sunbeam enable vault

[note type=“information”] **Information**: Vault is automatically
unsealed when enabled. [/note]

Disabling Vault
---------------

To disable Vault, simply run the following command:

::

   sunbeam disable vault

[note type=“caution”] **Caution**: Disabling Vault will completely
remove it from the infrastructure, all secrets will be lost. [/note]

Unsealing Vault
---------------

[note type=“information”] **Information**: This feature has been removed
in channel **2023.1/edge** and **2023.2**. [/note]

If Vault has been sealed, you can use the following command to unseal
Vault:

::

   sunbeam vault unseal
