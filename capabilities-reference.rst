.. _capabilities_taxonomy:

Capabilities Reference
======================

Capabilities are core to the SmartThings architecture.
They allow us to abstract specific devices into their underlying capabilities.


An application interacts with devices based on their capabilities, so once we understand the capabilities that are needed by a SmartApp, and the capabilities that are provided by a device, we can understand which devices (based on the Device's declared capabilities) are eligible for use within a specific SmartApp.

Capabilities themselves are decomposed into both Commands and Attributes.
Commands represent ways in which you can control or actuate the device, whereas Attributes represent state information or properties of the device.

Capabilities are created and maintained by the SmartThings internal development team.


This page serves as a reference for the supported capabilities.

.. note::

    This document is a work in progress.
    Many capabilities are not yet fully documented.
    We are continually working to document all the capabilities.

At a Glance
-----------

The Capabilities reference table below lists all capabilities. The various columns are:

Name:
    The name of the capability that is used by a Device Handler.
Preferences Reference:
    The string you would use in a SmartApp to allow a user to select from devices supporting this capability.
Attributes:
    The attributes that the capability defines.
Commands:
    The commands (and their signatures) that the capability defines.

============================= ====================================== ===================================== ========================
       Name                   Preferences Reference                  Attributes                            Commands
============================= ====================================== ===================================== ========================
:ref:`acceleration-sensor`    capability.accelerationSensor          - acceleration

:ref:`actuator`               capability.actuator
:ref:`alarm`                  capability.alarm                       - alarm                               - off()
                                                                                                           - strobe()
                                                                                                           - siren()
                                                                                                           - both()

:ref:`battery`                capability.battery                     - battery
:ref:`beacon`                 capability.beacon                      - presence
:ref:`button`                 capability.button                      - button
:ref:`c_m_detector`           capability.carbonMonoxideDetector      - carbonMonoxide
:ref:`color_control`          capability.colorControl                - hue                                 - setHue(number)

                                                                     - saturation                          - setSaturation(number)
                                                                     - color                               - setColor(color_map)
:ref:`configuration`          capability.configuration                                                     - configure()
:ref:`contact_sensor`         capability.contactSensor               - contact
:ref:`door_control`           capability.doorControl                 - door                                - open()
                                                                                                           - close()
:ref:`energy_meter`           capability.energyMeter                 - energy
:ref:`illuminance_mesurmnt`   capability.illuminanceMeasurement      - illuminance
:ref:`image_capture`          capability.imageCapture                - image                               - take()
:ref:`lock`                   capability.lock                        - lock                                - lock()
                                                                                                           - unlock()
:ref:`media_controller`       capability.mediaController             - activities                          - startActivity(string)
                                                                     - currentActivity                     - getAllActivities()
                                                                                                           - getCurrentActivity()
:ref:`momentary`              capability.momentary                                                         - push()
:ref:`motion_sensor`          capability.motionSensor                - motion
:ref:`music_player`           capability.musicPlayer                 - status                              - play()
                                                                     - level                               - pause()
                                                                     - trackDescription                    - stop()
                                                                     - trackData                           - nextTrack()
                                                                     - mute                                - playTrack(string)
                                                                                                           - setLevel(number)
                                                                                                           - playText(string)
                                                                                                           - mute()
                                                                                                           - previousTrack()
                                                                                                           - unmute()
                                                                                                           - setTrack(string)
                                                                                                           - resumeTrack(string)
                                                                                                           - restoreTrack(string)
:ref:`notification`           capability.notification                                                      - deviceNotification(string)
:ref:`polling`                capability.polling                                                           - poll()
:ref:`power_meter`            capability.powerMeter                  - power
:ref:`presence_sensor`        capability.presenceSensor              - presence
:ref:`refresh`                capability.refresh                                                           - refresh()
:ref:`rel_hmdty_mesurmnt`     capability.relativeHumidityMeasurement - humidity
:ref:`relay_switch`           capability.relaySwitch                 - switch                              - on()
                                                                                                           - off()
:ref:`sensor`                 capability.sensor
:ref:`signal_strength`        capability.signalStrength              - lqi
                                                                     - rssi

:ref:`sleep_sensor`           capability.sleepSensor                 - sleeping
:ref:`smoke_detector`         capability.smokeDetector               - smoke
:ref:`speech_synthesis`       capability.speechSynthesis                                                   - speak(string)
:ref:`step_sensor`            capability.stepSensor                  - steps
                                                                     - goal
:ref:`switch`                 capability.switch                      - switch                              - on()
                                                                                                           - off()
:ref:`switch_level`           capability.switchLevel                 - level                               - setLevel(number, number)
:ref:`temp_measurement`       capability.temperatureMeasurement      - temperature
:ref:`thermostat`             capability.thermostat                  - temperature                         - setHeatingSetpoint(number)
                                                                     - heatingSetpoint                     - setCoolingSetpoint(number)
                                                                     - coolingSetpoint                     - off()
                                                                     - thermostatSetpoint                  - heat()
                                                                     - thermostatMode                      - emergencyHeat()
                                                                     - thermostatFanMode                   - cool()
                                                                     - thermostatOperatingState            - setThermostatMode(enum)
                                                                                                           - fanOn()
                                                                                                           - fanAuto()
                                                                                                           - fanCirculate()
                                                                                                           - setThermostatFanMode(enum)
                                                                                                           - auto()
:ref:`therm_cooling_setpoint` capability.thermostatCoolingSetpoint   - coolingSetpoint                     - setCoolingSetpoint(number)
:ref:`thermostat_fan_mode`    capability.thermostatFanMode           - thermostatFanMode                   - fanOn()
                                                                                                           - fanAuto()
                                                                                                           - fanCirculate()
                                                                                                           - setThermostatFanMode(enum)
:ref:`therm_heating_setpoint` capability.thermostatHeatingSetpoint   - heatingSetpoint                     - setHeatingSetpoint(number)
:ref:`thermostat_mode`        capability.thermostatMode              - thermostatMode                      - off()
                                                                                                           - heat()
                                                                                                           - emergencyHeat()
                                                                                                           - cool()
                                                                                                           - auto()
                                                                                                           - setThermostatMode(enum)
:ref:`therm_operating_state`  capability.thermostatOperatingState    - thermostatOperatingState
:ref:`thermostat_setpoint`    capability.thermostatSetpoint          - thermostatSetpoint
:ref:`three_axis`             capability.threeAxis                   - threeAxis
:ref:`tone`                   capability.tone                                                              - beep()
:ref:`touch_sensor`           capability.touchSensor                 - touch
:ref:`valve`                  capability.valve                       - contact                             - open()
                                                                                                           - close()
:ref:`water_sensor`           capability.waterSensor                 - water
============================= ====================================== ===================================== ========================

.. _acceleration-sensor:

Acceleration Sensor
-------------------

The Acceleration Sensor capability allows for acceleration detection.

Some use cases for SmartApps using this capability would be detecting if a washing machine is vibrating, or if a case has moved (particularly useful for knowing if a weapon case has been moved).

==================== =====================
Capability Name      Preferences Reference
==================== =====================
Acceleration Sensor  capability.accelerationSensor
==================== =====================

**Attributes:**

============ ====== ===============
Attribute    Type   Possible Values
============ ====== ===============
acceleration String ``"active"`` if acceleration is detected.

                    ``"inactive"`` if no acceleration is detected.
============ ====== ===============

**Commands:**

None.

**SmartApp Example**

.. code-block:: groovy

  // preferences reference
  preferences {
    input "accelerationSensor", "capability.accelerationSensor"
  }

  def installed() {
    // subscribe to active acceleration
    subscribe(accelerationSensor, "acceleration.active", accelerationActiveHandler)

    // subscribe to inactive acceleration
    subscribe(accelerationSensor, "acceleration.inactive", accelerationInactiveHandler)

    // subscribe to all acceleration events
    subscribe(accelerationSensor, "acceleration", accelerationBothHandler)
  }



.. _actuator:

Actuator
--------

The Actuator capability is a "tagging" capability. It defines no attributes or commands.

In SmartThings terms, it represents that a Device has commands.

----

.. _alarm:

Alarm
-----

The Alarm capability allows for interacting with devices that serve as alarms.

.. note::

    Z-Wave sometimes uses the term "Alarm" to refer to an important notification.
    The *Alarm* Capability is used in SmartThings to define a device that acts as an Alarm in the traditional sense (e.g., has a siren and such).

+------------------+--------------------------------+
| Capability Name  | SmartApp Preferences Reference |
+==================+================================+
| Alarm            | capability.alarm               |
+------------------+--------------------------------+

**Attributes:**

=========   =========   ===============
Attribute   Type        Possible Values
=========   =========   ===============
alarm       String      ``"strobe"`` if the alarm is strobing.

                        ``"siren"`` if the alarm is sounding the siren.

                        ``"off"`` if the alarm is turned off.

                        ``"both"`` if the alarm is strobing and sounding the alarm.
=========   =========   ===============

**Commands:**

*strobe()*
    Strobe the alarm

*siren()*
    Sound the siren on the alarm

*both()*
    Strobe and sound the alarm

*off()*
    Turn the alarm (siren and strobe) off

**SmartApp Example:**

.. code-block:: groovy

  // preferences reference
  preferences {
    input "alarm", "capability.alarm"
  }

  def installed() {
    // subscribe to alarm strobe
    subscribe(alarm, "alarm.strobe", strobeHandler)
    // subscribe to all alarm events
    subscribe(alarm, "alarm", allAlarmHandler)
  }

  def strobeHandler(evt) {
    log.debug "${evt.value}" // => "strobe"
  }

  def allAlarmHandler(evt) {
    if (evt.value == "strobe") {
      log.debug "alarm strobe"
    } else if (evt.value == "siren") {
      log.debug "alarm siren"
    } else if (evt.value == "both") {
      log.debug "alarm siren and alarm"
    } else if (evt.value == "off") {
      log.debug "alarm turned off"
    } else {
      log.debug "unexpected event: ${evt.value}"
    }
  }

----

.. _battery:

Battery
-------

Defines that the device has a battery.

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Battery          capability.battery
================ ==============================

**Attributes:**

========== ======= ===============
Attribute  Type    Possible Values
========== ======= ===============
battery
========== ======= ===============

**Commands:**

None

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section() {
      input "thebattery", "capability.battery"
    }
  }

  def installed() {
    def batteryValue = thebattery.latestValue("battery")
    log.debug "latest battery value: $batteryValue"
    subscribe(thebattery, "battery", batteryHandler)
  }

  def batteryHandler(evt) {
    log.debug "battery attribute changed to ${evt.value}"
  }

