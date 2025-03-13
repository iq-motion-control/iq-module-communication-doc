
.. include:: ../text_colors.rst
.. toctree::

.. _frequently_asked_questions:

==========================
Frequently Asked Questions
==========================

#. What hardware do I need prior to receiving my module?
    * Certain modules come with included hardware, such as prop adapters and heatshrink. Please refer to your module’s family page found in the sidebar under the Modules section for everything that is included in the box.
    * You will need screws to attach a propeller to your module and to attach your module to the drone arm. Please check your module’s datasheet found on the module's family page under “Documentation” on our `website <https://www.vertiq.co/>`_. It is also highly recommended to apply a threadlocker, such as Loctite 243, to your screws when attaching a propeller to your module.
    * For all modules, you will need a UART-to-Serial adapter in order to communicate with your module using your computer. We recommend using a `CP2102 FTDI adapter <https://www.amazon.com/Ximimark-Module-Serial-Converter-CP2102/dp/B07T1XR9FT?>`_.
    * If you are using the UART-to-Serial adapter we have recommended and are not seeing it as an available port, you may need to install the appropriate drivers from `here <https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=overview>`_.

#. I just purchased my motor, how do I get started with Vertiq's IQ Control Center software? How can I communicate with my module?
    * Issues with spinning your module can be related to improper configuration of the motor. Control Center is the recommended tool for configuring your module. Before connecting with Control Center, refer to your module’s family page found under the sidebar's Modules section. Here, you will find the hardware setup instructions for your specific module. After your hardware is set up, refer to our :ref:`Control Center Manual <control_center_start_guide>` for instructions on how to install and run Control Center. Then, follow the *Getting Started with Control Center* instructions for your module's specific firmware style: :ref:`Speed <speed_module_start_guide>`, :ref:`Servo <servo_module_start_guide>`, or :ref:`Pulsing <pulsing_module_start_guide>`. After getting familiar with configuring your module with Control Center, see our documentation under the Tutorials section of the sidebar for additional information, such as integrating with your flight controller, retrieving flight controller logs, and debugging communication issues. 
    * We also have a supplementary application called :ref:`Vertiq Testing Tool <vertiq_testing_tool_guide>` to help you test your module. You can download and run Vertiq Testing Tool for more advanced testing of your module's configuration.

#. Why does my module not spin when I send it a PWM signal?
    * Vertiq modules require initial configuration before being able to spin with PWM commands. You can configure your motor using :ref:`Control Center <control_center_start_guide>`. The most commonly missed configuration when trying to spin with PWM is the :ref:`Motor Direction <throttle_direction>`, which must be set to something other than unconfigured. We recommend setting it to 2D Counter-Clockwise for your initial setup. You should also check what :ref:`Mode <throttle_mode>` is set on your module and that the corresponding :ref:`Maximum <throttle_maximums>`  for that mode is set. If you are still having trouble, refer to the :ref:`PWM and DSHOT tutorial <hobby_fc_tutorial>` or your module's Getting Started documentation: :ref:`Speed <speed_module_start_guide>`, :ref:`Servo <servo_module_start_guide>`, or :ref:`Pulsing <pulsing_module_start_guide>`.

#. Why does my module not spin when I send it DroneCAN Raw Command messages?
    * A common issue that may cause your module not to spin is that its parameters are not configured correctly. Please confirm that you have properly configured the :ref:`Motor Direction <throttle_direction>`, :ref:`Mode <throttle_mode>`, and :ref:`Maximums <throttle_maximums>` for your module. Also, confirm that your configured :ref:`ESC Index <dronecan_px4_tutorial_esc_index>` is correct for the module that you are trying to control. If your module is not using :ref:`Arming Bypass <dronecan_arming_and_bypass>`, confirm that you are sending DroneCAN commands that will :ref:`arm <dronecan_arming_and_bypass>` the module initially. By default, commands close to 0% throttle should arm the module. For more information on how to use DroneCAN with your module, please refer to our :ref:`DroneCAN Integration with PX4 and Ardupilot <dronecan_fc_tutorial>` documentation.

#. Why is my module slowing down, even though I am not adjusting the throttle? Why is my module derating?
    * Each motor has built in safeties to protect the module from overheating failures. The module will start to spin slower, or derate, when the safeties are in effect to keep it at a safe operating point. To learn more about these safety features, please refer to the :ref:`Safety Systems <manual_safety>` documentation.
    * Each module has a max-continuous thrust region depending on the size of the propeller you are using. Please refer to your module’s `Thrust Data <https://www.vertiq.co/thrust-data>`_ to learn more about its max-contintinous thrust.
    * Each module also has a continuous torque limit, which you can find on your module’s datasheet on our `website <https://www.vertiq.co/>`_ by navigating to your module’s family page under the Documentation section. Derating can occur when your module operates outside of the maximum continuous torque and thrust regions. This datasheet can also help you choose the correct propeller size for your module.

#. How do I update my module’s firmware?
    * You can find the latest firmware for your module on our `website <https://www.vertiq.co/>`_ by navigating to your module’s family page under the Firmware section. Please refer to our :ref:`Updating Firmware with Control Center <updating_firmware_control_center>` documentation on how to flash new firmware onto your module using Control Center.

