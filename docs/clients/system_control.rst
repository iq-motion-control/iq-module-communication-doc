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
of a SystemControlClient object. See the Message Table below for available messages. All message objects use the Short
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
of a SystemControlClient object. See the Message Table below for available messages. All message objects use the Short Name with a
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
the creation of a SystemControlClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the SystemControlClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a System Control object with obj_id 0
    SystemControl = SystemControlClient(’com’,com);
    
    % Use the System Control object
    time = SystemControl.get(’time’);

Python
~~~~~~

To use the System Control Client in Python, import ``iqmotion`` and create a module that has the System Control Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the System Control Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    FW = |variable_name|.get("system_control", "firmware_version")  # Firmware Version Number
    print(f"Firmware: {FW}")

.. _sc_message_table:

Message Table
~~~~~~~~~~~~~

Type ID 5 | System Control
    .. table:: System Control
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | Sub ID |     Short Name      |     Access     | Data Type | Unit                |                               Note                                |
        +========+=====================+================+===========+=====================+===================================================================+
        | 0      | reboot_program      | set            |           |                     | Reboots the motor controller with saved values                    |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 1      | reboot_boot_loader  | set            |           |                     | Reboots into the boot loader                                      |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 2      | dev_id              | get            | uint16    |                     |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 3      | rev_id              | get            | uint16    |                     |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 4      | uid1                | get            | uint32    |                     |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 5      | uid2                | get            | uint32    |                     |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 6      | uid3                | get            | uint32    |                     |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 7      | mem_size            | get            | uint16    | :math:`\text{Kb}`   |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 8      | build_year          | get            | uint16    | :math:`\text{year}` |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 9      | build_month         | get            | uint8     | :math:`\text{mon}`  |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 10     | build_day           | get            | uint8     | :math:`\text{day}`  |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 11     | build_hour          | get            | uint8     | :math:`\text{hour}` |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 12     | build_minute        | get            | uint8     | :math:`\text{min}`  |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 13     | build_second        | get            | uint8     | :math:`s`           |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 14     | module_id           | get, set, save | uint8     | :math:`\text{ID}`   | The ID used for all obj_id on this module                         |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 15     | time                | get, set       | float     | :math:`s`           | Internal clock time. If unchanged through software this is uptime |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 16     | firmware_version    | get            | uint32    | :math:`\text{ver}`  |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 17     | hardware_version    | get, set, save | uint32    | :math:`\text{ver}`  |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 18     | electronics_version | get, set, save | uint32    | :math:`\text{ver}`  |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 19     | firmware_valid      | get            | uint8     | :math:`\text{bool}` |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 20     | application_present | get            | uint8     | :math:`\text{bool}` |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 21     | bootloader_version  | get            | uint32    | :math:`\text{ver}`  | This is the version of the bootloader.                            |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 22     | upgrade_version     | get            | uint32    | :math:`\text{ver}`  | This is the version of the upgrader.                              |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 23     | system_clock        | get            | uint32    | :math:`\text{Hz}`   | This the frequency of the microcontroller.                        |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 24     | control_flags       | get            | uint32    | :math:`\text{enum}` |                                                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        | 25     | pcb_version         | get            | uint32    | :math:`\text{ver}`  | This is the version of the PCB.                                   |
        +--------+---------------------+----------------+-----------+---------------------+-------------------------------------------------------------------+
        
