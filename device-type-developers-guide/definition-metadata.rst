Definition
==========

The definition metadata defines core information about your device handler. The initial values are set from the values entered when creating your device handler.

Example definition metadata:

.. code-block:: groovy

    metadata {
        definition(name: "test device", namespace: "yournamespace", author: "your name") {

            capability "Alarm"
            capability "battery"

            attribute "customAttribute", "string"

            command "customCommand"

            fingerprint profileId: "0104", inClusters: "0000,0003,0006",
                        outClusters: "0019"
        }

        ...
    }

The definition method takes a map of parameters, and a closure.

The supported parameters are:

*name*
    The name of the device handler
*namespace*
    The namespace for this device handler. This should be your github user name. This is used when looking up device handlers by name to ensure the correct one is found, even if someone else has used the same name.
*author*
    The author of this device handler.

The closure defines the capabilities, attributes, commands, and fingerprint information for your device handler.

Capabilities
------------

To define that your device supports a capability, simply call the ``capability`` method in the closure passed to ``definition``.

The argument to the ``capability`` method is the Capability name.

.. code-block:: groovy

    capability "Actuator"
    capability "Power Meter"
    capability "Refresh"
    capability "Switch"


Attributes
----------

If you need to define a custom attribute for your device handler, call the ``attribute`` method in the closure passed to the ``definition`` method:

**attribute(String attributeName, String attributeType, List possibleValues = null)**

*attributeName*
    Name of the attribute

*attributeType*
    Type of the attribute. Available types are "string", "number", and "enum"

*possibleValues*
    Optional. The possible values for this attribute. Only valid with the "enum" attributeType.

.. code-block:: groovy

    // String attribute with name "someName"
    attribute "someName", "string"

    // enum attribute with possible values "light" and "dark"
    attribute "someOtherName", "enum", ["light", "dark"]



Commands
--------

To define a custom command for your device handler, call the ``command`` method in the closure passed to the ``definition`` method:

**command(String commandName, List parameterTypes = [])**

*commandName*
    The name of the command. You must also define a method in your device handler with the same name.
*parameterTypes*
    Optional. An ordered list of the parameter types for the command method, if needed.

.. code-block:: groovy

    // command name "myCommand" with no parameters
    command "myCommand"

    // comand name myCommandWithParams that takes a string and a number parameter
    command "myCommandWithParams", ["string", "number"]

    ...

    // each command specified in the definition must have a corresponding method

    def myCommand() {
        // handle command
    }

    // this command takes parameters as defined in the definition
    def myCommandWithParams(stringParam, numberParam) {
        // handle command
    }


Fingerprinting
--------------

When trying to connect your device to the SmartThings hub, we need a way to identify and join a particular device to the hub. This process is known as a "join" process, or "fingerprinting".

The fingerprinting process is dependent on the type of device you are
looking to pair. SmartThings attempts to match devices coming in based on
the input and output clusters a device uses, as well as a profileId
(for ZigBee) or deviceId (for Z-Wave). Basically, by determining what
capabilities your device has, SmartThings determines what your device is.

ZigBee Fingerprinting
~~~~~~~~~~~~~~~~~~~~~

For ZigBee devices, the main profileIds you will need to use are

-  HA: Home Automation (0104)
-  SEP: Smart Energy Profile
-  ZLL: ZigBee Light Link (C05E)

The input and output clusters are defined specifically by your device
and should be available via the device's documentation.

An example of a ZigBee fingerprint definition:

.. code-block:: groovy

        fingerprint profileId: "C05E", inClusters: "0000,0003,0004,0005,0006,0008,0300,1000", outClusters: "0019"


Z-Wave Fingerprinting
~~~~~~~~~~~~~~~~~~~~~

For Z-Wave devices, the fingerprint should include the deviceId of the
device and the command classes it supports in the inClusters list. The
easiest way to find these values is by adding the actual device to
SmartThings and looking for the *Raw Description* in its details view in
the SmartThings developer tools. The device class ID is the four-digit
hexadecimal number (eg. 0x1001) and the command classes are the two-digit
hexadecimal numbers. So if the raw description is ::

    0 0 0x1104 0 0 0 8 0x26 0x2B 0x2C 0x27 0x73 0x70 0x86 0x72

The fingerprint will be

.. code-block:: groovy

    fingerprint deviceId:"0x1104", inClusters:"0x26, 0x2B, 0x2C, 0x27, 0x73, 0x70, 0x86, 0x72"

If the raw description has two lists of command classes separated by a
single digit 'count' number, the second list is the outClusters. So for
the raw description ::

    0 0 0x2001 0 8 0x30 0x71 0x72 0x86 0x85 0x84 0x80 0x70 1 0x20

The fingerprint will be

.. code-block:: groovy

    fingerprint deviceId:"0x2001", inClusters:"0x30, 0x71, 0x72, 0x86, 0x85, 0x84, 0x80, 0x70", outClusters: "0x20"

Note that the fingerprint clusters lists are comma separated while the raw
description is not.

Fingerprinting Best Practices and Important Information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Include Manufacturer and Model
++++++++++++++++++++++++++++++

Try and include the manufacturer and model name to your fingerprint (only supported for ZigBee devices right now - you can add them to Z-Wave devices as well, but they won't be used yet):

.. code-block:: groovy

    fingerprint inClusters: "0000,0001,0003,0020,0406,0500", manufacturer: "NYCE", model: "3014"

When adding the manufacturer model and name, you'll likely need to add the following raw commands in the ``refresh()`` command. This is used to report back the manufacturer/model name from the device.

The reply will be hexadecimal that you can convert to ascii using the hex-to-ascii converter (we'll be adding a utility method to do this, but in the meantime you can use an online converter like `this one <http://www.rapidtables.com/convert/number/hex-to-ascii.htm>`__):

.. code-block:: groovy

    def refresh() {
        "st rattr 0x${zigbee.deviceNetworkId} 0x${zigbee.endpointId} 0 4", "delay 200",
        "st rattr 0x${zigbee.deviceNetworkId} 0x${zigbee.endpointId} 0 5"
    }

Adding Multiple Fingerprints
++++++++++++++++++++++++++++

You can have multiple fingerprints. This is often desirable when a Device Handler should work with multiple versions of a device.

The platform will use the fingerprint with the longest possible match.

Device Pairing Process
++++++++++++++++++++++

The order of the ``inClusters`` and ``outClusters`` lists is not important to the pairing process. It is a best practice, however, to list the clusters in ascending order.

The device can have more clusters than the fingerprint specifies, and it will still pair. If one of the clusters specified in the fingerprint is incorrect, the device will *not* pair.
