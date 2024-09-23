Arming Handler
--------------
Vertiq Advanced Speed modules can support an :ref:`Advanced Arming <manual_advanced_arming>` feature, 
allowing the user to control the armed state of the module with throttle commands or manually 
and to configure specific behaviors to occur at armed state transitions. 
Modules will not react to throttle messages until they have been armed, providing improved safety. 
The configurable behaviors on armed state transitions allow users to easily integrate advanced behaviors into their setup 
just by controlling the throttle messages they send, simplifying flight controller integration. 

Arduino
~~~~~~~

To use the Arming Handler in Arduino, ensure arming_handler_client.hpp is included. This
allows the creation of a ArmingHandlerClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the ArmingHandlerClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    ArmingHandlerClient armingHandler(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int alwaysArmed = 0;
        if(ser.get(armingHandler.always_armed, alwaysArmed))
        Serial.println(alwaysArmed);
    }

C++
~~~

To use the Arming Handler client in C++, include arming_handler_client.hpp. This allows the
creation of an ArmingHandler object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the ArmingHandlerClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "arming_handler_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Arming Handler object with obj_id 0
        ArmingHandlerClient armingHandler(0);

        // Use the Arming Handler Client
        armingHandler.always_armed_.get(com);

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the Arming Handler client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a ArmingHandlerClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the ArmingHandlerClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a ArmingHandlerClient object with obj_id 0
    ArmingHandler = ArmingHandlerClient(’com’,com);
    % Use the ArmingHandlerClient object
    alwaysArmed = ArmingHandler.get(’always_armed’);

Python
~~~~~~

To use the Arming Handler Client in Python, import ``iqmotion`` and create a module that has the Arming Handler Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Arming Handler Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    always_armed_status = |variable_name|.get("arming_handler", "always_armed") 
    print(f"Always armed status: {always_armed_status}")

Message Table
~~~~~~~~~~~~~

Type ID 86 | Arming Handler
    .. table:: Arming Handler
        :widths: 8 18 15 10 5 100
        :class: tight-table

        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name                          | Access          | Data Type | Unit                     | Note                                                                                                                                                                                                                                                 |
        +========+=====================================+=================+===========+==========================+======================================================================================================================================================================================================================================================+
        | 1      | always_armed                        | get, set, save  | uint8     |                          | If True, handler will automatically arm when the first throttle message is received.                                                                                                                                                                 |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | arm_on_throttle                     | get, set, save  | uint8     |                          | If True, throttle will be used to arm.                                                                                                                                                                                                               |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | arm_throttle_upper_limit            | get, set, save  | float     | :math:`\text{PU}`        | Upper limit for throttle to arm.                                                                                                                                                                                                                     |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 4      | arm_throttle_lower_limit            | get, set, save  | float     | :math:`\text{PU}`        | Lower limit for throttle to arm.                                                                                                                                                                                                                     |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 5      | disarm_on_throttle                  | get, set, save  | uint8     |                          | If True, throttle will be used to disarm.                                                                                                                                                                                                            |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 6      | disarm_throttle_upper_limit         | get, set, save  | float     | :math:`\text{PU}`        | Upper limit for throttle to disarm.                                                                                                                                                                                                                  |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 7      | disarm_throttle_lower_limit         | get, set, save  | float     | :math:`\text{PU}`        | Lower limit for throttle to disarm.                                                                                                                                                                                                                  |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 8      | consecutive_arming_throttles_to_arm | get, set, save  | uint32    | :math:`\text{throttles}` | Number of consecutive throttles before arming.                                                                                                                                                                                                       |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        || 9     || disarm_behavior                    || get, set, save || uint8    ||                         || The state that determines how the module will try to come to a stop and what that final drive mode will be. See row below for each state:                                                                                                           |
        ||       ||                                    ||                ||          ||                         || 0 = Coast: The module will coast when it begins disarming by spinning freely and letting drag + friction slow it down. After the song, its final state will remain coasting.                                                                        |
        ||       ||                                    ||                ||          ||                         || 1 = 0V to Coast: The Module will drive itself to 0V when it begins disarming, actively trying to come to a rapid stop. After the song, its final state will be to coast.                                                                            |
        ||       ||                                    ||                ||          ||                         || 2 = 0V to Brake: The Module will drive itself to 0V when it begins disarming, actively trying to come to a rapid stop. After the song, its final state will be to brake.                                                                            |
        ||       ||                                    ||                ||          ||                         || 3 = Stow: The module will trigger a move to the stow postiion when it begins disarming. After the song, its final state will be determined by whatever the stow postiion feature is configured to do after completing a stow.                       |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        || 10    || disarm_song_option                 || get, set, save || uint8    ||                         || The state that determines if and how many times the module will play its disarm song after coming to a stop. See row below for each state:                                                                                                          |
        ||       ||                                    ||                ||          ||                         || 0 = Never Play: The disarm song is skipped entirely, and the module will transition directly to its final state after stopping.                                                                                                                     |
        ||       ||                                    ||                ||          ||                         || 1 = Play Once: The disarm song will play once, and then the module will transition to its final state.                                                                                                                                              |
        ||       ||                                    ||                ||          ||                         || 2 = Play Continuously: The disarm song will play continuously until the module is armed again or commanded to sping without arming through IQUART. The song will never finish, so the module will never transtion to its final state.               |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        || 11    || manual_arming_throttle_source      || get, set, save || uint8    ||                         || The :ref:`throttle source <throttle_sources>` used as its armed throttle source for manual arming. The default manual throttle source is Unknown. In order to use manual arming, a throttle source must be set first. See row below for each state: |
        ||       ||                                    ||                ||          ||                         || 0 = Unknown: Default configuration. There will be no throttle source if you manually arm, so don't leave this as Unknown if you are manuall arming.                                                                                                 |
        ||       ||                                    ||                ||          ||                         || 1 = :ref:`Hobby <hobby_protocol>`                                                                                                                                                                                                                   |
        ||       ||                                    ||                ||          ||                         || 2 = :ref:`DroneCAN <dronecan_protocol>`                                                                                                                                                                                                             |
        ||       ||                                    ||                ||          ||                         || 3 = :ref:`IQUART <uart_messaging>`                                                                                                                                                                                                                  |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 12     | motor_armed                         | get, set        | uint8     |                          | Returns True if motor is armed.                                                                                                                                                                                                                      |
        +--------+-------------------------------------+-----------------+-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        