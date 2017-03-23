GoCD
====

This contains the infrastructure powering Jenkins.

Usage
-----

Prerequisites include

- An IAM user with the `AdminstratorAccess AWS managed policy`__
- The keys associated with this user
- `Packer`__ (version 0.12.3)
- network_ stack

First, create the `AMI`__ that will be used in the `CloudFormation`__ templates

.. code-block:: bash

    $ packer build -var 'aws_access_key=...' -var 'aws_secret_key=...' ami.json

Then launch a stack using the following template:

- gocd_

.. _network: ../network.yml
.. _gocd: gocd.yml
.. _iam: https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html
.. _packer: https://www.packer.io/intro/getting-started/setup.html
.. _ami: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html
.. _cf: https://aws.amazon.com/cloudformation/

__ iam_
__ packer_
__ ami_
__ cf_
