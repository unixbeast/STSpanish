.. _storing-data:

Storing Data
============

SmartApps and Device Handlers are all provided a ``state`` variable that will allow you to store data across executions.

In this guide, you will learn:

- How to store data across executions using the ``state`` property.
- A basic understanding of how ``state`` works.
- When ``state`` may not be the best solution, and what to use instead.

.. contents::

Overview
--------

Recall that SmartApps (and Device Handlers) are not always running, but rather execute according to a certain schedule or in response to events being triggered. But what if we need our application to retain some information between executions? We can use ``state`` to persist data across executions.

Here's a quick example showing how to work with state:

.. code-block:: groovy

  state.myData = "some data"
  log.debug "state.myData = ${state.myData}"

  state.myCounter = state.myCounter + 1

How it Works
------------

Each executing instance of a SmartApp or Device Handler has access to a simple, map-like storage mechanism through the ``state`` property.
When an application executes, it populates the ``state`` property from the backing store. The application can then interact with it, through simple map-like operations.

When the application is finished executing, the values in ``state`` are written back to persistent storage.

.. important::

  When an application stores data in ``state``, or reads from it, it is only modifying (or querying) the local ``state`` instance variable within the running SmartApp or Device Handler. Only when the application is done executing are the values written to persistent storage.

The contents of ``state`` are stored as a string, in JSON format. This means that anything stored in ``state`` must be serializable to JSON.

.. tip::

  State is stored in JSON format; for most data types this works fine, but for more complex object types this may cause issues.

  This is particularly worth noting when working with dates. If you need to store time information, consider using an epoch time stamp, conveniently available via the ``now()`` method:

  .. code-block:: groovy

    def installed() {
      state.installedAt = now()
    }

    def someEventHandler(evt) {
      def millisSinceInstalled = now() - state.installedAt
      log.debug "this app was installed ${millisSinceInstalled / 1000} seconds ago"

      // you can also create a Date object back from epoch time:
      log.debug "this app was installed at ${new Date(state.installedAt)}"
    }

Using State
-----------

You can use ``state`` to store strings, numbers, lists, booleans, maps, etc.
To use ``state``, simply use the ``state`` variable that is injected into every SmartApp and Device Handler. You can think of it as a map that will persist its value across executions.

As usual, the best way to describe code is by showing code itself.

.. code-block:: groovy

  def installed() {
    // simple number to keep track of executions
    state.count = 0

    // we can store maps in state
    state.myMap = [foo: "bar", baz: "fee"]

    // booleans are ok of course
    state.myBoolean = true

    // we can use array index notation if we want
    state['key'] = 'value'

    // we can store lists and maps, so we can make some interesting structures
    state.myListOfMaps = [[key1: "val1", bool1: true],
                          [otherKey: ["string 1", "string 2"]]]

  }

  def someEventHandler(evt) {

    // increment by 1
    state.count = state.count + 1

    log.debug "this event handler has been called ${state.count} times since installed"

    log.debug "state.myMap.foo: ${state.myMap.foo}" // => prints "bar"

    // we can access state value using array notation if we wish
    log.debug "state['myBoolean']: ${state['myBoolean']}"

    // we can navigate our list of maps
    state.myListOfMaps.each { map ->
      log.debug "entry: $map"
      map.each {
        log.debug "key: ${it.key}, value: ${it.value}"
      }
    }

Atomic State
------------

.. note::

    Atomic State is currently only available for SmartApps. Device Handlers do not support Atomic State.

Since ``state`` is initialized from persistent storage when a SmartApp executes, and is written to storage only when the application is done executing, there is the possibility that another execution *could* happen within that time window, and cause the values stored in ``state`` to appear inconsistent.

Consider the scenario of a SmartApp that keeps a counter of executions. Each time the SmartApp executes, it increments the counter by 1. Assume that the initial value of ``state.counter`` is ``0``.

1. An execution ("Execution 1") occurs, and increments ``state.counter`` by one:

.. code-block:: groovy

  state.counter = state.counter + 1 // counter == 1

2. Another execution ("Execution 2") occurs *before "Execution 1" has finished*. It reads ``state.counter`` and increments it by one.

.. code-block:: groovy

  state.counter = state.counter + 1 // counter == 1!!!

Because "Execution 1" hasn't finished executing by the time that "Execution 2" begins, the value of ``counter`` is still 0!

Additionally, because the contents of ``state`` are only persisted when execution is complete, it's also possible to inadvertently overwrite values (last finished execution "wins").

To avoid this type of scenario, you can use ``atomicState``. ``atomicState`` writes to the data store when a value is *set*, and reads from the data store when a value is *read* - not just when the application execution initializes and completes. You use it just as you would use ``state``:

.. code-block:: groovy

  atomicState.counter = atomicState.counter + 1.

.. important::

  Using ``atomicState`` instead of ``state`` incurs a higher performance cost, since external storage is touched on read and write operations, not just when the application is initialized or done executing.

  Use ``atomicState`` only if you are sure that using ``state`` will cause problems.

  It's also worth noting that you should **not** use both ``state`` and ``atomicState`` in the same SmartApp. Doing so will likely cause inconsistencies in in state values.

Examples
--------

Here are some SmartApps that make use of state. You can find them in the IDE along with the other example SmartApps.

- "Smart Nightlight" - shows using state to store time information.
- "Laundry Monitor" - uses state to store boolean state and time information.
- "Good Night" - shows using state to store time information, including constructing a Date object from a value stored in state.
