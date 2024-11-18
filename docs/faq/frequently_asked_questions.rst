
.. include:: ../text_colors.rst
.. toctree::

.. _frequently_asked_questions:

##########################
Frequently Asked Questions
##########################

#. I just purchased my motor, how do I get started with Vertiq's IQ Control Center software? How can I communicate with my module?
    * | Your module must be configured properly through the Control Center before your module can spin. In order to connect with Control Center, refer to your module’s family page found under the sidebar's Modules section. Here, you will find the hardware setup instructions for your specific module. After your hardware is set up, refer to our :ref:`Control Center Manual <control_center_start_guide>` for instructions on how to install and run Control Center. Then, follow the instructions on configuring your module using Control Center depending on its firmware: :ref:`Speed <speed_module_start_guide>`, :ref:`Servo <servo_module_start_guide>`, or :ref:`Pulsing <pulsing_module_start_guide>`. Please see our documentation under the Tutorials section of the sidebar for additional information, such as integrating with your flight controller, retrieving flight controller logs, and debugging communication issues. 
      | We also have a supplementary application called :ref:`Vertiq Testing Tool <vertiq_testing_tool_guide>` to help you test your module. After you have configured your module with Control Center, download and run Vertiq Testing Tool to verify that your parameters are set correctly.

#. Why does my module not spin when I send it a PWM signal?
    * Vertiq modules require initial configuration before being able to spin with PWM commands. You can configure your motor using :ref:`Control Center <control_center_start_guide>`. The most commonly missed configuration when trying to spin with PWM is the :ref:`Motor Direction <throttle_direction>`, which must be set to something other than unconfigured. We recommend setting it to 2D Counter-Clockwise for your initial setup. You should also check what :ref:`Mode <throttle_mode>` is set on your module and that the corresponding :ref:`Maximum <throttle_maximums>`  for that mode is set. If you are still having trouble, refer to the :ref:`PWM and DSHOT tutorial <hobby_fc_tutorial>` or your module's Getting Started documentation: :ref:`Speed <speed_module_start_guide>`, :ref:`Servo <servo_module_start_guide>`, or :ref:`Pulsing <pulsing_module_start_guide>`.

#. I’m trying to use DroneCAN but my motor isn’t spinning.
    * A common issue that may cause your module not to spin is that its parameters are not configured correctly. Please confirm that you have properly configured the :ref:`Motor Direction <throttle_direction>`, :ref:`Mode <throttle_mode>`, and :ref:`Maximums <throttle_maximums>` for your module. Also, confirm that your configured :ref:`ESC Index <dronecan_px4_tutorial_esc_index>` is correct for the module that you are trying to control. If your module is not using :ref:`Arming Bypass <dronecan_arming_and_bypass>`, confirm that you are sending DroneCAN commands that will :ref:`arm <dronecan_arming_and_bypass>` the module initially. By default, commands close to 0% throttle should arm the module. For more information on how to use DroneCAN with your module, please refer to our :ref:`DroneCan Integration with PX4 and Ardupilot <dronecan_fc_tutorial>` documentation.

#. What hardware do I need prior to receiving my module?
    * Certain modules come with included hardware, such as prop adapters and heatshrink. Please refer to your module’s family page found in the sidebar under the Modules section for everything that is included in the box.
    * You will need screws to attach a propeller to your module and to attach your module to the drone arm. Please check your module’s datasheet found on the module's family page under “Documentation” on our `website <https://www.vertiq.co/>`_. It is also highly recommended to apply a threadlocker, such as Loctite 243, to your screws when attaching a propeller to your module.
    * For all modules, you will need a UART-to-Serial adapter in order to communicate with your module using your computer. We recommend using a `CP2102 FTDI adapter <https://www.amazon.com/Ximimark-Module-Serial-Converter-CP2102/dp/B07T1XR9FT?>`_.

#. Why is my module slowing down, even though I am not adjusting the throttle? Why is my module derating?
    * Each motor has built in safeties to protect the module from overheating failures. The module will start to spin slower, or derate, when the safeties are in effect to keep it at a safe operating speed. To learn more about these safety features, please refer to the :ref:`Safety Systems <manual_safety>` documentation.
    * Each module has a max-continuous thrust region depending on the size of the propeller you are using. Please refer to your module’s `Thrust Data <https://www.vertiq.co/thrust-data>`_ to learn more about its max-contintinous thrust.
    * Each module also has a continuous torque limit, which you can find on your module’s datasheet on our `website <https://www.vertiq.co/>`_ by navigating to your module’s family page under the Documentation section. This datasheet can also help you choose the correct propeller size for your module

#. How do I update my module’s firmware?
    * You can find the latest firmware for your module on our `website <https://www.vertiq.co/>`_ by navigating to your module’s family page under the Firmware section. Please refer to our :ref:`Updating Firmware with Control Center <updating_firmware_control_center>` documentation on how to flash new firmware onto your module using Control Center.