----

.. _beacon:

Beacon
------

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Beacon           capability.beacon
================ ==============================

**Attributes:**

=========== ======= =================
Attribute   Type    Possible Values
=========== ======= =================
presence    String  ``"present"``
                    ``"not present"``
=========== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section() {
      input "thebeacon", "capability.beacon"
    }
  }

  def installed() {
    def currBeacon = thebeacon.currentValue("presence")
    log.debug "beacon is currently: $currBeacon"
    subscribe(thebeacon, "presence", beaconHandler)
  }

  def beaconHandler(evt) {
    log.debug "beacon presence is: ${evt.value}"
  }

----

.. _button:

Button
------

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Button           capability.button
================ ==============================

**Attributes:**

=========== ======= ====================================
Attribute   Type    Possible Values
=========== ======= ====================================
button      String  ``"held"`` if the button is held (longer than a push)

                    ``"pushed"`` if the button is pushed
=========== ======= ====================================

**Commands:**

None.

**SmartApp Code Example:**

.. code-block:: groovy

  preferences {
    section() {
      input "thebutton", "capability.button"
    }
  }

  def installed() {
    // subscribe to any change to the "button" attribute
    // if we wanted to only subscribe to the button be held, we would use
    // subscribe(thebutton, "button.held", buttonHeldHandler), for example.
    subscribe(thebutton, "button", buttonHandler)
  }

  def buttonHandler(evt) {
    if (evt.value == "held") {
      log.debug "button was held"
    } else if (evt.value == "pushed") {
      log.debug "button was pushed"
    }

    // Some button devices may have more than one button. While the
    // specific implementation varies for different devices, there may be
    // button number information in the jsonData of the event:
    try {
      def data = evt.jsonData
      def buttonNumber = data.buttonNumber as Integer
      log.debug "evt.jsonData: $data"
      log.debug "button number: $buttonNumber"
    } catch (e) {
      log.warn "caught exception getting event data as json: $e"
    }
  }

