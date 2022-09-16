Power Monitor
-------------

The Power Monitor measures the power coming into the module. It reports input voltage and current as
well as calculates power and energy consumed. A built in low pass filter with adjustable cutoff frequency
smooths these values.

Arduino
~~~~~~~

To use Power Monitor in Arduino, ensure iq module communication.hpp is included. This allows the creation
of a PowerMonitorClient object. See Table 9 for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerMonitorClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    PowerMonitorClient pwr(0);
    
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

To use Power Monitor in C++, include power monitor client.hpp. This allows the creation of a 
PowerMonitorClient object. See Table 9 for available messages. All message objects use the Short Name with a trailing
underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerMonitorClient is:

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "power_monitor_client.hpp"
    
    float volts;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Power Monitor object with obj_id 0
        PowerMontitorClient pwr(0);

        // Use the Power Monitor object
        pwr.volts_.get(com);

        // Insert code for interfacing with hardware here
        // Read response
        volts = pwr.volts_.get_reply();
    }

Matlab
~~~~~~

To use Power Monitor in Matlab, all IQ communication code must be included in your path. This allows
the creation of a PowerMonitorClient object. See Table 9 for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerMonitorClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a PowerMontitorClient object with obj_id 0
    pwr = PowerMontitorClient(’com’,com);
    
    % Use the PowerMontitorClient object
    volts = pwr.get(’volts’);


Python
~~~~~~

To use the Power Monitor Client in Python, include ``iqmotion`` and create a module that has the Power Monitor Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Power Monitor Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0|module_firmware|)
    
    volts = |variable_name|.get("power_monitor", "volts")  # returns the input voltage to module
    print(f"Voltage coming into module: {volts}")

Message Table
~~~~~~~~~~~~~

Type ID 69 | Power Monitor

+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| Sub ID | Short Name   | Access         | Data Type | Unit  | Note                                          |
+========+==============+================+===========+=======+===============================================+
| 0      | volts        | get            | float     | V     | Input voltage to module                       |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 1      | amps         | get            | float     | A     | Input amperage to module                      |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 2      | watts        | get            | float     | W     | Input wattage to module                       |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 3      | joules       | get            | float     | J     | Total energy consumed by module               |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 4      | reset_joules | set            |           |       | Sets joules to zero                           |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 5      | filter_fs    | get            | uint32    | Hz    | Low pass filter sample frequency              |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 6      | filter_fc    | get, set, save | uint32    | Hz    | Low pass filter cutoff frequency              |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 7      | volts_raw    | get            | uint16    | bit   | ADC value for the voltage measurement         |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 8      | amps_raw     | get            | uint16    | bit   | ADC value for the current measurement         |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 9      | volts_gain   | get, set, save | float     | V/bit | Gain for the ADC to voltage conversion        |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 10     | amps_gain    | get, set, save | float     | A/bit | Gain for the ADC to current conversion        |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 11     | amps_bias    | get, set, save | float     | bit   | Offset bias for the ADC to current conversion |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
| 12     | vref_raw     | get            | uint16    | bit   |                                               |
+--------+--------------+----------------+-----------+-------+-----------------------------------------------+
