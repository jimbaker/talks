% State of Jython
% Jim Baker
% jim.baker@python.org, jibaker@linkedin.com

# Overview

* We are not dead yet! And our apologies.
* Jython 2.7.2
* Project team
* Jython 3
* Jython 3.8 (or later is our plan)*
* Much of what I will present is based on recent discussion with our current lead
  developer, Jeff Allen

*NB - Jython major.minor semantic versioning correspond to CPython's, but our
patch version is incremental. We also reserve the right to support a reasonable subset of CPython features, as well as provide for strong interop with Java.

# Jython 2.7.2 apologies

* Jeff: "I almost feel we should apologise to the PSF for bringing out a 2.7.x at all!" at this time
* Jim: It's been a long journey!

# Jython 2.7.2

* We released a Jython 2.7.2 final in March.
* There was enough interest (feedback on the two betas) to prove downstream projects still care.
* It was a long journey from 2.7.1 (July 2017).
* It took a long time to work off the list of must-fix bugs.
* And a few more contributions and bugs turned up in the 2 years since we made the triage list.
* Thanks to PSF practical support (Mercurial repositories, domain name) and consideration in discussions and PEPs.

# One difficult challenge in 2.7.2 - type publication on Java imports

* Significant reworking of `PyJavaType` types $\Leftrightarrow$ `java.lang.Class` objects
* Solve data race (https://bugs.jython.org/issue2834)
* which followed up on taking into account inner class publication races (https://bugs.jython.org/issue2487)
* which followed up on the deadlocks identified here (https://bugs.jython.org/issue2536)
* and https://bugs.jython.org/issue2609 in `re`
* which followed up on some additional reworking to use weak references to track this mapping to prevent memory leaks
* Completely orthogonal to actual mapping of Java types to Python - goes back to Jim Hugunin's original work on JPython, which also has seen improvements in the 2.7.x series

# Project status

* Capacity in the project is low: three regular contributors and a few "emeritus" members help out.
* We have remained open to one-off contributors and try always to use their work.
* There has also been some contribution to the related "Jython book" (Juneau, Baker, et. al.).
* Regulars are responsive to user enquiries, but not so much to bug reports.
* About a dozen people contributed to 2.7.2. Support for `locale` is a notable one.
* But it's fair to say most of the bug-fixing in 2.7.2 was Jeff's.

# What about Jython 3

* There is currently no Jython 3 we can recommend (although some options like a sandbox effort or Graal in Jython emulation mode).
* We continue to be asked for plans for Jython 3.
* We answer honestly but guardedly, knowing the journey is unpredictable and resources few.
* There is interest in helping with Jython 3 among existing project members and some potential ones.
* Most significantly, we had an offer to pay for development time (contingent on 2.7.2 release).
* This will be challenging but still feels like a worthy endeavor.
* It would be more exciting if we are taking new directions in the core.
* We lack as yet an architectural description to guide contributors (particularly new/paid ones).

# One path not taken for Jython 3

* Github sandbox https://github.com/jython/jython3
* It would be a difficult place to start because ...
* It is a fork from way back (2016).
* A lot of work was done over a short period by one person, with no history in the project, then stopped.
* It has never pulled any bug-fixes from Jython 2.7 (even when it was active).
* It perpetuates things we always said we would put right in Jython 3.
* The pervasive nature of the changes make a merge look very difficult.
* It would still be good to use (and credit) parts we can.

# Planned path for Jython 3 - Jython 3.8

* We have ideas for a significantly different implementation of the core.
* Target Python 3.8 language and stdlib - f-strings, asyncio
* Take advantage of dynamic language features in Java. (Can you believe we don't in 2020?)
* Continue our strengths:
    * Almost seamless Java integration
    * Strong support for concurrency, including underlying concurrent data stuctures or now functional data structures
* An architecture close to CPython internally. (Except around `ThreadState` and the GIL.)
* Comparable performance with CPython (and better if we can), favouring numerical applications.

# More in the same vein

* Plan to do more in Python and less in Java (then go back to optimize hot spots)
* Most likely apply PEG parsing, especially because we can take advantage of corner cases - REPL comes to mind
* Java 11 minimum, modules for API control, Gradle build, stop bundling dependencies.

# Graal Python

* Graal Python is a really interesting experiment. The main draw for Python users is performance.
* It is worth understanding to what extent GraalVM might become a mainstream platform.
* Different approach, eg lack of Python classes inheriting from Java classes (which can inherit from Python classes!)

# The very slow Jython project

* Revisiting our design decisions to clarify our thinking for Jython 3
* Also useful for people interested in what takes to implement a Python
* https://the-very-slow-jython-project.readthedocs.io/en/latest/

# Type stubs for Java

* https://github.com/jimbaker/jstubgen
* POC - not yet generating good type stubs, eg `Optional` (or `X | None`)
* Just a quick hack that shows it's possible to readily extract from Java in a form that can be usable