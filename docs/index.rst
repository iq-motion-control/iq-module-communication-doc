.. Python API documentation master file, created by
   sphinx-quickstart on Wed Mar 17 12:58:20 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


.. toctree::
   :caption: API Documentation
   :hidden:

   api_docs/getting_started_with_apis
   api_docs/python
   api_docs/arduino
   api_docs/cpp
   api_docs/matlab
   

.. toctree::
   :caption: Modules
   :maxdepth: 2
   :hidden:
   
   modules/vertiq_23xx_family
   modules/vertiq_40xx_family
   modules/vertiq_60xx_family
   modules/vertiq_81xx_family
   modules/fortiq_42XX

.. toctree::
   :caption: IQ Control Center
   :hidden:

   control_center_docs/control_center_start_guide
   control_center_docs/speed_module_getting_started
   control_center_docs/servo_module_getting_started
   control_center_docs/pulsing_module_getting_started

.. toctree::
   :caption: Vertiq Testing Tool
   :hidden:

   vertiq_testing_tool/vertiq_testing_tool_start_guide

.. toctree::
   :caption: Messaging Protocols
   :hidden:

   communication_protocols/iquart_protocol
   communication_protocols/dronecan_protocol
   communication_protocols/canopen_protocol
   communication_protocols/hobby_protocol

.. toctree::
   :hidden:
   :caption: Tutorials

   tutorials/pwm_control_flight_controller
   tutorials/hobby_calibration
   tutorials/dronecan_flight_controller
   tutorials/ifci_px4_flight_controller
   tutorials/extracting_log_qgroundcontrol
   tutorials/dronecan_firmware_upgrade
   tutorials/flight_controller_telemetry_tutorial
   tutorials/up12_installation
   tutorials/up12_initial_configuration
   tutorials/motor_noise_debugging
   tutorials/vibration_and_jittering

.. toctree::
   :hidden:
   :caption: Feature Reference Manual

   manual/manual_intro
   manual/manual_angle_control_mechanisms
   manual/manual_velocity_control_mechanisms
   manual/manual_safety_systems
   manual/manual_telemetry
   manual/manual_throttle
   manual/manual_advanced_arming
   manual/manual_stow
   manual/manual_timeout
   manual/manual_zero_spin
   manual/manual_adc_interface
   manual/manual_high_power_pwm_output
   manual/manual_gpio_interface
   manual/manual_step_direction
   manual/manual_ifci_control
   manual/manual_underactuated_torque_correction
   manual/manual_buzzer_control
   manual/manual_stop_detection
   manual/manual_stock_led
   manual/manual_appendix

.. toctree::
   :hidden:
   :caption: IQUART Client Table Reference

   client_reference/client_reference_table

Welcome to Vertiq's Documentation
============================================
The purpose of this site is to get your module spinning as quickly as possible, and to provide
you with additional tutorials on accessing your module's more advanced features.

Looking to get started with your module for the first time, or just for a refresher on the features supported by your module? Please click on the image of your
module found below, and you'll be walked through first time setup, the correct Getting Started manual to get spinning, and a list of applicable features, clients, and tutorials.

Already an experienced user looking for help with a specific feature or behavior? Please use the Feature Reference Manual or the Tutorials meant for your
module found on your module's family page.

Looking for guidance on using our Python, C++, Arduino, or Matlab APIs, or need help with the IQ Control Center configuration software? Please 
use the guides found on the left hand side.

Need to contact us? Please use support@vertiq.co.

.. _module_families:

.. list-table:: Vertiq Module Families
   :class: borderless
   :align: center

   * - .. figure:: ./_static/module_pictures/23xx/23xx_family.png
            :target: modules/vertiq_23xx_family.html
            :alt: Vertiq 23-XX Family
            :align: center

            Vertiq 23-XX Family
     - .. figure:: ./_static/module_pictures/40xx/40xx_family.png
            :target: modules/vertiq_40xx_family.html
            :alt: Vertiq 40-XX Family
            :align: center
            
            Vertiq 40-XX Family
     - .. figure:: ./_static/module_pictures/60xx/60xx_isolated.png
            :target: modules/vertiq_60xx_family.html
            :alt: Vertiq 60-XX Family
            :align: center
            
            Vertiq 60-XX Family
     - .. figure:: ./_static/module_pictures/81xx/81xx_family_index.png
            :target: modules/vertiq_81xx_family.html
            :alt: Vertiq 81-XX Family
            :align: center
            
            Vertiq 81-XX Family
     - .. figure:: ./_static/module_pictures/fortiq_family.png
            :target: modules/fortiq_42XX.html
            :alt: Vertiq Fortiq Family
            :align: center
            
            Vertiq Fortiq Family

Reporting Documentation Issues
******************************
We try our best to keep this documentation as clear, concise, and updated as possible.
If you come across any typos, bugs in example code, confusing verbiage, or simply have a question on how to do something, 
Please report it on our public github issues page!

`Reporting Issues or Questions about the Documentation <https://github.com/iq-motion-control/iq-module-communication-doc/issues>`_