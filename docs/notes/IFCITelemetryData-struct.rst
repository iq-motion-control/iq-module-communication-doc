.. _ifcitelemetrydata_note:

**IFCITelemetryData Struct**

The data returned by the telemetry endpoint contains 16 bytes. A struct called IFCITelemetryData is required to store this data.
Here is a description of the IFCITelemetryData struct:

.. code-block:: c++

    struct IFCITelemetryData 
    {
        int16_t mcu_temp;       // Temperature of the microcontroller in centi °C
        int16_t coil_temp;      // Temperature of the coils in centi °C
        int16_t voltage;        // Supply voltage in centi-volts        
        int16_t current;        // Supply current in centi-amps
        int16_t consumption;    // Consumtion in mAh
        int16_t speed;          // Velocity of the motor in rad/s
        uint32_t uptime;        // Uptime in seconds 
    };