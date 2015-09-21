Architecture
============

Our architecture is designed in a way that abstracts away the details of a specific device (ZigBee, Z-Wave, Wifi/IP/UPnP, etc) and allows the developer to focus solely on the capabilities and actions supported by the device (lock/unlock, on/off, etc).

The conceptual architecture looks like this:

.. figure:: ../img/overview/conceptual-architecture.png
   :alt: Conceptual Architecture

Overview
--------

We made the decision at SmartThings to support a “Cloud First” approach
for our platform. This means that in our initial release, there is a
dependency on the Cloud. This means that your hub will need to be online and connected to the SmartThings cloud.

The second generation of our hub, the Samsung SmartThings Hub, allows for some hub-local capabilities. Certain automations can execute even when disconnected from the SmartThings cloud.  This allows us to improve performance and insulate the customer from intermittent internet outages.

This is accomplished by delivering certain automations to the Samsung SmartThings Hub itself, where it can execute locally. The engine that executes these automations are typically referred to as "App Engine". Events will still be sent to the SmartThings cloud - this is necessary to ensure that the mobile application reflects the current state of the home, as well as to send any notifications or perform other cloud-based services.

The specific automations that execute locally is managed by the SmartThings internal team.

That said, there are a number of important scenarios where the Cloud is
simply required and where we can’t reduce or eliminate dependence on the
Cloud:

**There may not be a hub at all**

Many devices are now connected devices, via WiFi/IP.

The advantage of Wifi devices is that they can eliminate the need for a gateway
device (hub) and connect directly to the cloud.
When you consider the breadth of devices like this that are coming onto the market, it’s easy to imagine that there will be customers who want to be able to add intelligence to those devices through SmartApps, but that may not have a SmartThings Hub at all because all of their devices are directly connected to the vendor cloud or the SmartThings Cloud.

Put simply, if there is no Hub, then the SmartApps layer must run in the cloud!

**SmartApps May Run Across both Cloud and Hub Connected Devices**

As a corollary to the first point above, since there are cases where
devices are not hub-connected, SmartApps might be installed to use
one device that is hub-connected, and another device that is
cloud-connected, all in the same app. In this case, the SmartApp
needs to run in the Cloud.

**There may be Multiple Hubs**

While the mesh network standards for ZigBee and Z-Wave generally eliminate the need for multiple SmartThings Hubs, we didn’t want to exclude this as a valid
deployment configuration for large homes or even business
applications of our technology.
In the multi-hub case, SmartApps that use multiple devices that are split across hubs will run in the Cloud in order to simplify the complexity of application deployment.

**External Service Integration**

SmartApps may call external web services and calling them from our Cloud reduces risk because it allows us to easily monitor for errors and ensure the security and privacy of our customers.

In some cases, the external web services might even use IP white-listing such that they simply can’t be called from the Hub running at a user’s home or place of business.

Accordingly, SmartApps that use web services will run in the Cloud as well.

.. important::

   Keep in mind that because of the abstraction layer, SmartApp developers never have to understand where or how devices connect to the SmartThings platform. All of that is hidden from the developer so that whether a device (such as a Garage Door opener) is Hub-Connected or Cloud-Connected, all they need to understand is:

   .. code-block:: groovy

       myGarageDoor.open()

Benefits
--------

There are a number of important benefits to the overall SmartThings approach:

**Bringing Supercomputing Power to SmartApps and the Physical World**

No matter how much computing power we put into the SmartThings Hub, there are scenarios where it simply wouldn’t be enough.

Take for example the ability to apply advanced facial recognition algorithms to a photo taken by a connected camera to automatically determine who  just walked into your house while you were away. In the Cloud, we can bring all necessary computing power to solve complex problems, that we would not have if limited to the local processing power in a hub.

**The Value of the Network Effect**

Our vision is to make your physical world smarter, and we are doing that not just for our Hub and Devices, but for lots of different devices and scenarios.
The easier that we make it to create that intelligence (through SmartApps), the bigger that ecosystem of developers and makers will be.

For consumers, that will mean the power of choice and the ability to solve problems with a solution that best fits their needs.

As a developer or maker, it means broad access to consumers and distribution channels for your product.

**Increased Ease of Use, Accessibility, Reliability & Availability**

By centralizing many capabilities into the SmartThings Cloud, we increase our ability to monitor, manage, and respond to any failures or other issues. More importantly, we can simplify the customer experience and make our solution easier to use than ever before. Further, we ensure that customers have an increased level of access and visibility.

This is not a new trend - there are many examples where on-premise capabilities have migrated to the service provider, because it improved the overall service reliability and customer experience.

----

Big Picture
-----------

