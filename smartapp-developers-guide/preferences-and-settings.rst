.. _prefs_and_settings:

Preferences & Settings
======================

.. note::

    This topic discusses preferences and settings as it pertains to SmartApps. Information about device type preferences can be found in the `Device Type Developer's Guide <../device-type-developers-guide/index.html>`__


The preferences section of a SmartApp specifies what
kinds of devices and other information is needed in order for the
application to run. Inputs for each of these are presented to the user
during installation of the SmartApp from the mobile UI.  You can present all of these
inputs on a single page, or break them up into multiple pages.

As usual, the best way to become comfortable with something is through trying it yourself.
So, fire up the `web IDE <http://ide.smartthings.com>`__ and try things out!

Preferences Overview
--------------------

Preferences are made up of one or more pages, which contain one or more sections, which in turn contain
one more elements. The general form of creating preferences looks like:

.. code-block:: groovy

    preferences {
        page() {
            section() {
                paragraph "some text"
                input "motionSensors", "capability.motionSensor",
                    title: "Motions sensors?", multiple: true
            }
            section() {
                ...
            }
        }
        page() {
            ...
        }
    }

All inputs from the user are stored in a read-only map called ``settings``. You can access the value entered by the user by indexing into the map using the name as the key (``settings.someName``)

Page Definition
---------------

Pages can be defined a couple different ways:

*page(String pageName, String pageTitle) {}*

.. code-block:: groovy

    preferences {
        // page with name and title
        page("page name", "page title") {
            // sections go here
        }
    }

*page(options) {}*

This form takes a comma-separated list of name-value arguments.

.. note::

    this is a common Groovy pattern that allows for named arguments to be passed to a method. More info can be found `here <http://groovy.codehaus.org/Extended+Guide+to+Method+Signatures>`__.

.. code-block:: groovy

    preferences {
        page(name: "pageName", title: "page title",
             nextPage: "nameOfSomeOtherPage", uninstall: true) {
            // sections go here
        }
    }


The valid options are:

*name* (required)
    String - Identifier for this page.
*title*
    String - The display title of this page
*nextPage*
    String - Used on multi-page preferences only. Should be the name of the page to navigate to next.
*install*
    Boolean - Set to ``true`` to allow the user to install this app from this page. Defaults to ``false``. Not necessary for single-page preferences.
*uninstall*
    Boolean - Set to ``true`` to allow the user to uninstall from this page. Defaults to false. Not necessary for single-page preferences.


We will see more in-depth examples of pages in the following sections.

Section Definition
------------------

Pages can have one or more sections. Think of sections as way to group the input you want to gather from the user.

Sections can be created in a few different ways:

*section{}*

.. code-block:: groovy

    preferences {
        // section with no title
        section {
            // elements go here
        }
    }


*section(String sectionTitle){}*

.. code-block:: groovy

    preferences {
        // section with title
        section("section title") {
            // elements go here
        }
    }


*section(options, String sectionTitle) {}*

.. code-block:: groovy

    preferences {
        // section will not display in IDE
        section(mobileOnly: true, "section title")
    }

The valid options are:

*hideable*
    Boolean - Pass ``true`` to allow the section to be collapsed. Defaults to ``false``.
*hidden*
    Boolean - Pass ``true`` to specify the section is collapsed by default. Used in conjunction with ``hideable``. Defaults to ``false``.
*mobileOnly*
    Boolean - Pass ``true`` to suppress this section from the IDE simulator. Defaults to ``false``.


Single Preferences Page
-----------------------

A single page preferences declaration is composed of one or more *section* elements, which in turn contain one or more
*elements*. Note that there is no *page* defined in the example below. When creating a single-page preferences app, there's no need to define the page explicitly - it's implied. Here's an example:

.. code-block:: groovy

    preferences {
        section("When activity on any of these sensors") {

            input "contactSensors", "capability.contactSensor",
                title: "Open/close sensors", multiple: true

            input "motionSensors", "capability.motionSensor",
                title: "Motion sensors?", multiple: true
        }
        section("Turn on these lights") {
            input "switches", "capability.switch", multiple: true
        }
    }

Which would be rendered in the mobile app UI as:

