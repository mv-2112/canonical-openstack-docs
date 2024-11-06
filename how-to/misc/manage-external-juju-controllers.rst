Register the Juju controller
----------------------------

As a prerequisite, perform the following steps in existing Juju
controller

-  `Add a Juju
   user <https://juju.is/docs/juju/manage-users#heading--add-a-user>`__.
-  `Grant necessary
   permissions <https://juju.is/docs/juju/juju-grant>`__ to the Juju
   user.

Adding the Juju user will generate a registration token which is
required to register the Juju controller in the MicroStack deployment.

To register the controller in MicroStack use the ``register-controller``
command:

::

   sunbeam juju register-controller NAME TOKEN

``NAME`` is an arbitrary name to refer the Juju controller in
MicroStack.

``TOKEN`` is the registration token generated during Juju user creation.

For example, to register an existing controller with the name
``prod-controller-01`` using a token generated as detailed above:

::

   sunbeam juju register-controller prod-controller-01 \
       Tm90IGEgcGFzc3dkIGlmIHlvdSBjYXJlIHRvIGRlY29kZSB0aGlzCg==

Unregister the Juju controller
------------------------------

To unregister the controller in MicroStack use the
``unregister-controller`` command:

::

   sunbeam juju unregister-controller NAME

For example, to unregister an existing controller with the name
``prod-controller-01``:

::

   sunbeam juju unregister-controller prod-controller-01
