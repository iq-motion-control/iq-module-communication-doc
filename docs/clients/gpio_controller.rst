GPIO Controller
---------------

**TODO**

Arduino
~~~~~~~

To use the GPIO Controller in Arduino, ensure gpio_controller_client.hpp is included. This
allows the creation of a GpioControllerClient object. See the Message Table below for available messages. 
All message objects use the Short Name with a trailing underscore. 
All messages use the standard Get/Set/Save functions. 

A minimal working example for the GpioControllerClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    GpioControllerClient gpioController(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int modeRegister = 0;
        if(ser.get(gpioController.mode_register_, modeRegister))
        Serial.println(modeRegister);
    }

C++
~~~

To use the GPIO Controller client in C++, include gpio_controller_client.hpp. This allows the
creation of an GpioController object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the GpioControllerClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "gpio_controller_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a GPIO Controller object with obj_id 0
        GpioControllerClient gpioController(0);

        // Use the GPIO Controller Client
        gpioController.mode_register_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the GPIO Controller client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a GpioControllerClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the GpioControllerClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a GpioControllerClient object with obj_id 0
    gpioController = GpioControllerClient(’com’,com);
    % Use the GpioControllerClient object
    modeRegister = gpioController.get(’mode_register’);

Python
~~~~~~

To use the GPIO Controller Client in Python, include ``iqmotion`` and create a module that has the GPIO Controller Client within it's firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the GPIO Controller Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    mode_register = |variable_name|.get("gpio_controller", "mode_register") 
    print(f"mode register: {mode_register}")

Message Table
~~~~~~~~~~~~~

Type ID 90 | GPIO Controller

+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| Sub ID | Short Name                       | Access         | Data Type | Unit | Note                                                       |
+========+==================================+================+===========+======+============================================================+
| 0      | mode_register                    | get, set, save | uint8     |      | Access to the GPIO Modes register                          |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 1      | inputs_register                  | get            | uint8     |      | Access to the GPIO Inputs register                         |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 2      | outputs_register                 | get, set, save | uint8     |      | Access to the GPIO Outputs Register                        |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 3      | use_pull_register                | get, set, save | uint8     |      | Access to the GPIO Use-Pull Register                       |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 4      | pull_type_register               | get, set, save | uint8     |      | Access to the GPIO Pull Type Register                      |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 5      | push_pull_open_drain_register    | get, set, save | uint8     |      | Access to the GPIO PP/OD Register                          |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 6      | addressable_gpio_mode            | set            | uint8     |      | Addressable write access to a single GPIO's Mode           |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 7      | addressable_outputs              | set            | uint8     |      | Addressable write access to a single GPIO's Output         |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 8      | addressable_use_pull             | set            | uint8     |      | Addressable write access to a single GPIO's Use Pull Value |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 9      | addressable_pull_type            | set            | uint8     |      | Addressable write access to a single GPIO's PUll Type      |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
| 10     | addressable_push_pull_open_drain | get, set, save | uint8     |      | Addressable write access to a single GPIO's PP/OD Mode     |
+--------+----------------------------------+----------------+-----------+------+------------------------------------------------------------+
