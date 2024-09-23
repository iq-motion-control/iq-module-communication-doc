.. include:: common_client_variables.rst

Power Safety
-------------

The Power Safety protects the motor by checking various parameters in the motor, such as voltage and current, to make sure the values are within a specific range. If the values are above or below the thresholds, the motor will coast and lock down to prevent any commands from being processed. The motor will unlock once the parameters are back within the thresholds. 

Arduino
~~~~~~~

To use Power Safety in Arduino, ensure iq_module_communication.hpp is included. This allows the creation
of a PowerSafetyClient object. See the Message Table below for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerSafetyClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    PowerSafetyClient ps(0);
    
    void setup() {
        ser.begin();
        
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }

    void loop() {
        float current_fault = 0;
        if(ser.get(ps.fault_now_, current_fault))
        Serial.println(current_fault);
    }

C++
~~~

To use Power Safety in C++, include power monitor client.hpp. This allows the creation of a 
PowerSafetyClient object. See the Message Table below for available messages. All message objects use the Short Name with a trailing
underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerSafetyClient is:

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "power_safety_client.hpp"
    
    float volts;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Power Safety object with obj_id 0
        PowerSafetyClient ps(0);

        // Use the Power Safety object
        ps.fault_now_.get(com);

        // Insert code for interfacing with hardware here
        // Read response
        volts = ps.fault_now_.get_reply();
    }

Matlab
~~~~~~

To use Power Safety in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a PowerSafetyClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerSafetyClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface('COM18', 115200);

    % Make a Power Safety object with obj_id 0
    PowerSafety = PowerSafetyClient('com', com);

    % Use the Power Safety object
    faultNow = PowerSafety.get('fault_now');


Python
~~~~~~

To use the Power Safety Client in Python, import ``iqmotion`` and create a module that has the Power Safety Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Power Safety Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    fault_now = |variable_name|.get("power_safety", "fault_now")  
    print(f"current fault: {fault_now}")

Message Table
~~~~~~~~~~~~~

Type ID 84 | Power Safety
    .. table:: Power Safety
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name            | Access         | Data Type | Unit                   | Note                                                                                                                                                                                                                                               |
        +========+=======================+================+===========+========================+====================================================================================================================================================================================================================================================+
        | 0      | fault_now             | get            | uint8     | :math:`\text{Bitmask}` | The fault(s) that are currently triggering                                                                                                                                                                                                         |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 1      | fault_ever            | get, set       | uint8     | :math:`\text{Bitmask}` | Record of all faults that were triggered since last modified by user. The user can clear all of the records by setting the value to 0, or individual faults by properly setting bitmask value                                                      |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | fault_latching        | get, set, save | uint8     | :math:`\text{Bitmask}` | Determines if the motor is latching. When latching, if there are any bits set in fault_ever, motor will stay in Safe Mode until they are cleared. When not latching, motor will enter and exit Safe Mode only on current faults based on fault_now |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | volt_input_low        | get, set, save | float     | :math:`V`              | Minimum threshold value for voltage                                                                                                                                                                                                                |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 4      | volt_input_high       | get, set, save | float     | :math:`V`              | Maximum threshold value for voltage                                                                                                                                                                                                                |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 5      | vref_int_low          | get, set, save | float     | :math:`V`              | Minimum threshold value for reference voltage                                                                                                                                                                                                      |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 6      | vref_int_high         | get, set, save | float     | :math:`V`              | Maximum threshold value for reference voltage                                                                                                                                                                                                      |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 7      | current_input_low     | get, set, save | float     | :math:`A`              | Minimum threshold value for current                                                                                                                                                                                                                |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 8      | current_input_high    | get, set, save | float     | :math:`A`              | Maximum threshold value for current                                                                                                                                                                                                                |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 9      | motor_current_low     | get, set, save | float     | :math:`A`              | Minimum threshold value for motor current                                                                                                                                                                                                          |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 10     | motor_current_high    | get, set, save | float     | :math:`A`              | Maximum threshold value for motor current                                                                                                                                                                                                          |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 11     | temperature_uc_low    | get, set, save | float     | :math:`^{\circ}C`      | Minimum threshold value for microcontroller temperature                                                                                                                                                                                            |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 12     | temperature_uc_high   | get, set, save | float     | :math:`^{\circ}C`      | Maximum threshold value for microcontroller temperature                                                                                                                                                                                            |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 13     | temperature_coil_low  | get, set, save | float     | :math:`^{\circ}C`      | Minimum threshold value for coil temperature                                                                                                                                                                                                       |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 14     | temperature_coil_high | get, set, save | float     | :math:`^{\circ}C`      | Maximum threshold value for coil temperature                                                                                                                                                                                                       |
        +--------+-----------------------+----------------+-----------+------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
