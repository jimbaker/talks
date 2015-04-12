# FIXME

we now support six
say something about characteristic
executable zip files - link a CPython bug, a Jython bug

# Jython background

* Implementation of Python for the Java platform
* Compiles to Java bytecode
* Under development since 1997
* Jython 2.7 release candidate 2 now available

# Questions you might have or heard from others

* Isn't Jython a dead project?
* Doesn't Jython have a GIL, just like CPython?
* Isn't Jython much slower than other Python implementations?
* Doesn't Jython only implement a subset of Python?

# Questions you might have or heard from others

> * Isn't Jython a dead project?
> * Doesn't Jython have a GIL, just like CPython?
> * Isn't Jython much slower than other Python implementations?
> * Doesn't Jython only implement a subset of Python?

Answer: **No**

# About me

I have a vested interest:

* Core developer of Jython
* Co-author of *Definitive Guide to Jython* from Apress
* Committing on Jython since 2007...
* User of Python (including Jython) since 2003
* Software developer at Rackspace

# Various tweets

December 2013:

> Can Jython be saved or is it pretty much dead at this point?

# Various tweets

December 2013:

> Can Jython be saved or is it pretty much dead at this point?

July 2014:

> So is jython basically dead or what?

# Various tweets

December 2013:

> Can Jython be saved or is it pretty much dead at this point?

July 2014:

> So is jython basically dead or what?

January 2015:

> Is it still around? I thought the project was dead :V

# Sometimes we are optimistic

From my note to reviewers of this talk proposal:

> I assume Jython 2.7.0 will see a final release by the end of this
> year, and certainly well before PyCon.

# Demo RC2

But here is release candidate 2!

# Try importing the GIL

```
$ bin/jython
Jython 2.7rc2+ (default:0213400c518f, Apr 9 2015, 23:01:27)
[Java HotSpot(TM) 64-Bit Server VM (Oracle Corporation)] on java1.7.0_75
Type "help", "copyright", "credits" or "license" for more information.
>>> from __future__ import GIL
```

# No global interpreter lock:

```
$ bin/jython
Jython 2.7rc2+ (default:0213400c518f, Apr 9 2015, 23:01:27)
[Java HotSpot(TM) 64-Bit Server VM (Oracle Corporation)] on java1.7.0_75
Type "help", "copyright", "credits" or "license" for more information.
>>> from __future__ import GIL
  File "<stdin>", line 1
SyntaxError: Never going to happen!
```

# One performance benchmark...

```
$ python2.7 -m test.pystone 1000000
Pystone(1.1) time for 1000000 passes = 8.87509
This machine benchmarks at 112675 pystones/second
```

# One performance benchmark...

```
$ python2.7 -m test.pystone 1000000
Pystone(1.1) time for 1000000 passes = 8.87509
This machine benchmarks at 112675 pystones/second
```

vs

```
$ jython27 -m test.pystone 1000000
Pystone(1.1) time for 1000000 passes = 6.24945
This machine benchmarks at 160014 pystones/second
```


# One **terrible** performance benchmark...

```
$ jython -m test.pystone 1000000
Pystone(1.1) time for 1000000 passes = 6.24945
This machine benchmarks at 160014 pystones/second
```
vs

```
$ python2.7 -m test.pystone 1000000
Pystone(1.1) time for 1000000 passes = 8.87509
This machine benchmarks at 112675 pystones/second
```

* Really, pystone??!!!
* Does not consider JVM startup time nor JIT warmup
* Ignores GC issues

# PyPy 2.5.1

Only 21x faster!

```
$ pypy -m test.pystone 1000000
Pystone(1.1) time for 1000000 passes = 0.29528
This machine benchmarks at 3.38662e+06 pystones/second
```

# On the other hand...

* But still, considered that we only focused on compatibility, not bad performance
* Most of the performance improvement from the efforts to improve Java 7 or Java 8
* Actually do care about pystone because it measures some of the costs of dynamic overhead

# Understanding projects