.. image:: ../img/smartapps/single-page-preferences.png
    :width: 250 px
    :height: 447 px

Note that in the above example, we did not specify the name or mode input, yet they appeared on our preferences page.
When defining single-page preferences, name and mode are automatically added. Also note that inputs that are marked as ``required: true`` are displayed differently by the mobile application, so that the user knows they are required. The mobile application will prevent the user from going to the next page or installing the SmartApp without entering required inputs.

Multiple Preferences Pages
--------------------------

Preferences can also be broken up into multiple pages. Each page must contain one or more *section*
elements. Each page specifies a *name* property that is referenced by the *nextPage* property. The *nextPage*
property is used to define the flow of the pages. Unlike single page preferences, the app name and mode control
fields are not automatically added, and must be specified on the desired page or pages.

Here's an example that defines three pages:

.. code-block:: groovy

    preferences {
        page(name: "pageOne", title: "When there's activity on any of these sensors", nextPage: "pageTwo", uninstall: true) {
            section("Choose sensors to trigger the action") {

                input "contactSensors", "capability.contactSensor",
                    title: "Open/close sensors", multiple: true

                input "motionSensors", "capability.motionSensor",
                    title: "Motion sensors?", multiple: true
            }
        }
        page(name: "pageTwo", title: "Turn on these lights", nextPage: "pageThree") {
            section {
                input "switches", "capability.switch", multiple: true
            }
        }
        page(name: "pageThree", title: "Name app and configure modes", install: true, uninstall: true) {
            section([mobileOnly:true]) {
                label title: "Assign a name", required: false
                mode title: "Set for specific mode(s)", required: false
            }
        }
    }

The resulting pages in the mobile app would show the name and mode control fields only on the third page, and the
uninstall button on the first and third pages:

====================================================    ====================================================    ====================================================
Page 1                                                  Page 2                                                  Page 3
====================================================    ====================================================    ====================================================
.. image:: ../img/smartapps/multiple-pages-page1.png    .. image:: ../img/smartapps/multiple-pages-page2.png    .. image:: ../img/smartapps/multiple-pages-page1.png
====================================================    ====================================================    ====================================================

Preference Elements & Inputs
----------------------------

Preference pages (single or multiple) are composed of one or more sections, each of which contains one or more of the
following elements:

----

paragraph
~~~~~~~~~

Text that's displayed on the page for messaging and instructional purposes.

Example:

.. code-block:: groovy


    preferences {
        section("paragraph") {
            paragraph "This us how you can make a paragraph element"
            paragraph image: "https://s3.amazonaws.com/smartapp-icons/Convenience/Cat-Convenience.png",
                      title: "paragraph title",
                      required: true,
                      "This is a long description that rambles on and on and on..."
        }
    }



The above preferences definition would render as:

.. image:: ../img/smartapps/prefs-paragraph.png
    :width: 250 px
    :height: 447 px

Valid options:

*title*
    String - The title of the paragraph
*image*
    String - URL of image to use, if desired
*required*
    Boolean - ``true`` or ``false`` to specify this input is required. Defaults to ``false``.

----

icon
~~~~

Allows the user to select an icon to be used when displaying the app in the mobile UI

Example:

.. code-block:: groovy


    preferences {
        section("paragraph") {
            icon(title: "required:true",
                 required: true)
        }
    }

The above preferences definition would render as:

.. image:: ../img/smartapps/prefs-icon.png
    :width: 250 px
    :height: 447 px

Tapping the element would then allow the user to choose an icon:

.. image:: ../img/smartapps/prefs-icon-chooser.png
    :width: 250 px
    :height: 447 px

Valid options:

*title*
    String - The title of the icon
*required*
    Boolean - ``true`` or ``false`` to specify this input is required. Defaults to ``false``.

----

href
~~~~

A control that selects another preference page or external HTML page.

Example of using href to visit a URL:

