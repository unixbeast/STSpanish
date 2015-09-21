.. _state_ref:

State
=====

A State object encapsulates information about a particular :ref:`attribute_ref` at a particular moment in time.

State objects are associated with a :ref:`device_ref` - a Device may have zero-to-many :ref:`attribute_ref` s, and an Attribute has zero-to-many associated State records.

Refer to the `Devices <../smartapp-developers-guide/devices.html>`_ section of the SmartApp Guide for more information about the relationship between Devices, Attributes, and State.

A few ways to get a State object instance from a device (See the :ref:`device_ref` API reference for detailed information):

.. code-block:: groovy

    preferences {
        section() {
            input "thecontact", "capability.contactSensor"
        }
    }
    ...
    // <device>.<attributeName>State
    def latestState = thecontact.contactState

    // <device>.currentState(<attributeName>)
    def latestState2 = thecontact.currentState("contact")

    // get a list of states between two dates
    def recentStates = thecontact.statesBetween(new Date() - 5, new Date())

----

date
~~~~

The date and time the State object was created.

**Signature:**
    ``Date date``

**Returns:**
    `Date`_ - the Date this State object was created.

**Example:**

.. code-block:: groovy

    def stateDate = contactSensor?.currentState("contact").date

----

dateValue
~~~~~~~~~

The value of the underlying attribute as a Date.

**Signature:**
    ``Date dateValue``

**Returns:**
    `Date`_ - the value if the underlying attribute as a Date. Returns ``null`` if the attribute value cannot be parsed into a Date.

----

doubleValue
~~~~~~~~~~~

The value of the underlying Attribute as a Double.

**Signature:**
    ``Double doubleValue``

**Returns:**
    `Double`_ - the value of the underlying attribute as a Double.

.. warning::
    
    ``doubleValue`` throws an Exception if the underlying attribute value cannot be parsed into a Double. 

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy
    
    try {
        def latestStateAsDouble = someDevice.currentState("someAttribute").doubleValue
        log.debug "latestStateAsDouble: $latestStateAsDouble"
    } catch (e) {
        log.debug "caught exception trying to get double for state record"
    }

----

floatValue
~~~~~~~~~~

The value of the underlying Attribute as a Float.

**Signature:**
    ``Float floatValue``

**Returns:**
    `Float`_ - the value of the underlying Attribute as a Float.

.. warning::
    
    ``doubleValue`` throws an Exception if the underlying attribute value cannot be parsed into a Double. 

    You should wrap calls in a try/catch block.

**Example:**
    
.. code-block:: groovy

    try {
        def latestStateAsFloat = someDevice.currentState("someAttribute").floatValue
        log.debug "latestStateAsFloat: $latestStateAsFloat"
    } catch (e) {
        log.debug "caught exception trying to get floatValue for state record"
    }    

----

id
~~

The unique system identifier for the State object.

**Signature:**
    ``String id``

**Returns:**
    `String`_ - the unique system identifer for the State object.

**Example:**

.. code-block:: groovy

    def latestState = someDevice.currentState("someAttribute")
    log.debug "latest state id: ${latestState.id}"

----

integerValue
~~~~~~~~~~~~

The value of the underlying Attribute as an Integer.

**Signature:**
    ``Integer floatValue``

**Returns:**
    `Integer`_ - the value of the underlying Attribute as a Integer.

.. warning::
    
    ``integerValue`` throws an Exception if the underlying attribute value cannot be parsed into a Integer. 

    You should wrap calls in a try/catch block.

**Example:**
    
.. code-block:: groovy

    try {
        def latestStateAsInt = someDevice.currentState("someAttribute").integerValue
        log.debug "latestStateAsInt: $latestStateAsInt"
    } catch (e) {
        log.debug "caught exception trying to get integerValue for state record"
    }    


----

isoDate
~~~~~~~

The acquisition time of this State object as an ISO-8601 String

**Signature:**
    ``String isoDate``

**Returns:**
    `String`_ - the time this Sate object was created as an ISO-8601 Strring

**Example:**

.. code-block:: groovy

    def latestState = someDevice.currentState("someAttribute")
    log.debug "latest state isoDate: ${latestState.isoDate}"

----

jsonValue
~~~~~~~~~

Value of the underlying Attribute parsed into a JSON data structure.

**Signature:**
    ``Object jsonValue``

**Returns:**
    `Object`_ - the value if the underlying Attribute parsed into a JSON data structure.

.. warning::
    
    ``jsonValue`` throws an Exception of the underlying attribute value cannot be parsed into a Integer. 

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    try {
        def latestStateAsJSONValue = someDevice.currentState("someAttribute").jsonValue
        log.debug "latestStateAsJSONValue: $latestStateAsJSONValue"
    } catch (e) {
        log.debug "caught exception trying to get jsonValue for state record"
    }    

----

longValue
~~~~~~~~~

The value of the underlying Attribute as a Long.

**Signature:**
    ``Long longValue``

**Returns:**
    `Long`_ - the value if the underlying Attribute as a Long.

.. warning::
    
    ``longValue`` throws an Exception of the underlying attribute value cannot be parsed into a Long. 

    You should wrap calls in a try/catch block.

