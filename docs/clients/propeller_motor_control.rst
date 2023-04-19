Propeller Motor Control
-----------------------

The Propeller Motor Controller is an open and closed loop controller designed to drive propeller loads. If
given thrust coefficients, this controller can be commanded in units of thrust, seamlessly accepting values
from flight controllers in their native units. An added benefit is the decoupling of flight controller gains
from motor choice, propeller choice, battery level, and more. Thrust commands are fed into a PID velocity
controller with a second order polynomial feed forward. This sits on top of a voltage controller, which
compensates for varying input voltages. Finally, the core is a raw PWM controller. Any of the above
controllers can be used by the user.

.. note:: 

    The Propeller Motor Control will timeout and beep 3 times if it hasn't received a command before the timeout is reached.
    You can extend the timeout by changing the timeout client entry

Arduino
~~~~~~~

To use Propeller Motor Controller in Arduino, ensure iq_module_communication.hpp is included. This allows
the creation of a PropellerMotorControlClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PropellerMotorControlMessaging is:

.. code-block:: Arduino

    #include <iq_module_communicaiton.hpp>

    IqSerial ser(Serial2);
    PropellerMotorControlClient prop(0);

    void setup() {
        ser.begin();
    }

    void loop() {
        ser.set(prop.ctrl_velocity_,50.0f);
    }

C++
~~~

To use Propeller Motor Controller in C++, include propeller motor control client.hpp. This allows the
creation of a PropellerMotorControlClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PropellerMotorControlClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "propeller_motor_control_client.hpp"
    
    float velocity = 100.0f; // rad/s
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Propeller Motor Controller object with obj_id 0
        PropellerMotorControlClient prop(0);

        // Use the Propeller Motor Controller object
        prop.ctrl_velocity_.set(com, velocity);

        // Insert code for interfacing with hardware here

    }

Matlab
~~~~~~

To use Propeller Motor Controller in Matlab, all Vertiq communication code must be included in your path.
This allows the creation of a PropellerMotorControlClient object. See the Message Table below for available messages. All
message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PropellerMotorControlClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a PropellerMotorControlClient object with obj_id 0
    prop = PropellerMotorControlClient(’com’,com);

    % Use the PropellerMotorControlClient object
    prop.set(’ctrl_velocity’,100.0);


Python
~~~~~~

To use the Propeller Motor Control Client in Python, include ``iqmotion`` and create a module that has the Propeller Motor Control Client within it's firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Propeller Motor Control Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0|module_firmware|)
    
    |variable_name|.set("propeller_motor_control", "ctrl_velocity", 5)  # Supplies 5V to motor


Message Table
~~~~~~~~~~~~~

Type ID 52 | Propeller Motor Controller

+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| Sub ID |    Short Name    |     Access     | Data Type |    Unit    |                                   .. centered:: Note                                    |
+========+==================+================+===========+============+=========================================================================================+
| 0      | ctrl_mode        | get            | int8      | enum       | -1 = no change, 0 = brake, 1 = coast, 2 = pwm, 3 = volts, 4 = velocity, 5 = thrust      |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 1      | ctrl_brake       | set            |           |            | Shorts motor leads, slows motor down dissipating energy in motor                        |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 2      | ctrl_coast       | set            |           |            | Disables all drive circuitry                                                            |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 3      | ctrl_pwm         | get, set       | float     | PWM        | [-1, 1] fraction of input voltage                                                       |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 4      | ctrl_volts       | get, set       | float     | V          | [-supply, supply] Voltage to apply to motor                                             |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 5      | ctrl_velocity    | get, set       | float     | rad/s      | Angular velocity command                                                                |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 6      | ctrl_thrust      | get, set       | float     | N          | Thrust command (requires kt values)                                                     |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 7      | velocity_kp      | get, set, save | float     | V/(rad/s)  | Proportional gain                                                                       |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 8      | velocity_ki      | get, set, save | float     | V/(rad)    | Integral gain                                                                           |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 9      | velocity_kd      | get, set, save | float     | V/(rad/s2) | Derivative gain                                                                         |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 10     | velocity_ff0     | get, set, save | float     | V          | Feed forward 0th order term                                                             |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 11     | velocity_ff1     | get, set, save | float     | V/(rad/s)  | Feed forward 1st order term                                                             |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 12     | velocity_ff2     | get, set, save | float     | V/(rad/s)2 | Feed forward 2nd order term                                                             |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 13     | propeller_kt_pos | get, set, save | float     | N/(rad/s)2 | T = ktω2thrust constant in positive direction                                           |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 14     | propeller_kt_neg | get, set, save | float     | N/(rad/s)2 | T = ktω2 thrust constant in negative direction                                          |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 15     | timeout          | get, set, save | float     | s          | The controller must receive a message within thistime otherwise it is set to coast mode |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+
| 16     | input_filter_fc  | get, set, save | uint32    | Hz         | Low pass cutoff frequency for input commands                                            |
+--------+------------------+----------------+-----------+------------+-----------------------------------------------------------------------------------------+