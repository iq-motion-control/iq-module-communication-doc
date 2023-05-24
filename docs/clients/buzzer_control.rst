Buzzer Control
--------------

The Buzzer Control handles all beeps and songs played by the motor. The controls mimic standard MIDI
commands allowing simple translation from MIDI to Buzzer Control commands. The volume max parameter
controls the absolute volume across all notes, measured in volts. To play a note on the buzzer, set the
frequency by sending a ’hz’ command, set a relative volume by sending a ’volume’ command, set a note
length by sending a ’duration’ command, and finally put the controller in note mode by sending ’ctrl note’.

Arduino
~~~~~~~

To use the Buzzer Control in Arduino, ensure iq_module_communication.hpp is included. This allows the
creation of a BuzzerControlClient object. See the Message Table below for available messages. All message objects use the
Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the BuzzerControlClient is:

.. code-block:: Arduino
    
    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    BuzzerControlClient buz(0);
    
    void setup() {
        ser.begin();
    }

    void loop() {
        ser.set(buz.hz_,(uint16_t)1000);
        ser.set(buz.volume_,(uint8_t)127);
        ser.set(buz.duration_,(uint16_t)500);
        ser.set(buz.ctrl_note_);
        delay(1000);
    }

C++
~~~

To use the Buzzer Control in C++, include buzzer control client.hpp. This allows the creation of a 
BuzzerControlClient object. See the Message Table below for available messages. All message objects use the Short Name with a
trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the BuzzerControlClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: C++

    #include "generic_interface.hpp"
    #include "buzzer_control_client.hpp"
    
    void main(){
        // Make a communication interface object
        GenericInterface com;
        
        // Make a Buzzer Control object with obj_id 0
        BuzzerControlClient buz(0);
        
        // Use the Buzzer Control object
        buz.hz_.set(com, 440); // A4
        buz.volume_.set(com, 127); // Max volume
        buz.duration_.set(com, 500); // 500ms
        buz.ctrl_note_.set(com); // Note mode
        
        // Insert code for interfacing with hardware here
    
    }    

Matlab
~~~~~~

To use the Buzzer Contol in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a BuzzerControlClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the BuzzerControlClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a BuzzerControlClient object with obj_id 0
    BuzzerControl = BuzzerControlClient(’com’,com);
    
    % Use the BuzzerControlClient object
    BuzzerControl.set(’hz’,440); % A4
    BuzzerControl.set(’volume’,127); % Max volume
    BuzzerControl.set(’duration’,500); % 500ms
    BuzzerControl.set(’ctrl_note’);


Python
~~~~~~

To use the Buzzer Control Client in Python, import ``iqmotion`` and create a module that has the Buzzer Control Client within its firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Buzzer Control Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    |variable_name|.set("buzzer_control", "hz", 440)         # A4
    |variable_name|.set("buzzer_control", "volume", 127)     # Max Volume
    |variable_name|.set("buzzer_control", "duration", 1000)  # 1000ms
    |variable_name|.set("buzzer_control", "ctrl_note")       # Start the Note

Message Table
~~~~~~~~~~~~~

Type ID 61 | Buzzer Control

+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| Sub ID | Short Name | Access         | Data Type | Unit  | Note                                                              |
+========+============+================+===========+=======+===================================================================+
| 0      | ctrl_mode  | get            | int8      | enum  | no_change = -1, brake=0, coast=1, note=2, song=3                  |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 1      | ctrl_brake | set            |           |       | Shorts motor phases, slows motor down dissipating energy in motor |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 2      | ctrl_coast | set            |           |       | Disables all drive circuitry, motor passively coasts              |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 3      | ctrl_note  | set            |           |       | Must have sent a 'hz' and 'volume' first                          |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 4      | volume_max | get, set, save | float     | V     | Uses this voltage command for maximum volume                      |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 5      | hz         | get, set       | uint16    | hz    | Frequency of the note                                             |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 6      | volume     | get, set       | uint8     | 0-127 | Individual note volume as fraction of 127                         |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+
| 7      | duration   | get, set       | uint16    | ms    | Note length. Assumed max (65535 ms) if not sent.                  |
+--------+------------+----------------+-----------+-------+-------------------------------------------------------------------+

