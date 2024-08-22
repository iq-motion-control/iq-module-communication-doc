RGB LED
---------------

Vertiq's :ref:`RGB LED controller <manual_led_support>` provides configuration parameters necessary for controlling the RGB LED attached to our LED add-on board. All details 
about controlling the LED as well as interacting with the add-on board are found in :ref:`manual_led_support`.

Arduino
~~~~~~~

To use the RGB LED Interface in Arduino, ensure rgb_led.hpp is included. This
allows the creation of a RgbLedClient object. See the Message Table below for available messages. 
All message objects use the Short Name with a trailing underscore. 
All messages use the standard Get/Set/Save functions. 

A minimal working example for the RgbLedClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    RgbLedClient rgbInterface(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int redIntensity = 0;
        if(ser.get(rgbInterface.red_, redIntensity))
        Serial.println(redIntensity);
    }

C++
~~~

To use the RGB LED Interface client in C++, include rgb_led.hpp. This allows the
creation of an RgbLedClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the RgbLedClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "rgb_led.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make an RGB LED Interface object with obj_id 0
        RgbLedClient rgbInterface(0);

        // Use the RGB LED Interface Client
        rgbInterface.red_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the RGB LED Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a RgbLedClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the RgbLedClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a RgbLedClient object with obj_id 0
    RgbInterface = RgbLedClient(’com’,com);

    % Use the RgbLedClient object
    red = RgbInterface.get(’red’);

Python
~~~~~~

To use the RGB LED Interface Client in Python, import ``iqmotion`` and create a module that has the RGB LED Interface Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the RGB LED Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0, firmware="speed")
    
    red = |variable_name|.get("rgb_led", "red") 
    print(f"Red Intensity: {red}")

.. _rgb_led_message_table:

Message Table
~~~~~~~~~~~~~

Type ID 100 | RGB LED Interface
    .. table:: RGB LED Interface
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name    | Access         | Data Type | Unit                | Note                                                                                                                                                      |
        +========+===============+================+===========+=====================+===========================================================================================================================================================+
        | 0      | red           | get, set, save | uint8     |                     | The intensity of the red LED [0, 255]                                                                                                                     |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 1      | green         | get, set, save | uint8     |                     | The intensity of the green LED [0, 255]                                                                                                                   |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | blue          | get, set, save | uint8     |                     | The intensity of the blue LED [0, 255]                                                                                                                    |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | update_color  | get, set, save |           |                     | Updates the RGB LED with the intensities specified by red, green, and blue                                                                                |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 4      | strobe_active | get, set, save | uint8     |                     | Determines whether or not the RGB LED is static or strobing (0 for static, 1 for strobe)                                                                  |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 5      | strobe_period | get, set, save | float     |   s                 | The amount of time, in seconds, it takes to complete the strobe pattern                                                                                   |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 6      | strobe_pattern| get, set, save | uint32    |                     | A 32-bit bitmask that represents the strobing pattern. A 1 means the LED is on for the duration of that segment :math:`\frac{\text{strobe_period}}{32}`   |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
