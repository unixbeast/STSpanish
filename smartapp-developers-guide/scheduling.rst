Scheduling
==========

.. contents:: Topics

SmartApps often have the need to schedule certain actions to take place at a given point in time. For example, an app may want to turn off the lights five minutes after someone leaves. Or maybe an app wants to turn the lights on every day at a certain time.

Broadly speaking, there are a few different ways we might want to schedule something to happen:

- Do something after a certain time amount from now.
- Do something once at certain time in the future.
- Do something on a recurring schedule.

We'll look at each scenario in detail, and what methods SmartThings makes available to address these requirements.

Schedule From Now
-----------------

A SmartApp may want to take some action in a certain amount of time after some event has occurred. Consider a few examples:

- When a door closes, turn a light off after two minutes.
- When everyone leaves, adjust the thermostat after ten minutes.
- If a door opens and is not shut after five minutes, send a notification.

All these scenarios follow a common pattern: when a certain event happens, take some action after a given amount of time. This can be accomplished this by using the *runIn* method.

**runIn(seconds, method, options)**

Executes the specified method in the specified number of seconds from now:

.. code-block:: groovy


    def someEventHandler(evt) {
        // execute handler in five minutes from now
        runIn(60*5, handler)
    }

    def handler() {
        switch.off()
    }


*options*
    Optional. Map of options. Valid options:

    [overwrite: ``true`` or ``false``]

    By default, if a method is scheduled to run in the future, and then another call to runIn with the same method is made, the last one overwrites the previously scheduled method. This is usually preferable. Consider the situation if we have a switch scheduled to turn off after five minutes of a door closing. First, the door closes at 2:50 and we schedule the switch to turn off after five minutes (2:55). Then two minutes later (2:52), the door opens and closes again - another call to runIn will be made to schedule the switch to turn off in five minutes from now (2:57). If we specified ``[overwrite: false]``, we'd now have two schedules to turn off the switch - one at 2:55, and one at 2:57. So, if you do specify ``[overwrite: false]``, be sure to write your handler so that it can handle multiple calls.

.. code-block:: groovy


    def someEventHandler(evt) {
        runIn(300, handler, [overwrite: false])
    }

    def handler() {
        // need to handle multiple calls since overwrite:false specified
    }

.. note::

    It is important to note that you should not rely on the method being called in *exactly* the specified number of seconds. SmartThings will *attempt* to execute the method within a minute of the time specified, but cannot guarantee it. See the :ref:`limitations_best_practices` topic below for more information.


Run Once in the Future
----------------------

Some SmartApps may need to schedule certain actions to happen *once* at a specific time and date. *runOnce* handles this case.

.. note::

    You may notice that some of the scheduling APIs accept a string to represent the the date/time to be executed. This is a result of when you define a preference input of the "time" type, it uses a String representation of the value entered. When using this value later to set up a schedule, the APIs need to be able to handle this type of argument.

    When simply using the input from preferences, you don't need to know the details of the specific date format being used. But, if you wish to use the APIs with string inputs directly, you will need to understand their expected format.

    SmartThings uses the Java standard format of "yyyy-MM-dd'T'HH:mm:ss.SSSZ". More technical readers may recognize this format as ISO-8601 (Java does not fully conform to this format, but it is very similar). Full discussion of this format is beyond the scope of this documentation, but a few examples may help:

    January 09, 2015 3:50:32 GMT-6 (Central Standard Time): "2015-01-09T15:50:32.000-0600"

    February 09, 2015 3:50:32:254 GMT-6 (Central Standard Time): "2015-02-09T15:50:32.254-0600"

    For more information about date formatting, you can review the `SimpleDateFormat JavaDoc <http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html>`__.


**runOnce(dateTime, handlerMethod, options)**

Executes the handlerMethod once at the specified date and time. The dateTime argument can be either a Date object or a date string.

*options*
    Optional. Map of options. Valid options:

    [overwrite: ``true`` or ``false``]

    Specify [overwrite: false] if you do not want the most recently created job for the handlerMethod to overwrite an existing job. See the discussion in the runIn documentation above for more information.

.. code-block:: groovy


    def someEventHandler(evt) {
        // execute handler tomorrow, at the current time
        runOnce(new Date() + 1, handler)
    }

    def handler() {
        switch.off()
    }


