.. _hub_ref:

===
Hub
===

The Hub object encapsulates information about the Hub.

Here's a code snippet of a SmartApp that logs all available information for the Hub when the SmartApp is installed:

.. code-block:: groovy

    def installed() {
        def hub = location.hubs[0]

        log.debug "id: ${hub.id}"
        log.debug "zigbeeId: ${hub.zigbeeId}"
        log.debug "zigbeeEui: ${hub.zigbeeEui}"

        // PHYSICAL or VIRTUAL
        log.debug "type: ${hub.type}"

        log.debug "name: ${hub.name}"
        log.debug "firmwareVersionString: ${hub.firmwareVersionString}"
        log.debug "localIp: ${hub.localIP}"
        log.debug "localSrvPortTCP: ${hub.localSrvPortTCP}"
    }

Below are the available properties on a Hub:

----


firmareVersionString
````````````````````

**Signature:**
    ``String firmwareVersionString``

**Returns:**
    `String`_ - The firmware version of the Hub

**Example:**

.. code-block:: groovy

    List getRealHubFirmwareVersions() {
        return location.hubs*.firmwareVersionString.findAll { it }
    }

----

id
``

The unique system identifier for this Hub.

**Signature:**
    ``String id``

**Returns:**
    `String`_ - the unique device identifier for this Hub.

**Example:**

.. code-block:: groovy

    def eventHandler(evt) {
        log.debug "Hub ID associated with event: ${evt?.hub.id}"
    }

----

localIp
```````

The value of the event as a `Date`_ object, if applicable.

**Signature:**
    ``String localIp``

**Returns:**
    `String`_ - The IP address of the Hub.

----

localSrvPortTCP
```````````````

The local server TCP port of the Hub.

**Signature:**
    ``String localSrvPortTCP``

**Returns:**
    `String`_ - the TCP port for the Hub.

----

name
````

The name of the Hub.

**Signature:**
    ``String name``

**Returns:**
    `String`_ - the name of the Hub.

----

type
````

The type of the Hub. Either "PHYSICAL" or "VIRTUAL".

**Signature:**
    ``String type``

**Returns:**
    `String`_ - the type of the hub.

----

zigbeeEui
`````````

The ZigBee Extended Unique Identifier of the Hub.

**Signature:**
    ``String zigbeeEui``

**Returns:**
    `String`_ - The ZigBee EUI

----

zigbeeId
````````

The ZigBee ID of the Hub.

**Signature:**
    ``String zigbeeId``

**Returns:**
    `String`_  - the ZigBee ID

----

.. _String: http://docs.oracle.com/javase/7/docs/api/java/lang/String.html
