Preferences
===========

.. note::

    This documentation is incomplete. Until it is expanded, you are encouraged to look at other device-type handlers for example usage. The *SmartSense Multi* and *HomeSeer Multisensor*, available to browse via the "Device Type Templates" menu in the IDE, both use preferences.

Device type handlers can define preferences, similar to how SmartApps do. They are not the same, however.

When you add a device, in addition to the “name your device” field you could show other fields, and they’ll be editable by tapping the “preferences” tile in the device details. This is a fairly uncommon scenario, but would be handled by adding a preferences block to the metadata:

.. code-block:: groovy
    
    metadata {
        ...
        preferences {
           input "sampleInput", "number", title: "Sample Input Title", 
                  description: "This is the sample input.", defaultValue: 20, 
                  required: false, displayDuringSetup: true
        }
        ...
    }
    

Device preferences are limited to input elements, and do not support multiple pages or sections.


