MicroStack can use an existing Juju controller during bootstrap instead
of deploying a Juju controller within the MicroStack deployment.

This allows operators to make use of an existing Juju controller that
could be used to control many Juju deployments of different types of
services - including multiple MicroStack deployments!

Register the Juju controller
----------------------------

:doc:`Register an existing Juju controller </how-to/misc/manage-external-juju-controllers>`
in MicroStack.

[note type=“note”] **Note:** Ensure the Juju user created in the
external Juju controller has ``superuser`` permissions granted. [/note]

Bootstrap with registered Juju controller
-----------------------------------------

Use the option ``--controller`` with the bootstrap command to make use
of the previously registered Juju controller.

In local mode the roles for the machine still need to be provided during
bootstrap:

::

   sunbeam cluster bootstrap --roles compute,storage,control \
       --accept-defaults --controller prod-controller-01

In MAAS mode the roles are determined by tags on the machines being
deployed, so the roles option is not used:

::

   sunbeam cluster bootstrap --controller prod-controller-01
