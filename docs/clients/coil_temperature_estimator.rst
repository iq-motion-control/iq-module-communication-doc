Coil Temperature Estimator
---------------------

!

Arduino
~~~~~~~

To use the Coil Temperature Estimator in Arduino, ensure coil_temperature_estimator_client.hpp is included. This
allows the creation of a CoilTemperatureEstimatorClient object. See Table 11 for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions.

A minimal working example for the CoilTemperatureEstimatorClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    CoilTemperatureEstimatorClient tmp(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        float temperature = 0.0f;
        if(ser.get(tmp.t_coil_, temperature))
        Serial.println(temperature);
    }

C++
~~~

To use the Coil Temperature Estimator client in C++, include coil_temperature_estimator_client.hpp. This allows the
creation of a CoilTemperatureEstimator object. See Table 11 for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the TemperatureEstimatorClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "coil_temperature_estimator_client.hpp"

    float temp;

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Temperature Estimator object with obj_id 0
        CoilTemperatureEstimatorClient temp_client(0);

        // Use the Temperature Estimator Client
        temp_client.temp_.get(com)

        // [Insert code for interfacing with hardware here]

        // temp = temp_client.temp_.get_reply();
    }

Matlab
~~~~~~

To use the Coil Temperature Estimator client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a CoilTemperatureEstimatorClient object. See Table 11 for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the CoilTemperatureEstimatorClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a CoilTemperatureEstimatorClient object with obj_id 0
    tes = CoilTemperatureEstimatorClient(’com’,com);
    % Use the CoilTemperatureEstimatorClient object
    coil_temp = tes.get(’t_coil’);

Python
~~~~~~

To use the Coil Temperature Estimator Client in Python, include ``iqmotion`` and create a module that has the Coil Temperature Estimator Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Temperature Estimator Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0|module_firmware|)
    
    coil_temp = |variable_name|.get("coil_temperature_estimator", "t_coil")  # Estimated Motor Temperature
    print(f"Estimated Motor Temperature: {coil_temp}")

Message Table
~~~~~~~~~~~~~

Type ID 83 | Coil Temperature Estimator

+--------+---------------+----------------+-----------+------+------+
| Sub ID | Short Name    | Access         | Data Type | Unit | Note |
+========+===============+================+===========+======+======+
| 0      | t_coil        | get            | float     | degC | !    |
+--------+---------------+----------------+-----------+------+------+
| 1      | t_alu         | get            | float     | degC | !    |
+--------+---------------+----------------+-----------+------+------+
| 2      | t_amb         | get, set, save | float     | degC | !    |
+--------+---------------+----------------+-----------+------+------+
| 3      | h_free_conv   | get, set, save | float     |      | !    |
+--------+---------------+----------------+-----------+------+------+
| 4      | h_coil_alu    | get, set, save | float     |      | !    |
+--------+---------------+----------------+-----------+------+------+
| 5      | h_forced_conv | get            | float     |      | !    |
+--------+---------------+----------------+-----------+------+------+
| 6      | c_coil        | get, set, save | float     |      | !    |
+--------+---------------+----------------+-----------+------+------+
| 7      | h_conv_coeff  | get, set, save | float     |      | !    |
+--------+---------------+----------------+-----------+------+------+
| 8      | otw           | get, set, save | float     | degC | !    |
+--------+---------------+----------------+-----------+------+------+
| 9      | otlo          | get, set, save | float     | degC | !    |
+--------+---------------+----------------+-----------+------+------+
| 10     | derate        | get            | float     |      | !    |
+--------+---------------+----------------+-----------+------+------+
| 11     | q_joule       | get            | float     | W    | !    |
+--------+---------------+----------------+-----------+------+------+
| 12     | q_conv        | get            | float     | W    | !    |
+--------+---------------+----------------+-----------+------+------+
| 13     | q_rad         | get            | float     | W    | !    |
+--------+---------------+----------------+-----------+------+------+
| 14     | q_cond        | get            | float     | W    | !    |
+--------+---------------+----------------+-----------+------+------+