----

.. _c_m_detector:

Carbon Monoxide Detector
------------------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Carbon Monoxide Detector    capability.carbonMonoxideDetector
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
carbonMonoxide  String  ``"tested"``
                        ``"clear"``
                        ``"detected"``
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section() {
      input "smoke", "capability.smokeDetector", title: "Smoke Detected", required: false, multiple: true
    }
  }

  def installed() {
    subscribe(smoke, "carbonMonoxide.detected", smokeHandler)
  }

  def smokeHandler(evt) {
    log.debug "carbon alert: ${evt.value}"
  }

----

.. _color_control:

Color Control
-------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Color Control               capability.colorControl
=========================   ==============================

**Attributes:**

=============== ======= =============================================
Attribute       Type    Possible Values
=============== ======= =============================================
hue             Number  ``0-100`` (percent)
saturation      Number  ``0-100`` (percent)
color           Map     See the table below for the color options
=============== ======= =============================================

Color Options:

============= ===============================
key           value
============= ===============================
hue           ``0-100 (percent)``
saturation    ``0-100 (percent)``
hex           ``"#000000" - "#FFFFFF" (Hex)``
level         ``0-100 (percent)``
switch        ``"on"`` or ``"off"``
============= ===============================

**Commands:**

*setHue(number)*
    Sets the colors hue value
