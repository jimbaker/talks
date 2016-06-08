% Craton product definition
% Jim Baker
% May 25, 2016

# Overview

* Fleet automation
* Example user stories
* Goal: define what Craton is and what it is not

# Fleet automation at Rackspace

* Inventory, using Mongo
* Audit workflows, using auditor workflow engine
* Remediation worfklows, using resolver workflow engine

# Insight from Rackspace cloud operations

* Not everything is in inventory: there are other sources of truth
* Scripting current fleet automation enables doing things like live migrate 19K hosts
* Will keep this integration mindset

# Craton, as "version 2.0"

Technical definition:

* Inventory, standard Python object model that makes it pluggable; reference implementation provided by SQLAlchemy
* Audit workflows, using OpenStack TaskFlow workflow engine, with Stevedore pluggability (OpenStack Ansible, ...)
* Remediation worfklows, using TaskFlow, pluggable

# Open source philosophy

* Do a few things well
* Being able to easily integrate is just as important as what the product does
* Craton follows both tenets

# Example user story

> As a cloud operator, I need to keep track of unused virtual resource
> capacity so that I can free up resources and use them more
> efficiently.

# Solution

* Inventory
* Audit workflow
* Integration with decisioning
* Remediation workflow

# Solution: inventory

* Craton tracks inventory of configurable hardware devices (example: hosts), in conjunction with any existing asset data
* Craton supports arbitrary grouping of devices with labels - cabinet, service usage labeling devices
* Nova, Cinder, Swift all map hardware devices to virtualized services, so just need to integrate

# Solution: audit workflow

* Additional information may need to be gathered
* Example: careful characterization of hardware necessary to determine **available** memory

# Solution: decisioning

Operators may use a variety of optimization tools, using information from Craton:

* Example: mixed integer-linear programming model with constraints, objective function to optimize
* Constraints: integer-based constraints support affinity, anti-affinity properties
* Other constraints: minimize change from current scheduling layout
* Objective function: current hardware in use, with asset costs included as coefficients
* Determine new layout by solving model (approximate solutions OK) 

# Solution: remediation workflow

Live migrate VMs to reflect new scheduling; do the following in batches:

* Quiesce source hosts to prevent further scheduling - OSA
* Build fresh compute host, include verification step (can run VMs!) - OSA
* Live migrate VMs to host
* Retry if unsuccessful, otherwise kickout to manual remediation

# Other integration examples

* Rackspace Core
* Walmart Labs OneOps

# OneOps capabilities

* CMDB, able to model arbitrary configuration components (CIs) and relationships ("directed acyclic graph")
* Governance processes, including logging all changes in the CMBD
* Workflows to manage full lifecycle  of deploying applications, from dev to production

# OneOps technology stack

* MyBatis, for mapping PostgreSQL to Java POJOs
* Activiti, to implement BPMN 2.0 based workflows, as defined in XML and implemented in Java
* Ruby on Rails front end

# Integration with OneOps

* SQLAlchemy mapper to map CIs into Craton object model
* Join CIs against Craton's key/value model
* OneOps workflows can drive Craton fleet automation

# Craton product definition

* Provide core implementation of workflow engine, that scales from small private clouds to the largest public clouds
* Provide reference implementation of inventory (SQLAlchemy), and make it easy to use partially (Python object model)
* Workflows are pluggable: reference for OSA, pluggable for other tooling (Kolla, ...)
* REST APIs to access, for integration and scripting

