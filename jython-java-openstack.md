% Using Java and Jython with OpenStack
% Jim Baker
% jim.baker@rackspace.com


# Run OpenStack on the JVM via Jython

Jython is Python for the Java platform:

* Compiles to Java bytecode
* Very good concurrency support - no GIL, so computational threads actually work
* `import java` - can readily import and use any Java package
* Jython 2.7.0 recently released, corresponds to CPython 2.7
* Support for Python C Extension API is under rapid development

OpenStack on the JVM:

* Feasible *now* for components like Keystone that don't use Eventlet
* Can readily work with transition away from Eventlet
* Or port Eventlet to Jython

# Java components for OpenStack

JVM ecosystem is extensive, performant, and scales:

* Apache Cassandra, Kafka, Spark, Storm, ZooKeeper, ...
* Repose - high performance servlet filters, can frontend OpenStack

But also different devops challenges:

* JVM-based services have different best practices - setup, tuning, logging, monitoring
* Python APIs generally lag official Java APIs

# Mixing it up: Python and Java

Best of both?

* PySpark - use Py4J to embed CPython via JNI
* Storm bolts/spouts, written in Python - Pyleus or Parse.ly or Jython
* Keystone backend against Spring Data, using Jython
* Repose Python - build mixed pipeline of Java servlet filters and any (PEP 3333 compliant) WSGI middleware using Falcon, Pecan, WebOb, Werkzeug, ...

Advantages:

* Keep the flexibility of Python development
* While maintaining good performance
* Can also eliminate IPC, pickling/serialization costs
