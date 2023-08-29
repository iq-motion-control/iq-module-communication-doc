Stopping Handler
----------------
A Vertiq module is considered stopped when it has been below its stopping speed continuously for some stopping time. 
Anytime the module’s velocity goes above the stopping speed, it will reset the countdown on the stopping time.

Arduino
~~~~~~~

To use the Stopping Handler in Arduino, ensure stopping_handler_client.hpp is included. This
allows the creation of a StoppingHandlerClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions.

A minimal working example for the StoppingHandlerClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    StoppingHandlerClient stoppingHandler(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        float stoppedSpeed = 0.0f;
        if(ser.get(stoppingHandler.stopped_speed_, stoppedSpeed))
        Serial.println(stoppedSpeed);
    }

C++
~~~

To use the Stopping Handler client in C++, include stopping_handler_client.hpp. This allows the
creation of a StoppingHandler object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the StoppingHandlerClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "stopping_handler_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Temperature Estimator object with obj_id 0
        StoppingHandlerClient temp_client(0);

        // Use the Temperature Estimator Client
        temp_client.stopped_speed_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the Stopping Handler client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a StoppingHandlerClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the StoppingHandlerClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a StoppingHandlerClient object with obj_id 0
    StoppingHandler = StoppingHandlerClient(’com’,com);

    % Use the StoppingHandlerClient object
    stoppedSpeed = StoppingHandler.get(’stopped_speed’);

Python
~~~~~~

To use the Stopping Handler Client in Python, import ``iqmotion`` and create a module that has the Stopping Handler Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Stopping Handler Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    stopped_speed = |variable_name|.get("stopping_handler", "stopped_speed") 
    print(f"Stopped speed: {stopped_speed}")

Message Table
~~~~~~~~~~~~~

Type ID 87 | Stopping Handler

+--------+---------------+----------------+-----------+-----------------------------+--------------------------------------------------------------------------------------------+
| Sub ID | Short Name    | Access         | Data Type | Unit                        | Note                                                                                       |
+========+===============+================+===========+=============================+============================================================================================+
| 0      | stopped_speed | get, set, save | float     | :math:`\frac{\text{rad}}{s}`| The speed at which the module needs to spin at or below to be considered stopped.          |
+--------+---------------+----------------+-----------+-----------------------------+--------------------------------------------------------------------------------------------+
| 1      | stopped_time  | get, set, save | float     | :math:`s`                   | The amount of time the module needs to spin at its stopped_speed to be considered stopped. |
+--------+---------------+----------------+-----------+-----------------------------+--------------------------------------------------------------------------------------------+
