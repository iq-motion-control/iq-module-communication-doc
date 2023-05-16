IQUART Flight Controller Interface
----------------------------------
This client is used to simplify communication between the flight controller and multiple modules that are connected to an IQUART bus.

Arduino
~~~~~~~

To use the IQUART Flight Controller Interface in Arduino, ensure iquart_flight_controller_interface_client.hpp is included. This
allows the creation of a IQUartFlightControllerInterfaceClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the IQUartFlightControllerInterfaceClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    IQUartFlightControllerInterfaceClient iquartFlightControllerInterface(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int pulsingVoltageMode = 0;
        if(ser.get(iquartFlightControllerInterface.telemetry_, pulsingVoltageMode))
        Serial.println(pulsingVoltageMode);
    }

C++
~~~

To use the IQUART Flight Controller Interface client in C++, include iquart_flight_controller_interface_client.hpp. This allows the
creation of an IQUartFlightControllerInterface object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

.. include:: ../notes/IFCITelemetryData-struct.rst

Simply create an ``IFCITelemetryData`` object to store the response from the telemetry endpoint.
A minimal working example for the IQUartFlightControllerInterfaceClient using the telemetry endpoint is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "iquart_flight_controller_interface_client.hpp"
    #include <iostream>

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a IQUART Flight Controller Interface object with obj_id 0
        IQUartFlightControllerInterfaceClient iquartFlightControllerInterface(0);

        // Use the IQUART Flight Controller Interface Client
        iquartFlightControllerInterface.telemetry_.get(com)

        // Insert code for interfacing with hardware here  

        // Store telemetry data in an IFCITelemetryData object
        IFCITelemetryData telemtry = iquart_flight_controller_interface.telemetry_.get_reply()

        // Examples on how to access each property of IFCITelemetryData
        cout << "telemetry coil_temp: " << telemetry.coil_temp << endl;
        cout << "telemetry consumption: " << telemetry.consumption << endl;
        cout << "telemetry current: " << telemetry.current << endl;
        cout << "telemetry mcu temp: " << telemetry.mcu_temp << endl;
        cout << "telemetry speed: " << telemetry.speed << endl;
        cout << "telemetry uptime: " << telemetry.uptime << endl;
        cout << "telemetry voltage: " << telemetry.voltage << endl;
    }

Matlab
~~~~~~

To use the IQUART Flight Controller Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a IQUartFlightControllerInterfaceClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the IQUartFlightControllerInterfaceClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface('COM18', 115200);
    % Make a IQUART Flight Controller Interface object with obj_id 0
    % IQUART Flight Controller Interface objects are always obj_id 0
    iquartFlightControllerInterface = IQUartFlightControllerInterfaceClient('com', com);
    % Use the IQUART Flight Controller Interface object
    throttleCvi= iquartFlightControllerInterface.get('throttle_cvi');

Python
~~~~~~

To use the IQUART Flight Controller Interface Client in Python, import ``iqmotion`` and create a module that has the Arming Handler Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the IQUART Flight Controller Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    telemetry = |variable_name|.get("iquart_flight_controller_interface", "telemetry") 
    print(f"Pulsing voltage mode: {telemetry}")

Message Table
~~~~~~~~~~~~~

Type ID 88 | IQUART Flight Controller Interface

+--------+--------------+----------------+-------------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name   | Access         | Data Type                                                   | Unit           | Note                                                                                                              |
+========+==============+================+=============================================================+================+===================================================================================================================+
| 1      | telemetry    | get            | See :ref:`IFCITelemetryData struct<ifcitelemetrydata_note>` | 16 Byte Array  | Returns 16 bytes of telemetry information. See the :ref:`IFCITelemetryData struct<ifcitelemetrydata_note>` notes. |
+--------+--------------+----------------+-------------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------+
| 2      | throttle_cvi | get, set, save | uint8                                                       | Index [0, 255] | The control value index for the throttle.                                                                         |
+--------+--------------+----------------+-------------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------+
| 3      | x_cvi        | get, set, save | uint8                                                       | Index [0, 255] | The control value index value for the x rectangular coordinate.                                                   |
+--------+--------------+----------------+-------------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------+
| 4      | y_cvi        | get, set, save | uint8                                                       | Index [0, 255] | The control value index value for the y rectangular coordinate.                                                   |
+--------+--------------+----------------+-------------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------+
