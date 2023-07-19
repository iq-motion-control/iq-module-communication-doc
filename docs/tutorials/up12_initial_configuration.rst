.. include:: ../text_colors.rst
.. toctree::

.. _up12_initial_configuration:

.. |propeller_name| replace:: UP12
.. |motor_name| replace:: Vertiq 2306

****************************************************
Setting up the |motor_name| for the |propeller_name|
****************************************************

The |motor_name| needs to have the appropriate firmware flashed as well as the appropriate settings applied to be able to use the |propeller_name| to create control torque. You can download the pulsing firmware for the |motor_name| here (firmware link). To flash the downloaded firmware use the instructions in :ref:`updating_firmware`.

Once the firmware is flashed connect the motor to IQ Control Center and go to the general tab. Use the ‘Import’ button to load the defaults file from here:
Link to defaults file

Use the ‘Set’ button to save the settings from the defaults file onto the |motor_name|. This will set all the required settings as well as change the |motor_name|’s baud rate. Reboot the |motor_name|. Once rebooted, to reconnect to IQ Control Center you must set the baudrate to 921600.

Now the forward direction of the |motor_name| must be decided. Usually this is done by mounting the |motor_name| to the aircraft with the adapter and main shoulder bolt mounted to the |motor_name|. After mounting your |motor_name| to your aircraft choose the direction you want to treat as forward and rotate the shoulder bolt to that angle.

.. figure:: ../_static/tutorial_images/up12_initial_configuration/motor_forward.png
    :align: center
    :width: 500
    :alt: Setting Aircraft Forward

    Setting Aircraft Forward

Connect the (Vertiq 2306) to IQ Control  Center (using 921600 as baudrate). Go to the ‘General’ tab and while the bolt is angled forwards select ‘Sample Pulsing Zero Angle’. This will tell the |motor_name| which direction is forward.

.. figure:: ../_static/tutorial_images/up12_initial_configuration/set_angle.png
    :align: center
    :width: 500
    :alt: Sampling Zero Angle

    Sampling Zero Angle

With all of this set, the motor will now respond properly to X, Y and Throttle commands sent to it via packed control messages explained in :ref:`controlling_ifci`. A final setup step is required that is aircraft dependent which is torque angle offset correction. This is explained in :ref:`Underactuated Propeller Torque Angle Correction<correcting_underactuated_torque>`.