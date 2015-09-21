.. _event_ref:

Event
=====

Events are core to the SmartThings platform. The allow SmartApps to respond to changes in the physical environment, and build automations around them.

Event instances are not created directly by SmartApp or Device Handlers. They are created internally by the SmartThings platform, and passed to SmartApp event handlers that have subscribed to those events.

.. note::

    In a SmartApp or Device Handler, the method ``createEvent`` exists to create a Map that defines properties of an Event. Only by returning the resulting map from a Device Handler's ``parse`` method is an actual Event instance created and propogated through the SmartThings system. 

The reference documentation here lists all properties and methods available on an Event object instance.

----

date
~~~~

Acquisition time of this device state record.

**Signature:** 
    ``Date date``

**Returns:**
    `Date`_ - the date and time this event record was created.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event created at: ${evt.date}"
    }

----

id
~~

The unique system identifier for this event.

**Signature:** 
    ``String id``

**Returns:**
    `String`_ - the unique device identifier for this event.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event id: ${evt.id}"
    }

----

dateValue
~~~~~~~~~

The value of the event as a `Date`_ object, if applicable.

**Signature:** 
    ``Date dateValue``

**Returns:**
    `Date`_ - If the value of this event is date, a Date will be returned. ``null`` will be returned if the value of the event is not parseable to a Date.

.. warning::

    Calling ``dateValue`` on an Event that does not have a value that is parseable into a Date object will throw an exception.

    You should wrap calls in a try/catch block.


**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as a Date
        // throws an exception if the value is not convertable to a Date
        try {
            log.debug "The dateValue of this event is ${evt.dateValue}"
            log.debug "evt.dateValue instanceof Date? ${evt.dateValue instanceof Date}"
        } catch (e) {
            log.debug "Trying to get the dateValue for ${evt.name} threw an exception: $e"
        }
    } 

----

description
~~~~~~~~~~~

The raw description that generated this Event.

**Signature:**
    ``String description``

**Returns:**
    `String`_ - the raw description that generated this Event.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event raw description: ${evt.description}"
    }

----

descriptionText
~~~~~~~~~~~~~~~

The description of the event that is to be displayed to the user in the mobile application.

**Signature:**
    ``String descriptionText``

**Returns:**
    `String`_ - the description of this event to be displayed to the user in the mobile application.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event description text: ${evt.descriptionText}"
    }

----

device
~~~~~~

The :ref:`device_ref` associated with this Event.

**Signature:**
    ``Device device``

**Returns:**
    :ref:`device_ref` - the Device associated with this Event, or ``null`` if no Device is associated with this Event.
----

displayName
~~~~~~~~~~~

**Signature:** 
    ``String displayName``

**Returns:**
    `String`_ - The user-friendly name of the source of this event. Typically the user-assigned device label.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event display name: ${evt.displayName}"
    }

----

deviceId
~~~~~~~~

The unique system identifer of the :ref:`device_ref` associated with this Event.

**Signature:**
    ``String deviceId``

**Returns:**
    `String`_  - the unique system identifier of the device assocaited with this Event, or null if there is no device associated with this Event.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "The device id for this event: ${evt.deviceId}"
    }

----


doubleValue
~~~~~~~~~~~

The value of this Event, if the value can be parsed to a Double.

**Signature:**
    ``Double doubleValue``

**Returns:**
    `Double`_ - the value of this Event as a Double.

.. warning::

    ``doubleValue`` will throw an Exception if the value of the event is not parseable to a Double.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHander(evt) {
        // get the value of this event as an Double
        // throws an exception of the value is not convertable to a Double
        try {
            log.debug "The doubleValue of this event is ${evt.doubleValue}"
            log.debug "evt.doubleValue instanceof Double? ${evt.doubleValue instanceof Double}"
        } catch (e) {
            log.debug "Trying to get the doubleValue for ${evt.name} threw an exception: $e"
        }
    }
----

floatValue
~~~~~~~~~~

The value of this Event as a Float, if it can be parsed into a Float.

**Signature:**
    ``Float foatValue``

**Returns:**
    `Float`_ - the value of this Event as a Float.

.. warning::
    
    ``floatValue`` will throw an Exception if the Event's value is not parseable to a Float.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as an Float
        // throws an exception if not convertable to Float
        try {
            log.debug "The floatValue of this event is ${evt.floatValue}"
            log.debug "evt.floatValue instanceof Float? ${evt.floatValue instanceof Float}"
        } catch (e) {
            log.debug "Trying to get the floatValue for ${evt.name} threw an exception: $e"
        }    
    }

