.. include:: ../text_colors.rst
.. toctree::

.. _vertiq_testing__guide:

########################################
Getting Started with Vertiq Testing Tool
########################################

*****
About
*****

Vertiq Testing Tool is a supplimental application to IQ Control Center. 
It provides a streamlined interface to control your module using ESC, Voltage, Velocity, and PWM inputs.


********
Download
********
Currently, Vertiq Testing Tool is only available on Windows (Linux coming soon).
You can download Vertiq Testing Tool on our `Support Page <https://www.vertiq.co/support>`_.


**********************
Hardware Configuration
**********************
Please follow the :ref:`Control Center documentation <connection_guide>` to connect your module to your computer.


********
Overview
********
Once downloaded, extract and run the Vertiq Testing Tool executable. When running this application for the first time, 
you may encounter a Windows Defender SmartScreen warning.

.. image:: ../_static/vertiq_testing_tool_pictures/windows_defender.png

Click on 'More info'.

.. image:: ../_static/vertiq_testing_tool_pictures/windows_defender_more_info.png

Then click 'Run anyway'.

.. image:: ../_static/vertiq_testing_tool_pictures/welcome_screen.png

Once the application is loaded, you will be presented with the welcome screen. Follow the instructions on this screen to connect your module.


.. image:: ../_static/vertiq_testing_tool_pictures/esc_input.png

#. The :gold:`Tabs` section are the different tabs that you can switch to in order to spin your module.

#. The :green:`Plotting` section contains a live plot in the upper section and a dashboard with widgets to control the live plot in the lower section.

#. The :purple:`Control Panel` section contains widgets to configure specific parameters on your module. The Control Panel on each tab contains different widgets. 

In this ESC Input tab, the Control Panel contains the same parameters that need to be configured as the example in :ref:`Control Center <flight_controller_config_with_control_center>`.
The only difference is that these parameters are in all on the same tab, as opposed to different tabs in Control Center.


.. image:: ../_static/vertiq_testing_tool_pictures/esc_input_command_widget.png

A notable feature in this tab is the 'Continuous command' at the bottom of the Control Panel. This feature is disabled by default, which means you need to click the 'Command ESC' button every time you want to send an ESC command.
When this feature is enabled, the ESC command will be continuously sent at the 'Command rate' with 10 Hz being the default value. The 'Command rate' can be adjust to send the ESC command at different rates. 
You can also use the slider to adjust the ESC input value.


.. image:: ../_static/vertiq_testing_tool_pictures/voltage_input.png

Another useful feature found in the Voltage Input, Velocity Input, and PWM Input tabs is 'Ramp to target' in the Control Panel.
This feature is enabled by default, which means when you click 'Send Command' after setting the desired Target Voltage, the module will attempt to ramp to the target voltage in increments. 
The increments are determined by the ramp time, so increasing the ramp time will make it so that the module takes longer to reach the target voltage.

The 'Auto Command' feature is disabled by default. When enabled, you will not have to click the 'Send Command' button to send a voltage command. 
Instead, the application will automatically send the command whenever the target voltage value changes.

Be very cautious when 'Auto Command' is enabled and 'Ramp to Target' is disabled. It is recommended to lower the 'Step Amount' to avoid large jumps between each target voltage command.


.. note::
    Thank you for using our application! Please note that it is currently in active development, and you may encounter some bugs or unexpected behavior. 
    Your experience and feedback are extremely valuable to us as we continue to improve the application. 
    If you notice any issues or have any questions reach out to us at support@vertiq.co. Suggestions and feature requests are welcomed!
