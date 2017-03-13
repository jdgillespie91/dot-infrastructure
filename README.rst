infrastructure
==============

This repository contains code relevant to the infrastructure that powers `my personal website`__.

.. _jakegillespie: https://jakegillespie.me/

__ jakegillespie_

Usage
-----

Currently, I'm not aware of best practice around `CloudFormation`__. As such, using the work in this repository is largely manual.

To launch the infrastructure, create a single stack using each of the base templates:

- auth.yml
- certificate.yml
- network.yml

Then create as many stacks as desired using the application template:

- application.yml

.. _cf: https://aws.amazon.com/cloudformation/

__ cf_
