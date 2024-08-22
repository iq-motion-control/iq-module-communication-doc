White LED
---------------

Vertiq's :ref:`White LED controller <manual_led_support>` provides configuration parameters necessary for controlling the white LEDs attached to our LED add-on board. All details 
about controlling the LED as well as interacting with the add-on board are found in :ref:`manual_led_support`.

Arduino
~~~~~~~

To use the white LED Interface in Arduino, ensure white_led.hpp is included. This
allows the creation of a WhiteLedClient object. See the Message Table below for available messages. 
All message objects use the Short Name with a trailing underscore. 
All messages use the standard Get/Set/Save functions. 

A minimal working example for the WhiteLedClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    WhiteLedClient ledInterface(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int intensity = 0;
        if(ser.get(ledInterface.intensity_, intensity))
        Serial.println(intensity);
    }

C++
~~~

To use the white LED Interface client in C++, include white_led.hpp. This allows the
creation of an WhiteLedClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the WhiteLedClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "white_led.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a white LED Interface object with obj_id 0
        WhiteLedClient ledInterface(0);

        // Use the white LED Interface Client
        ledInterface.intensity_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the white LED Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a WhiteLedClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the WhiteLedClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a WhiteLedClient object with obj_id 0
    LedInterface = WhiteLedClient(’com’,com);

    % Use the WhiteLedClient object
    intensity = LedInterface.get(’intensity’);

Python
~~~~~~

To use the white LED Interface Client in Python, import ``iqmotion`` and create a module that has the white LED Interface Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the white LED Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0, firmware="speed")
    
    intensity = |variable_name|.get("white_led", "intensity") 
    print(f"Intensity: {intensity}")

.. _white_led_message_table:

Message Table
~~~~~~~~~~~~~

Type ID 101 | White LED Interface
    .. table:: White LED Interface
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name    | Access         | Data Type | Unit                | Note                                                                                                                                                      |
        +========+===============+================+===========+=====================+===========================================================================================================================================================+
        | 0      | intensity     | get, set, save | uint8     |   :math:`\text{%}`  | The intensity of the white LEDs as a percentage [0, 100]%                                                                                                 |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 1      | strobe_active | get, set, save | uint8     |                     | Determines whether or not the white LED is static or strobing (0 for static, 1 for strobe)                                                                |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | strobe_period | get, set, save | float     |   :math:`s`         | The amount of time, in seconds, it takes to complete the strobe pattern                                                                                   |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | strobe_pattern| get, set, save | uint32    |                     | A 32-bit bitmask that represents the strobing pattern. A 1 means the LED is on for the duration of that segment :math:`\frac{\text{strobe_period}}{32}`   |
        +--------+---------------+----------------+-----------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
