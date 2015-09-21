.. _groovy:

Groovy – The SmartThings Programming Language
=============================================

The SmartApp programming language is a `domain-specific language`_  (DSL) based on the `Groovy programming language`_.

What is Groovy?
--------------

Groovy is an object-oriented_ programming language for the `Java platform`_.
It is a `dynamic language`_ with features similar to those of Python_, Ruby_,
Perl_, and Smalltalk_. 

It can be used as a `scripting language`_ for the Java Platform, is dynamically compiled to `Java Virtual Machine`_ (JVM) bytecode_,  and interoperates with other Java code and libraries. 

Groovy uses a Java-like `bracket syntax`_. Most Java code is also syntactically valid Groovy.

----

Why Groovy?
-----------

By basing our first programming language on Groovy, we provide the benefits of a dynamic language with the scalability and performance of Java. Longer term, we hope to enable other programming languages in the SmartThings Cloud so that you can ultimately chose from several supported languages. Likely candidates (no promises) for additional languages include Ruby, JavaScript, Clojure, and
Python.

For more documentation on the syntax, structure, and capabilities of Groovy,
visit the `Groovy Documentation <http://groovy-lang.org/documentation.html>`__. There you will find information for getting started with Groovy, and comprehensive language documentation.

Note, however, that because of the application “sandboxing” that we do in the SmartThings Cloud, some features of Groovy are disabled for security reasons. We will discuss this more in the `Groovy Sandboxing`_ topic below.

----

Groovy Sandboxing
-----------------

SmartThings runs with a sandboxed environment. This means that not all features of the Groovy programming language are available to SmartThings developers. This is done for reasons of security and simplicity. 

Here are some of the restrictions:

No Custom Classes or JARs
~~~~~~~~~~~~~~~~~~~~~~~~~

You cannot upload or import your own classes or JARs. 

Class Restrictions
~~~~~~~~~~~~~~~~~~

You cannot define your own classes within the SmartThings platform, or include any of your own classes. You cannot do this:

.. code-block:: groovy

    class Foo {
        def list
    }

SmartThings allows only certain classes to be used in the sandbox. A SecurityException will be thrown if using a class that is not allowed.

Closure Restrictions
~~~~~~~~~~~~~~~~~~~~

In SmartThings, you cannot define closures outside of methods. For example, you cannot do this:

.. code-block:: groovy

    def squareItClosure = {it * it} 

More information about closures can be found in the Tips & Tricks section below.

Builder Restrictions
~~~~~~~~~~~~~~~~~~~~

If you're familiar with Groovy, you likely know about the `Groovy builder pattern`_. Builders offer a nice way to build a hierarchichal data structure. 

Due to the way builders are implemented using closures, they will not work in SmartThings. This means things like XMLBuilder and JSONBuilder are not available to use.

Method Restrictions
~~~~~~~~~~~~~~~~~~~

Some of the methods you cannot use in SmartThings:

- getClass
- getMetaClass
- setMetaClass
- propertyMissing
- methodMissing
- invokeMethod
- println
- sleep 

Property Restrictions
~~~~~~~~~~~~~~~~~~~~~

You cannot use any of the following properties in SmartThings:

- class
- metaClass

Other restrictions
~~~~~~~~~~~~~~~~~~

A few other things you cannot do in SmartThings:

- Create and use new threads
- Use System methods, like System.out

----

Tips & Tricks
-------------

To get comfortable with Groovy, it's recommended you install it and try it out. You can find information about installing Groovy `here <http://groovy-lang.org/install.html>`__.

You can also use this handy `Groovy web console`_ if you don't have Groovy installed locally. Some features may not be available, but it's a handy way to try things out quick. 

A full discussion of Groovy is obviously beyond the scope of this document, but there are a few key language features that you'll see often in the SmartThings platform that are worth brief discussion here.

GStrings
~~~~~~~~

Groovy Strings. What were you thinking?

GStrings are declared inside double-quotes, and may include expressions. Among other things, this allows us to build strings dynamically without having to worry about concatenation. 

Expressions are defined using the ``${...}`` syntax.

.. code-block:: groovy

    def currentDateString = "The current date is ${new Date()}"

Properties can be referenced directly without the brackets:

.. code-block:: groovy

    def awesomePlatform = "SmartThings"
    def newString = "Programming with $awesomePlatform is fun!"