*setSaturation(number)*
    Sets the colors saturation value
*setColor(color_map)*
    Sets the color to the passed in maps values

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "contact", "capability.contactSensor", title: "contact sensor", required: true, multiple: false
      input "bulb", "capability.colorControl", title: "pick a bulb", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(contact, "contact", contactHandler)
  }

  def contactHandler(evt) {
    if("open" == "$evt.value") {
      bulb.on()  // Turn the bulb on when open (this method does not come directly from the colorControl capability)
      bulb.setHue(80)
      bulb.setSaturation(100)  // Set the color to something fancy
      bulb.setLevel(100)  // Make sure the light brightness is 100%
    } else {
      bulb.off()  // Turn the bulb off when closed (this method does not come directly from the colorControl capability)
    }
  }

----

.. _configuration:

Configuration
-------------

.. note::
    This capability is meant to be used only in device handlers. The implementation of the
    ``configure()`` method will be very specific to the physical device. The commands that
    populate the ``configure()`` method will most likely be found in the device manufacturer's
    documentation. During the device installation lifecycle, the ``configure()`` method is called
    after the device has been assigned a Device Handler.

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Configuration               capability.configuration
=========================   ==============================

**Attributes:**

None.

**Commands:**

*configure()*
    This is where the device specific configuration commands can be implemented.

**Device Handler Example:**

.. code-block:: groovy

  def configure() {
    // update reporting frequency
    def cmd = delayBetween([
      zwave.configurationV1.configurationSet(parameterNumber: 101, size: 4, scaledConfigurationValue: 4).format(), // combined power in watts
      zwave.configurationV1.configurationSet(parameterNumber: 111, size: 4, scaledConfigurationValue: 300).format(), // every 5 min
      zwave.configurationV1.configurationSet(parameterNumber: 102, size: 4, scaledConfigurationValue: 8).format(), // combined energy in kWh
      zwave.configurationV1.configurationSet(parameterNumber: 112, size: 4, scaledConfigurationValue: 300).format(), // every 5 min
      zwave.configurationV1.configurationSet(parameterNumber: 103, size: 4, scaledConfigurationValue: 0).format(), // no third report
      zwave.configurationV1.configurationSet(parameterNumber: 113, size: 4, scaledConfigurationValue: 300).format() // every 5 min
    ])
    log.debug cmd
    cmd
  }

----

.. _contact_sensor:

Contact Sensor
--------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Contact Sensor              capability.contactSensor
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
contact         String  ``"open"``
                        ``"closed"``
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Contact Example") {
      input "contact", "capability.contactSensor", title: "pick a contact sensor", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(contact, "contact", contactHandler)
  }

  def contactHandler(evt) {
    if("open" == evt.value)
      // contact was opened, turn on a light maybe?
      log.debug "Contact is in ${evt.value} state"
    if("closed" == evt.value)
      // contact was closed, turn off the light?
      log.debug "Contact is in ${evt.value} state"
  }

----

.. _door_control:

Door Control
------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Door Control                capability.doorControl
=========================   ==============================

**Attributes:**

========= ======= =================
Attribute Type    Possible Values
========= ======= =================
door      String  ``"unknown"``
                  ``"closed"``
                  ``"open"``
                  ``"closing"``
                  ``"opening"``
========= ======= =================

**Commands:**

*open()*
    Opens the door
*close()*
    Closes the door

----

.. _energy_meter:

Energy Meter
------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Energy Meter                capability.energyMeter
=========================   ==============================

**Attributes:**

========= ======= =================
Attribute Type    Possible Values
========= ======= =================
energy    Number  ``numeric value representing energy consumption``
========= ======= =================

**Commands:**

None.

