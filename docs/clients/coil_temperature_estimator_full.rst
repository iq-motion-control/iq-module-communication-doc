Coil Temperature Estimator
--------------------------

The Coil Temperature Estimator is a convection and conduction based thermal model to estimate the temperature
of motor coils when they are not directly sensed. The temperature is used to derate the motor if the 
temperature rises into dangerous levels. The temperature limits can be adjusted, though this is not recommended.
The convection coefficient should be tuned for the specific propeller used on the motor. A convection 
coefficient for an average sized propeller for the given motor is loaded by default. Consider changing the
convection coefficient if a relatively large or small propeller is used, or if extreme performance and accuracy are required. 

Arduino
~~~~~~~

To use the Coil Temperature Estimator in Arduino, ensure coil_temperature_estimator_client.hpp is included. This
allows the creation of a CoilTemperatureEstimatorClient object. See the Message Table below for available messages. All
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
creation of a CoilTemperatureEstimator object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the TemperatureEstimatorClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "coil_temperature_estimator_client.hpp"


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
path. This allows the creation of a CoilTemperatureEstimatorClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the CoilTemperatureEstimatorClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a CoilTemperatureEstimatorClient object with obj_id 0
    CoilTemperatureEstimator = CoilTemperatureEstimatorClient(’com’,com);
    % Use the CoilTemperatureEstimatorClient object
    coilTemp = CoilTemperatureEstimator.get(’t_coil’);

Python
~~~~~~

To use the Coil Temperature Estimator Client in Python, import ``iqmotion`` and create a module that has the Coil Temperature Estimator Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Temperature Estimator Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    coil_temp = |variable_name|.get("coil_temperature_estimator", "t_coil")  # Estimated Motor Temperature
    print(f"Estimated Motor Temperature: {coil_temp}")

Message Table
~~~~~~~~~~~~~

Type ID 83 | Coil Temperature Estimator

+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name                   | Access         | Data Type | Unit                              | Note                                                                                                                                 |
+========+==============================+================+===========+===================================+======================================================================================================================================+
| 0      | t_coil                       | get            | float     | :math:`^{\circ}C`                 | This is the estimated temperature of the motor coils.                                                                                |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 1      | t_alu                        | get            | float     | :math:`^{\circ}C`                 | This is the estimated temperature of the stator aluminum.                                                                            |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 2      | t_amb                        | get, set, save | float     | :math:`^{\circ}C`                 | This is the estimated temperature of the ambient air, which is usually conservative.                                                 |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 3      | h_coil_amb_free_conv         | get, set, save | float     |                                   | This is the free convection heat transfer coefficient used when the motor is not spinning.                                           |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 4      | h_coil_stator_cond           | get, set, save | float     |                                   | This is the conduction heat transfer coefficient between the coils and the stator aluminum.                                          |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 5      | h_coil_amb_forced_conv       | get            | float     |                                   | This is the the present calculated force heat transfer convection coefficient.                                                       |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 6      | c_coil                       | get, set, save | float     |                                   | This is the thermal heat capacitance/mass of the coils.                                                                              |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 7      | h_coil_amb_forced_conv_coeff | get, set, save | float     |                                   | This is the forced convection coefficient calculation coefficient. h_forced_conv = h_conv_coeff * sqrt(speed)                        |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 8      | otw                          | get, set, save | float     | :math:`^{\circ}C`                 | This is the over temperature warning. The motor begins to derate at this temperature.                                                |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 9      | otlo                         | get, set, save | float     | :math:`^{\circ}C`                 | This is the over temperature lock out. The derating of the motor ends at this temperature, where the motor is fully disabled.        |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 10     | derate                       | get            | float     |                                   | This is the amount of derating applied to motor [0, 1] where 1 is normal operation.                                                  |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 11     | q_coil_joule                 | get            | float     | :math:`W`                         | This is the present heating power in coils.                                                                                          |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 12     | q_coil_amb_conv              | get            | float     | :math:`W`                         | This is the present ambient convective cooling power.                                                                                |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 13     | q_coil_stator_cond           | get            | float     | :math:`W`                         | This is the present stator conductive cooling power.                                                                                 |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 14     | h_lam_alu                    | get, set, save | float     | :math:`\frac{W}{m^2 * ^{\circ}C}` | This is the the heat transfer coefficient from the laminations to the aluminum stator mount.                                         |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 15     | c_lam                        | get, set, save | float     |                                   | This is the thermal heat capacitance of the laminations.                                                                             |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 16     | k_lam_hist_coeff             | get, set, save | float     | :math:`\frac{W}{\text{rad}/s}`    | This is the constant for calculating the iron losses as a function of speed based on the laminations, hysteresis, and eddy currents. |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 17     | q_lam_hist                   | get, set, save | float     | :math:`W`                         | This is the iron loss coefficient of the stator.                                                                                     |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 18     | q_lam_alu                    | get, set, save | float     | :math:`W`                         | This is the present heating power in stator aluminum.                                                                                |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| 19     | t_lam                        | get, set, save | float     | :math:`^{\circ}C`                 | This is the estimated temperature of stator.                                                                                         |
+--------+------------------------------+----------------+-----------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
