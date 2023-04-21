IQUART Flight Controller Interface
----------------------------------
TODO

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

A minimal working example for the IQUartFlightControllerInterfaceClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "iquart_flight_controller_interface_client.hpp"

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a IQUART Flight Controller Interface object with obj_id 0
        IQUartFlightControllerInterfaceClient iquartFlightControllerInterface(0);

        // Use the IQUART Flight Controller Interface Client
        iquartFlightControllerInterface.telemetry_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the IQUART Flight Controller Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a IQUartFlightControllerInterfaceClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the IQUartFlightControllerInterfaceClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a IQUartFlightControllerInterfaceClient object with obj_id 0
    iquartFlightControllerInterface = IQUartFlightControllerInterfaceClient(’com’,com);
    % Use the IQUartFlightControllerInterfaceClient object
    pulsingVoltageMode = iquartFlightControllerInterface.get(’telemetry’);

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

+--------+-----------------------+----------------+-----------+------+------+
| Sub ID | Short Name            | Access         | Data Type | Unit | Note |
+========+=======================+================+===========+======+======+
| 0      | telemetry             | get            | uint8     |      |      |
+--------+-----------------------+----------------+-----------+------+------+
| 1      | pulsing_voltage_limit | get, set, save | float     |      |      |
+--------+-----------------------+----------------+-----------+------+------+