----

.. hub
.. ~~~~
.. ----

hubId
~~~~~

The unique system identifer of the Hub associated with this Event.

**Signature:**
    ``String hubId``

**Returns:**
    `String`_ - the unique system identifier of the Hub associated with this Event, or ``null`` if no Hub is associated with this Event.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "The hub id associated with this event: ${evt.hubId}"
    }

----

installedSmartAppId
~~~~~~~~~~~~~~~~~~~

The unique system identifier of the SmartApp instance associated with this Event.

**Signature:**
    ``String installedSmartApp``

**Returns:**
    `String`_ - the unique system identifier of the SmartApp instance associated with this Event.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "The installed SmartApp id associated with this event: ${evt.installedSmartAppId}"
    }

----

integerValue
~~~~~~~~~~~~

The value of this Event as an Integer.

**Signature:**
    ``Integer integerValue``

**Returns:**
    `Integer`_ - the value of this Event as an Integer.

.. warning:: 

    ``integerValue`` throws an Exception of the Event value cannot be parsed to an Integer.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as an Integer
        // throws an exception if not convertable to Integer
        try {
            log.debug "The integerValue of this event is ${evt.integerValue}"
            log.debug "The integerValue of this event is an Integer: ${evt.integerValue instanceof Integer}"
        } catch (e) {
            log.debug "Trying to get the integerValue for ${evt.name} threw an exception: $e"
        }
    }

----

isDigital()
~~~~~~~~~~~

``true`` if the Event is from the digital actuation (non-physical) of a Device, ``false`` otherwise.

**Signature:**
    ``Boolean physical()``

**Returns:**
    `Boolean`_ - ``true`` if the Event is from the digital actuation of a Device, ``false`` otherwise.

**Example:**
    
.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event from digital actuation? ${evt.isDigital()}"
    }

----

isoDate
~~~~~~~

Acquisition time of this Event as an ISO-8601 String.

**Signature:**
    ``String isoDate``

**Returns:**
    `String`_ - The acquisition time of this Event as an ISO-8601 String.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event isoDate: ${evt.isoDate}"
    }    

----

isPhysical()
~~~~~~~~~~~~

``true`` if the Event is from the physical actuation of a Device, ``false`` otherwise.

**Signature:**
    ``Boolean physical()``

**Returns:**
    `Boolean`_ - ``true`` if the Event is from the physical actuation of a Device, ``false`` otherwise.

**Example:**
    
.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "event from physical actuation? ${evt.isPhysical()}"
    }

----

isStateChange()
~~~~~~~~~~~~~~~

``true`` if the Attribute value for this Event is different than the previous one.

**Signature:**
    ``Boolean stateChange()``

**Returns:**
    `Boolean`_ - ``true`` if the Attribute value for this Event is different than the previous one.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "Is this event a state change? ${evt.isStateChange()}"
    }

----

jsonValue
~~~~~~~~~

Value of the Event as a parsed JSON data structure.

**Signature:**
    ``Object jsonValue``

**Returns:**
    `Object`_ - The value of the Event as a JSON structure

.. warning::
    
    ``jsonValue`` throws an Exception if the value of the Event cannot be parsed into a JSON object.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as a JSON structure
        // throws an exception if the value is not convertable to JSON
        try {
            log.debug "The jsonValue of this event is ${evt.jsonValue}"
        } catch (e) {
            log.debug "Trying to get the jsonValue for ${evt.name} threw an exception: $e"
        } 
    }

----

linkText
~~~~~~~~

.. warning:: 

    Deprecated.

    Using the ``linkText`` property is deprecated. Use `displayName`_ instead.

The user-friendly name of the source of this event. Typically the user-assigned device label.

----

location
~~~~~~~~

The Location associated with this Event.

**Signature:**
    ``Location location``

**Returns:**
    :ref:`location_ref` - The Location associated with this Event, or ``null`` if no Location is associated with this Event.

----

locationId
~~~~~~~~~~

The unique system identifier for the :ref:`location_ref` associated with this Event.

**Signature:**
    ``String locationId``

**Returns:**
    `String`_ - the unique system identifier for the :ref:`location_ref` associated with this Event.

----

longValue
~~~~~~~~~

The value of this Event as a Long.

**Signature:**
    ``Long longValue``

**Returns:**
    `Long`_ - the value of this Event as a Long.

