Submitting SmartApps for Publication
====================================

To submit your SmartApps for consideration for publication to the SmartThings Platform, you 
can create a Publication Request by clicking on the `My Publication Requests <https://graph.api.smartthings.com/ide/submissions>`__ 
tab in the `SmartThings IDE <http://ide.smartthings.com>`__, then clicking on the *New Request* 
button in the upper-right-hand corner of the screen.


Review Process
--------------

Once submitted, your SmartApp will undergo a review. Here is a detailed outline of the approval process:


Functional Review
~~~~~~~~~~~~~~~~~

- Does this SmartApp duplicate an existing SmartApp? If so, does it improve the current SmartApp?
- Does it have a good title, description, and configuration preferences? Will the user understand how it works?
- Does the SmartApp work as expected?


Code Review
~~~~~~~~~~~

- In the ``preferences`` section, are all inputs used?
- Naming conventions: do variable and function names appropriately describe their use?
- All ``subscribe()`` calls have corresponding ``unsubscribe()`` method.
- All ``schedule()`` calls have corresponding ``unschedule()`` as needed.
- Scheduling: check for any ``runIn()``, ``schedule()``, ``runDaily()``, etc and make sure they are not running too often.
- When communicating with web services, follow best practices below.
- Avoid redundant code and infinite loops.
- Comment your code!


Publication
~~~~~~~~~~~

Once your app has been approved, it will be published in our mobile app.


Best Practices
--------------

For the greatest likelihood of success, follow these guidelines:

- For greatest communication between yourself and the SmartThings reviewer, add your Github username as the namespace.
- Do not use offensive, profane, or libelous language.
- No advertising or sponsorships.

Web Services
~~~~~~~~~~~~

- If your SmartApp sends any data from the SmartThings Platform to an external service, include in the description exactly what data is sent to the remote service, how that data will be used, and include a link to the privacy policy of the remote service
- If your SmartApp exposes any Web Service APIs, describe what the APIs will be used for, what data may be accessed by those APIs, and where possible, include a link to the privacy policies of any remote services that may access those APIs.

Devices and Preferences
~~~~~~~~~~~~~~~~~~~~~~~

- Label your SmartApp preferences to match the devices that you are asking for access to.
- Use the *password* input type whenever you are asking a user for a password.

Scheduling
~~~~~~~~~~

- Do not aggressively loop or schedule.
- If using sunrise and sunset, make sure you are following the guidelines in the :doc:`sunset-and-sunrise` guide.

Style
~~~~~

- Use meaningful variable names.
- Maintain consistent formatting and indentation. We can't review code that we can't easily read. 