.. code-block:: groovy

  preferences {
    section("Title") {
      input "outlet", "capability.switch", title: "outlet", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(outlet, "energy", myHandler)
    subscribe(outlet, "switch", myHandler)
  }

  def myHandler(evt) {
    log.debug "$outlet.currentEnergy"
  }

----

.. _illuminance_mesurmnt:

Illuminance Measurement
-----------------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Illuminance Measurement     capability.illuminanceMeasurement
=========================   ==============================

**Attributes:**

=========== ======= =================
Attribute   Type    Possible Values
=========== ======= =================
illuminance Number  ``numeric value representing illuminance``
=========== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "lightSensor", "capability.illuminanceMeasurement"
      input "light", "capability.switch"
    }
  }

  def installed() {
    subscribe(lightSensor, "illuminance", myHandler)
  }

  def myHandler(evt) {
    def lastStatus = state.lastStatus
    if (lastStatus != "on" && evt.integerValue < 30) {
      light.on()
      state.lastStatus = "on"
    }
    else if (lastStatus != "off" && evt.integerValue > 50) {
      light.off()
      state.lastStatus = "off"
    }
  }

----

.. _image_capture:

Image Capture
-------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Image Capture               capability.imageCapture
=========================   ==============================

**Attributes:**

========= ======= =================
Attribute Type    Possible Values
========= ======= =================
image     String  ``string value representing the image captured``
========= ======= =================

**Commands:**

*take()*
    Capture an image

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Choose one or more, when..."){
      input "motion", "capability.motionSensor", title: "Motion Here", required: false, multiple: true
    }
    section("Take a burst of pictures") {
      input "camera", "capability.imageCapture"
    }
  }

  def installed() {
    subscribe(motion, "motion.active", takePhotos)
  }

  def takePhotos(evt) {
    camera.take()
    (1..4).each {
      camera.take(delay: (1000 * it))
    }
    log.debug "$camera.currentImage"
  }

----

.. _lock:

Lock
----

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Lock                        capability.lock
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
lock            String  ``"locked"``
                        ``"unlocked"``
=============== ======= =================

**Commands:**

*lock()*
    Lock the device
*unlock()*
    Unlock the device

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "lock", "capability.lock", title:"door lock", required: true, multiple: false
      input "motion", "capability.motionSensor", title:"motion", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(motion, "motion", myHandler)
  }

  def myHandler(evt) {
    if(!("locked" == lock.currentLock) && "active" == evt.value) {
      lock.lock()
    }
  }

----

.. _media_controller:

Media Controller
----------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Media Controller            capability.mediaController
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
activities
currentActivity
=============== ======= =================

**Commands:**

*startActivity(string)*
    Start the activity with the given name
*getAllActivities()*
    Get a list of all the activities
*getCurrentActivity()*
    Get the current activity

----

.. _momentary:

Momentary
---------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Momentary                   capability.momentary
=========================   ==============================

**Attributes:**

None.

**Commands:**

*push()*
    Press the momentary switch

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "doorOpener", "capability.momentary", title: "Door Opener", required: true, multiple: false
      input "presence", "capability.presenceSensor", title: "presence", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(presence, "presence", myHandler)
  }

  def myHandler(evt) {
    if("present" == evt.value) {
      doorOpener.push()
    }
  }

----

.. _motion_sensor:

Motion Sensor
-------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Motion Sensor               capability.motionSensor
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
motion          String  ``"active"``
                        ``"inactive"``
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Choose one or more, when..."){
      input "motion", "capability.motionSensor", title: "Motion Here", required: true, multiple: true
      input "myswitch", "capability.switch", title: "switch", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(motion, "motion", myHandler)
  }

  def myHandler(evt) {
    if("active" == evt.value) {
      myswitch.on()
    } else if("inactive" == evt.value) {
      myswitch.off()
    }
  }

----

.. _music_player:

Music Player
------------

.. note::
    The music player capability is still under development. It currently supports the Sonos system
    and as such is implemented in a way that is tailored to Sonos.

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Music Player                capability.musicPlayer
=========================   ==============================

**Attributes:**

================ ======= =================
Attribute        Type    Possible Values
================ ======= =================
status           String  ``state of the music player as a string``
level            Number  ``0-100`` (percent)
trackDescription String  ``description of the current playing track``
trackData        JSON    ``a JSON data structure that represents current track data``
mute             String  ``"muted"``
                         ``"unmuted"``
================ ======= =================

**Commands:**

*play()*
    Start music playback
*pause()*
    Pause music playback
*stop()*
    Stop music playback
*nextTrack()*
    Advance to next track
*playTrack(string)*
    Play the track matching the given string (the string is a URI for the track to be played)
*setLevel(number)*
    Set the volume to the specified level (the number represents a percent)
*playText(string)*
    play the given string as text to speech
*mute()*
    Mute playback
*previousTrack()*
    Go back to the previous track
*unmute()*
    Unmute playback
*setTrack(string)*
    Set the track to be played (does not play the track)
