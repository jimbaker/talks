% Cloud Computing
% Jim Baker
% jim.baker@{rackspace.com, python.org}

# Overview

* What is this cloud computing thing? - one very big idea
* APIs
* SaaS, PaaS, Iaas, and other marketing terms

# About me

* Architect at Rackspace, focused on platformization, cloud computing, and big data
* Once and future lecturer for CSCI 3155 Principles of Programming Languages
* Formerly on Ubuntu Server team at Canonical
* Formerly at Sauce Labs, supporting Selenium testing in the cloud
* Founding Juju team member, working on service orchestration, for the cloud
* Core developer of Jython and fellow of the Python Software Foundation
* Co-author of *Definitive Guide to Jython* from Apress
* Enjoy outdoor recreation and frequent travel!

# The Economist, October 10 issue

* Demonstrates "cloud computing" is a popular term in the wider economy
* "Marketectures" (marketing + architecture) and ad bingo do make it more cloudy...
* Also cloud and the law, specifically issues around data sovereignty
* (anyone interested in Silicon Flatirons?)

# One big idea

(blackboard)

# One big idea

* Delegation of responsibility
* = the client does not care about servers, just services
* This can include name lookups - Domain Name Service (DNS) or service catalogs like Keystone
* Enables horizontal scaling, across possibly globally distributed data centers

# Indirection and its power

> All problems in computer science can be solved by another level of indirection.

--- David Wheeler

# Need to have a base case...

> All problems in computer science can be solved by another level of indirection.

--- David Wheeler

> ... except for the problem of too many layers of indirection

--- Kevlin Henney

# Completing indirection

?

# Completing indirection of delegating responsibility

* DNS
* TCP/IP
* Certificate authorities for SSL/TLS to validate cert chain
* Schema catalogs in relational databases
* Content distribution networks (CDNs)
* etc etc

# Another good idea: APIs

* Services should have APIs...
* which supports programmability
* Enables further scaling
* Check out DevOps (Developer/Operations) and similar terms

# DevOps Boulder meetup

