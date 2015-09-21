.. _capability_ref:

Capability
==========

The Capability object encapsulates information about a certain Capability. 

A Capability object cannot be created. You can get the Capabilities for a given device using the `capabilities <device-ref.html#capabilities>`_ method on a :ref:`device_ref` instance:

.. code-block:: groovy
    
    def capabilities = mydevice.capabilities

For documentation for the available Capabilities, you can refer to the :ref:`capabilities_taxonomy`.

----

name
~~~~

The name of the capability.

**Signature:**
    ``String name``

**Returns:**
    `String`_ - the name of the capability.

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "mySwitch", "capability.switch"
        }
    }
    ...
    def mySwitchCaps = mySwitch.capabilities

    // log each capability supported by the "mySwitch" device
    mySwitchCaps.each {cap ->
        log.debug "Capability name: ${cap.name}"
    }
    ...
    
----

attributes
~~~~~~~~~~

**Signature:**
    ``List<Attribute> attributes``

**Returns:**
    `List`_ <:ref:`attribute_ref`> - A list of Attributes of this capability. An empty list will be returned if this Capability has no Attributes.

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "mySwitch", "capability.switch"
        }
    }
    ...
    def mySwitchCaps = mySwitch.capabilities

    // log each capability supported by the "mySwitch" device, along
    // with all its supported attributes
    mySwitchCaps.each {cap ->
        log.debug "Capability name: ${cap.name}"
        cap.attributes.each {attr ->
            log.debug "-- Attribute name; ${attr.name}"
        }
    }
    ...

----

commands
~~~~~~~~

**Signature:**
    ``List<Command> commands``

**Returns:**
    `List`_ <:ref:`command_ref`> - A list of Commands of this capability. An empty list will be returned if this Capability has no commands.

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "mySwitch", "capability.switch"
        }
    }
    ...
    def mySwitchCaps = mySwitch.capabilities

    // log each capability supported by the "mySwitch" device, along
    // with all its supported commands 
    mySwitchCaps.each {cap ->
        log.debug "Capability name: ${cap.name}"
        cap.commands.each {comm ->
            log.debug "-- Command name: ${comm.name}"
        }
    }
    ...

----

.. _String: http://docs.oracle.com/javase/7/docs/api/java/lang/String.html
.. _List: https://docs.oracle.com/javase/7/docs/api/java/util/List.html