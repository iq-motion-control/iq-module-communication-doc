Temperature Estimator
---------------------

The Temperature Estimator uses a conduction thermal model to estimate the temperature of components
not directly sensed. In the motor modules the Temperature Estimator estimates the motor coil temperature.
The temperature is used to derate the motor if the temperature rises into dangerous levels. The temperature
limits can be adjusted, though this is not recommended.

Arduino
~~~~~~~

To use the Temperature Estimator in Arduino, ensure iq_module_communication.hpp is included. This
allows the creation of a TemperatureEstimatorClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions.

A minimal working example for the TemperatureEstimatorClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    TemperatureEstimatorClient tmp(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        float temperature = 0.0f;
        if(ser.get(tmp.temp_,temperature))
        Serial.println(temperature);
    }

C++
~~~

To use the Temperature Estimator client in C++, include temperature estimator client.hpp. This allows the
creation of a TemperatureEstimatorClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the TemperatureEstimatorClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "temperature_estimator_client.hpp"


    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Temperature Estimator object with obj_id 0
        TemperatureEstimatorClient temp_client(0);

        // Use the Temperature Estimator Client
        temp_client.temp_.get(com);

        // [Insert code for interfacing with hardware here]
    }

Matlab
~~~~~~

To use the Temperature Estimator client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a TemperatureEstimatorClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the TemperatureEstimatorClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make an TemperatureEstimatorClient object with obj_id 0
    TemperatureEstimator = TemperatureEstimatorClient(’com’,com);

    % Use the TemperatureEstimatorClient object
    coilTemp = TemperatureEstimator.get(’temp’);

Python
~~~~~~

To use the Temperature Estimator Client in Python, import ``iqmotion`` and create a module that has the Temperature Estimator Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Temperature Estimator Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    temp = |variable_name|.get("temperature_estimator", "temp")  # Estimated Motor Temperature
    print(f"Estimated Motor Temperature: {temp}")

Message Table
~~~~~~~~~~~~~

Type ID 77 | Temperature Estimator
    .. table:: Temperature Estimator
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
        | Sub ID |     Short Name      |     Access     | Data Type | Unit                 |                                                     Note                                                     |
        +========+=====================+================+===========+======================+==============================================================================================================+
        | 0      | temp                | get            | float     | :math:`^{\circ}C`    | Temperature of the motor coils                                                                               |
        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
        | 1      | otw                 | get, set, save | float     | :math:`^{\circ}C`    | Over temperature warning. Derating of the motor begins at this temperature.                                  |
        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
        | 2      | otlo                | get, set, save | float     | :math:`^{\circ}C`    | Over temperature lock out. Derating of the motor end at this temperature, where the motor is fully disabled. |
        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
        | 3      | thermal_resistance  | get, set, save | float     | :math:`\frac{K}{W}`  | Model thermal resistance                                                                                     |
        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
        | 4      | thermal_capacitance | get, set, save | float     | :math:`\frac{J}{K}`  | Model thermal capacitance                                                                                    |
        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
        | 5      | derate              | get            | Fix16     | :math:`\text{PU}`    | Amount of derating applied to motor [0 65536] where 65536 is normal operation                                |
        +--------+---------------------+----------------+-----------+----------------------+--------------------------------------------------------------------------------------------------------------+
