********************************
Vertiq 81-XX Family
********************************

.. image:: ../_static/module_pictures/81xx_family.png
        :alt: Vertiq 81-XX Family
        :width: 400
        :align: center

.. csv-table:: Vertiq 81-XX Gen 1 Family of Modules
        :header: "Size", "Kv", "Default Firmware", "Available Firmware"
        :align: center

        "81-08", "85", "Speed", "Speed, Servo"
        "81-08", "150", "Speed", "Speed, Servo"
        "81-08", "220", "Speed", "Speed, Servo"

.. csv-table:: Vertiq 81-XX Gen 2 Family of Modules
        :header: "Size", "Kv", "Default Firmware", "Available Firmware"
        :align: center

        "81-08", "85", "Speed", "Speed, Servo"
        "81-08", "140", "Speed", "Speed, Servo"
        "81-08", "240", "Speed", "Speed, Servo"

Hardware Setup Walkthrough
==============================

Changing or Updating Firmware
==============================

Please follow the instructions found in :ref:`updating_firmware`

Getting Started
==============================

Complete the correct "Getting Started Guide" for your module's style (if you are using a servo module, please complete "Getting Started with Vertiq Servo Modules")

        * :ref:`Getting Started with Vertiq Speed Modules <control_center_tutorial>`
        * :ref:`Getting Started with Vertiq Servo Modules <manual_angle_control_mechanisms>`

.. I am putting these just to have some sort of placeholder link. Eventually we'll have actual getting started manuals

More Features
=====================

Once you have completed the proper "Getting Started Guide," you can begin to dive deeper into your module's capabilities. Below, you will find
a summary of all features supported on your module, IQUART Clients it can reach, as well as applicable tutorials. Please ensure that you are reading the feature
summary for your module's style.

Speed Module
-----------------
Supported Features
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        * :ref:`manual_angle_control_mechanisms`
        * :ref:`manual_velocity_control_mechanisms`
        * :ref:`manual_iquart`
        * :ref:`manual_dronecan`
        * :ref:`manual_hobby`
        * :ref:`manual_telemetry`
        * :ref:`manual_throttle`
        * :ref:`manual_advanced_arming`
        * :ref:`manual_stow_position`
        * :ref:`manual_timeout`
        * :ref:`manual_zero_spin`
        * :ref:`controlling_ifci`
        
Supported IQUART Clients
^^^^^^^^^^^^^^^^^^^^^^^^^^
        * :ref:`system_control`
        * :ref:`persistent_memory`
        * :ref:`serial_interface`
        * :ref:`brushless_drive`
        * :ref:`propeller_motor_controller`
        * :ref:`multi_turn_angle_control`
        * :ref:`esc_propeller_input_parser_ref`
        * :ref:`buzzer_control`
        * :ref:`power_monitor`
        * :ref:`temperature_monitor_microcontroller`
        * :ref:`hobby_input`
        * :ref:`temperature_estimator`
        * :ref:`uavcan_node`
        * :ref:`coil_temperature_estimator`
        * :ref:`power_safety`
        * :ref:`stow_user_interface`
        * :ref:`arming_handler`
        * :ref:`stopping_handler`
        * :ref:`iquart_flight_controller_interface`
        
Supported Tutorials
^^^^^^^^^^^^^^^^^^^^^^^^^^
        * :ref:`control_center_tutorial`
        * :ref:`hobby_fc_tutorial`
        * :ref:`hobby_calibration_tutorial`
        * :ref:`dronecan_fc_tutorial`
        * :ref:`fc_telemetry_tutorial`
        * :ref:`motor_noise_debugging`

Servo Module
----------------------------------------------
Supported Features
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        * :ref:`manual_angle_control_mechanisms`
        * :ref:`manual_velocity_control_mechanisms`
        * :ref:`manual_iquart`
        * :ref:`manual_hobby`
        * :ref:`manual_timeout`

Supported IQUART Clients
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        * :ref:`system_control`
        * :ref:`persistent_memory`
        * :ref:`serial_interface`
        * :ref:`brushless_drive`
        * :ref:`multi_turn_angle_control`
        * :ref:`buzzer_control`
        * :ref:`power_monitor`
        * :ref:`anticogging`
        * :ref:`temperature_monitor_microcontroller`
        * :ref:`hobby_input`
        * :ref:`temperature_estimator`
        * :ref:`servo_input_parser_ref`
        * :ref:`coil_temperature_estimator`
        * :ref:`power_safety`

Supported Tutorials
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^