**Example:**
    
.. code-block:: groovy

    try {
        def latestStateAsLong = someDevice.currentState("someAttribute").longValue
        log.debug "latestStateAsLong: $latestStateAsLong"
    } catch (e) {
        log.debug "caught exception trying to get longValue for state record"
    }    


----

name
~~~~

The name of the underlying Attribute.

**Signature:**
    ``String name``

**Returns:**
    `String`_ - the name of the underlying Attribute.

**Example:**

.. code-block:: groovy

    def latest = contactSensor.currentState("contact")
    log.debug "name: ${latest.name}"

----

numberValue
~~~~~~~~~~~

The value of the underlying Attribute as a BigDecimal.

**Signature:**
    ``BigDecimal numberValue``

**Returns:**
    `BigDecimal`_ - the value if the underlying Attribute as a BigDecimal.

.. warning::
    
    ``numberValue`` throws an Exception of the underlying attribute value cannot be parsed into a BigDecimal. 

    You should wrap calls in a try/catch block.

**Example:**
    
.. code-block:: groovy

    try {
        def latestStateAsNumber = someDevice.currentState("someAttribute").numberValue
        log.debug "latestStateAsNumber: $latestStateAsNumber"
    } catch (e) {
        log.debug "caught exception trying to get numberValue for state record"
    }    

----

numericValue
~~~~~~~~~~~~

The value of the underlying Attribute as a BigDecimal.

**Signature:**
    ``BigDecimal numericValue``

**Returns:**
    `BigDecimal`_ - the value if the underlying Attribute as a BigDecimal.

.. warning::
    
    ``numericValue`` throws an Exception of the underlying attribute value cannot be parsed into a BigDecimal. 

    You should wrap calls in a try/catch block.

**Example:**
    
.. code-block:: groovy

    try {
        def latestStateAsNumber = someDevice.currentState("someAttribute").numericValue
        log.debug "latestStateAsNumber: $latestStateAsNumber"
    } catch (e) {
        log.debug "caught exception trying to get numericValue for state record"
    }    

----

stringValue
~~~~~~~~~~~

The value of the underlying Attribute as a String

**Signature:**
    ``String stringValue``

**Returns:**
    `String`_ - the value of the underlying Attribute as a String.

**Example:**

.. code-block:: groovy

    def latest = contactSensor.currentState("contact")
    log.debug "stringValue: ${latest.stringValue}"

----

unit
~~~~

The unit of measure for the underlying Attribute.

**Signature:**
    ``String unit``

**Returns:**
    `String`_ - the unit of measure for the underlying Attribute, if applicable, ``null`` otherwise.

**Example:**

.. code-block:: groovy

    def latest = tempSensor.currentState("temperature")
    log.debug "unit: ${latest.unit}"

----

value
~~~~~

The value of the underlying Attribute as a String

**Signature:**
    ``String value``

**Returns:**
    `String`_ - the value of the underlying Attribute as a String.

**Example:**

.. code-block:: groovy

    def latest = contactSensor.currentState("contact")
    log.debug "stringValue: ${latest.value}"

----

xyzValue
~~~~~~~~

Value of the underlying Attribute as a 3-entry Map with keys 'x', 'y', and 'z' with BigDecimal values. For example:

.. code-block:: groovy

    [x: 1001, y: -23, z: -1021]

Typically only useful for getting position data from the "Three Axis" Capability.

**Signature:**
    ``Map<String, BigDecimal> xyzValue``

**Returns:**
    `Map`_ < `String`_ , `BigDecimal`_ > - A map representing the X, Y, and Z coordinates.

.. warning::
    
    ``xyzValue`` throws an Exception if the value of the Event cannot be parsed to an X-Y-Z data structure.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def latest = threeAxisDevice.currentState("threeAxis")

    // get the value of this event as a 3 entry map with keys 
    //'x', 'y', 'z', and BigDecimal values
    // throws an exception if the value is not convertable to a Date
    try {
        log.debug "The xyzValue of this event is ${latest.xyzValue}"
        log.debug "latest.xyzValue instanceof Map? ${latest.xyzValue instanceof Map}"
    } catch (e) {
        log.debug "Trying to get the xyzValue threw an exception: $e"
    }
    

----

.. _BigDecimal: http://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html
.. _Boolean: http://docs.oracle.com/javase/7/docs/api/java/lang/Boolean.html
.. _Date: http://docs.oracle.com/javase/7/docs/api/java/util/Date.html
.. _Double: https://docs.oracle.com/javase/7/docs/api/java/lang/Double.html?is-external=true
.. _Float: https://docs.oracle.com/javase/7/docs/api/java/lang/Float.html
.. _Integer: https://docs.oracle.com/javase/7/docs/api/java/lang/Integer.html
.. _Object: http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html
.. _String: http://docs.oracle.com/javase/7/docs/api/java/lang/String.html
.. _Map: http://docs.oracle.com/javase/7/docs/api/java/util/Map.html
.. _Number: http://docs.oracle.com/javase/7/docs/api/java/lang/Number.html
.. _Long: https://docs.oracle.com/javase/7/docs/api/java/lang/Long.