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

.. note:: 

    Your module can be configured to report speed rather than velocity in its IFCI Telemetry response. To do so, simply set ``Report Telemetry as Speed`` in :ref:`IQ Control Center's <control_center_start_guide>` 
    advanced tab to ``Enabled``.

    .. image:: ../_static/manual_images/telemetry/report_telem_as_speed.png