.. code-block:: groovy

    preferences {
        section("external") {
            href(name: "hrefNotRequired",
                 title: "SmartThings",
                 required: false,
                 style: "external",
                 url: "http://smartthings.com/",
                 description: "tap to view SmartThings website in mobile browser")
        }
        section("embedded") {
            href(name: "hrefWithImage", title: "This element has an image and a long title.",
                 description: "tap to view SmartThings website inside SmartThings app",
                 required: false,
                 image: "https://s3.amazonaws.com/smartapp-icons/Convenience/Cat-Convenience.png",
                 url: "http://smartthings.com/")
        }
    }


The above preferences would render as:

.. image:: ../img/smartapps/prefs-href-external-embedded.png
    :width: 250 px
    :height: 600 px

Example of using href to link to another preference page (dynamic pages are discussed later in this section):

.. code-block:: groovy

    preferences {
        page(name: "hrefPage")
        page(name: "deadEnd")
    }

    def hrefPage() {
        dynamicPage(name: "hrefPage", title: "href example page", uninstall: true) {
            section("page") {
                href(name: "href",
                     title: "dead end page",
                     required: false,
                     page: "deadEnd")
            }
        }
    }

    def deadEnd() {
        dynamicPage(name: "deadEnd", title: "dead end page") {
            section("dead end") {
                paragraph "this is a simple paragraph element."
            }
        }
    }

You can use the params option to pass data to dynamic pages:

.. code-block:: groovy

    preferences {
        page(name: "firstPage")
        page(name: "secondPage")
    }

    def firstPage() {
        def hrefParams = [
            foo: "bar",
            someKey: "someVal"
        ]

        dynamicPage(name: "firstPage", uninstall: true) {
            section {
                href(name: "toSecondPage",
                     page: "secondPage",
                     params: hrefParams,
                     description: "includes params: ${hrefParams}")
            }
        }
    }

    // page def must include a parameter for the params map!
    def secondPage(params) {
        log.debug "params: ${params}"
        dynamicPage(name: "secondPage", uninstall: true, install: true) {
            section {
                paragraph "params.foo = ${params?.foo}"
            }
        }
    }


Valid options:

*title*
    String - the title of the element
*required*
    Boolean - ``true`` or ``false`` to specify this input is required. Defaults to ``false``.
*description*
    String - the secondary text of the element
*external* (**deprecated - use style instead**)
    Boolean - ``true`` to open URL in mobile browser application, ``false`` to open URL within the SmartThings app. Defaults to ``false``
*style*
    String - Controls how the link will be handled. Specify "external" to launch the link in the mobile device's browser. Specify "embedded" to launch the link within the SmartThings mobile application. Specify "page" to indicate this is a preferences page.

    If ``style`` is not specified, but ``page`` is, then ``style:"page"`` is assumed. If ``style`` is not specified, but ``url`` is, then ``style:"embedded"`` is assumed.

    Currently, Android does not support the "external" style option.
