
Power Safety
-------------

!

Arduino
~~~~~~~

To use Power Safety in Arduino, ensure iq_module_communication.hpp is included. This allows the creation
of a PowerSafetyClient object. See Table 9 for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerSafetyClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    PowerSafetyClient pwr(0);
    
    void setup() {
        ser.begin();
        
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }

    void loop() {
        float voltage = 0;
        if(ser.get(pwr.volts_,voltage))
        Serial.println(voltage);
    }

C++
~~~

To use Power Safety in C++, include power monitor client.hpp. This allows the creation of a 
PowerSafetyClient object. See Table 9 for available messages. All message objects use the Short Name with a trailing
underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerSafetyClient is:

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "power_monitor_client.hpp"
    
    float volts;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Power Safety object with obj_id 0
        PowerMontitorClient pwr(0);

        // Use the Power Safety object
        pwr.volts_.get(com);

        // Insert code for interfacing with hardware here
        // Read response
        volts = pwr.volts_.get_reply();
    }

Matlab
~~~~~~

To use Power Safety in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a PowerSafetyClient object. See Table 9 for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerSafetyClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a PowerMontitorClient object with obj_id 0
    pwr = PowerMontitorClient(’com’,com);
    
    % Use the PowerMontitorClient object
    volts = pwr.get(’volts’);


Python
~~~~~~

To use the Power Safety Client in Python, include ``iqmotion`` and create a module that has the Power Safety Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Power Safety Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0|module_firmware|)
    
    fault_now = |variable_name|.get("power_safety", "fault_now")  
    print(f"current fault: {fault_now}")

Message Table
~~~~~~~~~~~~~

Type ID 83 | Power Safety

+--------+-----------------------+----------------+-----------+-------+------+
| Sub ID | Short Name            | Access         | Data Type | Unit  | Note |
+========+=======================+================+===========+=======+======+
| 0      | fault_now             | get            | uint8     |       |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 1      | fault_ever            | get, set, save | uint8     |       |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 2      | fault_latching        | get, set, save | uint8     |       |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 3      | volt_input_low        | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 4      | volt_input_high       | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 5      | vref_int_low          | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 6      | vref_int_high         | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 7      | current_input_low     | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 8      | current_input_high    | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 9      | temperature_uc_low    | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 10     | temperature_uc_high   | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 11     | temperature_coil_low  | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
| 12     | temperature_coil_high | get, set, save | float     | fix16 |      |
+--------+-----------------------+----------------+-----------+-------+------+
