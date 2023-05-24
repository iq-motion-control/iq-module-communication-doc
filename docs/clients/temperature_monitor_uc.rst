Temperature Monitor UC
----------------------

The Temperature Monitor Microcontroller reads, filters, and reports the microcontroller’s internal temperature. 
The temperature is used to derate the motor if the temperature rises into dangerous levels. The filter’s
cutoff frequency and the temperature limits can be adjusted, though this is not recommended.

Arduino
~~~~~~~

To use the Temperature Monitor Microcontroller in Arduino, ensure iq_module_communication.hpp is included. 
This allows the creation of a TemperatureMonitorUcClient object. See the Message Table below for available
messages. All message objects use the Short Name with a trailing underscore. All messages use the standard
Get/Set/Save functions.

A minimal working example for the TemperatureMonitorUcClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    TemperatureMonitorUcClient tmp(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }

    void loop() {
        float temperature = 0.0f;
        if(ser.get(tmp.uc_temp_,temperature))
            Serial.println(temperature);
    }

C++
~~~

To use the Temperature Monitor Microcontroller client in C++, include temperature monitor uc client.hpp.
This allows the creation of a TemperatureMonitorUcClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions.

A minimal working example for the TemperatureMonitorUcClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "temperature_monitor_uc_client.hpp"
    
    float uc_temp;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Temperature Monitor Microcontroller object with obj_id 0
        TemperatureMonitorUcClient tuc(0);

        // Use the Temperature Monitor Microcontroller object
        tuc.uc_temp_.get(com);

        // Insert code for interfacing with hardware here
        // Read response
        uc_temp = tuc.uc_temp_.get_reply();

    }

Matlab
~~~~~~

To use the Temperature Monitor Microcontroller client in Matlab, all Vertiq communication code must be
included in your path. This allows the creation of a TemperatureMonitorUcClient object. See the Message Table below for
available messages. All message strings use the Short Names. All messages use the standard Get/Set/Save
functions.

A minimal working example for the TemperatureMonitorUcClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make an TemperatureMonitorUcClient object with obj_id 0
    TemperatureMonitorUc = TemperatureMonitorUcClient(’com’,com);
    
    % Use the TemperatureMonitorUcClient object
    ucTemp = TemperatureMonitorUc.get(’uc_temp’);

Python
~~~~~~

To use the Temperature Monitor Microcontroller Client in Python, import ``iqmotion`` and create a module that has the Temperature Monitor Microcontroller Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Temperature Monitor Microcontroller Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    temp = |variable_name|.get("temperature_monitor_uc", "uc_temp")  # Internal UC Temperature
    print(f"Internal UC temperature: {temp}")

Message Table
~~~~~~~~~~~~~

Type ID 73 | Temperature Monitor Microcontroller

+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name |     Access     | Data Type | Unit |                                                     Note                                                     |
+========+============+================+===========+======+==============================================================================================================+
| 0      | uc_temp    | get            | float     | degC | Temperature of the microcontroller                                                                           |
+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
| 1      | filter_fs  | get            | uint32    | Hz   | Low pass filter sample frequency                                                                             |
+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
| 2      | filter_fc  | get, set, save | uint32    | Hz   | Low pass filter cutoff frequency                                                                             |
+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
| 3      | otw        | get, set, save | float     | degC | Over temperature warning. Derating of the motor begins at this temperature.                                  |
+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
| 4      | otlo       | get, set, save | float     | degC | Over temperature lock out. Derating of the motor end at this temperature, where the motor is fully disabled. |
+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
| 5      | derate     | get            | float     | PU   | Amount of derating applied to motor [0 1]                                                                    |
+--------+------------+----------------+-----------+------+--------------------------------------------------------------------------------------------------------------+
