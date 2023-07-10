PWM Interface
---------------

Vertiq's high power :ref:`PWM output interface <manual_high_power_pwm_>` provides access to a PWM output driver with read/write accessibility to the frequency, duty cycle, and mode.
At the hardware level, this driver is an **open-drain MOSFET without an internal pull up resistor**. 
The mode parameter determines which portion of the PWM cycle the duty cycle represents,
high or low, and is dependent on your application’s hardware setup.


Arduino
~~~~~~~

To use the PWM Interface in Arduino, ensure pwm_interface_client.hpp is included. This
allows the creation of a PwmInterfaceClient object. See the Message Table below for available messages. 
All message objects use the Short Name with a trailing underscore. 
All messages use the standard Get/Set/Save functions. 

A minimal working example for the PwmInterfaceClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    PwmInterfaceClient pwmInterface(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int pwmFrequency = 0;
        if(ser.get(pwmInterface.pwm_frequency_, pwmFrequency))
        Serial.println(pwmFrequency);
    }

C++
~~~

To use the PWM Interface client in C++, include pwm_interface_client.hpp. This allows the
creation of an PwmInterface object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PwmInterfaceClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "pwm_interface_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a PWM Interface object with obj_id 0
        PwmInterfaceClient pwmInterface(0);

        // Use the PWM Interface Client
        pwmInterface.pwm_frequency_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the PWM Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a PwmInterfaceClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PwmInterfaceClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a PwmInterfaceClient object with obj_id 0
    PwmInterface = PwmInterfaceClient(’com’,com);

    % Use the PwmInterfaceClient object
    pwmFrequency = PwmInterface.get(’pwm_frequency’);

Python
~~~~~~

To use the PWM Interface Client in Python, import ``iqmotion`` and create a fortiq module. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the PWM Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0, firmware="servo")
    
    pwm_frequency = |variable_name|.get("pwm_interface", "pwm_frequency") 
    print(f"pwm frequency: {pwm_frequency}")

Message Table
~~~~~~~~~~~~~

Type ID 92 | PWM Interface

+--------+---------------+----------------+-----------+------+--------------------------------------------------------------+
| Sub ID | Short Name    | Access         | Data Type | Unit | Note                                                         |
+========+===============+================+===========+======+==============================================================+
| 0      | pwm_frequency | get, set, save | uint32    | Hz   | Frequency between 1Hz and 5000Hz                             |
+--------+---------------+----------------+-----------+------+--------------------------------------------------------------+
| 1      | duty_cycle    | get, set, save | uint8     |      | The percentage of the full cycle that represents high or low |
+--------+---------------+----------------+-----------+------+--------------------------------------------------------------+
| 2      | pwm_mode      | get, set, save | uint8     |      | Determines which portion of PWM cycle represents high or low |
+--------+---------------+----------------+-----------+------+--------------------------------------------------------------+
