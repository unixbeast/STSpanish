.. _developing_with_smartthings:

Developing with SmartThings
===========================

Developers can create their own SmartApps to create new automations for their homes, or new Device Handlers to integrate a new device.

Who can Develop with SmartThings?
---------------------------------

Developing for SmartThings is free and open to all.

Our typical guideline is that anyone with a web developer (back-end) background will be best-equipped to develop with SmartThings.

That said, if you have programmed in any programming language before, you'll be able to learn how to develop with SmartThings.

Never programmed before? You may want to spend some time learning some programming basics first, but you certainly don't need a degree in Computer Science to be successful developing with SmartThings.

SmartThings uses Groovy as its development programming language. You can learn more about Groovy, and its use in SmartThings, in the :ref:`groovy` chapter.

----

Create Device Type Handlers
---------------------------

If you have a new device that we don't already support, you can actually write a new Device Handler for the SmartThings platform that integrates the device.

Building support for new device types means that you also get to design the device detail screens for how the device will appear in our mobile experience.

For information about developing for hub-connected devices, see the :ref:`device_type_dev_guide`.

For information about developing for Cloud or LAN-Connected devices, see the :ref:`cloud_lan_device_type_guide`.

----

Create Event-Handler SmartApps
------------------------------

You can write SmartApps that provide unique functionality across devices. 

These are SmartApps that don't have a UI except for configuring their behavior. They generally run in the background and handle events from devices and then issue commands back to other devices to control them.

For information about developing Event-Handler SmartApps, see the :ref:`smartapp_dev_guide`.

----

Create Integration SmartApps
----------------------------

SmartApps can call external web services and they can expose web services for external systems to call. 

Custom SmartApp APIs
~~~~~~~~~~~~~~~~~~~~

Within SmartApps, you can expose API endpoints that allow you (and other
users) to interact with your SmartApp through OAuth. Through these
endpoints, you can initiate methods within your SmartApp that can
interact with your devices, all from the web.

You can learn more about building SmartApps that expose endpoints in the :ref:`smartapp_web_services_guide`.

Calling Outbound Web Services
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There a variety of helper methods you can use within a SmartApp to
integrate with a third party API. 

For APIs that require authentication, this involves initiating an OAuth connection and setting up endpoints to handle authentication responses. From there, you can make any type of request (POST,GET,PUT, etc) that you need and parse through the response.

For information on calling external web services, see the :ref:`calling_web_services` chapter of the :ref:`smartapp_dev_guide`.
