% Using Java and Jython with OpenStack
% Jim Baker
% jim.baker@rackspace.com


# Run OpenStack on the JVM via Jython

Jython is Python for the Java platform:

* Compiles to Java bytecode
* Very good concurrency support, including no GIL
* `import java` - can readily import and use any Java package
* Jython 2.7.0 recently released, corresponds to CPython 2.7
* Support for Python C Extension API is under rapid development

OpenStack on the JVM:

* Feasible now for some components like Keystone
* Can readily work with the general transition away from Eventlet
* Trollius (backport of AsyncIO) or simply native threads

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
* Repose Python - mix standard WSGI middleware, Java servlet filters
* Storm bolts/spouts, written in Python - Pyleus or Parse.ly or Jython
* Demonstrated prototype of Java identity backend for Keystone

Advantages:

* Keep the flexibility of Python development
* Maintain performance if no IPC and no pickling/serialization