* [DevOps Boulder](http://www.meetup.com/DevOps-Boulder/events/222959617)
* Probably not OK if you crash this meeting tonight
* But do join and attend in the future!

# FIXME

Various stuff to cover:

* APIs!
* Horizontal scaling (maybe include quote from Morgan at Red Hat?)
* Latency/speed of light/round trips for protocols/data center design/using multiple data centers/eventual consistency


# SOA - Service Oriented Architecture

* 
* Enables a base set of products to be extended via combination and further refinement

# Steve Yegge and The Google Platforms Rant

Or lessons learned on how Amazon learned to love platforms

* [Summary](http://www.businessinsider.com/steve-yegge-google-platform-rant-2011-10)
* [Unfortunate public posting](https://plus.google.com/+RipRowan/posts/eVeouesvaVX) by Steve Yegge
* Steve was not fired after all...
* Steve is also an occasional user of and contributor to Jython, nice!

# Being cloudy

Going up the stack:

* Infrastructure as a Service (IaaS)
* Platform as a Service (PaaS)
* Microservices (no, we don't call this services as a service!)
* Software as a Service (SaaS)

# SaaS

* Started first - what if we took existing apps, made them available via a browser...
* Browser-native apps - Salesforce, moved to mobile
* Generally worked by sharding (by tenant, customer), lots of glueing
* Increasingly "cloud native" (do define!)

# PaaS

* Heroku, Cloud 9 (Sauce...) - labs - great workflows, easy to try out ideas
* Google Cloud
* OpenStack Magnum shades into this, but less limitations

# Microservices

Examples include:

* Mapping - including the original successful microservice, Google Maps
* Payment platforms
* Machine learning
* etc

# Microservices

Examples include:

> * Mapping - including the original successful microservice, Google Maps
> * Payment platforms
> * Machine learning
> * etc

Note the analogue to business to business services, such as credit card processing, including new variants like Square

# IaaS

* The data center has an API
* And we can connect to multiple data centers (DCs, aka regions) and availability zones (subdivided DCs)
* Instead of weeks, we get instances in seconds/minutes
* Generally using hypervisors, but also lighterweight containers (Docker/Kubernetes), bare metal (OpenStack Ironic)
* AWS, Azure, Google Cloud, OpenStack (such as provided by Rackspace or HP Public Cloud), or on your own DC
* Key terms include provisioning, discovery, ...

# Cloud and the law

* Data sovereignty/data residency
* European Court of High Justice recent ruling that bilateral *safe harbor* agreement between US and EU is invalid
* HIPAA, PII, e-commerce considerations

# Delegating the hard work to others

* Early microservices like PayPal - ecommerce is not just for Amazon and its affiliates
* Cloud services can be used to solve regulatory and legal compliance issues

# Amazon Web Services - AWS

* EC2 - "elastic computing cloud"
* S3 - something X 3 FIXME - *object* storage (does S3 support incremental patches, or only replacement?)
* Many other services - block storage (EBS), notification, stream processing (Kinesis), ...
* Or set up your own EC2 - Eucalyptus (now part of HP)

# OpenStack

Started as a collaboration between NASA and Rackspace, since has grown tremendously:

* Keystone - identity, service catalog
* Nova - compute
* Swift - storage
* Neutron - networking
* many, many other projects

# Contrast: use your own servers

* You might just choose Docker or Vagrant

# "docker docker docker" talk at the OpenStack Vancouver Summit 

* Just add Docker to your talk title?
* Why not just add it [multiple times](https://www.openstack.org/summit/vancouver-2015/summit-videos/presentation/docker-docker-docker-openstack)? ;)
* There are always **hot**, must know technologies out there

# Juju - a "canonical" example

Use a GUI or drive from the command line:

```
juju deploy mysql
juju deploy wordpress
juju add-relation wordpress mysql
juju expose wordpress
```

then scale up with

```
juju add-unit wordpress
```

# Deploying Hadoop with Juju

```
$ juju deploy hadoop hdfs-datacluster-02
$ juju add-unit -n 2 hdfs-datacluster-02
$ juju add-relation hdfs-namenode:namenode \
	hdfs-datacluster-02:datanode
```

# Contrast dedicated hosting

FIXME anything public on provisioning servers?


# Private vs public cloud

# Federation


# Implementing clouds

* Sharding/partitioning
* Multitenancy
* Shared nothing architectures
* SQL vs NoSQL
* Blue/green
* Virtualization - jails, ability to escape the jail
* Functional programming, referential transparency

# SQL vs NoSQL

It's not about "SQL" because many so-called NoSQL databases have a SQL-like query language. Instead it is about the cost of doing distributed:

* Transactions
* Joins

(break out into slides)

# Relativity

* Speed of light, etc

# Ping times

# Eventual consistency vs strong consistency

* CAP theorem, and its status
* Cassandra (DataStax)
* MySQL Galera

# Content distribution networks (CDNs)

* and the power of immutability! (in terms of being able to reason about it)
* Netflix - current stats?

# Servers vs services

# Chaos Monkey

# Atomic OS

Blue/green

* Fedora Atomic
* CoreOS (Rackspace relationship...)

# Containers

* Docker - LXC, copy-on-write/union file system of some kind - aufs, btrfs, zfs, overlayfs,

# Hypervisors

# Magnum

# Bare metal


# Cloud init

who am i?
what should i do?

(maybe quote the whale from the Hitchhikers Guide to the Galaxy...)

cannot simply run a saved VM
(would be worthwhile showing this start up...)

need a worker ready to do its work


ssh credentials


# Orchestration


AWS auto scaling
Heat
Juju
OpenShift

# Auto scaling




# Sites and how they are implemented

NYT vs DailyCamera...


# Various SOA terms

* SOA
* REST
* WSDL, ...
* Native clients
* Others: SAML, tokens, API keys, etc
* Service catalog - can discover what services exist AND their endpoints
* For IaaS: Can discover networks, instances, ...
* For PaaS: can discover other things being managed (GitHub API)