.. TODO: I think we need a nicer looking picture. (Jesse O'Neill-Oine)
|Container Hierarchy|

Devices
~~~~~~~

Devices are the building blocks of the SmartThings infrastructure. They
are the connection between the SmartThings system and the physical
world. There's a huge variety in the devices you can use, some created
by SmartThings but most are not.

SmartThings Devices
+++++++++++++++++++

SmartThings manufactures a variety of devices for you to use with your
SmartThings hub. Your initial kit comes with a few devices such as the
`SmartSense
Multi <https://shop.smartthings.com/#/products/smartsense-multi>`__
which reports motion, temperature, and a variety of other sensory
updates. SmartThings also manufactures and sells the `SmartSense Motion
Sensor <https://shop.smartthings.com/#/products/smartsense-motion>`__,
`SmartSense Presence
Sensor <https://shop.smartthings.com/#/products/smartsense-presence>`__,
`SmartSense Moisture
Sensor <https://shop.smartthings.com/#/products/smartsense-moisture>`__,
and `SmartPower
Outlets <https://shop.smartthings.com/#/products/smartpower-outlets-3-pack>`__.

Third Party Devices
+++++++++++++++++++

The real power of SmartThings is that our system works with most home
automation devices already on the market. We believe in a fully
integrated approach, where you aren't tied into a particular technology
or protocol. We offer compatibility with standards such as ZigBee,
Z-Wave, and IP/WiFi, so we work with literally hundreds of off the shelf
third-party devices. There is `an outlet made by
GE <https://shop.smartthings.com/#/products/ge-z-wave-wireless-lighting-control-lamp-module-dimmer>`__
that allows you to integrate with your SmartThings system to dim your
lights. There are
`sirens <https://shop.smartthings.com/#/products/fortrezz-siren-strobe-alarm>`__
for notifying you of happenings in the SmartThings system. We even have
solutions for things like `locking your
doors <https://shop.smartthings.com/#/bundles/solution-i-can-lock-and-unlock-my-doors-from-anywhere>`__.

Hub
+++

The SmartThings Hub connects directly to your broadband router and
provides communication between all connected Things and the SmartThings
cloud and mobile application.

-  Connects any SmartThings or SmartThings Ready device to your
   SmartThings account.
-  Simply plug into your Ethernet router and provide power.
-  Build your own SmartThings kit by combining with other SmartThings
   devices.
-  Also works with standard ZigBee and Z-Wave devices, such as GE Z-Wave
   in-wall switches and outlets.

The new Samsung SmartThings Hub (in addition to supporting local execution of automations as discussed above), also comes with four AA batteries. This allows for certain automations to continue, even without power. It also ships with USB ports and is Bluetooth Low Energy capable. While not active at launch, this allows for greater expansion in the future without requiring new hardware.

Connectivity Management
+++++++++++++++++++++++

Connectivity Management is the layer that connects your SmartThings hub
and client devices (mobile phones) to our servers, and the cloud as a
whole. We have two parts of this layer currently:

-  Hub Connectivity connects your hub to the cloud.
-  Client Connectivity connects your client devices to the cloud.

These are the highways by which your messages are sent to the internet.

Device-Type Execution
+++++++++++++++++++++

The SmartThings system determines what device type you are using based
on device type handlers. Once the device type handler is selected, the
incoming messages are parsed by that particular device type. The input
of the device type handler are device specific messages, and the output
is normalized SmartThings events. Note that one message can lead to many
SmartThings events.

Subscription Management
+++++++++++++++++++++++

When events are created in the SmartThings platform, they don't
inherently do anything besides publish that they've happened. Instead of
events triggering change, SmartApps are configured with subscriptions
that listen for defined events. The purpose of the subscription
management layer is to match up events that are triggered by the device
type handlers with which SmartApp is using them.

SmartApp Execution
++++++++++++++++++

The SmartApp is run when trigged via subscriptions, external calls to
SmartApp endpoints, or scheduled methods. It's transient in nature, as
it runs and then stops running on completion of its task. Any data that
needs to persist throughout SmartApp instances must be stored in a special ``state`` variable that is discussed in the :ref:`storing-data` documentation.

Web-UI & IDE
++++++++++++

The Web-UI sits on top of all of the other technology and allows you to
monitor your devices, hubs, locations and many other aspects of your
SmartThings system.

You have full control of the configuration, including editing, adding,
removing, and even creating SmartApps. To create, you can write code
within the IDE for SmartApps and Device Types. We also have an
integrated simulator that allows you to simulate any devices, so it's
not required to own the devices you develop for.

.. |Container Hierarchy| image:: ../img/architecture/overview.png