.. code-block:: groovy


    def someEventHandler(evt) {
        // execute handler at 4 PM CST on October 21, 2015 (e.g., Back to the Future 2 Day!)
        runOnce("2015-10-21T16:00:00.000-0600", handler)
    }

    def handler() {
        // do something awesome, like ride a hovercraft
    }


Run on a Schedule
-----------------

Oftentimes, there is a need to schedule a job to run on a specific schedule. For example, maybe you want to turn the lights off at 11 PM every night. SmartThings provides the *schedule* method to allow you to create recurring schedules.

The various *schedule* methods follow a similar form - they take an argument representing the desired schedule, and the method to be called on this schedule.
Each SmartApp or device-type handler can only have one handler method scheduled at any time. This means that, unlike *runIn* or *runOnce*, a job created with *schedule* must either execute or be canceled with the *unschedule* method before you can schedule another job with the same method. The *schedule* method does not accept the overwrite option like *runOnce* and *runIn*.

**schedule(dateTime, handlerMethod)**

Creates a scheduled job that calls the handlerMethod every day at the time specified by the dateTime argument. The dateTime argument can be a String, Date, or number (to schedule based on Unix epoch time).

Only the time information will be used to derive the recurring schedule.

Here's how you might use a preference to set up a daily scheduled job:

.. code-block:: groovy


    preferences {
        section("Time to run") {
            input "time1", "time"
        }
    }

    ...

    def someEventHandler(evt) {
        schedule(time1, handlerMethod)
    }

    def handlerMethod() {
        ...
    }

Of course, you can create and pass the dateTime string explicitly:

.. code-block:: groovy


    def someEventHandler(evt) {
        // call handlerMethod every day at 3:36 PM CST
        schedule("2015-01-09T15:36:00.000-0600", handlerMethod)
    }

    def handlerMethod() {
        ...
    }

You can also pass a Groovy Date object:

.. code-block:: groovy


    def someEventHandler(evt) {
        // call handlerMethod every day at the current time
        schedule(new Date(), handlerMethod)
    }

    def handlerMethod() {
        ...
    }

Finally, you can pass a Long representing the desired time in milliseconds (using `Unix time <http://en.wikipedia.org/wiki/Unix_time>`__) to schedule:

.. code-block:: groovy


    def someEventHandler(evt) {
        // call handlerMethod every day, at two minutes from the current time
        schedule(now() + 120000, handlerMethod)
    }

    def handlerMethod() {
        ...
    }

----

Scheduling jobs to execute at a particular time is useful, but what if we want to execute a job at some other interval? What if, for example, we want a method to execute at fifteen minutes past the hour, every hour?

SmartThings allows you to pass a cron expression to the schedule method to accomplish this. A cron expression is based on the cron UNIX tool, and is a way to specify a recurring schedule. They are extremely powerful, but can be pretty confusing.

**schedule(cronExpression, handlerMethod)**

Creates a scheduled job that calls the handlerMethod according to the specified cronExpression.

.. note::

    Full documentation of the cron expression format can be found in the `Quartz Cron Trigger Tutorial <http://quartz-scheduler.org/documentation/quartz-1.x/tutorials/crontrigger>`__.

.. code-block:: groovy


    def someEventHandler(evt) {
        // execute handlerMethod every hour on the half hour.
        schedule("0 30 * * * ?", handlerMethod)
    }

    def handlerMethod() {
        ...
    }


Scheduled jobs are limited to running no more often than once per minute.

In addition to creating schedules using cron expressions, SmartThings also provides some convenience methods to set up the schedule for you. They are in the form of *runEveryXMinutes(handlerMethod)*  or *runEveryXHours(handlerMethod)*.

These methods work by creating a random start time in the X minutes or hours, and then every X minutes or hours after that. For example, ``runEvery5Minutes(handlerMethod)`` will execute ``handlerMethod`` at a random time in the next five minutes, and then run every five minutes from then.

These methods have the advantage of randomizing the start time for schedules, which can reduce the load on the SmartThings cloud. As such, they should be preferred over cron expressions when available.

The currently available methods are:

**runEvery5Minutes(handlerMethod)**

**runEvery10Minutes(handlerMethod)**

**runEvery15Minutes(handlerMethod)**