.. warning::
    
    ``longValue`` throws an Exception if the value of the Event cannot be parsed to a Long.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as an Long
        // throws an exception if not convertable to Long
        try {
            def evtLongValue = evt.longVaue
            log.debug "The longValue of this event is evtLongValue"
            log.debug "evt.longValue instanceof Long? ${evtLongValue instanceof Long}"
        } catch (e) {
            log.debug "Trying to get the longValue for ${evt.name} threw an exception: $e"
        }
    }

----

name
~~~~

The name of this Event.

**Signature:**
    ``String name``

**Returns:**
    `String`_ - the name of this event.

**Example:**
    
.. code-block:: groovy
    
    def eventHandler(evt) {
        log.debug "the name of this event: ${evt.name}"
    }

----

numberValue
~~~~~~~~~~~

The value of this Event as a Number.

**Signature:**
    ``BigDecimal numberValue``

**Returns:**
    `BigDecimal`_ - the value of this event as a BigDecimal.

.. warning::
    
    ``numberValue`` throws an Exception if the value of the Event cannot be parsed to a BigDecimal.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as an Number
        // throws an exception if the value is not convertable to a Number
        try {
            def evtNumberValue = evt.numberValue
            log.debug "The numberValue of this event is ${evtNumberValue}"
            log.debug "evt.numberValue instanceof BigDecimal? ${evtNumberValue instanceof BigDecimal}"
        } catch (e) {
            log.debug "Trying to get the numberValue for ${evt.name} threw an exception: $e"
        }    
    }

----

numericValue
~~~~~~~~~~~~

The value of this Event as a Number.

**Signature:**
    ``BigDecimal numericValue``

**Returns:**
    `BigDecimal`_ - the value of this event as a BigDecimal.

.. warning::
    
    ``numericValue`` throws an Exception if the value of the Event cannot be parsed to a BigDecimal.

    You should wrap calls in a try/catch block.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        // get the value of this event as an Number
        // throws an exception if the value is not convertable to a Number
        try {
            def evtNumberValue = evt.numericValue
            log.debug "The numericValue of this event is ${evtNumberValue}"
            log.debug "evt.numericValue instanceof BigDecimal? ${evtNumberValue instanceof BigDecimal}"
        } catch (e) {
            log.debug "Trying to get the numericValue for ${evt.name} threw an exception: $e"
        }    
    }
    
----

source
~~~~~~

The source of the Event.

**Signature:**
    ``String source``

**Returns:**
    `String`_ - the source of the Event. The following table lists the possible sources and their meaning:

    ================ ===========
    Source           Description
    ================ ===========
    `"APP"`          Event originated by an app touch event in the mobile application.
    `"APP_COMMAND"`  Event originated by using the mobile application (for example, using the mobile application to turn a light off)
    `"COMMAND"`      Event originated by a SmartApp or Device Handler calling a command on a device.
    `"DEVICE`"       Event originated by the physical actuation of a device.
    `"HUB"`          Event originated on the hub.
    `"LOCATION"`     Event originated by a Location state change (for example, sunrise and sunset events)
    `"USER"`
    ================ ===========

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "The source of this event is: ${evt.source}"
    }

----

stringValue
~~~~~~~~~~~

The value of this Event as a String.

**Signature:**
    ``String stringValue``

**Returns:**
    `String`_ - the value of this event as a String.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "The value of this event as a string: ${evt.stringValue}"
    }

----

unit
~~~~

The unit of measure for this Event, if applicable.

**Signature:**
    ``String unit``

**Returns:**
    `String`_ - the unit of measure of this Event, if applicable. ``null`` otherwise.

**Example:**
    def eventHandler(evt) {
        log.debug "The unit for this event: ${evt.unit}"
    }

----

value
~~~~~

The value of this Event as a String.

**Signature:**
    ``String stringValue``

**Returns:**
    `String`_ - the value of this event as a String.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "The value of this event as a string: ${evt.value}"
    }

----

xyzValue
~~~~~~~~

Value of the event as a 3-entry Map with keys 'x', 'y', and 'z' with BigDecimal values. For example:

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

    def positionChangeHandler(evt) {
        // get the value of this event as a 3 entry map with keys 
        //'x', 'y', 'z', and BigDecimal values
        // throws an exception if the value is not convertable to a Date
        try {
            log.debug "The xyzValue of this event is ${evt.xyzValue }"
            log.debug "evt.xyzValue instanceof Map? ${evt.xyzValue  instanceof Map}"
        } catch (e) {
            log.debug "Trying to get the xyzValue for ${evt.name} threw an exception: $e"
        }
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