Building the Device Type
========================

The device handler for a LAN connected device is generally the same as any other device handler. The means in which
it handles sending and receiving messages from its device is a little bit different. Let's walk through a LAN connected
device handler example.

Making Outbound HTTP Calls with HubAction
-----------------------------------------

Depending on the type of device you are using, you will send requests to
your devices through the hub via REST or UPnP. You can do this using
the SmartThings provided ``HubAction`` class.

----

Overview
--------

The class ``physicalgraph.device.HubAction`` encapsulates request information
for communicating with the device.

When you create an instance of a ``HubAction``, you provide details about the
request, such as the request method, headers, and path. By itself, ``HubAction`` is little more than a wrapper for these request details.

It is when an instance of a ``HubAction`` is returned from a command method that it becomes useful.

When a command method of your device handler returns an instance of a ``HubAction``, the SmartThings platform will use the request information within it to actually perform the request. It will then call the device-handler's ``parse`` method with any response data.

Herein lies an important point - *if your HubAction instance is not returned from your command method, no request will be made.* It will just be an object allocating system memory. Not very useful.

So remember - the ``HubAction`` instance should be returned from your command method so that the platform can make the request!

----

Creating a HubAction Object
---------------------------

To create a HubAction object, you can pass in a map of parameters to the constructor that defines the request information:

.. code-block:: groovy

    def result = new physicalgraph.device.HubAction(
        method: "GET",
        path: "/somepath",
        headers: [
            HOST: "device IP address"
        ],
        query: [param1: "value1", param2: "value2"]
    )

A brief discussion of the options that can be provided follows:

*method*
    The HTTP method to use for the reqeust.
*path*
    The path to send the request to. You can add URL parameters to the request directly, or use the ``query`` option.
*headers*
    A map of HTTP headers and their values for this request. This is where you will provide the IP address of the device as the HOST.
*query*
    A map of query parameters to use in this request. You can use URL parameters directly on the path if you wish, instead of using this option.

----

Parsing the Response
--------------------

When you make a request to your device using ``HubAction``, any response will be passed to your device-handler's ``parse`` method, just like other device messages.

You can use the ``parseLanMessage`` method to parse the incoming message.

``parseLanMessage`` example:

.. code-block:: groovy

    def parse(description) {
        ...
        def msg = parseLanMessage(description)

        def headersAsString = msg.header // => headers as a string
        def headerMap = msg.headers      // => headers as a Map
        def body = msg.body              // => request body as a string
        def status = msg.status          // => http status code of the response
        def json = msg.json              // => any JSON included in response body, as a data structure of lists and maps
        def xml = msg.xml                // => any XML included in response body, as a document tree structure
        def data = msg.data              // => either JSON or XML in response body (whichever is specified by content-type header in response)

        ...
    }

For more information about the JSON or XML response formats, see the Groovy `JsonSlurper <http://docs.groovy-lang.org/latest/html/gapi/groovy/json/JsonSlurper.html>`__ and `XmlSlurper <http://docs.groovy-lang.org/latest/html/api/groovy/util/XmlSlurper.html>`__ documentation.

----

Getting the Addresses
---------------------

To use HubAction, you will need the IP address of the device, and sometimes the hub.

How the device IP and port are stored my vary depending on the device type. There's currently not a public API to get this information easily, so until there is, you will need to handle this in your device-type handler. Consider using helper methods like these to get this information:

.. code-block:: groovy

    // gets the address of the hub
    private getCallBackAddress() {
        return device.hub.getDataValue("localIP") + ":" + device.hub.getDataValue("localSrvPortTCP")
    }

    // gets the address of the device
    private getHostAddress() {
        def ip = getDataValue("ip")
        def port = getDataValue("port")

        if (!ip || !port) {
            def parts = device.deviceNetworkId.split(":")
            if (parts.length == 2) {
                ip = parts[0]
                port = parts[1]
            } else {
                log.warn "Can't figure out ip and port for device: ${device.id}"
            }
        }

        log.debug "Using IP: $ip and port: $port for device: ${device.id}"
        return convertHexToIP(ip) + ":" + convertHexToInt(port)
    }

    private Integer convertHexToInt(hex) {
        return Integer.parseInt(hex,16)
    }

    private String convertHexToIP(hex) {
        return [convertHexToInt(hex[0..1]),convertHexToInt(hex[2..3]),convertHexToInt(hex[4..5]),convertHexToInt(hex[6..7])].join(".")
    }

You'll see the rest of the examples in this document use these helper methods.

----

REST Requests
-------------

``HubAction`` can be used to make `REST <http://en.wikipedia.org/wiki/Representational_state_transfer>`__ calls to communicate with the device.

Here's a quick example:

.. code-block:: groovy

    def myCommand() {
        def result = new physicalgraph.device.HubAction(
            method: "GET",
            path: "/yourpath?param1=value1&param2=value2",
            headers: [
                HOST: getHostAddress()
            ]
        )
        return result
    }

----

UPnP/SOAP Requests
------------------

Alternatively, after making the initial connection you can use UPnP.
UPnP uses `SOAP <http://en.wikipedia.org/wiki/SOAP_%28protocol%29>`__
(Simple Object Access Protocol) messages to communicate with the device.

SmartThings provides the ``HubSoapAction`` class for this purpose. It is similar to the HubAction class (it actually extends the HubAction class), but it will handle creating the soap envelope for you.

Here's an example of using ``HubSoapAction``:

.. code-block:: groovy

    def someCommandMethod() {
        return doAction("SetVolume", "RenderingControl", "/MediaRenderer/RenderingControl/Control", [InstanceID: 0, Channel: "Master", DesiredVolume: 3])
    }

    def doAction(action, service, path, Map body = [InstanceID:0, Speed:1]) {
        def result = new physicalgraph.device.HubSoapAction(
            path:    path,
            urn:     "urn:schemas-upnp-org:service:$service:1",
            action:  action,
            body:    body,
            headers: [Host:getHostAddress(), CONNECTION: "close"]
        )
        return result
    }

----

Subscribing to Device Events
----------------------------

If you'd like to hear back from a LAN connected device upon a particular
event, you can subscribe using a ``HubAction``. The ``parse`` method will be called when this event is fired on the device.

Here's an example using UPnP:

.. code-block:: groovy

    def someCommand() {
        subscribeAction("/path/of/event")
    }

    private subscribeAction(path, callbackPath="") {
        log.trace "subscribe($path, $callbackPath)"
        def address = getCallBackAddress()
        def ip = getHostAddress()

        def result = new physicalgraph.device.HubAction(
            method: "SUBSCRIBE",
            path: path,
            headers: [
                HOST: ip,
                CALLBACK: "<http://${address}/notify$callbackPath>",
                NT: "upnp:event",
                TIMEOUT: "Second-28800"
            ]
        )

        log.trace "SUBSCRIBE $path"

        return result
    }

----

References and Resources
------------------------

- `UPnP <http://en.wikipedia.org/wiki/Universal_Plug_and_Play>`__
- `SOAP <http://en.wikipedia.org/wiki/SOAP>`__
- `REST <http://en.wikipedia.org/wiki/Representational_state_transfer>`__
