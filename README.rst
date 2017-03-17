infrastructure
==============

This repository contains code relevant to the infrastructure that powers `my personal website`__.

.. _jakegillespie: https://jakegillespie.me/

__ jakegillespie_

Usage
-----

Currently, I'm not aware of the best practice around infrastructure. I'll begin with a very manual deployment process (described below) and will learn through its optimisation.

Prerequisites include

- An IAM user with the `AdminstratorAccess AWS managed policy`__
- The keys associated with this user
- `Packer`__ (version 0.12.3)

First, create the `AMI`__ that will be used in the `CloudFormation`__ templates

.. code-block:: bash

    $ packer build -var 'aws_access_key=...' -var 'aws_secret_key=...' application-ami.json

Then launch the infrastructure. Begin by creating a single stack using each of the following templates:

- auth_
- certificate_

These are, in some sense, *global* resources.

Then create as many stacks as necessary (likely just one) using the network template:

- network_

This creates an isolated network in which to bring up other resources. For example, this might be useful when wanting isolated staging and production environments.

Then create as many stacks as desired using the application template (note that you'll need to pass the AMI ID created earlier as a parameter when creating this stack):

- application_

Finally for infrastructure, create a single routes stack using the routes template:

- routes_

Depending on where your domain is registered, you might need to do some further configuration so that your registrar points to the right name servers. My domain is registered with GoDaddy so `these steps`__ are required.

Now, you're ready to configure the application server. On the box, do the following:

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

- Then add `haproxy.cfg`_ to :code:`/etc/haproxy/haproxy.cfg` and restart the service.

.. code-block:: bash

    sudo service haproxy restart

Now, everything should work as intended.

.. _application: application.yml
.. _auth: auth.yml
.. _certificate: certificate.yml
.. _network: network.yml
.. _haproxy.cfg: haproxy.cfg
.. _routes: routes.yml
.. _iam: https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html
.. _packer: https://www.packer.io/intro/getting-started/setup.html
.. _ami: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html
.. _cf: https://aws.amazon.com/cloudformation/
.. _godaddy_ns: https://uk.godaddy.com/help/set-custom-nameservers-for-domains-registered-with-godaddy-12317
.. _projects: https://github.com/jdgillespie91/projects-api/

__ iam_
__ packer_
__ ami_
__ cf_
__ godaddy_ns_
__ projects_