* Look at the email lists, wikis, code, what's new, bug reports, linked PRs and patches
* And especially the [commit log](https://hg.python.org/jython), as guided by the above
* Commits to tell you what has changed

# What about subset of Python language?

* Not just language features
* Standard library
* Python ecosystem

# What about subset of Python language?

> * Not just language features
> * Standard library
> * Python ecosystem

How have we responded?

# Example: Deleted text in ACKNOWLEDGMENTS


```diff
- Python's inventor Guido van Rossum and
- the rest of PythonLabs continues to help
- and support Jython by their understanding
- of how Jython must live with the limits of
- Java.
```

# Example: Deleted text in ACKNOWLEDGMENTS

```diff
- Python's inventor Guido van Rossum and
- the rest of PythonLabs continues to help
- and support Jython by their understanding
- of how Jython must live with the limits of
- Java.
```

Let's not do that!

# Example: Inserted text in ACKNOWLEDGMENTS

This change makes it much better:

```diff
+ Jython: Python for the Java Platform

+ Jython follows closely the Python language
+ and its reference implementation CPython,
+ as created by Guido van Rossum.
+ Jython 2.7 corresponds to CPython 2.7.
```

# Examples of compatibility changes

Support we have added:

* six and all of its crazy import hook magic - single source for Python 2 and 3
* characteristic and its class decorator magic
* socket/select/ssl using Netty
* requests and its beautiful API
* pip and setuptools, via ensurepip
* while also support this functionality for Windows
* including executable zip archives
* and on localized platforms, including fixed issues to support Finnish, Japanese, Turkish, and more
* which also means CJK encodings

# Demo

Demo testing requests on Windows

# Some changes reported in NEWS

* Enable mixing Python and Java types in the bases of a class when using a metaclass
* Added support for buffer and memoryview
* Console and encoding support, such as unicodedata, IDNA, and CJK support
* Relative star imports, which seems to impact a number of interesting projects.
* Many, many small fixes (bz2 support, including tarfile, ...)

  New Features
    - bytearray complete
    - a buffer API
    - memoryview
    - bz2 module

* Java 7 JVM is now the minimum version - get to use `AutoCloseable`



# Jython tools

 Develop tooling outside the usual release schedule and problems of being in core

* Clamp - improve integration of Jython from Java
* Jiffy - support cffi for Jython
* Fireside - blazing fast WSGI bridge for servlet containers
* Logbridge - Use SL4J (simple logging facade for Java) as a Python logging handler

# Clamp

* Precise integration with Java
* Java can directly import Python modules (at last!)
* Integrates with setuptools to produce jars
* Includes future integration as well with Maven via Aether.
* Sprint Saturday to add Java annotation as class and method decorators


# Python class, extending Java interfaces

```python
from java.io import Serializable
from java.util.concurrent import Callable

class BarClamp(Callable, Serializable):

  def call(self):
    return 42
```

# Python class, clamped

To import a Python class that you want to import into Java, add a couple of lines:

```python
from java.io import Serializable
from java.util.concurrent import Callable
from clamp import clamp_base

BarBase = clamp_base("bar")  # Java package prefix

class BarClamp(BarBase, Callable, Serializable):

  def call(self):
    return 42
```

# Clamping your class

Key insight: ahead-of-time builds through setuptools to produce a jar for Java linkage:

```python
import ez_setup
ez_setup.use_setuptools()

from setuptools import setup, find_packages

setup(
  name = "clamped",
  version = "0.1",
  packages = find_packages(),
  install_requires = ["clamp"],
  clamp = ["clamped"],
)
```

# Using from Java

**Simply import clamped Python classes into Java code!**

```java
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
```

# Boring

# Fireside

* Blazing fast WSGI bridge for servlet containers
* Uses Clamp
* Passes standard WSGI tests in `wsgiref.validate`
* Sprinting on adding `ServletFilter` support this Friday (April 17)

# Plug in with standard WAR support

Add to your `web.xml` these config directives:

```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
 ...
 <servlet>
  <servlet-name>fireside</servlet-name>
  <servlet-class>
  org.python.tools.fireside.servlet.WSGIServlet
  </servlet-class>
  <init-param>
   <param-name>wsgi.handler</param-name>
   <param-value>hellowsgi.simple_app</param-value>
  </init-param>
 </servlet>
```

# Fireside code

218 LOC

Start with

```python
from javax.servlet.http import HttpServlet
from clamp import clamp_base

ToolBase = clamp_base("org.python.tools")
```

# Code bridge

Implement `init` method for `javax.servlet.http.HttpServlet`:

```python
class WSGIServlet(ToolBase, HttpServlet):
  def init(self, config):
    application_name = config.getInitParameter(
	  "wsgi.handler")
    parts = application_name.split(".")
    if len(parts) < 2 or not all(parts):
      raise Exception(...)
    module_name = ".".join(parts[:-1])
    module = __import__(module_name)  # DYNAMIC CODE!
    self.application = getattr(module, parts[-1])
    self.servlet_environ = dict(BASE_ENVIRONMENT)
    self.servlet_environ.update({
      "wsgi.errors": AdaptedErrLog(self)
    })
```

# JyNI

* Idea: simply add JyNI jar to the Java CLASSPATH to enable C extension API support
* Written by Stefan Richthofer
* Now works for a number of packages - tkinter
* Challeng is adding full GC support (!)
* Next steps: ctypes, cffi, ...
* Stefan has applied for GSOC

# Future

* Mostly around performance, Java integration, and of course the usual bug fixes
* Python bytecode compiler for Android, large complex methods
* More hooks for Java integration
* Integrating Zippy to provide PyPy-like performance (requires Graal JVM)
* Java 9 may also add more features to optimize dynamic languages

# Jython 3.x?!

Plan to sprint on this!

* Comes up periodically!
* Would be nice for unicode strings and bytestrings to have direct correspondence to Java
* Remove code!
* Release schedule: we will get there at some point!


# What is Jython

* Alternative implementation, with CPython as the reference standard
* Python for the Java Platform
* We no longer accept the idea that Jython lives within "Java's limitations" ;)
* Although there can be profiles of usage - in a container, with a security manager, etc

