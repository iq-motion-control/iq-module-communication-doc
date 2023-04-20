ADC Interface
-------------

**TODO**

Arduino
~~~~~~~

To use the ADC Interface in Arduino, ensure adc_interface_client.hpp is included. This
allows the creation of a AdcInterfaceClient object. See the Message Table below for available messages. 
All message objects use the Short Name with a trailing underscore. 
All messages use the standard Get/Set/Save functions. 

A minimal working example for the AdcInterfaceClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    AdcInterfaceClient adcInterface(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        float adc_voltage = 0.0f;
        if(ser.get(adcInterface.adc_voltage_, adc_voltage))
        Serial.println(adc_voltage);
    }

C++
~~~

To use the ADC Interface client in C++, include adc_interface_client.hpp. This allows the
creation of an AdcInterface object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the AdcInterfaceClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "adc_interface_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a ADC Interface object with obj_id 0
        AdcInterfaceClient adcInterface(0);

        // Use the ADC Interface Client
        adcInterface.adc_voltage_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the ADC Interface client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a AdcInterfaceClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the AdcInterfaceClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a AdcInterfaceClient object with obj_id 0
    adcInterface = AdcInterfaceClient(’com’,com);
    % Use the AdcInterfaceClient object
    adc_voltage = adcInterface.get(’adc_voltage’);

Python
~~~~~~

To use the ADC Interface Client in Python, include ``iqmotion`` and create a module that has the ADC Interface Client within it's firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the ADC Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    adc_voltage = |variable_name|.get("adc_interface", "adc_voltage") 
    print(f"adc voltage : {adc_voltage}")

Message Table
~~~~~~~~~~~~~

Type ID 91 | ADC Interface

+--------+-------------+--------+-----------+------+------+
| Sub ID | Short Name  | Access | Data Type | Unit | Note |
+========+=============+========+===========+======+======+
| 0      | adc_voltage | get    | float     |      |      |
+--------+-------------+--------+-----------+------+------+
| 1      | raw_value   | get    | uint16    |      |      |
+--------+-------------+--------+-----------+------+------+
