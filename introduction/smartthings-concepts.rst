Important Concepts
==================

Asynchronous & Eventually Consistent Programming
------------------------------------------------

When dealing with the physical graph there will always be a delay between when you request something to happen and when it actually happens. There is latency in all networks, but it's especially pronounced when dealing with the physical graph.

To deal with this, the SmartApps platform utilizes asynchronous execution. This means that anytime you execute a command, it doesn't stop everything else from running. This helps everyone's code run the most efficiently.

Our basic methodology towards executing a command, such as turning a light switch on, is "fire and forget". This means that you execute a command, and assume it will turn on in due time, without any sort of follow up.

You cannot be guaranteed that your command has been executed, because another SmartApp could interact with your end device, and change its state. For example, you might turn a light switch on, but another app might sneak in and turn it off.

If you needed to know if a command was executed, you can subscribe to an event triggered by the command you executed and check its timestamp to ensure it fired after you told it to. You will, however, still have latency issues to take into consideration, so it's impossible to know the exact current status at any given time.

The SmartApps platform follows eventually consistent programming, meaning that responses to a request for a value in SmartApps will eventually be the same, but in the short term they might differ.

.. note::

    In the future, we'd like to move towards providing levels of consistency for the end user, so you could specify how consistent you need your data to be.

    Also, as we move some of our logic into the hub, we may consider allowing blocking methods (synchronous) as they wouldn't weigh down our network as a whole.

----

Containers
----------

Within the SmartThings platform, there are three different “containers”
that are important concepts to understand. These are: accounts,
locations, and groups. These containers represent both security
boundaries and navigation containers that make it easy for users to
browse their devices.

The diagram below shows the hierarchical relationship between these
containers. Each type of container is described below in more detail.

.. figure:: ../img/overview/container-hierarchy.png
   :alt: Container Hierarchy

Accounts
~~~~~~~~

Accounts are the top-level container that represents the SmartThings
‘customer’. Accounts contain only Locations and no other types of
objects.

Locations & Users
~~~~~~~~~~~~~~~~~

Locations are meant to represent a geo-location such as “Home” or
“Office”. Locations can optionally be tagged with a geo-location
(lat/long). In addition, Locations don’t have to have a SmartThings Hub,
but generally do. Finally, locations contain Groups or Devices.

Groups
~~~~~~

Groups are meant to represent a room or other physical space within a
location. This allows for devices to be organized into groups making
navigation and security easier. A group can contain multiple devices,
but devices can only be in a single group. Further, nesting of groups is
not currently supported.

----

Capability Taxonomy
-------------------

Capabilities represent the common taxonomy that allows us to link SmartApps with Device Handlers. An application interacts with devices based on their
capabilities, so once we understand the capabilities that are needed by
a SmartApp, and the capabilities that are provided by a device, we can
understand which devices (based on their device type and inherent
capabilities) are eligible for use within a specific SmartApp.

The :ref:`capabilities_taxonomy` is
evolving and is heavily influenced by existing standards like ZigBee
and Z-Wave.

Capabilities themselves may decompose into both ‘Actions’ or ‘Commands’ (these are synonymous), and Attributes. Actions represent ways in which you can
control or actuate the device, whereas Attributes represent state
information or properties of the device.

Attributes & Events
~~~~~~~~~~~~~~~~~~~

Attributes represent the various properties or characteristics of a
device. Generally speaking device attributes represent a current device
state of some kind. For a temperature sensor, for example, ‘temperature’
might be an attribute. For a door lock, an attribute such as ‘status’
with values of ‘open’ or ‘closed’ might be a typical.

Commands
~~~~~~~~

Commands are ways in which you can control the device. A capability is
supported by a specific set of commands. For example, the ‘Switch’
capability has two required commands: ‘On’ and ‘Off’. When a device
supports a specific capability, it must generally support all of the
commands required of that capability.

Custom Capabilities
~~~~~~~~~~~~~~~~~~~

We do not currently support creating custom capabilities. You can, however,
create a device-type handler that exposes custom commands or attributes.