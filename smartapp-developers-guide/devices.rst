=======
Devices
=======

SmartApps almost always interact with devices. We often need to get information about a specific device (is this switch on?), or send a device a command (turn this switch off).

Device Overview
---------------

Devices are the "things" that SmartApps interact with. Devices may support one or many capabilities.

Capabilities represent the things a device knows (attributes) and the things they can do (commands). They are an abstraction that allows us to work with many different manufacturer's devices transparently.

To build a flexible SmartApp, we should write our SmartApp to work with any device that supports a given capability. We don't want to write a SmartApp that only works with a specific manufacturer's switch for example. We want to write an app that works with any device that supports the switch capability.

Preferences - Selecting the Devices
-----------------------------------

To allow the user to select devices that support a given capability, we use the preferences input element:

.. code-block:: groovy

    preferences {
        section {
            input "presenceSensors", "capability.presenceSensor"
        }
    }

The above example will allow the user to select any device that supports the presence sensor capability. This could be a mobile phone, or a `SmartSense presence sensor <https://shop.smartthings.com/#!/products/smartsense-presence>`__. We don't care about the specific device - we just declare we want a device that supports the presence sensor capability.

You can refer to the :ref:`capabilities_taxonomy` for information on all the supported capabilities. The "Preferences Reference" column tells you what to use in your preferences for a given capability.

Interacting with Devices
------------------------

After you have declared the devices your SmartApp needs to interact with, a Device object instance will be available in your SmartApp, with the name that you provided.

.. code-block:: groovy

    preferences {
        section {
            input "theSwitch", "capability.switch"
        }
    }

    def someEventHandler(evt) {
        theSwitch.on()
    }

Device Attributes
-----------------

Attributes represent the state of a device. A device that supports the "temperatureMeasurement" capability has a "temperature" attribute, for example.

Attributes have state -  the "temperature" attribute has an associated :ref:`state_ref` object that contains information about the temperature (its value, the date it was recorded, etc.)

Device Commands
---------------

Devices may expose one or many commands. Commands are the things that devices can do. A switch supports the "on" and "off" commands, that turn the switch "on" and "off", respectively.

Not all devices have commands. Commands typically perform some sort of physical actuation (turn a switch on, or unlock a lock, for example). A humidity sensor has nothing to physically actuate, for example.


Getting Device Current Values
-----------------------------

You can retrieve information about a device's current state in a few ways.

The ``currentState`` method and ``<attributeName>State`` properties both return a :ref:`state_ref` object representing the current state of this device.

.. code-block:: groovy

    preferences {
        section {
            input "tempSensor", "capability.temperatureMeasurement"
        }
    }

    def someEventHandler(evt) {

        def currentState = tempSensor.currentState("temperature")
        log.debug "temperature value as a string: ${currentState.value}"
        log.debug "time this temperature record was created: ${currentState.date}"

        // shortcut notation - temperature measurement capability supports
        // a "temperature" attribute. We then append "State" to it.
        def anotherCurrentState = tempSensor.temperatureState
        log.debug "temperature value as an integer: ${anotherCurrentState.integerValue}"
    }

You can get the current value directly by using the ``currentValue(attributeName)`` and its shortcut, ``current<Uppercase attribute name>``:

.. code-block:: groovy

    preferences {
        section {
            input "myLock", "capability.lock"
        }
    }

    def someEventHandler(evt) {
        def currentValue = myLock.currentValue("lock")
        log.debug "the current value of myLock is $currentValue"

        // Lock capability has "lock" attribute.
        // <deviceName>.current<uppercase attribute name>:
        def anotherCurrentValue = myLock.currentLock
        log.debug "the current value of myLock using shortcut is: $anotherCurrentValue"
    }


Querying Event History
----------------------

To get a list of events in reverse chronological order (newest first), use the ``events()`` method:

.. code-block:: groovy

    // returns the last 10 by default
    myDevice.events()

    // use the max option to get more results
    myDevice.events(max: 30)

----

To get a list of events in reverse chronological order (newest first) since a given date, use the ``eventsSince`` method:

.. code-block:: groovy

    // get all events for this device since yesterday (maximum of 1000 events)
    myDevice.eventsSince(new Date() - 1)

    // get the most recent 20 events since yesterday
    myDevice.eventsSince(new Date() - 1, [max: 20])

----

To get a list of events between two dates, use the ``eventsBetween`` method:

.. code-block:: groovy

    // get all events between two days ago and yesterday (up to 1000 events)
    // returned events sorted in inverse chronological order (newest first)
    myDevice.eventsBetween(new Date() - 2, new Date() - 1)

    // get the most recent 50 events in the last week
    myDevice.eventsBetween(new Date() - 7, new Date(), [max: 50])

Similar date-constrained methods exist for getting State information for a device.

Refer to the full :ref:`device_ref` API documentation for more information.

Sending Commands
----------------

SmartApps often need to send commands to a device - tell a switch to turn on, or a lock to unlock, for example.

The commands available to your device will vary by device. You can refer to the :ref:`capabilities_taxonomy` to see the available commands for a given capability.

Sending a command is as simple as calling the command method on the device:

.. code-block:: groovy

    myLock.lock()
    myLock.unlock()

Some commands may expect parameters. All commands can take an optional map parameter, as the last argument, to specify delay time in milliseconds to wait before the command is sent to the device:

.. code-block:: groovy

    // wait two seconds before sending on command
    mySwitch.on([delay: 2000])


.. note::

    Because specific devices *can* provide more commands than its supported capabilities, it is possible to have more available commands than the capability declares. As a best practice, you should write your SmartApp to the capabilities specification, and not to any specific device. If, however, you are writing a SmartApp for a very specific case, and are willing to forgo the flexibility, you may make use of this ability.

Interacting with Multiple Devices
---------------------------------

If you specified ``multiple:true`` in your device preferences, the user may have selected more than one device. Your device instance will refer to a list of objects if this is the case.

You can send commands to all the devices without needing to iterate over each one:

.. code-block:: groovy

    preferences {
        section {
            input "switches", "capability.switch", multiple: true
        }
    }

    def someEventHandler(evt) {
        log.debug "will send the on() command to ${switches.size()} switches"
        switches.on()
    }

You can also retrieve state and event history for multiple devices, using the methods discussed above. Instead of single values or objects, they will return a list of values or objects.

Here's a simple example of getting all switch state values and logging the switches that are on:

.. code-block:: groovy

    preferences {
        section {
            input "switches", "capability.switch", multiple: true
        }
    }

    def someEventHandler(evt) {
        // returns a list of the values for all switches
        def currSwitches = switches.currentSwitch

        def onSwitches = currSwitches.findAll { switchVal ->
            switchVal == "on" ? true : false
        }

        log.debug "${onSwitches.size() out of ${switches.size()} switches are on"
    }

See Also
--------

 - :ref:`capabilities_taxonomy`
 - `Preferences and Settings <preferences-and-settings>`__
 - `Events and Subscriptions <simple-event-handler-smartapps.html>`__
 - :ref:`device_ref` API Documentation
 - :ref:`event_ref` API Documentation
 - :ref:`state_ref` API Documentation


.. _Preferences and Settings: :doc:`preferences-and-settings`