# Jython 2.7

What I submitted as reviewer notes for this talk:

> I assume Jython 2.7.0 will see a final release by the end of this
> year, and certainly well before PyCon.

Sadly, one last bug precludes this being true. But we were close!


## Intro to Jython 2.7 - 10 minutes total

Overview. 2 min.

Runtime differences between CPython and Jython:

mark-sweep GC instead of hybrid ref counting/GC



# But sequentially consistent

Atomic operations
Sequentially consistent
FIXME

But also where it isn't different, such as
what happens when one thread observes changes by another thread
(sequential consistency/volatile memory semantics). 3 min, with a
sequence diagram to describe observability.

# Compiles into Java bytecode

Compilation of Python into Java bytecode - what does that even look
like?
Demonstrate with a quick decompilation to Java, and how that
hooks into a Jython runtime that is intentionally modeled on what is
in CPython.

# Java bytecode fixes

Example: characteristic, lambda in class decorators - locate change
http://bugs.jython.org/issue2232,
https://hg.python.org/jython/rev/56b94dc065ff

Python AST is visited twice:

1. Scopes compilation - what are the variables used, and what is their scope?
2. Java bytecode generation

```java
     @Override
     public Object visitClassDef(ClassDef node) throws Exception {
        List<expr> decs = node.getInternalDecorator_list();
        for (int i = decs.size() - 1; i >= 0; i--) {
            visit(decs.get(i));
        }
        def(node.getInternalName());
        int n = node.getInternalBases().size();
        for (int i = 0; i < n; i++) {
```		 

Rest of definition:

```java
    @Override
    public Object visitClassDef(ClassDef node) throws Exception {
        List<expr> decs = node.getInternalDecorator_list();
        for (int i = decs.size() - 1; i >= 0; i--) {
            visit(decs.get(i));
        }
        def(node.getInternalName());
        int n = node.getInternalBases().size();
        for (int i = 0; i < n; i++) {
            visit(node.getInternalBases().get(i));
        }
        beginScope(node.getInternalName(), CLASSSCOPE, node, null);
        suite(node.getInternalBody());
        endScope();
        return null;
    }
```

`org.python.compiler.ScopesCompiler`

