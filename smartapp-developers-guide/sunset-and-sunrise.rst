==================
Sunset and Sunrise
==================

SmartApps often need to take some action at or around the local sunrise or sunset time. The SmartThings cloud provides access to this type of rich data, and even generates events for the location (if the geofence is set). We can also get access to sunrise and sunset times using a ZIP code.

Sunrise and Sunset Events
-------------------------

Using the sunrise and sunset events is the preferred (and simpler) way to take some action at (or around) sunrise or sunset. It is required that the location has set up a geofence.

**Taking action at sunrise or sunset**

If you wish to have certain actions take place at sunrise or sunset, you can use the *sunrise* and *sunset* events. These events will be fired at (gasp!) sunrise and sunset times for the user's location.

You can subscribe to the events by passing in the location (automatically injected into every SmartApp), the event ("sunrise" or "sunset"), and your handler method:

.. code-block:: groovy

    def installed() {
        subscribe(location, "sunset", sunsetHandler)
        subscribe(location, "sunrise", sunriseHandler)
    }

    def sunsetHandler(evt) {
        log.debug "Sun has set!"
        ...
    }

    def sunriseHandler(evt) {
        log.debug "Sun has risen!"
        ...
    }

**Taking action before or after sunrise/sunset times**

If you want to take some action a certain amount of time before or after sunset or sunrise, you can use the "sunriseTime" and "sunsetTime" events. These events are fired every day around the time of sunset or sunrise, and their value is the *next* sunrise or sunset. You can use this information to calculate an offset so that some action happens a certain amount of time before or after sunrise or sunset.

To use, you can subscribe to the events by passing the location, the event ("sunriseTime" or "sunsetTime"), and the handler method.

Consider the following example that turns on lights a specified number of minutes before sunset for the user's location:

.. code-block:: groovy

    preferences {
        section("Lights") {
            input "switches", "capability.switch", title: "Which lights to turn on?"
            input "offset", "number", title: "Turn on this many minutes before sunset"
        }
    }

    def installed() {
        initialize()
    }

    def updated() {
        unsubscribe()
        initialize()
    }

    def initialize() {
        subscribe(location, "sunsetTime", sunsetTimeHandler)
    
        //schedule it to run today too
        scheduleTurnOn(location.currentValue("sunsetTime"))
    }

    def sunsetTimeHandler(evt) {
        //when I find out the sunset time, schedule the lights to turn on with an offset
        scheduleTurnOn(evt.value)
    }

    def scheduleTurnOn(sunsetString) {
        //get the Date value for the string
        def sunsetTime = Date.parse("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'", sunsetString)
    
        //calculate the offset
        def timeBeforeSunset = new Date(sunsetTime.time - (offset * 60 * 1000))
    
        log.debug "Scheduling for: $timeBeforeSunset (sunset is $sunsetTime)"
    
        //schedule this to run one time
        runOnce(timeBeforeSunset, turnOn)
    }

    def turnOn() {
        log.debug "turning on lights"
        switches.on()
    }

Because the ``sunriseTime`` and ``sunsetTime`` events are fired every day for the *next* sunrise/sunset event, we use ``runOnce`` to schedule one execution. Sunrise and sunset times change, so the next time the events are fired, we will create another scheduled execution using the ``runOnce`` method for that time. 

We want it to run today too, so we use the sunsetTime value of the user's location to schedule the lights to turn on today. 

.. note:: 

    If a user changes their location's geofence, it could change the sunrise and sunset times. You can listen for position change events and reschedule accordingly: ``subscribe(location, "position", locationPositionChangeHandler)``


Looking up Sunrise or Sunset Directly
-------------------------------------

SmartApps can use the provided ``getSunriseAndSunset`` methods to get the sunrise and sunset time. You can pass in a ZIP code, which can be useful if the user has not set a geofence for their location.

**getSunriseAndSunset(Map options)**

The supported options are:

*zipCode*
    Optional. The ZIP code to get the sunrise and sunset data for. Defaults to the user's location if not provided.
*sunsetOffset*
    Optional. A string in the format of "HH:MM" to specify how long or after sunset to return. Use the "-" symbol to indicate time should be before sunset ("-00:30" for 30 minutes prior to sunset)
*sunriseOffset*
    Optional. A string in the format of "HH:MM" to specify how long or after sunrise to return. Use the "-" symbol to indicate time should be before sunrise ("-00:30" for 30 minutes prior to sunrise)
*date*
    Optional. If you want to find the sunrise or sunset time for a date other than today, you can specify a Date object.

The return value is a map in the following form:

``[sunrise: Date, sunset: Date]``

.. code-block:: groovy

    def initialize() {
        def noParams = getSunriseAndSunset()
        def beverlyHills = getSunriseAndSunset(zipCode: "90210")
        def thirtyMinsBeforeSunset = getSunriseAndSunset(sunsetOffset: "-00:30")

        log.debug "sunrise with no parameters: ${noParams.sunrise}"
        log.debug "sunset with no parameters: ${noParams.sunset}"
        log.debug "sunrise and sunset in 90210: $beverlyHills"
        log.debug "thirty minutes before sunset at current location: ${thirtyMinsBeforeSunset.sunset}"

    }

Polling for Sunrise/Sunset
--------------------------

You may have seen some SmartApp code that runs a task sometime after midnight (usually in a method called "astroCheck") and calls a third party weather API to get the sunrise/sunset times. This is strongly discouraged now; it is much more efficient to use location events as they do not rely on third party services.

Examples
--------

You can refer to these example SmartApps in the IDE to see how sunrise and sunset can be used:

- Smart Nightlight
- Sunrise/Sunset

You can also refer to the following examples in Github:

- `Sunset Event Example <https://github.com/SmartThingsCommunity/Code/blob/master/smartapps/sunrise-sunset/turn-on-at-sunset.groovy>`__
- `Sunset Offset Example <https://github.com/SmartThingsCommunity/Code/blob/master/smartapps/sunrise-sunset/turn-on-before-sunset.groovy>`__
- `Sunset by ZIP Code Example <https://github.com/SmartThingsCommunity/Code/blob/master/smartapps/sunrise-sunset/turn-on-by-zip-code.groovy>`__