*resumeTrack(map)*
    Set and play the given track and maintain queue position
*restoreTrack(map)*
    Restore the track with the given data

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "player", "capability.musicPlayer", title: "music player", required: true, multiple: false
      input "frontDoor", "capability.contactSensor", title: "front door", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(frontDoor, "contact", myHandler)
  }

  def myHandler(evt) {
    if("open" == evt.value) {
      player.playText("The front door is open")
    }
  }

----

.. _notification:

Notification
------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Notification                capability.notification
=========================   ==============================

**Attributes:**

None.

**Commands:**

*deviceNotification(string)*
    Send the device the specified notification.

----

.. _polling:

Polling
-------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Polling                     capability.polling
=========================   ==============================

**Attributes:**

None.

**Commands:**

*poll()*
    Poll devices

----

.. _power_meter:

Power Meter
-----------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Power Meter                 capability.powerMeter
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
power
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section {
      input(name: "meter", type: "capability.powerMeter", title: "When This Power Meter...", required: true, multiple: false, description: null)
      input(name: "threshold", type: "number", title: "Reports Above...", required: true, description: "in either watts or kw.")
    }
    section {
      input(name: "switches", type: "capability.switch", title: "Turn Off These Switches", required: true, multiple: true, description: null)
    }
  }

  def installed() {
    subscribe(meter, "power", meterHandler)
  }

  def meterHandler(evt) {
    def meterValue = evt.value as double
    def thresholdValue = threshold as int
    if (meterValue > thresholdValue) {
      log.debug "${meter} reported energy consumption above ${threshold}. Turning of switches."
      switches.off()
    }
  }

----

.. _presence_sensor:

Presence Sensor
---------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Presence Sensor             capability.presenceSensor
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
Presence        String  ``"present"``
                        ``"not present"``
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "presence", "capability.presenceSensor", title: "presence", required: true, multiple: false
      input "myswitch", "capability.switch", title: "switch", required: true, multiple: true
    }
  }

  def installed() {
    subscribe(presence, "presence", myHandler)
  }

  def myHandler(evt) {
    if("present" == evt.value) {
      myswitch.on()
    } else {
      myswitch.off()
    }
  }

----

.. _refresh:

Refresh
-------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Refresh                     capability.refresh
=========================   ==============================

**Attributes:**

None.

**Commands:**

*refresh()*
    Refresh

----

.. _rel_hmdty_mesurmnt:

Relative Humidity Measurement
-----------------------------

=============================   ==============================
Capability Name                 SmartApp Preferences Reference
=============================   ==============================
Relative Humidity Measurement   capability.relativeHumidityMeasurement
=============================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
humidity
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Bathroom humidity sensor") {
      input "bathroom", "capability.relativeHumidityMeasurement", title: "Which humidity sensor?"
    }
    section("Coffee maker to turn on") {
      input "coffee", "capability.switch", title: "Which switch?"
    }
  }

  def installed() {
    subscribe(bathroom, "humidity", coffeeMaker)
  }

  def coffeeMaker(shower) {
    if (shower.value.toInteger() > 50) {
      coffee.on()
    }
  }

----

.. _relay_switch:

Relay Switch
------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Relay Switch                capability.relaySwitch
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
switch          String  ``"off"``
                        ``"on"``
=============== ======= =================

**Commands:**

*off()*
    Turn the switch off
*on()*
    Turn the switch on

----

.. _sensor:

Sensor
------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Sensor                      capability.sensor
=========================   ==============================

**Attributes:**

None.

**Commands:**

None.

----

.. _signal_strength:

Signal Strength
---------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Signal Strength             capability.signalStrength
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
lqi             Number  A number representing the Link Quality Indication
rssi            Number  A number representing the Received Signal Strength Indication
=============== ======= =================

**Commands:**

None.

----

.. _sleep_sensor:

Sleep Sensor
------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Sleep Sensor                capability.sleepSensor
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
sleeping        String  ``"not sleeping"``
                        ``"sleeping"``
=============== ======= =================

**Commands:**

None.

----

.. _smoke_detector:

Smoke Detector
--------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Smoke Detector              capability.smokeDetector
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
smoke           String  ``"detected"``
                        ``"clear"``
                        ``"tested"``
=============== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "smoke", "capability.smokeDetector", title: "smoke", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(smoke, "smoke", myHandler)
  }

  def myHandler(evt) {
    if("detected" == evt.value) {
      // Sound an alarm! Send a SMS! or Change a HUE bulb color
    }
  }

----

.. _speech_synthesis:

Speech Synthesis
----------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Speech Synthesis            capability.speechSynthesis
=========================   ==============================

