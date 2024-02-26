
.. _vertiq_23xx_family:

*********************************
Vertiq 23-XX Family 
*********************************

.. csv-table:: Vertiq 23-XX Family of Modules
        :header: "Size", "Kv", "Default Firmware", "Available Firmware"
        :align: center

        "23-06", "220", "Servo", "Servo, Step/Direction"
        "23-06", "2200", "Speed", "Speed, Pulsing"
        "23-14", "920", "Speed", "Servo, Speed"
        

.. sidebar:: Vertiq 23-06

    .. image:: ../_static/IQ2306_2200kv.png
        :alt: Vertiq 23-06 2200Kv

The Vertiq 23-06 2200Kv as well as the Vertiq 23-14 920Kv are integrated motor-controllers with a wide range of velocity based applications. They have open and closed loop controllers
designed primarily to drive propeller loads.

The Vertiq 23-06 220Kv is an integrated motor
and controller with a wide range of position based applications. Its performance is comparable to or better
than other 23-06 sized (NEMA 11, 28mm stepper) motors and can operate at any speed between -4,800 and
4800 RPM thanks to its sensored control. Its closed loop PID controller tracks targets across multiple revolutions, 
making it ideal for applications with transmissions, both rotary and linear. This sits on top of
a voltage controller, which compensates for varying
input voltages. Finally, the core is a raw PWM controller. Any of the above controllers can be used by
the user. The Vertiq 23-06 Position Module has a built-in
rotary to linear calculation converter, which allows
the user to communicate to the firmware in native
linear units. The onboard minimum jerk trajectory
generator with 32 segment queue produces smooth,
human like motions with minimal computation and
communication overhead from the application controller.

| :download:`Speed Module Datasheet <../_static/2306_speed_datasheet.pdf>`
| :download:`Position Module Datasheet <../_static/2306_position_datasheet.pdf>` 

.. |module_name| Replace:: Vertiq23xx
.. |variable_name| Replace:: vertiq

.. |module_firmware| unicode:: 0xA0
.. include:: speed.rst

.. |module_firmware| Replace:: , firmware="servo"
.. include:: servo.rst

.. |module_firmware| Replace:: , firmware="stepdir"
.. include:: stepdir.rst

.. |module_firmware| Replace:: , firmware="pulsing"
.. include:: pulsing.rst