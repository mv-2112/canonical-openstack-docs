Secrets as a Service
====================

This feature deploys `Barbican <https://docs.openstack.org/barbican>`__,
the OpenStack Key Manager service.

Enabling Secrets
----------------

To enable Secrets, run the following command:

::

   sunbeam enable secrets

The openstack CLI can now be used to manage Secrets. See the upstream
`Built-in Barbican
documentation <https://docs.openstack.org/python-barbicanclient/latest/cli/cli_usage.html#secret-create>`__
for details.

[note type=“note”] **Note**: The `Vault </t/37158>`__ feature is a
dependency of the Secrets feature. Make sure to enable it. [/note]

Disabling Secrets
-----------------

To disable Secrets, run the following command:

::

   sunbeam disable secrets

Usage
-----

Users need the role ``creator`` to be able to create / read / destroy
secrets.

Verify if a user belongs to this role with (admin rights needed):

::

   openstack role assignment list --user <user id> --role creator
   +----------------------------------+----------------------------------+-------+----------------------------------+--------+--------+-----------+
   | Role                             | User                             | Group | Project                          | Domain | System | Inherited |   
   +----------------------------------+----------------------------------+-------+----------------------------------+--------+--------+-----------+
   | 3ef18094c76a403291ccf727851616ae | 4f2e8ef6b897403fb9865123b7b57a34 |       | 3e5bb39a247b471494e051ae8d0530fb |        |        | False     |
   +----------------------------------+----------------------------------+-------+----------------------------------+--------+--------+-----------+

Create a secret consisting of the string ``my_payload``, and request
just the ``Secret href`` field as output:

::

   openstack secret store --name my_secret --payload my_payload -c "Secret href"
   +-------------+-----------------------------------------------------------------------------------------+
   | Field       | Value                                                                                   |
   +-------------+-----------------------------------------------------------------------------------------+
   | Secret href | http://10.206.54.241/openstack-barbican/v1/secrets/65ad38a3-811e-4445-8472-13aa2fa5042d |
   +-------------+-----------------------------------------------------------------------------------------+

Retrieve the original secret (``my_payload``) via the secret href value:

::

   openstack secret get --payload http://10.206.54.241/openstack-barbican/v1/secrets/65ad38a3-811e-4445-8472-13aa2fa5042d
   +---------+-------------+
   | Field   | Value       |
   +---------+-------------+
   | Payload | my_payload  |
   +---------+-------------+
