Multi Turn Angle Control
------------------------

The Multi-turn Angle Controller is a non-wrapping, PID, position controller. It is capable of storing angular
to linear transmission information, mimicking a linear position controller. It also features a minimum jerk
trajectory generator. The controller has a 32 trajectory buffer so that transitions between trajectories are
seamless.

Arduino
~~~~~~~

To use Multi-turn Angle Controller in Arduino, ensure iq_module_communication.hpp is included. This
allows the creation of a MultiTurnAngleControlClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. 

A minimal working example for the MultiTurnAngleControlClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    MultiTurnAngleControlClient mult(0);
    
    void setup() {
        ser.begin();
    }

    void loop() {
        ser.set(mult.trajectory_angular_displacement_,3.14f);
        ser.set(mult.trajectory_duration_,0.5f);
        ser.set(mult.trajectory_angular_displacement_,0.0f);
        ser.set(mult.trajectory_duration_,0.5f);
        delay(1000);
    }

C++
~~~

To use Multi-turn Angle Controller in C++, include multi turn angle control client.hpp. This allows the
creation of a MultiTurnAngleControlClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. 

A minimal working example for the MultiTurnAngleControlClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "multi_turn_angle_control_client.hpp"
    
    float angle;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Multi-turn Angle Controller object with obj_id 0
        MultiTurnAngleControlClient angle_ctrl(0);

        // Use the Multi-turn Angle Controller object
        angle_ctrl.obs_angular_displacement_.get(com);
        angle_ctrl.ctrl_angle_.set(com,0.0f);

        // Insert code for interfacing with hardware here
        // Read response
        angle = angle_ctrl.ctrl_angle_.get_reply();
    
    }

Matlab
~~~~~~

To use Multi-turn Angle Controller in Matlab, all Vertiq communication code must be included in your path.
This allows the creation of a MultiTurnAngleControlClient object. See the Message Table below for available messages. All
message strings use the Short Names. 

A minimal working example for the MultiTurnAngleControlClient is:
    
.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a MultiTurnAngleControlClient object with obj_id 0
    angle_ctrl = MultiTurnAngleControlClient(’com’,com);
    
    % Use the MultiTurnAngleControlClient object
    velocity_filtered = angle_ctrl.get(’obs_angular_displacement’);
    angle_ctrl.set(’ctrl_angle’,0);

Python
~~~~~~

To use the Multi-Turn Angle Control Client in Python, include ``iqmotion`` and create a module that has the Multi-Turn Angle Control Client within it's firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 

A minimal working example for the Multi-Turn Angle Control Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq
    import math

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)  # Servo Firmware uses this client
    
    # Set the trajectory for the motor to complete 1 full rotation
    |variable_name|.set("multi_turn_angle_control", "trajectory_angular_displacement", 2*math.pi)

    # Sets trajectory duration for 2 seconds
    |variable_name|.set("multi_turn_angle_control", "trajectory_duration", 2)  


Message Table
~~~~~~~~~~~~~

Type ID 59 | Multi-turn Angle Controller

+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| Sub ID | Short Name                      | Access         | Data Type | Unit              | Note                                                                                             |
+========+=================================+================+===========+===================+==================================================================================================+
| 0      | ctrl_mode                       | get            | int8      | enum              | no_change = -1, brake=0, coast=1, pwm=2, volts=3, velocity=4, angle=5, trajectory=6              |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 1      | ctrl_brake                      | set            |           |                   | Shorts motor phases, slows motor down dissipating energy in motor                                |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 2      | ctrl_coast                      | set            |           |                   | Disables all drive circuitry, motor passively coasts                                             |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 3      | ctrl_angle                      | get, set       | float     | rad               | Angular location command                                                                         |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 4      | ctrl_velocity                   | get, set       | float     | rad/s             | Angular velocity command                                                                         |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 5      | angle_Kp                        | get, set, save | float     | V/rad             | Proportional gain                                                                                |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 6      | angle_Ki                        | get, set, save | float     | V/(rad*s)         | Integral gain                                                                                    |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 7      | angle_Kd                        | get, set, save | float     | V/(rad/s)         | Derivative gain                                                                                  |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 8      | timeout                         | get, set, save | float     | s                 | The controller must receive a message within this time otherwise it is set to coast mode         |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 9      | ctrl_pwm                        | get, set       | float     | pwm               | Spins motor with this throttle [-1, 1]                                                           |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 10     | ctrl_volts                      | get, set       | float     | V                 | Spins motor with this voltage                                                                    |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 11     | obs_angular_displacement        | get, set       | float     | rad               | Observed angular location                                                                        |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 12     | obs_angular_velocity            | get            | float     | rad/s             | Observed angular velocity                                                                        |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 13     | meter_per_rad                   | get, set, save | float     | m/rad             | Transmission between angular and linear motion                                                   |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 14     | ctrl_linear_displacement        | get, set,      | float     | m                 | Linear equivalent to ctrl_angle                                                                  |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 15     | ctrl_linear_velocity            | get, set,      | float     | m/s               | Linear equivalent to ctrl_velocity                                                               |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 16     | obs_linear_displacement         | get, set,      | float     | m                 | Observed linear location                                                                         |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 17     | obs_linear_velocity             | get            | float     | m/s               | Observed linear velocity                                                                         |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 18     | angular_speed_max               | get, set, save | float     | rad/s             | The controller will never attempt to exceed this speed                                           |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 19     | trajectory_angular_displacement | get, set       | float     | rad               | Final absolute displacement of trajectory.                                                       |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 20     | trajectory_angular_velocity     | get, set       | float     | rad/s             | Final velocity of the trajectory. Defaults to 0.                                                 |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 21     | trajectory_angular_acceleration | get, set       | float     | .. math:: rad/s^2 | Final acceleration of the trajectory. Defaults to 0.                                             |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 22     | trajectory_duration             | set            | float     | s                 | Duration of trajectory. Trajectory is executed or queued once this is sent.                      |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 23     | trajectory_linear_displacement  | get, set       | float     | m                 | Final absolute displacement of trajectory.                                                       |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 24     | trajectory_linear_velocity      | get, set       | float     | m/s               | Final velocity of the trajectory. Defaults to 0.                                                 |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 25     | trajectory_linear_acceleration  | get, set       | float     | .. math:: m/s^2   | Final acceleration of the trajectory. Defaults to 0.                                             |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 26     | trajectory_average_speed        | get, set       | float     | .. math:: m/s^2   | Average speed of a trajectory. Trajectory is executed or queued once this is sent. Must be $>0$. |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 27     | trajectory_queue_mode           | get, set, save | int8      | .. math:: m/s^2   | append=0, overwrite=1                                                                            |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 29     | ff                              | get, set       | uint32    |                   | Feed forward term                                                                                |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 30     | sample_zero_angle               | set            |           |                   | Sets the module's current postiion as the zero angle.                                            |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
| 31     | zero_angle                      | get, set, save | float     | rad               | The anglular position that the module considers as its 'zero' position.                          |
+--------+---------------------------------+----------------+-----------+-------------------+--------------------------------------------------------------------------------------------------+