*url*
    String - The URL of the page to visit. You can use query parameters to pass additional information to the URL (For example, \http://someurl.com?param1=value1&param2=value1\)
*params*
    Map - Use this to pass parameters to other preference pages. If doing this, make sure your page definition method accepts a single parameter (that will be this params map). See the page-params-by-href example at the end of this document for more information.
*page*
    String - Used to link to another preferences page. Not compatible with the external option.
*image*
    String - URL of an image to use, if desired.

----

.. _mode_pref:

mode
~~~~

Allows the user to select which modes the app executes in. Automatically generated by single-page preferences.

Example:

.. code-block:: groovy

    preferences {
        page(name: "pageOne", title: "page one", nextPage: "pageTwo", uninstall: true) {
            section("section one") {
                paragraph "just some text"
            }
        }
        page(name: "pageTwo", title: "page two") {
            section("page two section one") {
                mode(name: "modeMultiple",
                     title: "pick some modes",
                     required: false)
                mode(name: "modeWithImage",
                     title: "This element has an image and a long title.",
                     required: false,
                     multiple: false,
                     image: "https://s3.amazonaws.com/smartapp-icons/Convenience/Cat-Convenience.png")
            }
        }
    }


The second page of the above example would render as:

.. image:: ../img/smartapps/prefs-mode.png
    :width: 250 px
    :height: 447 px

Valid options:

*title*
    String - the title of the mode field
*required*
    Boolean - ``true`` or ``false`` to specify this input is required. Defaults to ``false``.
*multiple*
    Boolean - ``true`` or ``false`` to specify this input allows selection of multiple values. Defaults to ``true``.
*image*
    String - URL of an image to use, if desired.

.. note::
    There are a couple of different ways to use modes that are worth pointing out. The first way is to use modes as a type of enum input like this:

    .. code-block:: groovy

        input "modes", "mode", title: "only when mode is", multiple: true, required: false

    This method will automatically list the defined modes as the options. Keep in mind when using modes in this way that the modes are just data
    and can be accessed in the SmartApp as such.
    This does not effect SmartApp execution. In this scenario, it is up to the SmartApp itself to react to the mode changes.

    The second example actually controls whether the app is executed based on the modes selected:

    .. code-block:: groovy

        mode(title: "set for specific mode(s)")

    Both of these methods of using modes are valid. The impact on SmartApp execution is different in each scenario and
    it is up to the SmartApp developer to properly label whichever form is used and code the app accordingly.

----

label
~~~~~

Allows the user to name the app installation. Automatically generated by single-page preferences.

Example:

.. code-block:: groovy

    preferences {
        section("labels") {
            label(name: "label",
                  title: "required:false",
                  required: false,
                  multiple: false)
            label(name: "labelRequired",
                  title: "required:true",
                  required: true,
                  multiple: false)
            label(name: "labelWithImage",
                  title: "This element has an image and a title.",
                  description: "image and a title",
                  required: false,
                  image: "https://s3.amazonaws.com/smartapp-icons/Convenience/Cat-Convenience.png")
        }
    }


The above preferences definition would render as:

.. image:: ../img/smartapps/prefs-label.png
    :width: 250 px
    :height: 447 px

Valid options:

*title*
    String - the title of the label field
*description*
    String - the default text in the input field
*required*
    Boolean - ``true`` or ``false`` to specify this input is required. Defaults to ``false``. Defaults to ``true``.
*image*
    String - URL to an image to use, if desired

----

app
~~~

Provides user-initiated installation of child apps. Typically used in dashbard solution SmartApps, which are currently not supported for community development.

----

input
~~~~~

Allows the user to select devices or enter values to be used during execution of the smart app.

Inputs are the most commonly used preference elements. They can be used to prompt the user to select devices that
provide a certain capability, devices of a specific type, or constants of various kinds. Input element method calls
take two forms. The "shorthand" form passes in the name and type unnamed as the required first two parameters, and any
other arguments as named options:

.. code-block:: groovy

    preferences {
        section("section title") {
            // name is "temperature1", type is "number"
            input "temperature1", "number", title: "Temperature"
        }
    }

The second form explicitly specifies the name of each argument:

.. code-block:: groovy

    preferences {
        section("section title") {
            input(name: "color", type: "enum", title: "Color", options: ["Red","Green","Blue","Yellow"])
        }
    }

Valid input options:

*name*
    String - name of variable that will be created in this SmartApp to reference this input
*title*
    String - title text of this element.
*description*
    String - default value of the input element
*multiple*
    Boolean - ``true`` to allow multiple values or ``false`` to allow only one value. Not valid for all input types.
*required*
    Boolean - ``true`` to require the selection of a device for this input or ``false`` to not require selection.
*submitOnChange*
    Boolean - ``true`` to force a page refresh after input selection or ``false`` to not refresh the page. This is useful
    when creating a dynamic input page.
*options*
    List - used in conjunction with the enum input type to specify the values the user can choose from. Example: ``options: ["choice 1", "choice 2", "choice 3"]``
*type*
    String - one of the names from the following table:

    ===========================  ===========================================================================================
    **Name**                     **Comment**
    ---------------------------  -------------------------------------------------------------------------------------------
    cacapability.capabilityName  Prompts for all the devices that match the specified capability.

                                 See the *Preferences Reference* column of the :ref:`capabilities_taxonomy`
                                 table for possible values.
    device.deviceTypeName        Prompts for all devices of the specified type.
    bool                         A ``true`` or ``false`` value (value returned as a boolean).
    boolean                      A ``"true"`` or ``"false"`` value (value returned as a string). It's recommended that you use the "bool" input instead, since the simulator and mobile support for this type may not be consistent, and using "bool" will return you a boolean (instead of a string). The "boolean" input type may be removed in the near future.
    decimal                      A floating point number, i.e. one that can contain a decimal point
    email                        An email address
    enum                         One of a set of possible values. Use the *options* element to define the possible values.
    hub                          Prompts for the selection of a hub
    icon                         Prompts for the selection of an icon image
    number                       An integer number, i.e. one without decimal point
    password                     A password string. The value is obscured in the UI and encrypted before storage
    phone                        A phone number
    time                         A time of day. The value will be stored as a string in the Java `SimpleDateFormat <http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html>`__ (e.g., "2015-01-09T15:50:32.000-0600")
    text                         A text value
    ===========================  ===========================================================================================


Dynamic Preferences
-------------------

One of the most powerful features of multi-page preferences is the ability to dynamically generate the content of a page
based on previous selections or external inputs, such as the data elements returned from a web services call. The
following example shows how to create a two-page preferences SmartApp where the content of the second page depends
on the selections made on the first page.

.. code-block:: groovy

     preferences {
        page(name: "page1", title: "Select sensor and actuator types", nextPage: "page2", uninstall: true) {
            section {
                input("sensorType", "enum", options: [
                    "contactSensor":"Open/Closed Sensor",
                    "motionSensor":"Motion Sensor",
                    "switch": "Switch",
                    "moistureSensor": "Moisture Sensor"])

                input("actuatorType", "enum", options: [
                    "switch": "Light or Switch",
                    "lock": "Lock"]
                )
            }
        }

        page(name: "page2", title: "Select devices and action", install: true, uninstall: true)

    }

    def page2() {
        dynamicPage(name: "page2") {
            section {
                input(name: "sensor", type: "capability.$sensorType", title: "If the $sensorType device")
                input(name: "action", type: "enum", title: "is", options: attributeValues(sensorType))
            }
            section {
                input(name: "actuator", type: "capability.$actuatorType", title: "Set the $actuatorType")
                input(name: "action", type: "enum", title: "to", options: actions(actuatorType))
             }

        }
    }

    private attributeValues(attributeName) {
        switch(attributeName) {
            case "switch":
                return ["on","off"]
            case "contactSensor":
                return ["open","closed"]
            case "motionSensor":
                return ["active","inactive"]
            case "moistureSensor":
                return ["wet","dry"]
            default:
                return ["UNDEFINED"]
        }
    }

    private actions(attributeName) {
        switch(attributeName) {
            case "switch":
                return ["on","off"]
            case "lock":
                return ["lock","unlock"]
            default:
                return ["UNDEFINED"]
        }
    }

The previous example shows how you can achieve dynamic behavior between pages. With the ``submitOnChange`` input attribute
you can also have dynamic behavior in a single page.

.. code-block:: groovy

    preferences {
        page(name: "examplePage")
    }

    def examplePage() {
        dynamicPage(name: "examplePage", title: "", install: true, uninstall: true) {

            section {
                input(name: "dimmers", type: "capability.switchLevel", title: "Dimmers",
                      description: null, multiple: true, required: false, submitOnChange: true)
            }

            if (dimmers) {
                // Do something here like update a message on the screen,
                // or introduce more inputs. submitOnChange will refresh
                // the page and allow the user to see the changes immediately.
                // For example, you could prompt for the level of the dimmers
                // if dimmers have been selected:

                section {
                    input(name: "dimmerLevel", type: "number", title: "Level to dim lights to...", required: true)
                }
            }
        }
    }

.. note:: When a submitOnChange input is changed, the whole page will be saved. Then a refresh is triggered with the
    saved page state. This means that all of the methods will execute each time you change a submitOnChange input.

Examples
--------

`page-params-by-href.groovy <https://github.com/SmartThingsCommunity/Code/blob/master/smartapps/preferences/page-params-by-href.groovy>`__ shows how to pass parameters to dynamic pages using the href element.

Almost every SmartApp makes use of preferences to some degree. You can browse them in the IDE under the "Browse SmartApp Templates" menu.
