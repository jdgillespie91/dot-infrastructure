infrastructure
==============

This repository details how the infrastructure that powers `my personal website`__ can be built.

.. _jakegillespie: https://jakegillespie.me/

__ jakegillespie_

Usage
-----

In the images_ directory, you'll find a set of Packer_ templates for building the various required AMIs. To build, run

.. code-block:: bash

    $ packer build <template>

In the infrastructure_ directory, you'll find a set of CloudFormation_ templates for creating the various required resources in AWS. Before using the templates, note the following rules:

- Folder structure corresponds to the resource tier.
- Lower tiers are dependent on the presence of higher tiers.
- Resources cannot be shared across a tier.

This is best explained in a working example. Suppose that we want to create resources for the app_. Since app_ is a subdirectory of security_groups_ and security_groups_ is a subdirectory of infrastructure_, we have to create those resources first. Let's begin with the infrastructure_ templates.

.. code-block:: bash

    $ aws cloudformation create-stack --stack-name certificate --template-body file://infrastructure/certificate.yml
    $ aws cloudformation create-stack --stack-name iam --template-body file://infrastructure/iam.yml --capabilities CAPABILITY_NAMED_IAM
    $ aws cloudformation create-stack --stack-name network --template-body file://infrastructure/network.yml

Observe their progress with

.. code-block:: bash

    $ aws cloudformation describe-stacks

Once all are complete, create the security_groups_ resources.

.. code-block:: bash

    $ aws cloudformation create-stack --stack-name security-groups --template-body file://infrastructure/security_groups/security_groups.yml --parameters ParameterKey=NetworkStackName,ParameterValue=network

Now, we can create the resources for the app_.

.. code-block:: bash

    $ aws loudformation create-stack --stack-name app --template-body file://infrastructure/security_groups/app/resources.yml --parameters ParameterKey=SecurityGroupsStackName,ParameterValue=security-groups ParameterKey=AppAMI,ParameterValue=ami-4abca92e

Using the same process, create the rest of the resources specified in the infrastructure_ folder. Be sure that parent resources are created successfully before creating their children. You'll then be in a position to configure the application server. On the box, do the following:

- Start by cloning the `projects-api`__ application and running the production configuration.

.. code-block:: bash

    $ git clone https://github.com/jdgillespie91/projects-api
    $ cd projects-api
    $ git checkout 7f87fa875b6dc989f36c27f98bb4d2d2f64b0a57
    $ sudo ./bin/run-prod.sh
    $ cd ..

- Do similarly for the `jakegillespie.me` application.

.. code-block:: bash

    $ git clone https://github.com/jdgillespie91/jakegillespie.me
    $ cd jakegillespie.me
    $ git checkout 23be6eb95776578167b4a6bc205d32f5d2e253ef
    $ sudo ./bin/run-prod.sh
    $ cd ..

- Then add the `haproxy configuration`__ to :code:`/etc/haproxy/haproxy.cfg` and restart the service.

.. code-block:: bash

    sudo service haproxy restart

Finally, ensure that your domain is configured correctly. Depending on where your domain is registered, you'll need to ensure your registrar points to the right name servers. My domain is registered with GoDaddy so `these steps`__ are required. At last, everything *should* work as intended!

.. _CloudFormation: https://aws.amazon.com/cloudformation/
.. _Packer: https://www.packer.io/
.. _app: infrastructure/security_groups/app
.. _godaddy_ns: https://uk.godaddy.com/help/set-custom-nameservers-for-domains-registered-with-godaddy-12317
.. _haproxy: haproxy.cfg
.. _images: images
.. _infrastructure: infrastructure
.. _projects: https://github.com/jdgillespie91/projects-api
.. _security_groups: infrastructure/security_groups

__ projects_
__ haproxy_
__ godaddy_ns_
