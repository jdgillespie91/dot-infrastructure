infrastructure
==============

This repository contains code relevant to the infrastructure that powers `my personal website`__.

.. _jakegillespie: https://jakegillespie.me/

__ jakegillespie_

Usage
-----

Currently, I'm not aware of best practice around `CloudFormation`__. As such, using the work in this repository is largely manual.

To launch the infrastructure, first create a single stack using each of the following templates:

- auth.yml
- certificate.yml

These are resources that I intend to be shared across all resources.

Second, create as many stacks as necessary (likely just one) using the network template:

- network.yml

This creates an isolated network in which to bring up other resources. For example, this might be useful when wanting isolated staging and production environments.

Third and finally, create as many stacks as desired using the application template:

- application.yml

.. _cf: https://aws.amazon.com/cloudformation/

__ cf_
