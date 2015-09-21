.. _attribute_ref:

Attribute
=========

An Attribute represents specific information about the state of a device. For example, the "Temperature Measurement" capability has an attribute named "temperature" that represents the temperature data.

The Attribute object contains metadata information about the Attribute itself - its name, data type, and possible values.

You will typically interact with Attributes values directly, for example, using the :ref:`currentAttributeName` method on a :ref:`device_ref` instance. That will get the *value* of the Attribute, which is typically what SmartApps are most interested in.

You can get the supported Attributes of a Device through the Device's :ref:`supportedAttributes` method.

.. warning::

    Referring to an Attribute directly from a Device by calling ``someDevice.attributeName`` will return an Attribute object with only the ``name`` property available. This is available for legacy purposes only, and will likely be removed at some time. 

    To get a reference to an Attribute object, you should use the ``supportedAttributes`` method on the Device object, and then find the desired Attribute in the returned List.

You can view the available attributes for all Capabilities in our :ref:`capabilities_taxonomy`.

----

dataType
~~~~~~~~~

Gets the data type of this Attribute.

**Signature:**
    ``String dataType``

**Returns:**
    `String`_ - the data type of this Attribute. Possible types are "STRING", "NUMBER", "VECTOR3", "ENUM".

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "thetemp", "capbility.temperatureMeasurement"
        }
    }
    ...
    def attrs = thetemp.supportedAttributes
    attrs.each {
        log.debug "${thetemp.displayName}, attribute ${it.name}, dataType: ${it.dataType}"
    }
    ...

name
~~~~

The name of the Attribute.

**Signature:**
    ``String name``

**Returns:**
    `String`_ - the name of this attribute

**Example:**

.. code-block:: groovy
    
    preferences {
        section() {
            input "myswitch", "capability.switch"
        }
    }
    ...
    // switch capability has an attribute named "switch"
    def switchAttr = myswitch.switch
    log.debug "switch attribute name: ${switchAttr.name}"
    ...

----

values
~~~~~~

The possible values for this Attribute, if the data type is "ENUM".

**Signature:**
    ``List<String> values``

**Returns:**
    `List`_ < `String`_ > - the possible values for this Attribute, if the data type is "ENUM". An empty list is returned if there are no possible values or if the data type is not "ENUM".

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "thetemp", "capbility.temperatureMeasurement"
        }
    }
    ...
    def attrs = thetemp.supportedAttributes
    attrs.each {
        log.debug "${thetemp.displayName}, attribute ${it.name}, values: ${it.values}
        log.debug "${thetemp.displayName}, attribute ${it.name}, dataType: ${it.dataType}"
    }
    ...

----

.. _List: http://docs.oracle.com/javase/7/docs/api/java/util/List.html
.. _String: http://docs.oracle.com/javase/7/docs/api/java/lang/String.html