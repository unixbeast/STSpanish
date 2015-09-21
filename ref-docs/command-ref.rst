.. _command_ref:

Command
=======

A Command represents an action you can perform on a Device.

An instance of a Command object encapsulates information about that Command. You cannot create a Command object; you can retrieve them from a :ref:`capability_ref` or from a :ref:`device_ref`:

.. code-block:: groovy

    preferences {
        section() {
            input "theswitch", "capability.switch"
        }
    }
    ...

    // Get a list of Commands supported by theswitch:
    def switchCommands = theswitch.supportedCommands
    log.debug "switchCommands: $switchCommands"

    // Iterate through the supported capabilities, log all suported commands:
    // commands property available via the Capability object
    def caps = theswitch.capabilities
    caps.commands.each {comm ->
        log.debug "-- Command name: ${comm.name}"
    }

----

arguments
~~~~~~~~~

The list of argument types for the command.

**Signature:**
    ``List<String> arguments``

**Returns:**
    `List`_ < `String`_ > - A list of the argument types for this command. One of `"STRING"`, `"NUMBER"`, `"VECTOR3`", or `"ENUM"`.

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "theSwitchLevel", "capability.switchLevel"
        }
    }
    ...
    def supportedCommands = theSwitchLevel.supportedCommands

    // logs each command's arguments
    supportedCommands.each {
        log.debug "arguments for swithLevel command ${it.name}: ${it.arguments}"
    }
    ...
----

name
~~~~

The name of the command.

**Signature:**
    ``String name``

**Returns:**
    `String`_ - the name of this command.

**Example:**

.. code-block:: groovy

    preferences {
        section() {
            input "theswitch", "capability.switch"
        }
    }
    ...
    def supportedCommands = theswitch.supportedCommands

    // logs each command name supported by theswitch
    supportedCommands.each {
        log.debug "command name: ${it.name}"
    }
    ...
----

.. _List: https://docs.oracle.com/javase/7/docs/api/java/util/List.html
.. _String: http://docs.oracle.com/javase/7/docs/api/java/lang/String.html
