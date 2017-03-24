infrastructure
==============

This repository details how the infrastructure that powers `my personal website`__ can be built.

.. _jakegillespie: https://jakegillespie.me/

__ jakegillespie_

Usage
-----

In the images_ directory, you'll find a set of Packer_ templates for building the various required AMIs. To build, run

.. code-block:: bash

    $ packer build -var 'aws_access_key=foo' -var 'aws_secret_key=bar' <template>

In the infrastructure_ directory, you'll find a set of CloudFormation_ templates for creating the various required resources in AWS. Before using the templates, note the following rules:

- Folder structure corresponds to the resource tier.
- Lower tiers are dependent on the presence of higher tiers.
- Resources cannot be shared across a tier.

This is best explained in a working example. Suppose that we want to create resources for the app_. Since app_ is a subdirectory of security_groups_ and security_groups_ is a subdirectory of infrastructure_, we have to create those resources first.

# TODO Include instructions on how to create the infrastructure and security_groups resources, presumably via the CLI.

Now, we can create the resources for the app_.

# TODO Include instructions on how to create the app resources.

# TODO Add the example of creating a coloured version of the app.

.. _CloudFormation: https://aws.amazon.com/cloudformation/
.. _Packer: https://www.packer.io/
.. _app: infrastructure/security_groups/app
.. _images: images
.. _infrastructure: infrastructure
.. _security_groups: infrastructure/security_groups
