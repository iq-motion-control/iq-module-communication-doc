ADC Interface
-------------
Vertiq's :ref:`ADC Interface <manual_adc_interface>` provides access to an on-board Analog to Digital Converter (ADC). An ADC makes it possible for your module to read input analog voltages. The ADC handles voltages from 0.0V to 3.3V with a 12-bit resolution. For example, if you input 1V to the ADC interface, reading the voltage would return 1V, and reading the "raw value" would return 1241 (:math:`\frac{V_{\text{in}} * 4096}{3.3}`). 

The ADC interface provides read-only access to both the voltage read and the raw ADC value.

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
        float adcVoltage = 0.0f;
        if(ser.get(adcInterface.adc_voltage_, adcVoltage))
        Serial.println(adcVoltage);
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
    adcVoltage = adcInterface.get(’adc_voltage’);

Python
~~~~~~

To use the ADC Interface Client in Python, import ``iqmotion`` and create a fortiq module. 
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

+--------+-------------+--------+-----------+-------+--------------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name  | Access | Data Type | Unit  | Note                                                                                                                     |
+========+=============+========+===========+=======+==========================================================================================================================+
| 0      | adc_voltage | get    | float     | Volts | Read only access to input analog voltage                                                                                 |
+--------+-------------+--------+-----------+-------+--------------------------------------------------------------------------------------------------------------------------+
| 1      | raw_value   | get    | uint16    |       | Read only access to raw ADC value. Ex: 1V to ADC interface would return 1241: (:math:`\frac{V_{\text{in}} * 4096}{3.3}`) |
+--------+-------------+--------+-----------+-------+--------------------------------------------------------------------------------------------------------------------------+