Optional Parentheses
~~~~~~~~~~~~~~~~~~~~

Method invocations with arguments in Groovy do not always require the arguments to be enclosed in parentheses. 

These are equivalent:

.. code-block:: groovy

    "SmartThings".contains "Smart"
    "SmartThings".contains("Smart")

Optional Return Statements
~~~~~~~~~~~~~~~~~~~~~~~~~~

The return statement may be omitted from a method. The value of the last statement in a method will be the returned value, if the return keyword is not present.

These two methods are equivalent:

.. code-block:: groovy

    def yell() {
        return "all caps".toUpperCase()
    }

    def yellAgain() {
        "all caps".toUpperCase()
    }

Closures
~~~~~~~~

One of the more powerful features of Groovy is its support for closures. We'll leave the exact definition of closures to computer scientists (See the Google machine if you're interested), but for our purposes, think of closures as a way to pass a function to another function.

Why would you want to do that? It allows us to be more expressive in our code, and focus on the *what*, not the *how*. 

The Groovy Collections APIs make heavy use of closures. Consider this example:

.. code-block:: groovy

    def names = ['Erlich', 'Richard', 'Gilfoyle', 'Dinesh', 'Big Head']
    def programmers = names.findAll {
        it != 'Erlich'
    }
    // programmers => ['Richard', 'Gilfoyle', 'Dinesh', 'Big Head']

If you're new to Groovy or functional-style programming, the above code block may look pretty strange. We'll break it down a bit.

The findAll method accepts a closure as an argument. The closure is defined between the brackets. findAll will call the closure (``it != 'Erlich'``) on each element in ``names``. If the item does not equal 'Erlich', it will be added to the returned list (remember the optional return statement).

``it`` is the default variable name for each item the closure will be called with. We can specify a different name if we wish by providing a name followed by ``->``:

.. code-block:: groovy

    def names = ['Erlich', 'Richard', 'Gilfoyle', 'Dinesh', 'Big Head']
    def programmers = names.findAll {dude ->
        dude != 'Erlich'
    }

References and Resources
------------------------

Groovy is simple enough to be able to jump in and start writing code quickly, but powerful enough to get yourself stuck pretty quickly.

Here are a few resources you can use to sharpen your Groovy skills:

- `Groovy Documentation Portal`_
- `Groovy Closures`_
- `Groovy Collections`_
- `Groovy Web Console`_
- `Learn Groovy in 5 Minutes`_

.. _domain-specific language: http://en.wikipedia.org/wiki/Domain-specific_language
.. _Groovy programming language: http://www.groovy-lang.org/
.. _object-oriented: http://en.wikipedia.org/wiki/Object-oriented_programming
.. _Java platform: http://en.wikipedia.org/wiki/Java_platform 
.. _dynamic language: http://en.wikipedia.org/wiki/Dynamic_programming_language 
.. _Python: http://en.wikipedia.org/wiki/Python_(programming_language) 
.. _Ruby: http://en.wikipedia.org/wiki/Ruby_%28programming_language%29 
.. _Perl: http://en.wikipedia.org/wiki/Perl 
.. _Smalltalk: http://en.wikipedia.org/wiki/Smalltalk
.. _scripting language: http://en.wikipedia.org/wiki/Scripting_language
.. _Java Virtual Machine: http://en.wikipedia.org/wiki/Java_Virtual_Machine
.. _bytecode: http://en.wikipedia.org/wiki/Bytecode
.. _bracket syntax: http://en.wikipedia.org/wiki/Curly_bracket_programming_language
.. _closures: http://en.wikipedia.org/wiki/Closure_%28computer_programming%29
.. _Groovy Beginners Tutorial: http://groovy.codehaus.org/Beginners+Tutorial
.. _Groovy Collections: http://groovy-lang.org/groovy-dev-kit.html#_working_with_collections
.. _Groovy Closures: http://groovy-lang.org/closures.html
.. _Groovy builder pattern: http://groovy-lang.org/dsls.html#_builders
.. _Groovy Console: http://groovy.codehaus.org/Groovy+Console
.. _Groovy web console: https://groovyconsole.appspot.com/
.. _Groovy Documentation Portal: http://groovy-lang.org/documentation.html
.. _Learn Groovy in 5 Minutes: http://learnxinyminutes.com/docs/groovy/