**Attributes:**

None.

**Commands:**

*speak(string)*
    It can talk!
----

.. _step_sensor:

Step Sensor
-----------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Step Sensor                 capability.stepSensor
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
steps
goal
=============== ======= =================

**Commands:**

None.

----

.. _switch:

Switch
------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Switch                      capability.switch
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
switch          String  ``"off"``
                        ``"on"``
=============== ======= =================

**Commands:**

*on()*
    Turn the switch on
*off()*
    Turn the switch off

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "myswitch", "capability.switch", title: "switch", required: true, multiple: false
      input "motion", "capability.motionSensor", title: "motion", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(motion, "motion", myHandler)
  }

  def myHandler(evt) {
    if("active" == evt.value && "on" != myswitch.currentSwitch) {
      myswitch.on()
    } else if ("inactive" == evt.value && "off" != myswitch.currentSwitch) {
      myswitch.off()
    }
  }

----

.. _switch_level:

Switch Level
------------

=========================   ==============================
Capability Name             SmartApp Preferences Reference
=========================   ==============================
Switch Level                capability.switchLevel
=========================   ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
level           Number  A number that represents the current light level, usually ``0 - 100`` in percent
=============== ======= =================

**Commands:**

*setLevel(number, number)*
    Set the level to the given numbers

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Title") {
      input "myswitch", "capability.switchLevel", title: "switch", required: true, multiple: false
      input "motion", "capability.motionSensor", title: "motion", required: true, multiple: false
    }
  }

  def installed() {
    subscribe(motion, "motion", myHandler)
  }

  def myHandler(evt) {
    if("active" == evt.value && "on" != myswitch.currentSwitch) {
      myswitch.setLevel(90) // also turns on the switch
    } else if ("inactive" == evt.value && "off" != myswitch.currentSwitch) {
      myswitch.setLevel(10)
    }
  }

----

.. _temp_measurement:

Temperature Measurement
-----------------------

=========================== ==============================
Capability Name             SmartApp Preferences Reference
=========================== ==============================
Temperature Measurement     capability.temperatureMeasurement
=========================== ==============================

**Attributes:**

======================== ======= =================
Attribute                Type    Possible Values
======================== ======= =================
temperature              Number  A number that usually represents the current temperature
======================== ======= =================

**Commands:**

None.

**SmartApp Example:**

.. code-block:: groovy

  preferences {
    section("Cooling based on the following devices") {
      input "sensor", "capability.temperatureMeasurement", title: "Temp Sensor", required: true, multiple: false
      input "outlet", "capability.switch", title: "outlet", required: true, multiple: false
    }
    section("Set the desired temperature to cool to..."){
      input "setpoint", "decimal", title: "Set Temp"
    }
  }

  def installed() {
    subscribe(sensor, "temperature", myHandler)
  }

  def myHandler(evt) {
    if(evt.doubleValue > setpoint && "off" == outlet.currentSwitch) {
      outlet.on()
    } else if(evt.doubleValue < setpoint && "on" == outlet.currentSwitch) {
      outlet.off()
    }
  }

----

.. _thermostat:

Thermostat
----------

=========================== ==============================
Capability Name             SmartApp Preferences Reference
=========================== ==============================
Thermostat                  capability.thermostat
=========================== ==============================

**Attributes:**

======================== ======= =================
Attribute                Type    Possible Values
======================== ======= =================
temperature
heatingSetpoint
coolingSetpoint
thermostatSetpoint
thermostatMode           String  ``"auto"``
                                 ``"emergency heat"``
                                 ``"heat"``
                                 ``"off"``
                                 ``"cool"``
thermostatFanMode        String  ``"auto"``
                                 ``"on"``
                                 ``"circulate"``
thermostatOperatingState String  ``"heating"``
                                 ``"idle"``
                                 ``"pending cool"``
                                 ``"vent economizer"``
                                 ``"cooling"``
                                 ``"pending heat"``
                                 ``"fan only"``
======================== ======= =================

**Commands:**

*setHeatingSetpoint(number)*

*setCoolingSetpoint(number)*

*off()*

*heat()*

*emergencyHeat()*

*cool()*

*setThermostatMode(enum)*

*fanOn()*

*fanAuto()*

*fanCirculate()*

*setThermostatFanMode(enum)*

*auto()*

----

.. _therm_cooling_setpoint:

Thermostat Cooling Setpoint
---------------------------

=========================== ==============================
Capability Name             SmartApp Preferences Reference
=========================== ==============================
Thermostat Cooling Setpoint capability.thermostatCoolingSetpoint
=========================== ==============================

