
Stow User Interface
-------------------
The :ref:`Stow Position <manual_stow_position>` feature allows a Vertiq module to return to a configurable position on a transition from armed to disarmed,
on timeouts, or when given an explicit command to stow. 
This can be useful for holding propellers in an aerodynamic position, or preparing vehicles for storage.
Users can control what this position is, when the module should attempt to move into the stow position, 
how aggressively it moves to the stow position, and the module’s behavior once it reaches the stow position.

Arduino
~~~~~~~

To use the Stow User Interface in Arduino, ensure stow_user_interface_client.hpp is included. This
allows the creation of a StowUserInterfaceClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions.

A minimal working example for the StowUserInterfaceClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    StowUserInterfaceClient stowUserInterface(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        float zeroAngle = 0.0f;
        if(ser.get(stowUserInterface.zero_angle_, zeroAngle))
        Serial.println(zeroAngle);
    }

C++
~~~

To use the Stow User Interface client in C++, include stow_user_interface_client.hpp. This allows the
creation of a StowUserInterface object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the StowUserInterfaceClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "stow_user_interface_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Temperature Estimator object with obj_id 0
        StowUserInterfaceClient stowUserInterface(0);

        // Use the Temperature Estimator Client
        stowUserInterface.zero_angle_.get(com)

        // [Insert code for interfacing with hardware here]
    }

Matlab
~~~~~~

To use the Stow User Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a StowUserInterfaceClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the StowUserInterfaceClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a StowUserInterfaceClient object with obj_id 0
    stowUserInterface = StowUserInterfaceClient(’com’,com);
    % Use the StowUserInterfaceClient object
    zeroAngle = stowUserInterface.get(’zero_angle’);

Python
~~~~~~

To use the Stow User Interface Client in Python, import ``iqmotion`` and create a module that has the Stow User Interface Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Stow User Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    zero_angle = |variable_name|.get("stow_user_interface", "zero_angle") 
    print(f"Zero angle: {zero_angle}")

Message Table
~~~~~~~~~~~~~

Type ID 85 | Stow User Interface

+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name          | Access          | Data Type | Unit              | Note                                                                                                                                                                                            |
+========+=====================+=================+===========+===================+=================================================================================================================================================================================================+
| 0      | zero_angle          | get, set, save  | float     | rad               | The anglular position that the module considers as its 'zero' position.                                                                                                                         |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1      | target_angle        | get, set, save  | float     | rad               | The angular postion of the stow postition with reference to the zero angle.                                                                                                                     |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2      | target_acceleration | get, set, save  | float     | .. math:: rad/s^2 | The maximum acceleration allowed when moving to the stow position.                                                                                                                              |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3      | sample_zero         | set             |           |                   | Sets the module's current postiion as the zero angle.                                                                                                                                           |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4      | stow                | set             |           |                   | Setting this triggers the module to stow.                                                                                                                                                       |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5      | stow_kp             | get, set, save  | float     | V/rad             | The proportional gain to use in the closed loop position controller moving the module to the stow position. A higher gain can lead to a more accurate position, but can also cause oscillation. |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6      | stow_ki             | get, set, save  | float     | V/(rad*s)         | The integral gain to use in the closed loop position controller moving the module to the stow position.                                                                                         |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7      | stow_kd             | get, set, save  | float     | V/(rad*s)         | The differential gain to use in the closed loop position controller moving the module to the stow position.                                                                                     |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8      | hold_stow           | get, set, save  | uint8     |                   | If True, module will actively hold the stole angle once it is reached. If False, the module will coast.                                                                                         |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|| 9     || stow_status        || get, set, save || uint8    ||                  || The current state of stowing. See row below for each state:                                                                                                                                    |
||       ||                    ||                ||          ||                  || 0 = Idle: No stowing happening and module is ready for new commands.                                                                                                                           |
||       ||                    ||                ||          ||                  || 1 = In Progress: A move to the stow position is in progress but the module has not reached its stow postition yet.                                                                             |
||       ||                    ||                ||          ||                  || 2 = In Progress: A move to the stow position is in progress but the module has not reached its stow postition yet.                                                                             |
||       ||                    ||                ||          ||                  || 3 = Holding: The module is actively holding its stow postion.                                                                                                                                  |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|| 10    || stow_result        || get, set, save || uint8    ||                  || The result of how the previous stow attempt ended. See row below for each possible result:                                                                                                     |
||       ||                    ||                ||          ||                  || 0 = No Result: No previous stow attempts. Default result after module reboot.                                                                                                                  |
||       ||                    ||                ||          ||                  || 1 = Completed: The previous stow attempt successfully made it to its stow postion without issue.                                                                                               |
||       ||                    ||                ||          ||                  || 2 = Interrupted: The previous stow attempt was interrupted before compeleting.                                                                                                                 |
||       ||                    ||                ||          ||                  || 3 = Error: An unexpected error occurred during the previous stow attempt.                                                                                                                      |
+--------+---------------------+-----------------+-----------+-------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
