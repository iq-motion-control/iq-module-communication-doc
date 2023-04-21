Step Direction Input
--------------------

The Step Direction Input allows the module to accept commands normally used by stepper motor drivers.
One signal line controls the direction of motor rotation, while each rising and falling edge of the other line
advances the motor’s target angle by a user programmable amount. Steps can be as small as 1/65536th of a
rotation (9.5874e-05 radians). Step sizes that are a multiple of 1/65536th of a rotation will be exact, while
non-multiples will be floored to the nearest multiple. There is virtually no upper limit on the step size. Use
negative step sizes if the direction of the motor is reversed from desired.

Arduino
~~~~~~~

To use Step Direction Input in Arduino, ensure iq_module_communication.hpp is included. This allows the
creation of a StepDirectionInputClient object. See the Message Table below for available messages. All message objects use
the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the StepDirectionInputClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    StepDirectionInputClient sdi(0);
    
    void setup() {
        ser.begin();
        ser.set(sdi.angle_step_,0.125f); // set to 8 edges per rotation
        ser.save(sdi.angle_step_);
    }

    void loop() {
    }

C++
~~~

To use Step Direction Input in C++, include step direction input.hpp. This allows the creation of a 
StepDirectionInputClient object. See the Message Table below for available messages. All message objects use the Short Name with
a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the StepDirectionInputClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "step_direction_input_client.hpp"
    
    float angle_step;
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Step Direction Input object with obj_id 0
        StepDirectionInputClient step_dir(0);

        // Use the Step Direction Input object
        step_dir.angle_step_.set(com,2.0f*PI/65536.0f); // Set the minimum step angle
        step_dir.angle_step_.save(com);
        step_dir.angle_step_.get(com);

        // Insert code for interfacing with hardware here
        // Read response
        angle_step = step_dir.angle_step_.get_reply();
    }

Matlab
~~~~~~

To use Step Direction in Matlab, all Vertiq communication code must be included in your path. This allows the
creation of a StepDirectionInputClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the StepDirectionInputClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a MultiTurnAngleControlClient object with obj_id 0
    step_dir = StepDirectionInputClient(’com’,com);
    
    % Use the StepDirectionInputClient object
    angle_step = step_dir.get(’angle_step’);
    step_dir.set(’angle_step’,2*pi/65536);
    step_dir.save(’angle_step’);

Python
~~~~~~

To use the Step Direction Input Client in Python, import ``iqmotion`` and create a module that has the Step Direction Input Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Step Direction Input Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq
    import math

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    |variable_name|.set("step_direction_input", "angle_step", (2*math.pi)/65536))  # Set min step angle
    
Message Table
~~~~~~~~~~~~~

Type ID 58 | Step Direction Input

+--------+------------+----------------+-----------+------+--------------------------+
| Sub ID | Short Name |     Access     | Data Type | Unit |           Note           |
+========+============+================+===========+======+==========================+
| 0      | angle      | get, set       | float     | rad  | Current commanded angle  |
+--------+------------+----------------+-----------+------+--------------------------+
| 1      | angle_step | get, set, save | float     | rad  | Angle step per step edge |
+--------+------------+----------------+-----------+------+--------------------------+
