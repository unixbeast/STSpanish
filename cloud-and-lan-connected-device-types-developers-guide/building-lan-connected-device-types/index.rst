Building LAN-Connected Device Types
===================================

LAN connected devices communicate with the SmartThings hub over the LAN. An example of such a device is the
Sonos system.

When developing a device handler for a LAN device, you must create a service manager
SmartApp that will handle discovery of devices on the LAN, in some cases communicate with the device, and react to any
device changes that occur via events.

This guide overviews the concept of the service manager/device handler architecture
and also gives an example of both the service manager and device handler creation.

Table of Contents:

.. toctree::
   :maxdepth: 2

   division-of-labor
   building-the-service-manager
   building-the-device-type
