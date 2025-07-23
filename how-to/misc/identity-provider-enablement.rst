Managing OpenStack Identity Providers
=====================================

Overview
--------

Perhaps one of the most important user facing features of any cloud is the authentication, authorization
and service discovery workflows that allow users to log into their account and create resources.
OpenStack, through the use of Keystone provides a plugable architecture for authentication while still
retaining the responsibility for authorization and service discovery.

In this document we'll walk through enabling additional authentication back-ends for keystone by integrating
with OpenID Connect providers such as Google, Okta, Entra ID or `Canonical Identity Platform <https://charmhub.io/topics/canonical-identity-platform>`_.

Implementation
--------------

Keystone does not directly support the needed authentication workflows needed for OpenID or SAML2. Rather,
it delegates that responsibility to Apache2, which then passes onto keystone the result of the authentication
via a set of variables which hold details about the user (full name, email, remote user ID, etc).

Keystone then uses this information to match the authenticated user to a project, based on a set of
rules configured by the cloud operator (detailed later).

Configuring federation has two major components:

* Making an IdP available to Keystone via Apache2 configurations.
* Enabling the IdP in OpenStack by creating the required keystone resources using the `openstack` command

Canonical OpenStack is responsible for the first part of that configuration process, which involves adding the needed URLs
in Apache2 and secrets to enable the authentication workflows. Making use of the newly enabled
capabilities falls on the cloud administrator, by leveraging standard openstack commands.


Requirements
------------

Before we begin, make sure that you have enabled TLS through the use of Vault. Virtually all providers require that
the redirect URL is TLS enabled. Moreover, some providers require that the redirect URL uses a host name and not an IP
address. This means that for the external host name you must also set a valid hostname. Both of these operations can be
done by enabling Vault.

Adding an Identity provider
---------------------------

There are two types of relations supported by Canonical OpenStack:

* External OpenID connect providers (Google, Okta, Entra ID)
* `Canonical Identity Platform <https://charmhub.io/topics/canonical-identity-platform>`_ which is expected to be deployed in a different juju model.

Enabling Canonical Identity Platform as an IdP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For the purpose of this document we will assume you have a model called `iam` which contains a deployment of Canonical Identity Platform.
Integration is done by consuming an offer for the `oauth` endpoint of the `Hydra <https://charmhub.io/hydra>`_ charm. If your
identity platform uses a custom CA, you also need to consume the `send-ca-cert` endpoint deployed in your `iam` model.

Create the offers:

::

    juju offer iam.hydra:oauth
    juju offer iam.self-signed-certificates:send-ca-cert

Get the offer URLs. We'll need them when creating the config file:

::

    HYDRA_OFFER=$(juju list-offers --format=json -m iam hydra | jq -r '.hydra.["offer-url"]')
    SEND_CA_CERT_OFFER=$(
        juju list-offers \
        --format=json \
        -m iam \
        self-signed-certificates | jq -r '.["self-signed-certificates"].["offer-url"]')

Create the config file:

::

    cat << EOF > canonical-iam.yaml
    oauth_offer: $HYDRA_OFFER
    cert_offer: $SEND_CA_CERT_OFFER
    EOF

And finally add the provider:

::

    sunbeam identity provider add \
        canonical openid canonical-identity-platform \
        --config canonical-iam.yaml

In the above example we added a new identity provider of type `canonical` with the `openid` protocol and the name `canonical-identity-platform`.
the name is important in the case of providers of type `canonical` as the name is also used as a label in `Horizon` when selecting the IdP. So make
sure to use a descriptive name.

Now we can list the providers:

::

    sunbeam identity provider list
    +-----------------------------+------------+----------+
    | Name                        | Provider   | Protocol |
    +-----------------------------+------------+----------+
    | Keystone Credentials        | Built-in   | keystone |
    | canonical-identity-platform | canonical  | openid   |
    +-----------------------------+------------+----------+

At this point the configurations exist in apache, keystone and horizon that enable a cloud administrator to configure it as an authentication backend. We will
cover that part later.

External IdPs
~~~~~~~~~~~~~

Canonical OpenStack currently supports the following IdP types:

* `Google <https://developers.google.com/identity/openid-connect/openid-connect>`_
* `Okta <https://help.okta.com/en-us/content/topics/apps/apps_app_integration_wizard_oidc.htm>`_
* `Entra ID <https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc#enable-id-tokens>`_

Each of these provider types require specific configurations in order to enable them and each have their own procedure for creating the client credentials needed to initiate the authentication workflow.
You will have to consult the official documentation for each, in order to generate the required client credentials. The configuration formats that Canonical OpenStack requires are detailed below. The configuration
files are in `yaml` format.

When creating an OpenID Connect integration meant to be used with Canonical OpenStack, you will need the redirect URL that the IdP needs to call back into when a user authenticates. To display the redirect URL, you can run
the following command:

::

    sunbeam identity provider get-oidc-redirect-url
    https://sunbeam.example.com/openstack-keystone/v3/OS-FEDERATION/protocols/openid/redirect_uri

Note, the schema **must** be **https** and you **should** have a fully qualified domain name configured instead of an IP address. Depending on IdP, this might be a requirement (Google for example). If that is not the case,
you should enable TLS in sunbeam, using Vault.