#. Why are there no throttle percentages on your thrust data?
    * We do not include a column for throttle percentages in our thrust data because our modules support multiple throttle modes (PWM, Voltage, and Velocity), which means “throttle” can be interpreted differently between each mode. For example, a 75% throttle command in PWM mode can be interpreted as “Command 75% of the supplied voltage.” On the other hand, a 75% throttle command in Velocity mode can be interpreted as “Command 75% of the max velocity,” which can be configured by the user. Due to these differences in the interpretation of “throttle,” we display our data in increments of 1V or 2V, which eliminates the variable of a depleting battery supply voltage that makes other companies’ data confusing. Please refer to our blog post about `Understanding Vertiq Thrust Data <https://www.vertiq.co/blog/understanding-vertiq-thrust-data>`_  and our documentation about :ref:`Maximum Voltage and Maximum Velocity <throttle_maximums>` for more information.

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



.. _upf-23-12_frequently_asked_questions:

=======================================
UPF-23-12 Frequently Asked Questions
=======================================

#. How does the underactuated propeller hardware mechanism work?
    * The underactuated propeller consists of a 45 degree hinge in the hub of the propeller and flapping hinges at the blades. It is connected to a Vertiq module, which consists of an integrated motor, ESC, and position sensor. 
    * The underactuated propeller is a variable-pitch propeller, and its blade pitch is coupled with its lead/lag position. When the motor accelerates, the propeller hinges (about the hub hinge) into a lag position; when the motor decelerates, the propeller hinges into a lead position; and when the propeller spins at a constant speed, the propeller returns to a neutral position. The change in blade pitch creates torques and forces that control the vehicle.
    * While the hub hinge is used for propeller pitch control, the flapping hinges are designed to absorb vibration and vector thrust.

#. How is the underactuated propeller controlled?
    * Vertiq’s pulsing firmware does the math for you! Our ESC translates standard flight controller commands into a drive voltage and applies a sinusoidal pulse (oscillating voltage) on top of the drive signal. The frequency, amplitude, and phase of the oscillating voltage allows us to accelerate and decelerate the motor at specific points throughout the motor’s rotation. This, in turn, makes the propeller hinge into a lead or lag position, creating a coherent torque and/or force in the desired direction. 

#. What messages are being sent from the FC to the ESC to enable pulsing at the right time?
    * To control the motor we are using something we call the IQUART flight controller interface (IFCI). A throttle, X, and Y signal is sent. Additionally a telemetry request can be sent.

#. Are there instructions for FC integration?
    * Yes, you can find the documentation on setting up the UPF-23-12 :ref:`here <up12_initial_configuration>`.

#. What mechanisms are the underactuated propeller replacing?
    * By creating thrust, as well as forces/torques to control roll and pitch, the underactuated propeller effectively replaces additional motors and swashplates. An underactuated motor and propeller gives you 3 degrees of freedom (DOF). Quadrotors fly with 4 DOF, using 4 motors and propellers to control thrust, roll, pitch, and yaw. You only need 1 underactuated motor and propeller and 1 standard motor and propeller to achieve the same degrees of freedom. Similarly, helicopters use a swashplate mechanism, which is a series of motors and complex linkages, to control blade pitch. The underactuated solution is a simpler, lighter replacement for a swashplate, eliminating the additional motors and complexities.

#. How is the propeller mounted to the motor?
    * The hub of the propeller is mounted to the motor using 4 M2 screws.

#. What communication protocols can be used for pulsing the motor?
    * The 23-06 2200Kv and 23-14 920Kv modules, which pair with the UPF-23-12, only have 2 input pins available, making IQUART the only control protocol available for the pulsing functionality. When we release larger propellers for larger modules, there will be more inputs available, allowing for communication via DroneCAN.

#. What size vehicle should I have in mind for the UPF-23-12?
    * While there are various vehicles that can be built around the UPF-23-12, we designed the propeller to optimize flight time and payload capacity for a 250g vehicle. The specific vehicle in mind was a 2-motor helicopter that could replace a quadrotor with an equivalent total propeller disc area, increasing flight time by 30% and payload capacity by up to 6x. Other possible vehicles include 6 DOF quadrotors, coaxial vehicles, tailsitters, and more!

#. Are other propeller sizes available?
    * We are developing new propellers, but the UPF-23-12 is the only one available for purchase at this time.

#. What are the different styles of underactuated propellers available?
    * Flapping: The UPF-23-12 is a flapping propeller. It has the main hub hinge and two flap hinges at its blades. The flapping version vectors both forces and torques. If you move the flapping hinges closer together, you vector relatively more force and less torque (until you reach the center, at which point it becomes a teeter hinge and only creates force). If you move the flapping hinges away from each other, you vector relatively more torque and less force (until the flapping hinges are so far away that they are beyond the tip of the propeller, at which point it becomes the rigid version and only generates torques on the vehicle).
    * Teeter: A teetering underactuated propeller has the main hub hinge and a single teeter hinge between the blades. 
    * Rigid: A rigid underactuated propeller just has the main hub hinge. This version only creates torques on the vehicle.