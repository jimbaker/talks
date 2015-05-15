% Using Java and Jython with OpenStack
% Jim Baker
% jim.baker@rackspace.com

# Java components for OpenStack

JVM ecosystem is extensive, performant, and scales:

* Apache Cassandra, Kafka, Spark, Storm, ZooKeeper, ...
* Python interfaces for Apache ecosystem
* Repose - high performance servlet filters, can frontend OpenStack
* Monasca - data streaming and analysis, integrates with OpenStack

# Run OpenStack on the JVM via Jython

FIXME what is Jython

Deployment possibilities on the JVM, via Jython:

* Jython 2.7.0 recently released
* Demonstrated running Keystone on Jython
* Supporting Eventlet is feasible - map greenlets to threads - but it might not be wise
* Jython 3.5 with `async` and `await` is least two years from release
* (Core of Python 2.7 is the same as 3.2; Jython 2.7 supports Six package)
* Support for Python C Extension API is under rapid development

# Mixing it up: Python and Java

FIXME advantages - ensuring performance

Best of both?

* PySpark - use Py4J to embed CPython via JNI
* Repose Python - mix standard WSGI middleware, Java servlet filters
* Storm bolts/spouts, written in Python - Pyleus or Parse.ly or Jython
* Wrap custom Java identity backend for Keystone
