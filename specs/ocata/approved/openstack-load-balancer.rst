..
  Copyright 2016, Canonical UK

  This work is licensed under a Creative Commons Attribution 3.0
  Unported License.
  http://creativecommons.org/licenses/by/3.0/legalcode

..
  This template should be in ReSTructured text. Please do not delete
  any of the sections in this template.  If you have nothing to say
  for a whole section, just write: "None". For help with syntax, see
  http://sphinx-doc.org/rest.html To test out your formatting, see
  http://www.tele3.cz/jbar/rest/rest.html

===============================
OpenStack Endpoint Load Balancer
===============================

To enable Openstack services for a single cloud to be installed in a highly
available configuration without requiring that each unit of a service is in
the same broadcast domain.

Problem Description
===================

1) As a cloud administrator I would like to simplify my deployment so that I
don't have to manage a corosync and pacemaker per OpenStack API service.

2) As a cloud architect I am designing a new cloud where all services will be
in a single broadcast domain. I see no need to use the new central loadbalancer
and would like to continue to have each service manage its own VIP.

3) As a cloud architect I would like to spread my control plane across N racks
for redundancy. Each rack is in its own broadcast domain. I do not want the
users of the cloud to require knowledge of this topology. I want the endpoints
registered in Keystone to work regardless of a rack level failure. I am using
network spaces to segragate traffic in my cloud and the OpenStack loadbalancer
has access to all spaces so I only require one set of loadbalancers for the
deployment.

4) As a cloud architect I would like to spread my control plane across N racks
for redundancy. Each rack is in its own broadcast domain. I do not want the
users of the cloud to require knowledge of this topology. I want the endpoints
registered in Keystone to work regardless of a rack level failure. I am using
network spaces to segragate traffic in my cloud. I want the segration to extend
to the load balancers and so will be requiring a set of load balancers per
network space.

5) As a cloud architect I am designing a new internal cloud and have no
interest in IPv6, I wish to deploy a pure IPv4 solution.

6) As a cloud architect I am designing a new cloud. I appreciate that it has
been 18 years since the IETF brought us IPv6 and feel it maybe time to enable
IPv6 within the cloud. I am happy to have some IPv4 where needed and am looking
to deploy a dual stack IPv4 and IPv6.

7) As a cloud architect I am designing a new cloud. I appreciate that it has
been 18 years since the IETF brought us IPv6 and wish to never see an IPv4
address again. I am looking to deploy a pure IPv6 cloud.

8) As a cloud architect I wish to use DNS HA in conjunction with the OpenStack
loadbalancer for REASONS.

9) As a cloud administrator I would like to have the OpenStack load balancers
look after HA and so will be deploying in an Active/Passive deployment.  I will
need to use a VIP for the loadbalancer in this configuration.

10) As a cloud architect I have an existing hardware loadbalancers I wish to
use. I do not want to have to update it with the location of each API service
backend. Instead I would like to have the OpenStack load balancers in an
Active/Active configuration and have the hardware loadbalancers manager traffic
between haproxy instance in the OpenStack loadbalancer service. I do not need
to use a VIP for the loadbalancer in this configuration. My hardware
loadbalancers utilise vip(s) which will need to be regestired as the endpoints
for services in Keystone.

11) As a cloud administrator haproxy statistics are fascinating to me and I
want the statistics from all haproxy instances to be aggregated.

12) As a cloud administrator I would like haproxy to be able to perfom health
checks on the backends which assert the health of a service more conclusively
than simple open port checking.

13) As a cloud administrator I which to be able to configure max connections
and timeouts as my cloud evolves.

14) As a charm author of a service which is behind the OpenStack load balancer
I would like the ability to tell the loadbalancer to drain connection to a
specific unit and take it out of service. This will allow the unit to go into
maintenance mode.


Proposed Change
===============

One new charm - OpenStack Loadbalancer with corresponding tests & QA CI/setup.
One new interface - Both requires and provides ends of a new a interface for
                    the backend services.

Alternatives
------------

1) Extend existing HAProxy charm.
2) Use DNS HA.

Implementation
==============

Assignee(s)
-----------

Primary assignee:
  unknown

Gerrit Topic
------------

Use Gerrit topic "osbalancer" for all patches related to this spec.

.. code-block:: bash

    git-review -t osbalancer

Work Items
----------

Provide OpenStack Loadbalancer Charm
++++++++++++++++++++++++++++++++++++

- Write draft interface for LB <-> Backend
- Write unit tests for Keystone endpoint registration code
- Write Keystone endpoint registration code


Mojo specification deploying and testing Mistral
++++++++++++++++++++++++++++++++++++++++++++++++

- Write Mojo spec for deploying LB in an HA configuration

Repositories
------------

A new git repository will be required for the Mistral charm:

.. code-block:: bash

    git://git.openstack.org/openstack/charm-openstack-loadbalancer

Documentation
-------------

The OpenStack Loadbalancer charm should contain a README with instructions on
deploying the charm. A blog post is optional but would be a useful addition.

Security
--------

No additional security concerns.

Testing
-------

Code changes will be covered by unit tests; functional testing will be done
using a combination of Amulet, Bundle tester and Mojo specification.

Dependencies
============

None
