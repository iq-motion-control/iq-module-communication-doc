System Control
--------------

System Control allows the user to perform low level tasks on the motor controller’s microcontroller and gather
basic information. The motor’s uptime can be read and set, allowing for flexible timing and synchronizing.
System Control also has a Module ID parameter, which allows motors to be bussed on a single serial line yet
addressed uniquely. System Control is unique since its ID is always 0 even when the Module ID has been
changed.

Arduino
~~~~~~~

To use System Control in Arduino, ensure iq_module_communication.hpp is included. This allows the creation
of a SystemControlClient object. See Table 12 for available messages. All message objects use the Short
Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the SystemControlClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    SystemControlClient sys(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }

    void loop() {
        float sys_time = 0.0f;
        if(ser.get(sys.time_,sys_time))
            Serial.println(sys_time);
    }

C++
~~~

To use System Control in C++, include system control client.hpp. This allows the creation 
of a SystemControlClient object. See Table 12 for available messages. All message objects use the Short Name with a
trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the SystemControlClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "system_control_client.hpp"
    
    float time;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a System Control object with obj_id 0
        // System Control objects are always obj_id 0
        SystemControlClient system_control(0);

        // Use the System Control object
        system_control.time_.get(com);

        // Insert code for interfacing with hardware here
        // time = system_control.time_.get_reply();
    }

Matlab
~~~~~~

To use System Control in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a SystemControlClient object. See Table 12 for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the SystemControlClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a System Control object with obj_id 0
    % System Control objects are always obj_id 0
    system_control = SystemControlClient(’com’,com);
    
    % Use the System Control object
    time = system_control.get(’time’);

Python
~~~~~~

To use the System Control Client in Python, include ``iqmotion`` and create a module that has the System Control Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the System Control Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0|module_firmware|)
    
    FW = |variable_name|.get("system_control", "firmware_version")  # Firmware Version Number
    print(f"Firmware: {FW}")

Message Table
~~~~~~~~~~~~~

Type ID 5 | System Control

+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| Sub ID |     Short Name      |     Access     | Data Type | Unit |                               Note                                |
+========+=====================+================+===========+======+===================================================================+
| 0      | reboot_program      | set            |           |      | Reboots the motor controller with saved values                    |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 1      | reboot_boot_loader  | set            |           |      | Reboots into the boot loader                                      |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 2      | dev_id              | get            | uint16    |      |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 3      | rev_id              | get            | uint16    |      |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 4      | uid1                | get            | uint32    |      |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 5      | uid2                | get            | uint32    |      |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 6      | uid3                | get            | uint32    |      |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 7      | mem_size            | get            | uint16    | Kb   |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 8      | build_year          | get            | uint16    | year |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 9      | build_month         | get            | uint8     | mon  |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 10     | build_day           | get            | uint8     | day  |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 11     | build_hour          | get            | uint8     | hour |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 12     | build_minute        | get            | uint8     | min  |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 13     | build_second        | get            | uint8     | s    |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 14     | module_id           | get, set, save | uint8     | id   | The ID used for all obj_id on this module                         |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 15     | time                | get, set       | float     | s    | Internal clock time. If unchanged through software this is uptime |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 16     | firmware_version    | get            | uint32    | ver  |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 17     | hardware_version    | get, set, save | uint32    | ver  |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 18     | electronics_version | get, set, save | uint32    | ver  |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+
| 19     | firmware_valid      | get            | uint8     | bool |                                                                   |
+--------+---------------------+----------------+-----------+------+-------------------------------------------------------------------+

