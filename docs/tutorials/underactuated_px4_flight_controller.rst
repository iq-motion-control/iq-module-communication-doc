.. include:: ../text_colors.rst
.. toctree::

.. _underactuated_px4_flight_controller:

*************************************************************
Setting up PX4 Firmware for Use with Underactuated Propellers
*************************************************************

This tutorial will cover how to set up an underactuated propeller with PX4. Before this tutorial is completed, ensure that your flight controller is flashed with a PX4 version that can use the :ref:`IQUART Flight Controller Interface (IFCI)<controlling_ifci>` and has access to the underactuated propeller settings. More information on building PX4 for this can be found in the :ref:`Setting up PX4 Firmware for Use with IFCI <ifci_px4_flight_controller>` document.


.. set up motor -- all done in ifci px4 flight controller or 
..      ensure the motor is flashed with underactuated
..      If using multiple motors ensure that the IDs are different
..      Set the baudrate to 921600 or higher
..      Ensure the proper default file is selected
..      point forward and zero offset
..      More information can be found here *up12 setup page*
..  
.. set up px4 -- all done in ifci px4 flight controller
..      assume flashed with correct stuff
..      enable Vertiq stuff on correct port
..          vertiq io cfg? 
..      set vertiq_baud to matching baud (921600)
..      set num cvs
..      set telemetry ids
..      set Disarm trigger

..      if using a teeter propeller use motors (6dof)
..      else use custom 6dof
.. set up hardware connection
..      plug things into the right port

.. set up underactuated propeller
..      set control method for throttle and pulse for each motor
..      set CVIs for throttle, X, and Y
..      go to actuators page
Setting Up the Motor Module
=============================

Set up PX4
=============================

Set up the Hardware Connection
===============================

Setting Up the Underactuated Propeller with PX4
===============================================
Once the flight controller and motor are set up to communicate via IFCI, power on the system and connect to the flight controller using QGroundControl. Go to the flight controller setup menu and in the parameter menu select the Vertiq IO section on the left hand menu.

pix of that menu selection

Change target module ID to the the module you are attempting to control. The values in the menu should update. Change 'Control_Mode' to 'Velocity' and 'Pulse_Volt_Mode' to 'Voltage Limit Mode'. Set the 'Max_Velocity' to the desired maximum velocity in rad/s and the 'Pulse_Volt_Lim' to the maximum desired rad/s for pulsing. For each motor set the 'THROTTLE_CVI', 'X_CVI', and 'Y_CVI' to different ESC #s. It is recommended that you start with ESC 1 (corresponding to a CVI of 0).

pics of this

Set 'TRIGGER_READ' to enable and confirm that the changes have been saved.

Finally, change the parameter 'CA_AIRFRAME' to 'Motors (6DOF)'.

pic of this

After all of these settings have been set, go to the 'Actuators' page on the left menu. For each underactuated propeller, add 3 motors in the geometry section. Select the 'Advanced' checkbox in the top right. Assuming the rotor is facing upwards, for throttle motor select the direction as 'Upwards', for the X select 'Forwards', and for the Y select 'Leftwards'. Set the position X, Y, and Z position to the position of the rotor on the aircraft. They should all have the same location settings for the associated throttle, X, and Y thrusts. If you plan on using rotor differential throttle for Yaw control, set the moment coefficient. For a CCW rotor, set it positive. For a CW rotor, set it negative. For the X and Y thrusts, set the moment coefficient to 0.

On the Actuator Ouputs tab, select 'Vertiq IO'. Determine which ESC outputs you will be using for the motors in the Geometry section. The ESC # will be the corresponding CVI # + 1. For the ESC #, select the appropriate motor function. For example, if the throttle CVI is set to 0 and the motor function associated with upwards thrust is Motor 1, set ESC #1 (CVI 0 + 1) to a function of Motor 1. If the X CVI is set to 1 and the motor function associated with forwards thrust is Motor 2, set ESC #2 (CVI 1 + 1) to Motor 2. Continue this for all underactuated propellers you will be using.

Pic of final settings

Now there are two tests to confirm that you have set this up correctly. First use the actuator testing section in the bottom left of the actuators tab. With the aircraft securely strapped down, confirm that when the motor number associated with the throttle is raised, the propeller starts spinning. Now confirm that the motor number associated with X causes the propeller to tilt directly fowards, and then one associated with Y causes the propeller to tilt directly leftwards. If any of these are swapped, the easiest way to fix this is to just vary the 'forwards' and 'leftwards' parameters to match what they actually cause the propeller to do.

video? pic?

Once the first test has been done and the results match the expected results, the goal is to confirm that the propellers are reacting as expected when roll, pitch, yaw, and throttle commands are sent. To do this, you must keep the vehicle strapped down securely and force arm the vehicle with QGroundControl. Using the sticks (or virtual sticks), confirm that the the correct control axes control the expected propeller axes.