# Java bytecode generation

```java
    @Override
    public Object visitClassDef(ClassDef node) throws Exception {
        setline(node);

        int baseArray = makeArray(node.getInternalBases());

        //Get class name
        String name = getName(node.getInternalName());
        code.ldc(name);

        code.aload(baseArray);

        ScopeInfo scope = module.getScopeInfo(node);

        scope.setup_closure();
        scope.dump();
        //Make code object out of suite

        module.codeConstant(new Suite(node, node.getInternalBody()), name, false, name,
                            getDocStr(node.getInternalBody()), true, false, node.getLine(), scope,
                            cflags).get(code);

        //Make class out of name, bases, and code
        if (!makeClosure(scope)) {
            code.invokestatic(p(Py.class), "makeClass", sig(PyObject.class, String.class,
                    PyObject[].class, PyCode.class));
        } else {
            code.invokestatic(p(Py.class), "makeClass", sig(PyObject.class, String.class,
                    PyObject[].class, PyCode.class, PyObject[].class));
        }

        applyDecorators(node.getInternalDecorator_list());

        //Assign this new class to the given name
        set(new Name(node, node.getInternalName(), expr_contextType.Store));
        freeArray(baseArray);
        return null;
    }


# Better semantic equivalence

Jython from the start has provided a nearly seamless experience in
combining Java with Python. Jython 2.7 continues this by making all
objects implementing `java.util.Map` be `dict`-like.

# `java.util.HashMap` as a `dict`-like type


Demonstrate what this means by simply importing a class like
`HashMap`. Or calling a method that returns `Map`. But duck types like
`dict`. 2 min.

# Can change Python wrapper at runtime

Method injection (monkeypatching) can provide a workaround, since Java
classes are proxied so they look like Python classes, and from Python,
can be changed.

# Import time experience

Replacement with support by Jython itself. Demonstrate
what this looks like from PyJavaType since code is straightforward. 5
min.

# Abstract base classes

Another feature that goes with this is abstract base classes, so
`issubclass(java.util.Map, collections.Mapping)` now returns
`True`.

Discuss ABC registration and the `__subclasshook__`
protocol. How that supports `java.util.Map` and similar interfaces
from Java. 3 min.

# Implementing `collections.defaultdict` - 6 minutes total

Being on the JVM means Jython can take advantage of some fantastic
libraries. One of these is the Google collections
library. `defaultdict` uses `LoadingCache` objects to ensure it has
thread safe behavior when implmenting missing behavior. But it is easy
to use, as we will demonstrate. 4 min.

Show corresponding tests from Python test suite. In particular,
discuss the extension we did in Jython to test for thread safety of
defaultdict ops, and contrast with CPython. 2 min.

With this motivation, discuss other places we use Google collections
for both thread safety and preventing memory leaks, especially support
for concurrent usage of weakkey and/or weakvalue maps. 4 min.

# Running Python bytecode on Jython - 8 minutes total

Overview. Seriously, is this even possible? (Yes.) Discuss motivation,
including large Python methods (such as the top method represented by
a module) or reduced Jython startup time. 2 min.

Introduce a few Python bytecodes and their semantics - adding numbers
and invoking a function should suffice. 2 min.

Describe interpreter loop provided by ceval.c in CPython. Simply look
at the corresponding cases for the discussed bytecodes in the large
switch provided by ceval.c. 2 min.

Some simple code

```python
def f(a):
    return a + 1
```

the code body of this function diassembles like so:

```
>>> import dis
>>> import simple
>>> dis.dis(simple.f)
  2           0 LOAD_FAST                0 (a)
              3 LOAD_CONST               1 (1)
              6 BINARY_ADD
              7 RETURN_VALUE
