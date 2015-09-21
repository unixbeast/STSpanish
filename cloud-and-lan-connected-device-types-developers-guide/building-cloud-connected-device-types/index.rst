Building Cloud-Connected Device Types
=====================================

Cloud connected devices use a 3rd party service to accomplish device communication. An example of such a device is the
Ecobee thermostat.

When developing a device handler for a cloud connected device, you must create a service manager
SmartApp that will handle authenticating with the 3rd party service, communicating with the device, and reacting to any
device changes that occur.

This guide overviews the concept of the service manager/device handler architecture and
also gives an example of both the service manager and device handler creation.

Table of Contents:

.. toctree::
   :maxdepth: 2

   division-of-labor
   building-the-service-manager
   building-the-device-type
