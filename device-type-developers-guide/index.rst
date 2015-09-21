.. _device_type_dev_guide:

Device Handlers
===============

Device Handlers (sometimes also referred to as Device Type Handlers, or SmartDevices), are the virtual representation of a physical device.

If you are new to writing device handlers, start with the :doc:`quick-start`.

After that, read the :doc:`overview` for a broad discussion about device handlers and where they fit in the SmartThings architecture.

The rest of the guide discusses the various components of Device Handlers primarily targeted for hub-connected (ZigBee or Z-Wave) devices (though the common Device Handler principles and patterns apply to other devices as well).

.. note::

   This guide discusses hub-connected device handlers. For information about LAN and cloud-connected device handlers, see `this guide <../cloud-and-lan-connected-device-types-developers-guide/index.html>`__


Table of Contents:

.. toctree::
   :maxdepth: 3

   quick-start
   overview
   simulator-metadata
   definition-metadata
   tiles-metadata
   device-preferences
   parse
   z-wave-primer
   building-z-wave-device-handlers
   z-wave-example
   zigbee-primer
   building-zigbee-device-handlers
   zigbee-example
   submitting-device-types-for-publication
