What is SmartThings?
====================

SmartThings is the platform for what we call the “Open Physical Graph”.
The Physical Graph is the virtual, online representation of the physical
world. We believe that virtual representation should be open and
easily accessible to consumers, developers, and device
makers/manufacturers.

When you interact with the physical graph, it automatically reflects
that interaction in the physical world. And when you interact with
connected devices in the physical world, it automatically reflects that
interaction in the physical graph.

This is what will make the physical world programmable - and when we say
programmable, we don’t mean by firmware developers with highly
specialized skill sets. We mean programmable by anyone with a typical
web-developer skill set.

In order to make this vision a reality, we realized early on that we
needed to provide an end-to-end solution of hardware, software, a
fantastic user experience, and incredible user support.

Each person's usage of the SmartThings platform will be as diverse and varied as are their lives.
And to support that diversity, we need a diverse catalog of applications
and devices. That is where you, as a developer, can help!

----

What We Believe
---------------

As a starting point in understanding our approach, it is important to
recognize that we believe strongly in the separation of *intelligence*
from devices. Said another way, we think that most of the value will be
created in the *space between the devices*. We believe that the time has
come when devices themselves can be limited to their primitive
capabilities (open/close, on/off, heat/cool, brew/don’t brew), and that
the intelligence layer should be kept separate.

By doing this we allow the intelligence (or application) layer to apply
flexibly across a wide range of devices, and make it easier to create
applications that interact with and across the physical world. In many
cases, we also benefit from lower-cost end devices, less maintenance
complexity and longer battery life.

----

Key Concepts
------------

The SmartThings platform provides methods to abstract away the underlying complexity of devices and protocols (using Device Type Handlers) from the application of intelligence (using SmartApps).

Each device in SmartThings has “capabilities”, which define and standardize available attributes and commands for a device. This allows you to develop an application for a device type, regardless of the connection protocol or the manufacturer.

All of the code that developers can write on our platform is written in Groovy, which is a dynamic, object-oriented language built for the Java platform. You can learn more about Groovy on the :ref:`groovy` page.

SmartApps
~~~~~~~~~

SmartApps let users connect devices, actions, and external services to create automations.

*“Turn off the lights when I arrive”*

*“When there’s motion, alert me via SMS”*

*“When I’m not home, and a window is opened, sound an alarm”*

As a SmartApp developer, you define the types of devices you require based on their capabilities, then write logic based on actions, schedules, and events.

A quick example of just how easy it is to write a SmartApp that turns a light on when a door opens:

.. code-block:: groovy

   preferences {
      input "theswitch", "capability.switch"
      input "thedoor", "capability.contactSensor"
   }

   def installed() {
      subscribe(thedoor, "contact.open", doorOpenHandler)
   }

   def doorOpenHandler(event) {
      theswitch.on()
   }

You can learn more about SmartApps in the :ref:`smartapp_dev_guide`.

Device Type Handlers
~~~~~~~~~~~~~~~~~~~~

Device Type Handlers parse raw messages from devices to create standardized capabilities for developers to use.

A SmartApp developer who just wants to turn on a light doesn’t, and in fact can’t, know whether that light is a Z-Wave device, a ZigBee device, an IP device, etc. Device Type Handlers parse raw messages from devices to create standardized capabilities for SmartApps to use.

They also define how the devices are visually represented in our mobile apps & IDE/simulator.

Broadly speaking, there are two different types of device type handlers:

Hub-connected Device Handlers
+++++++++++++++++++++++++++++

These abstract devices that connect directly to the hub, via ZigBee or Z-Wave. You can learn more about these in the :ref:`device_type_dev_guide`.

Cloud or LAN-Connected Device Handlers
++++++++++++++++++++++++++++++++++++++

These abstract devices that connect to SmartThings via the Local Area Network (LAN) or through the cloud services of the device manufacturer (e.g., a Sonos player). You can read more about these in the :ref:`cloud_lan_device_type_guide`.

----

Supported Protocols
-------------------

The following protocols are supported in the SmartThings Hub:

- ZigBee - A Personal Area Mesh Networking standard for connecting and controlling devices. ZigBee is an open standard supported by the ZigBee Alliance. For more information on ZigBee see `http://en.wikipedia.org/wiki/ZigBee <http://en.wikipedia.org/wiki/ZigBee>`__.

- Z-Wave - A proprietary wireless protocol for Home Automation and Lighting Control. For more information on Z-Wave see `http://en.wikipedia.org/wiki/Z-Wave <http://en.wikipedia.org/wiki/Z-Wave>`__.

- IP-Connected Devices - Local Area Network (LAN) connected devices (both hard-wired and WiFi) within the home can be connected to the SmartThings Hub.

-  Cloud-Connected Devices - Some device manufacturers have their own Cloud solutions that support their devices and that we can connect to. Most of these devices are actually WiFi connected devices, but they connect to a proprietary set of Cloud services, and therefore we have to go through those services to gain access to the device.

The Samsung SmartThings Hub also ships with a Bluetooth Low Energy (BLE) chip to support future expandability. While not enabled at launch, having the hardware available allows us to support BLE in the future via firmware and software updates.
