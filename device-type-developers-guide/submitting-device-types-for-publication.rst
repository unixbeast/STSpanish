Submitting Device Types for Publication
=======================================

To submit your Device Types for consideration for publication to the SmartThings Platform, you 
can create a Publication Request by clicking on the `My Publication Requests <https://graph.api.smartthings.com/ide/submissions>`__ 
tab in the `SmartThings IDE <http://ide.smartthings.com>`__, then clicking on the *New Request* 
button in the upper-right-hand corner of the screen.

Review Guidelines
-----------------

Once submitted, your Device Type will undergo a review and approval process.  For the greatest likelihood of success, follow these guidelines:

General
~~~~~~~

- Make sure your Device Type compiles and runs, which means it works in the IDE and mobile devices.
- Do not use offensive, profane, or libelous language.
- No advertising or sponsorships.
- Every class and nontrivial public method you write should contain a comment with at least one sentence describing what it does. This sentence should start with a 3rd person descriptive verb.
- Use the *password* input type whenever you are asking a user for a password.
- Split Device Type and SmartApp functionality into two different submissions.
- Do not aggressively loop or schedule.

Web Services
~~~~~~~~~~~~

- If your Device Type sends any data from the SmartThings Platform to an external service, include in the description exactly what data is sent to the remote service, how that data will be used, and include a link to the privacy policy of the remote service
- If your Device Type exposes any Web Service APIs, describe what the APIs will be used for, what data may be accessed by those APIs, and where possible, include a link to the privacy policies of any remote services that may access those APIs.

Style
~~~~~

- Use meaningful variable and method names.
- Maintain consistent formatting and indentation. We can't review code that we can't easily read.

Reasons for Rejection
~~~~~~~~~~~~~~~~~~~~~

- The device type adds minor addition or change that may be changed with a core product or UX change in a future update.
- SmartThings is already developing a first-party integration and will not accept a device type for this device. 
- The device type should actually be a SmartApp instead, because it's actuating or changing a device.
- Suggested change does not fit our philosophy.
- No discovery mechanism is provided. For LAN-Connected devices, a `Service Manager SmartApp <http://docs.smartthings.com/en/latest/cloud-and-lan-connected-device-types-developers-guide/understanding-the-service-manage-device-handler-design-pattern.html>`_ should serve to discover and create the device.
- Multiple community submissions exist and we’re rolling several improvements together, so this specific one is being rejected.


