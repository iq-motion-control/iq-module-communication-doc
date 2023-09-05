Power Monitor
-------------

The Power Monitor measures the power coming into the module. It reports input voltage and current as
well as calculates power and energy consumed. A built in low pass filter with adjustable cutoff frequency
smooths these values.

Arduino
~~~~~~~

To use Power Monitor in Arduino, ensure iq_module_communication.hpp is included. This allows the creation
of a PowerMonitorClient object. See the Message Table below for available messages. All message objects use the Short Name
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
PowerMonitorClient object. See the Message Table below for available messages. All message objects use the Short Name with a trailing
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

To use Power Monitor in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a PowerMonitorClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PowerMonitorClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a PowerMontitorClient object with obj_id 0
    PowerMonitor = PowerMontitorClient(’com’,com);
    
    % Use the PowerMontitorClient object
    volts = PowerMonitor.get(’volts’);


Python
~~~~~~

To use the Power Monitor Client in Python, import ``iqmotion`` and create a module that has the Power Monitor Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Power Monitor Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    volts = |variable_name|.get("power_monitor", "volts")  # returns the input voltage to module
    print(f"Voltage coming into module: {volts}")

Message Table
~~~~~~~~~~~~~

Type ID 69 | Power Monitor
    .. table:: Power Monitor
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | Sub ID | Short Name   | Access         | Data Type | Unit                  | Note                                          |
        +========+==============+================+===========+=======================+===============================================+
        | 0      | volts        | get            | float     | :math:`V`             | Input voltage to module                       |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 1      | amps         | get            | float     | :math:`A`             | Input amperage to module                      |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 2      | watts        | get            | float     | :math:`W`             | Input wattage to module                       |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 3      | joules       | get            | float     | :math:`J`             | Total energy consumed by module               |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 4      | reset_joules | set            |           |                       | Sets joules to zero                           |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 5      | filter_fs    | get            | uint32    | :math:`\text{Hz}`     | Low pass filter sample frequency              |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 6      | filter_fc    | get, set, save | uint32    | :math:`\text{Hz}`     | Low pass filter cutoff frequency              |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 7      | volts_raw    | get            | uint16    | :math:`\text{bit}`    | ADC value for the voltage measurement         |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 8      | amps_raw     | get            | uint16    | :math:`\text{bit}`    | ADC value for the current measurement         |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 9      | volts_gain   | get, set, save | float     | :math:`\frac{V}{bit}` | Gain for the ADC to voltage conversion        |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 10     | amps_gain    | get, set, save | float     | :math:`\frac{A}{bit}` | Gain for the ADC to current conversion        |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        | 11     | amps_bias    | get, set, save | float     | :math:`\text{bit}`    | Offset bias for the ADC to current conversion |
        +--------+--------------+----------------+-----------+-----------------------+-----------------------------------------------+
        
        