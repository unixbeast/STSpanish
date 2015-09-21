.. _events_and_subscriptions:

========================
Events and Subscriptions
========================

Turn on a light when a door opens. Turn the lights off at sunrise. Send a message if a door opens when you're not home. These are all examples of event-handler SmartApps. They follow a common pattern - subscribe to some event, and take action when the event happens. 

This section will discuss events and how you can subscribe to them in your SmartApp. 

Subscribe to Specific Device Events
-----------------------------------

The most common use case for event subscriptions is for device events:

**subscribe(deviceName, "eventToSubscribeTo", handlerMethodName)**

.. code-block:: groovy 

    preferences {
        section {
            input "theSwitch", "capability.switch"
        }
    }

    def install() {
        subscribe(theSwitch, "switch.on", switchOnHandler)
    }

    def switchOnHandler(evt) {
        log.debug "switch turned on!"
    }

The handler method must accept an event parameter. 

Refer to the :ref:`event_ref` API documentation for more information about the Event object.

You can find the possible events to subscribe to by referring to the Attributes column for a capability in the :ref:`capabilities_taxonomy`. The general form we use is "<attributeName>.<attributeValue>". If the attribute does not have any possible values (for example, "battery"), you would just use the attribute name. 

In the example above, the switch capability has the attribute "switch", with possible values "on" and "off". Putting these together, we use "switch.on".

.. note::

Subscribe to All Device Events
------------------------------

You can also subscribe to all states by just specifying the attribute name:

.. code-block:: groovy
    
    subscribe(theSwitch, "switch", switchHandler)

    def switchHandler(evt) {
        if (evt.value == "on") {
            log.debug "switch turned on!"
        } else if (evt.value == "off") {
            log.debug "switch turned off!"
        }
    }


In this case, the ``switchHandler`` method will be called for both the "on" and "off" events.

Subscribe to Multiple Devices
-----------------------------

If your SmartApp allows multiple devices, you can subscribe to events for all the devices:

.. code-block:: groovy

    preferences {
        section {
            input "switches", "capability.switch", multiple: true
        }
    }

    def installed() {
        subscribe(switches, "switch", switchesHandler)
    }

    def switchesHandler(evt) {
        log.debug "one of the configured switches changed states"
    }

Subscribe to Location Events
----------------------------

In addition to subscribing to device events, you can also subscribe to events for the user's location.

You can subscribe to the following location events:

*mode*
    Triggered when the mode changes.
*position*
    Triggered when the geofence position changes for this location. Does not get triggered when the fence is widened or narrowed - only fired when the position changes.
*sunset*
    Triggered at sunset for this location.
*sunrise*
    Triggered at sunrise for this location.
*sunriseTime*
    Triggered around sunrise time. Used to get the time of the next sunrise for this location.
*sunsetTime*
    Triggered around sunset time. Used to get the time of the next sunset for this location.

Pass in the location property automatically injected into every SmartApp as the first parameter to the subscribe method.

.. code-block:: groovy

    subscribe(location, "mode", modeChangeHandler)

    // shortcut for mode change handler
    subscribe(location, modeChangeHandler)

    subscribe(location, "position", positionChange)
    subscribe(location, "sunset", sunsetHandler)
    subscribe(location, "sunrise", sunriseHandler)
    subscribe(location, "sunsetTime", sunsetTimeHandler)
    subscribe(location, "sunriseTime", sunriseTimeHandler)

Refer to the `Sunset and Sunrise <http://docs.smartthings.com/en/latest/smartapp-developers-guide/sunset-and-sunrise.html>`__ section for more information about sunrise and sunset.

The Event Object
----------------

Event-handler methods must accept a single parameter, the event itself.

Refer to the :ref:`event_ref` API documentation for more information.

A few of the common ways of using the event:

.. code-block:: groovy

    def eventHandler(evt) {
        // get the event name, e.g., "switch"
        log.debug "This event name is ${evt.name}"

        // get the value of this event, e.g., "on" or "off"
        log.debug "The value of this event is ${evt.value}"

        // get the Date this event happened at
        log.debug "This event happened at ${evt.date}"
        
        // did the value of this event change from its previous state?
        log.debug "The value of this event is different from its previous value: ${evt.isStateChange()}"
    }

.. note:: 
    The contents of each Event instance will vary depending on the exact event. If you refer to the Event reference documentation, you will see different value methods, like "floatValue" or "dateValue". These may or may not be populated depending on the specific event, and may even throw exceptions if not applicable. 

See Also
--------

 - `Sunset and Sunrise <sunset-and-sunrise.html>`__
 - :ref:`event_ref` API Documentation
 - :ref:`location_ref` API Documentation
 - `Interacting with Devices <devices.html>`__