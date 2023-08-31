Voltage Superposition
---------------------
This client is used to set up and change settings related to Vertiq underactuated pulsing propellers.

Arduino
~~~~~~~

To use the Voltage Superposition in Arduino, ensure voltage_superposition_client.hpp is included. This
allows the creation of a VoltageSuperpositionClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the VoltageSuperpositionClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    VoltageSuperpositionClient voltageSuperposition(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int zeroAngle = 0;
        if(ser.get(voltageSuperposition.zero_angle_, zeroAngle))
        Serial.println(zeroAngle);
    }

C++
~~~

To use the Voltage Superposition client in C++, include voltage_superposition_client.hpp. This allows the
creation of an VoltageSuperposition object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the VoltageSuperpositionClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "voltage_superposition_client.hpp"

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Voltage Superposition object with obj_id 0
        VoltageSuperpositionClient voltageSuperposition(0);

        // Use the Voltage Superposition Client
        voltageSuperposition.zero_angle_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the Voltage Superposition client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a VoltageSuperpositionClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the VoltageSuperpositionClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a VoltageSuperpositionClient object with obj_id 0
    VoltageSuperposition = VoltageSuperpositionClient(’com’,com);

    % Use the VoltageSuperpositionClient object
    zeroAngle = VoltageSuperposition.get(’zero_angle’);

Python
~~~~~~

To use the Voltage Superposition Client in Python, import ``iqmotion`` and create a module that has the Arming Handler Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Voltage Superposition Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    zero_angle= |variable_name|.get("voltage_superposition", "zero_angle") 
    print(f"Zero angle: {zero_angle}")

.. _vsp_message_table:
Message Table
~~~~~~~~~~~~~

Type ID 74 | Voltage Superposition

+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name                    | Access         | Data Type | Unit                                  | Note                                                                                                                                                                                     |
+========+===============================+================+===========+=======================================+==========================================================================================================================================================================================+
| 0      | zero_angle                    | get, set, save | float     | :math:`\text{rad}`                    | This is the angular position that the module considers as its 'zero' position to start pulsing.                                                                                          |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1      | frequency                     | get, set, save | uint8     | :math:`\text{Hz}`                     | This is the number of pulses that happen per rotation.                                                                                                                                   |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2      | phase                         | get, set       | float     | :math:`\frac{1}{s} \text{[rad]}`      | This value is added to the zero angle to set where the pulse occurs.                                                                                                                     |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3      | amplitude                     | get, set       | float     | :math:`V`                             | This value represents the strength of the pulse. Setting the amplitude too high at lower speeds can cause the pulsing to overcome the inertia of the motor spinning, causing it to stop. |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4      | voltage                       | get            | float     | :math:`V`                             | This is the voltage that is currently being applied by the pulsing client.                                                                                                               |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5      | max_allowed_amplitude         | get            | float     | :math:`V`                             | This is the maximum pulsing amplitude based on settings and velocity.                                                                                                                    |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6      | velocity_cutoff               | get, set, save | float     | :math:`\frac{\text{rad}}{s}`          | This is the velocity at which pulsing is allowed. Any velocity between cutoff and \-cutoff will not pulse.                                                                               |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7      | poly_coeff_zero               | get, set, save | float     | :math:`V`                             | This is the zeroth coefficient of the cutoff polynomial.                                                                                                                                 |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8      | poly_coeff_one                | get, set, save | float     | :math:`V * (\frac{s}{\text{rad}})`    | This is the first coefficient of the cutoff polynomial.                                                                                                                                  |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 9      | poly_coeff_two                | get, set, save | float     | :math:`V * (\frac{s}{\text{rad}})^2`  | This is the second coefficient of the cutoff polynomial.                                                                                                                                 |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 10     | poly_coeff_three              | get, set, save | float     | :math:`V * (\frac{s}{\text{rad}})^3`  | This is the third coefficient of the cutoff polynomial.                                                                                                                                  |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 11     | phase_lead_time               | get, set, save | float     |                                       | This is the phase lead time setting for tuning pulsing propellers.                                                                                                                       |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 12     | phase_lead_angle              | get            | float     | :math:`rad`                           | This is the instantaneous phase lead angle determined by the phase lead time.                                                                                                            |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 13     | phase_act                     | get            | float     | :math:`rad`                           | This is the instantaneous phase being used by the pulsing client.                                                                                                                        |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 14     | amplitude_act                 | get            | float     | :math:`V`                             | This is the instantaneous amplitude being used by the pulsing client.                                                                                                                    |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 15     | sample_mechanical_zero        | set            |           |                                       | This sets the zero_angle to the current motor position.                                                                                                                                  |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 16     | propeller_torque_offset_angle | get, set, save | float     | :math:`rad`                           | This offsets where the pulse starts around the motor to allow for propeller mechanical properties.                                                                                       |
+--------+-------------------------------+----------------+-----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