**Attributes:**

================== ======= =================
Attribute          Type    Possible Values
================== ======= =================
coolingSetpoint
================== ======= =================

**Commands:**

*setCoolingSetpoint(number)*

----

.. _thermostat_fan_mode:

Thermostat Fan Mode
-------------------

=========================== ==============================
Capability Name             SmartApp Preferences Reference
=========================== ==============================
Thermostat Fan Mode         capability.thermostatFanMode
=========================== ==============================

**Attributes:**

================== ======= =================
Attribute          Type    Possible Values
================== ======= =================
thermostatFanMode  String  ``"on"``
                           ``"auto"``
                           ``"circulate"``
================== ======= =================

**Commands:**

*fanOn()*

*fanAuto()*

*fanCirculate()*

*setThermostatFanMode(enum)*

----

.. _therm_heating_setpoint:

Thermostat Heating Setpoint
---------------------------

=========================== ==============================
Capability Name             SmartApp Preferences Reference
=========================== ==============================
Thermostat Heating Setpoint capability.thermostatHeatingSetpoint
=========================== ==============================

**Attributes:**

=============== ======= =================
Attribute       Type    Possible Values
=============== ======= =================
heatingSetpoint
=============== ======= =================

**Commands:**

*setHeatingSetpoint(number)*

----

.. _thermostat_mode:

Thermostat Mode
---------------

========================== ==============================
Capability Name            SmartApp Preferences Reference
========================== ==============================
Thermostat Mode            capability.thermostatMode
========================== ==============================

**Attributes:**

============== ======= =================
Attribute      Type    Possible Values
============== ======= =================
thermostatMode String  ``"emergency heat"``
                       ``"heat"``
                       ``"cool"``
                       ``"off"``
                       ``"auto"``
============== ======= =================

**Commands:**

*off()*

*heat()*

*emergencyHeat()*

*cool()*

*auto()*

*setThermostatMode(enum)*

----

.. _therm_operating_state:

Thermostat Operating State
--------------------------

========================== ==============================
Capability Name            SmartApp Preferences Reference
========================== ==============================
Thermostat Operating State capability.thermostatOperatingState
========================== ==============================

**Attributes:**

======================== ======= ======================
Attribute                Type    Possible Values
======================== ======= ======================
thermostatOperatingState String  ``"idle"``
                                 ``"fan only"``
                                 ``"vent economizer"``
                                 ``"cooling"``
                                 ``"pending heat"``
                                 ``"heating"``
                                 ``"pending cool"``
======================== ======= ======================

**Commands:**

None.

----

.. _thermostat_setpoint:

Thermostat Setpoint
-------------------

=================== ==============================
Capability Name     SmartApp Preferences Reference
=================== ==============================
Thermostat Setpoint capability.thermostatSetpoint
=================== ==============================

**Attributes:**

=================== ======= =================
Attribute           Type    Possible Values
=================== ======= =================
thermostatSetpoint
=================== ======= =================

**Commands:**

None.

----


.. _three_axis:

Three Axis
----------

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Three Axis       capability.threeAxis
================ ==============================

**Attributes:**

=========== ======= =================
Attribute   Type    Possible Values
=========== ======= =================
threeAxis
=========== ======= =================

**Commands:**

None.

----

.. _tone:

Tone
----

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Tone             capability.tone
================ ==============================

**Attributes:**

None.

**Commands:**

*beep()*
    Beep the device.

----

.. _touch_sensor:

Touch Sensor
------------

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Touch Sensor     capability.touchSensor
================ ==============================

**Attributes:**

=========== ======= =================
Attribute   Type    Possible Values
=========== ======= =================
touch       String  ``"touched"``
=========== ======= =================

**Commands:**

None.

----

.. _valve:

Valve
-----

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Valve            capability.valve
================ ==============================

**Attributes:**

=========== ======= =================
Attribute   Type    Possible Values
=========== ======= =================
contact     String  ``"closed"``
                    ``"open"``
=========== ======= =================

**Commands:**

=========== ========== =================
Command     Parameters Description
=========== ========== =================
open()
close()
=========== ========== =================

----

.. _water_sensor:

Water Sensor
------------

================ ==============================
Capability Name  SmartApp Preferences Reference
================ ==============================
Water Sensor     capability.waterSensor
================ ==============================

**Attributes:**

=========== ======= =================
Attribute   Type    Possible Values
=========== ======= =================
water       String  ``"dry"``
                    ``"wet"``
=========== ======= =================

**Commands:**

None.
