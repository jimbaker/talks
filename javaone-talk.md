% Intro to Jython at Rackspace
% Jim Baker, Werner Mendizabal, Jorge Williams
% {jim.baker|werner.mendizabal|jorge.williams}@rackspace.com

# Overview

# About Jim Baker

# About Werner Mendizabal

# About Jorge Williams

# OpenStack

FIXME mention REST APIs

# Keystone

# Using Keystone

FIXME thinking some sort of schematic of how clients interact with Keystone
FIXME thereby showing centrality of Keystone (befitting its name)
FIXME sequence diagrams are probably best for thisx

# Problem

# Elegant solution

# Jython background

* Implementation of Python for the Java platform
* Compiles to Java bytecode
* From the beginning great Java integration via some cleverness
* Small team of committers
* Under on-and-off development since 1997
* Extensively used

# Demo

# WAR construction

FIXME gradle

# Deployment considerations

# Jython for other OpenStack components?

# Technical barriers

# Risks

# Interest in wider OpenStack community

(actually we really need to follow up on this!)

# Overcoming them

# FIXME stuff we would like to do

FIXME Clamp support of Java annotations to remove boilerplate

# Run Keystone v3 using OpenStack Python Code

* Target the following operations initially
* AuthN including Password and 2 Factor (POST /v3/auth/token)
* Token Validation (GET /v3/auth/token and HEAD /v3/auth/token)
* Service discovery (GET /v3)
* Target other operations if possible, but initial plan is to 
target core operations first

# Questions

FIXME contact info





