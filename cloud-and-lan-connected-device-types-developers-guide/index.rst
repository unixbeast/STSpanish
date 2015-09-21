.. _cloud_lan_device_type_guide:

Cloud and LAN-Connected Devices
===============================

Cloud and LAN connected devices are devices that use either a 3rd party service, like the Ecobee thermostat, or
communicate over the LAN (local area network) like the Sonos system. These devices require a unique implementation of
their device handlers. Cloud and LAN connected devices use a service manager SmartApp along with a device handler
for authentication, maintaining connections, and device communications. This guide will walk you through
service manager and device handler creation for both of these scenarios.

Table of Contents:

.. toctree::
   :maxdepth: 3

   understanding-the-service-manage-device-handler-design-pattern
   building-cloud-connected-device-types/index
   building-lan-connected-device-types/index