**runEvery30Minutes(handlerMethod)**

**runEvery1Hour(handlerMethod)**

**runEvery3Hours(handlerMethod)**

Other Scheduling-related Methods
--------------------------------

**canSchedule()**

returns ``true`` if a job can be scheduled, ``false`` otherwise. Only four jobs may be scheduled for the future at any time.

.. code-block:: groovy


    def someEventHandler(evt) {
        runIn(300, someHandlerMethod1)
        runIn(300, someHandlerMethod2)
        runIn(300, someHandlerMethod3)
        runIn(300, someHandlerMethod4)

        // false, since we already have four jobs scheduled
        canSchedule()
    }

----

**unschedule(nameOfMethod = '')**

Removes the method from the schedule queue, if specified. Note that this is not specific to jobs created with any of the ``schedule`` methods - any job scheduled for the future (using ``runIn``, ``runOnce``, or any of other scheduling methods) may be canceled using ``unschedule``.

.. note::

    Due to the way that the scheduling service is currently implemented, ``unschedule`` is a fairly expensive operation, and may take many seconds to execute.

    We plan to address this in the future, but until then, you should be aware of the potential performance impacts.

.. code-block:: groovy

    // unschedule the someHandlerMethod
    unschedule("someHandlerMethod")

unschedule can also be called with no arguments to unschedule all jobs.

.. code-block:: groovy


    // unschedule all jobs
    unschedule()

.. _limitations_best_practices:

Scheduling Limitations, Best Practices, and Things Good to Know
---------------------------------------------------------------

When using any of the scheduling APIs, it's important to understand some limitations and best practices. These limitations are due in part to the fact that execution occurs in the cloud, and are thus subject to limiting factors like load, network connectivity, etc.

----

**Do not expect exact execution time in scheduled jobs**

SmartThings will *try* to execute your scheduled job at the specified time, but cannot guarantee it will execute at that exact moment. As a general rule of thumb, you should expect that your job will be called within the minute of scheduled execution. For example, if you schedule a job at 5:30:20 (20 seconds past 5:30) to execute in five minutes, we expect it to be executed at some point in the 5:35 minute.

When using ``runIn`` with values less than one minute, your mileage will vary.

----

**Do not use runIn to set up a recurring schedule of less than sixty seconds**

You may have noticed that none of the schedule APIs allow you to schedule jobs for less than sixty second intervals. You may be tempted to work around this limitation by using runIn to create such a schedule (i.e., a handler method that reschedules itself). This is discouraged, and at some point may be prevented by the SmartThings framework.

The primary reason for discouraging jobs that run more often than every sixty seconds is overall system resource utilization. Using runIn to circumvent this is problematic because any failure to execute, even once, will cause the scheduled event to stop triggering.

----

**Only four jobs may be scheduled at any time**

To prevent any one SmartApp or device-type handler from using too many resources, only four jobs may be scheduled for future execution at any time.

----

**Do not excessively schedule/poll**

While there are some limitations in place to prevent excessive scheduling, it's important to note that excessive polling or scheduling is discouraged. It is one of the items we look for when reviewing community-developed SmartApps or device-type handlers.

----

**Missed job executions will not accumulate**

Due to a variety of issues (perhaps the local Internet connection has been dropped, or there is heavy load on the SmartThings server, or some other extreme circumstance), it's possible that a scheduled job could be missed. For example, say you have set up a job to execute every minute, and for some reason, it doesn't execute for three minutes.

When the job does execute again, it will resume its schedule (once every minute) - your handler won't suddenly be called three times, for example.

----

**unschedule can take several seconds to execute**

As discussed above, ``unschedule`` is currently a potentially expensive operation.

We plan to address this in the near future. Until we do, be aware of the potential performance impacts of calling ``unschedule``.

Examples
--------

These SmartApps can be viewed in the IDE using the "Browse Templates" button:

- "Once a Day" uses ``schedule`` to turn switches on and off every day at a specified time.
- "Turn It On For 5 Minutes" uses ``runIn`` to to turn a switch off after five minutes.
- "Left It Open" uses ``runIn`` to see if a door has been left open for a specified number of minutes.
- "Medicine Reminder" uses ``schedule`` to check if a medicine door has been opened at a certain time.







