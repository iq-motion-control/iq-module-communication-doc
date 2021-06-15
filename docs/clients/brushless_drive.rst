Brushless Drive
---------------

Brushless Drive is the low level driver of the motor’s phase voltage.

Arduino
~~~~~~~

To use Brushless Drive in Arduino, ensure iq module communication.hpp is included. This allows the creation of a BrushlessDriveClient object. 
See Table 3 for available messages. All message objects use the Short
Name with a trailing underscore. All messages use the standard ``Get/Set/Save`` functions.

A minimal working example for the BrushlessDriveClient is:

.. code-block:: Arduino

    #include <iq_module_communicaiton.hpp>
    
    IqSerial ser(Serial2);
    BrushlessDriveClient mot(0);
    
    void setup() {
        ser.begin();
    }
    
    void loop() {
        ser.set(mot.drive_spin_volts_,0.1f);
    }

C++ 
~~~

To use Brushless Drive in C++, include brushless drive client.hpp. This allows the creation of a ``BrushlessDriveClient`` object. 
See Table 3 for available messages. All message objects use the Short Name with a trailing underscore. All messages use the standard ``Get/Set/Save`` functions.

A minimal working example for the BrushlessDriveClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++
    
    #include "generic_interface.hpp"
    #include "propeller_motor_control_client.hpp"

    float voltage = 3.0f; // volts
    
    int main(){

    // Make a communication interface object
    // This is what creates and parses packets
    GenericInterface com;

    // Make a Temperature Estimator Client object with obj_id 0
    BrushlessDriveClient brushless(0);

    // Drives the motor at 3 Volts
    brushless.drive_spin_volts_.set(com, voltage);

    // Insert code for interfacing with hardware here  

    }


Matlab
~~~~~~

To use Brushless Drive Controller in Matlab, all IQ communication code must be included in your path.
This allows the creation of a BrushlessDriveClient object. See Table 2 for available messages. All
message strings use the Short Names. All messages use the standard ``Get/Set/Save`` functions.

A minimal working example for the BrushlessDriveClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a BrushlessDriveClient object with obj_id 0
    drive = BrushlessDriveClient(’com’,com);

    % Use the BrushlessDriveClient object
    drive.set(’drive_spin_volts’,3.0);

Python
~~~~~~

To use the Brushless Drive Client in Python, include ``iqmotion`` and create a module that has the Brushless Drive Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the BrushlessDriveClient is:

.. code-block:: Python
    :substitutions:

    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0|module_firmware|)
    
    |variable_name|.set("brushless_drive", "drive_spin_volts", 5) # Spins motor at 5 volts

Message Table
~~~~~~~~~~~~~

Type ID 50 | Brushless Drive

+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| Sub ID |     Short Name      |     Access     | Data Type |   Unit   |                                                                 Note                                                                  | Note |
+========+=====================+================+===========+==========+=======================================================================================================================================+======+
| 0      | drive_mode          | get            | uint8     |          | 0 = phase_pwm, 1 = phase_volts, 2 = spin_pwm, 3 = spin_volts, 4 = brake, 5 = coast                                                    |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 1      | drive_phase_pwm     | set            | float     | pwm      | Open loop (gimbal) mode with this trottle [-1, 1].  Use with phase_angle.                                                             |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 2      | drive_phase_volts   | set            | float     | V        | Open loop (gimbal) mode with this voltage.  Use with phase_angle.                                                                     |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 3      | drive_spin_pwm      | set            | float     | pwm      | Spins motor with this throttle [-1, 1]                                                                                                |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 4      | drive_spin_volts    | set            | float     | V        | Spins motor with this voltage                                                                                                         |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 5      | drive_brake         | set            |           |          | Shorts motor phases, slows motor down dissipating energy in motor                                                                     |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 6      | drive_coast         | set            |           |          | Disables all drive circuitry, motor passively coasts                                                                                  |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 7      | drive_angle_offset  | get            | float     | rad      | Analogous to motor timing. This is internally computed by the motor.                                                                  |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 8      | drive_pwm           | get            | float     | pwm      | The applied pwm after all computation and limiting [-1, 1]                                                                            |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 9      | drive_volts         | get            | float     | V        | The applied pwm after all computation and limiting                                                                                    |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 10     | mech_lead_angle     | get            | float     | rad      |                                                                                                                                       |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 11     | obs_supply_volts    | get            | float     | V        | Observed supply voltage                                                                                                               |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 12     | obs_angle           | get            | float     | rad      | Observed motor angle                                                                                                                  |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 13     | obs_velocity        | get            | float     | rad/s    | Observed motor velocity                                                                                                               |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 14     | motor_pole_pairs    | get, set, save | uint16    |          | Number of motor pole pairs (magnets/2)                                                                                                |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 15     | motor_emf_shape     | get, set, save | uint8     |          |                                                                                                                                       |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 16     | permute_wires       | get, set, save | uint8     | bool     |                                                                                                                                       |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 17     | calibration_angle   | get, set, save | float     | rad      |                                                                                                                                       |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 18     | lead_time           | get, set, save | float     | s        |                                                                                                                                       |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 19     | commutation_hz      | get, set, save | uint32    | Hz       | Frequency of commutation.  Higher frequencies run faster and more efficient, but may not give the controller enough computation time. |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 20     | phase_angle         | get, set       | float     | rad      | Angle used for open loop (gimbal) mode. Use with drive_phase_pwm or drive_phase_volts                                                 |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 32     | motor_Kv            | get, set, save | float     | RPM/V    | Motor's voltage constant                                                                                                              |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 33     | motor_R_ohm         | get, set, save | float     | ohm      | Motor's resistance                                                                                                                    |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 34     | motor_I_max         | get, set, save | float     | A        | Max allowable motor current                                                                                                           |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 35     | volts_limit         | get, set, save | float     | V        | Max regen voltage                                                                                                                     |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 36     | est_motor_amps      | get            | float     | A        | Estimated motor amps                                                                                                                  |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 37     | est_motor_torque    | get            | float     | Nm       | Estimated motor torque                                                                                                                |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 38     | motor_redline_start | get, set, save | float     | rad/s    | Speed at which motor begins to derate                                                                                                 |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 39     | motor_redline_end   | get, set, save | float     | rad/s    | Speed at which the motor is fully derated                                                                                             |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 40     | motor_l             | get, set, save | float     | H        | Cross inductance                                                                                                                      |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 41     | derate              | get            | int32     | PU fix16 | Amount of derating.  No derate = 65536, full derate = 0                                                                               |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 42     | i_soft_start        | get, set, save | float     | A        | Current at which motor begins to derate                                                                                               |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
| 43     | i_soft_end          | get, set, save | float     | A        | Current at which the motor is fully derated                                                                                           |      |
+--------+---------------------+----------------+-----------+----------+---------------------------------------------------------------------------------------------------------------------------------------+------+
