ZigBee Primer
=============

Before we start, lets take a look at a full ZigBee message as it would
look in a SmartThings Device Type Handler. Then we’ll break up the
message into its parts and dive into what each part means. Make sure you
download the ZigBee Cluster Library as a reference for ZigBee message
formatting and what is possible for each device.

Here is a full command:
``"st cmd 0x${device.deviceNetworkId} ${endpointId} 8 4 {FFFF 0000}”``

The 3 Main types of ZigBee Messages

-  st cmd - SmartThings Command which formats the message as a ZigBee
   Command
-  st rattr - SmartThings Read Attribute which formats the message as a
   ZigBee Read Attribute
-  st wattr - SmartThings Write Attribtue which as you guessed formats
   the message as a ZigBee Write Attribute

Device Network ID
-----------------

All connected devices have a Device Network ID that is used to route
messages correctly to the device. In the loosest terms think of the
Network ID as the IP Address. It is a 4 digit hex number that the device
gets while pairing. Since the Network ID is different by device, you can
reference it dynamically in a Device Handler like this:
``0x${device.deviceNetworkId}``

Endpoints
---------

Endpoints are simple. Think of them basically as ports. Different
endpoints can support different clusters and a device can have multiple
endpoints to do different things. Endpoints can be used to separate
functionality when needed. For example a temperature sensor can have the
Temperature Measurement Cluster on endpoint 1 and have Over The Air Boot
loader Cluster on endpoint 2.

Clusters
--------

Clusters are a group of commands and attributes that define what a
device can do. Think of clusters as a group of actions by function. A
device can support multiple clusters to do a whole variety of tasks.
Majority of clusters are defined by the ZigBee Alliance and listed in
the ZigBee Cluster Library. There are also profile specific clusters that
are defined by their own ZigBee profile like Home Automation or ZigBee
Smart Energy, and Manufacture Specific clusters that are defined by the
manufacture of the device. These are typically used when no existing
cluster can be used for a device.

Most used clusters are

-  0x0006 - On/Off (Switch)
-  0x0008 - Level Control (Dimmer)
-  0x0201 - Thermostat
-  0x0202 - Fan Control
-  0x0402 - Temperature Measurement
-  0x0406 - Occupancy Sensing

Commands
--------

Commands are basically actions a device can take. It’s how we get things
to do stuff. We start a command with “st cmd”. Commands and whats
available are defined by the cluster.

Keeping on the On/Off cluster as an example, the available commands are:

-  0x00 - Off
-  0x01 - On
-  0x02 - Toggle

In a SmartThings Device Type the following line would turn a switch off
(look at the last number):
``"st cmd 0x${device.deviceNetworkId} ${endpointId} 6 0 {}”``

This would turn it on:
``"st cmd 0x${device.deviceNetworkId} ${endpointId} 6 1 {}”``

This would toggle it:
``"st cmd 0x${device.deviceNetworkId} ${endpointId} 6 2 {}"``

Read and Write Attributes
-------------------------

Attributes are used to get information from a device and to set
preferences on a device. The two main types are Read and Write. The data
type and values are specified by cluster.

An example of a Read Attribute that would read the current level of a
dimmer and return the value:

``"st rattr cmd 0x${device.deviceNetworkId} ${endpointId} 8 0 {}"``

Write Attributes are used to set specific preferences. Write attributes
can need specific data type that the payload is in. In this example the
0x21 in the message means Unsigned 16-bit integer.

An example of a Write Attribute that would set the transition time from
on to off of a dimmer look like this:

``"st wattr 0x${device.deviceNetworkId} 1 8 0x10 0x21 {0014}”``

In this case the payload ({0014}) translates to 2 seconds. Breaking the payload
down we see that the hex value of 0x0014 equals the decimal value of 20. 20 *
1/10 of a second equals 2 seconds.
