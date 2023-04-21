Pulsing Rectangular Input Parser
--------------------------------
TODO

Arduino
~~~~~~~

To use the Pulsing Rectangular Input Parser in Arduino, ensure pulsing_rectangular_input_parser_client.hpp is included. This
allows the creation of a PulsingRectangularInputParserClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the PulsingRectangularInputParserClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    PulsingRectangularInputParserClient pulsingRectangularInputParser(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int pulsingVoltageMode = 0;
        if(ser.get(pulsingRectangularInputParser.pulsing_voltage_mode_, pulsingVoltageMode))
        Serial.println(pulsingVoltageMode);
    }

C++
~~~

To use the Pulsing Rectangular Input Parser client in C++, include pulsing_rectangular_input_parser_client.hpp. This allows the
creation of an PulsingRectangularInputParser object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PulsingRectangularInputParserClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "pulsing_rectangular_input_parser_client.hpp"

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Pulsing Rectangular Input Parser object with obj_id 0
        PulsingRectangularInputParserClient pulsingRectangularInputParser(0);

        // Use the Pulsing Rectangular Input Parser Client
        pulsingRectangularInputParser.pulsing_voltage_mode_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the Pulsing Rectangular Input Parser client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a PulsingRectangularInputParserClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PulsingRectangularInputParserClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a PulsingRectangularInputParserClient object with obj_id 0
    pulsingRectangularInputParser = PulsingRectangularInputParserClient(’com’,com);
    % Use the PulsingRectangularInputParserClient object
    pulsingVoltageMode = pulsingRectangularInputParser.get(’pulsing_voltage_mode’);

Python
~~~~~~

To use the Pulsing Rectangular Input Parser Client in Python, import ``iqmotion`` and create a module that has the Arming Handler Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Pulsing Rectangular Input Parser Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    pulsing_voltage_mode = |variable_name|.get("pulsing_rectangular_input_parser", "pulsing_voltage_mode") 
    print(f"Pulsing voltage mode: {pulsing_voltage_mode}")

Message Table
~~~~~~~~~~~~~

Type ID 89 | Pulsing Rectangular Input Parser

+--------+-----------------------+----------------+-----------+------+------+
| Sub ID | Short Name            | Access         | Data Type | Unit | Note |
+========+=======================+================+===========+======+======+
| 0      | pulsing_voltage_mode  | get, set, save | uint8     |      |      |
+--------+-----------------------+----------------+-----------+------+------+
| 1      | pulsing_voltage_limit | get, set, save | float     |      |      |
+--------+-----------------------+----------------+-----------+------+------+