```

Show equivalent port provided by PyBytecode in Jython. 2 min.

                    case Opcode.BINARY_ADD: {
                        PyObject b = stack.pop();
                        PyObject a = stack.pop();
                        stack.push(a._add(b));
                        break;
                    }

ceval.c

```c
        case BINARY_ADD:
            w = POP();
            v = TOP();
            if (PyInt_CheckExact(v) && PyInt_CheckExact(w)) {
                /* INLINE: int + int */
                register long a, b, i;
                a = PyInt_AS_LONG(v);
                b = PyInt_AS_LONG(w);
                /* cast to avoid undefined behaviour
                   on overflow */
                i = (long)((unsigned long)a + b);
                if ((i^a) < 0 && (i^b) < 0)
                    goto slow_add;
                x = PyInt_FromLong(i);
            }
            else if (PyString_CheckExact(v) &&
                     PyString_CheckExact(w)) {
                x = string_concatenate(v, w, f, next_instr);
                /* string_concatenate consumed the ref to v */
                goto skip_decref_vx;
            }
            else {
              slow_add:
                x = PyNumber_Add(v, w);
            }
            Py_DECREF(v);
          skip_decref_vx:
            Py_DECREF(w);
            SET_TOP(x);
            if (x != NULL) continue;
            break;
```

Invoking a function is a bit more involved

```c
        case LOAD_GLOBAL:
            w = GETITEM(names, oparg);
			// elide inling...
            x = PyDict_GetItem(f->f_globals, w);
            if (x == NULL) {
                x = PyDict_GetItem(f->f_builtins, w);
                if (x == NULL) {
                  load_global_error:
                    format_exc_check_arg(
                                PyExc_NameError,
                                GLOBAL_NAME_ERROR_MSG, w);
                    break;
                }
            }
            Py_INCREF(x);
            PUSH(x);
            continue;
```

```c
        case CALL_FUNCTION:
        {
            PyObject **sp;
            PCALL(PCALL_ALL);
            sp = stack_pointer;
            x = call_function(&sp, oparg);
            stack_pointer = sp;
            PUSH(x);
            if (x != NULL)
                continue;
            break;
        }
```

vs

collapsed...

```java
                    case Opcode.CALL_FUNCTION: {
                        int na = oparg & 0xff;
                        int nk = (oparg >> 8) & 0xff;

                        if (nk == 0) {
                            call_function(na, stack);
                        } else {
                            call_function(na, nk, stack);
                        }
                        break;
                    }
```


General Python takeaway will be an understanding of generation,
rewriting, and introspection of Python bytecodes.  PyPy also exploits
bytecode interpretation deeply.

## Jython 3.x - 6 minutes total

Some thoughts on what will be in Jython 3.x - we are targeting 3.5 or 3.6, and hopefully not 3.7

# Static type annotations and Jython

# Eecent discussion on optional static
type annotations (4 min), and how this could help Jython support
better Java integration (2 min).

## Questions - 5 minutes total

## Variations

Possible other topics, instead of the above, include the following
changes in 2.7:

* Reworking of calling Java <=> Python integration to prevent resource
  leaks, in the supporting threadlocal management. Discussing this
  topic can give some useful insight into how exactly this integration
  can happen and work efficiently.

* Jython's new socket support, which with its ssl and select
  integration, now uses Netty 4. Jython's implementation provides
  useful guidance into unlocking what exactly are the semantics
  provided by the underlying C APIs, since it's written in Python
  itself.

* Mapping Python's trust model for SSL certificates based on OpenSSL
  to Java's

* Customizing site-packages support for jar inclusion. General Python
  takeaway: .pth files and their power.

* Supporting Java annotations as Python decorators, such that Python
  classes get appropriate metadata added. This includes how to
  leverage import hooks (PEP 302). [Anticipated feature.]

* Using the abstraction provided by super being implemented as a
  descriptor in Python to support calling protected Java super
  methods. [Anticipated bug fix.]

These would be discussed instead of some of the topics mentioned
above - IMHO they are all interesting, and except for the first, touch
upon topics that are of interest to Python generally and not just
Jython.

# Notes to reviewers

I assume Jython 2.7.0 will see a final release by the end of this
year, and certainly well before PyCon.

This will not be a state of Jython talk, with a laundry list of what
was done for Jython 2.7 or of outstanding bugs. Instead attendees will
get some interesting cases of differences running on the JVM, and this
can give them much better insight into what the semantics of Python
actually are and in depth. At the same time, this represents new and
interesting work as well!