Google config format
^^^^^^^^^^^^^^^^^^^^

There are two mandatory configuration parameters and one optional parameter:

* `client_id` - mandatory
* `client_secret` - mandatory
* `label` - optional

Example config:

::

    client-id: client_id_obtained_from_your_console
    client-secret: client_secret_associated_with_the_id
    label: "Log in with Google"


Okta config format
^^^^^^^^^^^^^^^^^^^^

There are three mandatory configuration parameters and one optional parameter:

* `client_id` - mandatory
* `client_secret` - mandatory
* `okta-org` - mandatory
* `label` - optional

Example config:

::

    client-id: client_id_obtained_from_your_console
    client-secret: client_secret_associated_with_the_id
    okta-org: dev-123456
    label: "Log in with Okta"

Entra ID config format
^^^^^^^^^^^^^^^^^^^^^^

There are three mandatory configuration parameters and one optional parameter:

* `client_id` - mandatory
* `client_secret` - mandatory
* `microsoft-tenant` - mandatory
* `label` - optional

Example config:

::

    client-id: client_id_obtained_from_your_console
    client-secret: client_secret_associated_with_the_id
    microsoft-tenant: tenant-uuid-goes-here
    label: "Log in with Entra ID"

Adding an external IdP
~~~~~~~~~~~~~~~~~~~~~~

Adding an external IdP is similar to adding a Canonical Identity Platform provider:

::

    sunbeam identity provider add \
        google openid my-google-idp \
        --config google.yaml


Now we can list the providers:

::

    sunbeam identity provider list
    +-----------------------------+------------+----------+
    | Name                        | Provider   | Protocol |
    +-----------------------------+------------+----------+
    | Keystone Credentials        | Built-in   | keystone |
    | canonical-identity-platform | canonical  | openid   |
    | my-google-idp               | google     | openid   |
    +-----------------------------+------------+----------+

Okta and Entra ID have identical procedures.

Make a note of the name of the provider and of the protocol. We will use them in the next steps to enable these providers in keystone.

Note, you should already see them in `Horizon`, but you will only be able to use them after we've mapped them to domains and projects. Examples below.

Removing a provider
~~~~~~~~~~~~~~~~~~~

Removing a provider is a matter of running:

::

    sunbeam identity provider remove my-google-idp --yes-i-mean-it


Note, this will not remove any resources created by the cloud administrator using the `openstack` command.

Making use of the new providers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that we've made the providers available to the cloud, we can enable them in keystone, map them to a domain and create rules on how users should
be mapped to projects.

You can create a new domain or you can use an existing domain to map it to the IdP. For the purposes of this guide, we'll create a new one:

::

    openstack domain create \
        --description="Federated Google domain" \
        google

Get the issuer URL for the desired IdP. In this case we'll go with `my-google-idp` from the output above:

::

    ISSUER_URL=$(sunbeam identity provider list \
        --format=yaml |  yq -r '."my-google-idp".issuer_url')

Create the identity provider in Keystone:

::

    openstack identity provider create \
        --remote-id $ISSUER_URL \
        --domain google \
        my-google-idp


Note, the name of the identity provider must match the name in the table outputted by sunbeam.

Create a group which we will assign to federated users:

::

    openstack group create federated_users \
        --domain google

Create a project. The following example creates a project named ``federated_project``:

::

    openstack project create \
        --domain google \
        federated_project

Add a role for the group on the project we want to use:

::

    openstack role add \
        --group federated_users \
        --project federated_project \
        --group-domain google \
        --project-domain google \
        member


Next, we need to create some mapping rules between the remote users that come in from the IdP and local openstack users. The rules instruct Keystone how
to automatically create local users and to assign them to groups, projects, domains, etc. You may consult `the official documentation <https://docs.openstack.org/keystone/latest/admin/federation/mapping_combinations.html>`_
on how to write the rules. In this guide we'll create a simple rule set which will be used for the `openid` protocol of the `my-google-idp` provider to map
users to the group we created above. That will automatically grant them **member** access in the **federated_project** of the **google** domain.

This file can be as complex as you need it to be, based on your needs.

Create a file with the rules:

::

    cat > rules.json <<EOF
    [
        {
            "local": [
                {
                    "user": {
                        "name": "{0}"
                    },
                    "group": {
                        "domain": {
                            "name": "google"
                        },
                        "name": "federated_users"
                    }
                }
            ],
            "remote": [
                {
                    "type": "REMOTE_USER"
                }
            ]
        }
    ]
    EOF

Note, we're using **REMOTE_USER** as the remote user ID, but you may also use other attributes like **OIDC-preferred_username** or **OIDC-email**. But that
is a call left to the cloud administrator. The above rules will create a user and add it to the group **federated_users** in the domain **google**. 

Create the mapping:

::

    openstack mapping create \
        --rules rules.json google_openid

You can only have one mapping per IdP/protocol combination. But the same mapping (created above) can be used for multiple providers.

And lastly, we can create the protocol:

::

    openstack federation protocol create \
        --identity-provider my-google-idp \
        --mapping google_openid \
        openid

Note, the identity provider name and the protocol must match the name and the protocol returned by the ``sunbeam identity provider list`` command.

And that should do it. You should now be able to log into the horizon dashboard using the Google IdP.