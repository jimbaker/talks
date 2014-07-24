% State of Jython
% Jim Baker
% jim.baker@{python.org, rackspace.com}

Overview
========

* 2.7.0 is under very active development
* Should have a final release by Q4
* Race through some of these changes


Some recent changes to trunk
============================

* Java 7 JVM is now the minimum version, which gives a larger base of functionality to work with (such as using Java 7's AutoCloseable to imply corresponding context manager support in using Python code)
* Enable mixing Python and Java types in the bases of a class when using a metaclass
* Added support for buffer and memoryview
* Console and encoding support, such as unicodedata, IDNA, and CJK support
* Relative star imports, which seems to impact a number of interesting projects.
* Many, many small fixes (bz2 support, including tarfile, ...)

socket-reboot
=============

* Now in Jython trunk
* Reimplements socket/select/ssl on top of Netty 4
* (Netty is a popular performant event loop networking framework for the JVM)
* Jython socket support is now very close to what is seen in Windows

requests
========

* Now works with Jython!
* Handy since it's used by pip
* socket-reboot enables requests

PyPA tooling support
====================

* pip now works (again!) but requires a branch for the moment, including wheel support
* Dependency is completing a small PR against html5lib-python so that it doesn't use isolated UTF-16 surrogates in literals, since this is not actually legal unicode, nor does it work in Jython's UTF-16 based representation.
* Ironically this usage is to detect such illegal use in input streams
* Will also support virtualenv and tox
* Plan to bundle pip support via ensurepip backport

Regular expressions
===================

* Performance tuning of Jython's port of sre
* Currently requires expansion of UTF-16 encoded strings into codepoints array
* Memoization of this expansion means beautifulsoup now works with decent performance (no extra $O(n)$ factor)
* May have implications for web frameworks

Jython tools
============

* Develop tooling outside the usual release schedule and the usual problem of being in core (such as eventual deprecation)
* Clamp - improve integration of Jython from Java
* Jiffy - support cffi for Jython
* Fireside - blazing fast WSGI bridge for servlet containers
* Logbridge - Use simple logging facade for Java as a Python logging handler
* What else?

Clamp
=====

* Precise integration with Java
* Enables such capabilities as Java directly importing Python modules without explicitly initializing the Jython runtime or using object factories
* Integrates with setuptools to produce jars
* Includes future integration as well with Maven via Aether.
* Sprint this Saturday to enables using Java annotations as class and method decorators


Python class, extending Java interfaces
=======================================

````python
from java.io import Serializable
from java.util.concurrent import Callable

class BarClamp(Callable, Serializable):

  def call(self):
    return 42
````

Python class, clamped
=====================

To import a Python class that you want to import into Java, add a couple of lines:

````python
from java.io import Serializable
from java.util.concurrent import Callable
from clamp import clamp_base

BarBase = clamp_base("bar")  # Java package prefix

class BarClamp(BarBase, Callable, Serializable):

  def call(self):
    return 42
````

Clamping your class
===================

Key insight: ahead-of-time builds through setuptools to produce a jar for Java linkage:

````python
import ez_setup
ez_setup.use_setuptools()

from setuptools import setup, find_packages

setup(
  name = "clamped",
  version = "0.1",
  packages = find_packages(),
  install_requires = ["clamp>=0.3"],
  clamp = ["clamped"],
)
````

Using from Java
===============

**Simply import clamped Python classes into Java code!**

````java
import bar.clamped.BarClamp;

public class UseClamped {
  public static void main(String[] args) {
    BarClamp barclamp = new BarClamp();
    try {
      System.out.println("BarClamp: " + 
        barclamp.call());
    } catch (Exception ex) {
      System.err.println("Exception: " + ex);
    }
  }
}
````


Jiffy
=====

* Provide a CFFI backend for Jython
* Pure vaporware
* Cursory examination of `cffi.backend_ctypes` suggests effort is straightforward/modest effort to provide a similar backend by using JNR's JFFI

Patois
======

* Started to collect examples for cross-implementation support
* Example: surrogate support

JyNI
====

* Add JyNI jar to the Java classpath enables C extension API support
* Works for a number of packages, but need to add GC support (!) for anything real
* Sprinting on JyNI this Monday in Aachen

Release schedule
================

* We have nearly completed bug triage
* Complete beta 4 (final beta)
* Release candidates as needed

Future
======

* Mostly around performance, Java integration, and of course the usual bug fixes
* Python bytecode compiler remains important, including for support targeting Android and removing restriction on getting too large a method for the JVM
* More hooks for Java integration, including managing generated bytecode
* Integrating Zippy could provide for PyPy-like performance, but requires Graal JVM
* Java 9 plans to provide more support for performance optimization for dynamic languages


Jython 3.x?!
============

* Comes up periodically!
* Would be nice for unicode strings and bytestrings to have direct correspondence to Java
* Remove code!
* Plan to kickoff development at PyCon in Montreal Spring 2015
* Release schedule: we will get there at some point!