#. Why are there no throttle percentages on your thrust data?
    * We do not include a column for throttle percentages in our thrust data because our modules support multiple throttle modes (PWM, Voltage, and Velocity), which means “throttle” can be interpreted differently between each mode. For example, a 75% throttle command in PWM mode can be interpreted as “Command 75% of the supplied voltage.” On the other hand, a 75% throttle command in Velocity mode can be interpreted as “Command 75% of the max velocity,” which can be configured by the user. Due to these differences in the interpretation of “throttle,” we display our data in increments of 1V or 2V, which eliminates the variable of a depleting battery supply voltage that makes other companies’ data confusing. Please refer to our blog post about `Understanding Vertiq Thrust Data <https://www.vertiq.co/blog/understanding-vertiq-thrust-data>`_  and our documentation about :ref:`Maximum Voltage and Maxiumum Velocity <throttle_maximums>` for more information.

#. Why is my drone vibrating/oscillating excessively, causing instability, after take-off?
    * Flight controllers typically have built-in accelerometers and gyroscopes that are sensitive to high vibrations. Extreme vibration can lead to bad readings from these sensors, which can lead to a range of problems, including instability and rapidly oscillating actuator outputs. These oscillating actuator outputs from the flight controller can lead to your motors seeming to “jitter,” switching their setpoint constantly instead of smoothly spinning. Please refer to our documentation about :ref:`Understanding and Addressing Flight Controller and Motor Vibrations <vibration_and_jittering>` for more information.

#. Can voltage regeneration be disabled or reduced?
    * Unfortunately, there is no way to completely prevent the module from regenerating current due to the limitations of physics. When attempting to step down the throttle with a load, such as a propeller, the module will always want to regenerate. However, how much current it regenerates depends on how aggressive the step-down is. There is a safety feature that is enabled by default that is designed to protect power supplies or other power sources that cannot absorb this current. When the supply voltage increases to a certain point, which generally happens when a power supply is being regenerated into, the module will begin to limit how aggressively the drive voltage changes to try and control how much current it regenerates. While eliminating regeneration completely is impossible, Vertiq modules have options to limit the maximum amount of regeneration to protect power supplies. Please refer to our documentation about :ref:`Protecting Against Voltage Regeneration <protect_against_regen>` for more information.

#. How do I get telemetry from my motors? How do I get Flight Controller logs?
    * Please refer to our documentation about :ref:`Exporting Logs <ifci_log>`. Basic telemetry can also be visualized using :ref:`Vertiq Testing Tool <vertiq_testing_tool_guide>`.
    
#. What causes electromagnetic interference (EMI) between the motor and the integrated ESC? Is electrical noise an issue when it comes to long communication wires? 
    * Electromagnetic interference (EMI) can be radiated, coupled, or conducted at different frequencies while the motor is spinning. Electrical noise that is coupled to the motor’s body can be conducted to conduits carrying communications cables, such as the vehicle’s frame or drone arm. Longer communication wires can lead to more noise. To learn more about EMI and how to mitigate electrical noise, please refer to the :ref:`Understanding and Debugging Communication Errors from Motor Noise <motor_noise_debugging>` documentation.

#. What flight controllers do you support? What firmware can I use (Ardupilot, PX4, etc.) ?
    * Vertiq modules have been tested primarily with flight controllers using :ref:`PX4 <ifci_px4_flight_controller>` and :ref:`Ardupilot <dronecan_fc_tutorial>` firmware, and our documentation is focused on those types of flight controllers and their accompanying hardware. Other types of flight controllers may be used if they support protocols that can communicate with a Vertiq module. Please refer to the Messaging Protocols section in the sidebar for all of our supported protocols.

#. I’m using voltage/velocity input commands through the Control Center. Why is my motor spinning counterclockwise? What determines which direction my motor spins? Why is it spinning the wrong way?
    * Voltage and Velocity inputs do not use the module's motor direction configuration, and will always result in a counter-clockwise rotation given a positive voltage/velocity input. A negative voltage/velocity input will always result in a clockwise rotation. Please refer to our documentation about :ref:`Velocity and Voltage Based Control Mechanisms <manual_velocity_control_mechanisms>` for more information. 
    * To test if your module’s direction is configured correctly, use ESC Input as explained in the :ref:`Example Module Flight Controller Configuration and ESC Testing with Control Center <flight_controller_config_with_control_center>` documentation.

#. Can the modules stow at multiple angles?
    * Currently, there is no support for storing multiple stow angles. Your module can only save one stow angle with reference to the zero angle. To learn more about stowing, please refer to our :ref:`Stow Position <manual_stow_position>` documentation.