=====
Modes
=====

SmartThings allows users to specify that SmartApps only execute when in certain *modes*.

In this chapter, you will learn:

- What modes are, and how they are used
- How to get and set the current mode
- How to configure your SmartApp to allow a user to select a mode

Overview
--------

Modes can be thought of as behavior filters for the smart home. Users can change how things act or behave based on the mode you're in. For example:

- When in "Home" mode, motion should turn on a light.
- When in "Away" mode, motion should send a text message and turn on an alarm.

SmartThings comes with a few pre-configured modes, such as "Home", "Away", and "Night". Users can also create their own modes for each location.

Getting the Current Mode
------------------------

You can get the current mode by using the ``mode`` or ``currentMode`` property on the ``location`` in a SmartApp:

.. code-block:: groovy

    def currMode = location.mode // "Home", "Away", etc.
    log.debug "current mode is $currMode"

    def anotherWay = location.currentMode
    log.debug "current mode is $anotherWay"

Getting all Modes
-----------------

You can get a list of all the modes for the location the SmartApp is installed into:

.. code-block:: groovy

    def allModes = location.modes // ex: [Home, Away, Night]
    log.debug "all modes for this location: $allModes"


Setting the Mode
----------------

You can use ``setLocationMode()`` or ``location.setMode()`` to set the mode for the location:

.. code-block:: groovy

    setLocationMode("Away")

.. code-block:: groovy

    location.setMode("Away")

These methods will raise an error if the mode specified does not exist for the location.

Allowing Users to Select Modes
------------------------------

In the SmartApp ``preferences`` block, you can specify that the user select a mode by using the ``"mode"`` input type:

.. code-block:: groovy

  input "modes", "mode", title: "select a mode(s)", multiple: true

This will allow the user to select a mode (or multiple modes), and the SmartApp can then vary its behavior based upon the mode(s) selected.

You can also use the ``mode()`` method to allow a user to select a mode that this SmartApp will execute for:

 .. code-block:: groovy

    mode(title: "Set for specific mode(s)")

The SmartApp will then only execute when in the selected mode, without any action needed by the developer to determine the correct mode.

You can learn more about the various ways to allow a user to select a mode :ref:`here <mode_pref>`.

Mode Events
-----------

You can listen for a mode change by subscribing to the ``"mode"`` on the ``location`` object:

.. code-block:: groovy

    def installed() {
        subscribe(location, "mode", modeChangeHandler)
    }

    def modeChangeHandler(evt) {
        log.debug "mode changed to ${evt.value}"
    }

In the example above ``modeChangeHandler()`` will be called whenever the mode changes for the location this SmartApp is installed into.

Example
-------

The following example is a simplified version of the "Scheduled Mode Change" SmartApp. You can view the SmartApp in the IDE templates for the full example.

This example shows how to use the ``"mode"`` input type to ask the user to select a mode, and then (based on the user-defined schedule), changes the mode as specified.

.. code-block:: groovy

    preferences {
        section("At this time every day") {
		      input "time", "time", title: "Time of Day"
	    }
        section("Change to this mode") {
            input "newMode", "mode", title: "Mode?"
        }
    }

    def installed() {
        initialize()
    }

    def updated() {
        unschedule()
        initialize()
    }

    def initialize() {
        schedule(time, changeMode)
    }

    def changeMode() {
        log.debug "changeMode, location.mode = $location.mode, newMode = $newMode, location.modes = $location.modes"

        if (location.mode != newMode) {
            if (location.modes?.find{it.name == newMode}) {
                setLocationMode(newMode)
            }  else {
                log.warn "Tried to change to undefined mode '${newMode}'"
            }
        }
    }

In the ``changeMode()`` method above, there are a few things worth calling out.

First, notice we first check if we are already in the mode specified - if we are, we don't do anything:

.. code-block:: groovy

    if (location.mode != newMode)

If we do need to change the mode, we first verify that the mode actually exists. This ensures that we don't try and set the mode to one that does not exist for the location.

.. code-block:: groovy

    if (location.modes?.find{it.name == newMode})

.. tip::

    Note the use of the ``?`` operator above. This Groovy operator is referred to as the *Safe Navigation* operator, and it allows us to avoid a ``NullPointerException`` that might occur if ``location.modes`` returned null, in the example above. If ``location.modes`` did evaluate to null, the rest of the statement simply wouldn't execute.

    If you come from a Java background, you might be used to writing the above with an ``if`` check for ``null``. The ``?`` operator allows you to accomplish the same task, without cluttering your code with such boilerplate instructions.

    You can read more about this operator `here <http://docs.groovy-lang.org/latest/html/documentation/#_safe_navigation_operator>`__.

Further Reading
---------------

- :ref:`Mode Input <mode_pref>`
- :ref:`Location Object <location_ref>`
- :ref:`Mode Object <mode_ref>`
