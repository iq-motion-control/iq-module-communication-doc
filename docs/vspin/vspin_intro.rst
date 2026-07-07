.. include:: ../text_colors.rst
.. toctree::

.. _vspin_intro:

#################################
What is VSpin?
#################################
Vertiq is introducing, for customer evaluation, a new style of firmware which significantly changes the fundamental motor control enabling new features and other potential improvements. 
This new structure introduces control based on torque or motor current targets, field weakening, and changes to how the safety systems operate. This new style of firmware is called “VSpin” to distinguish it from previous firmware styles. 

The documentation below will provide a brief overview of some of the changes that come with this new firmware style and links to more complete descriptions of these changes. 

.. note::
    **VSpin is not currently intended as a strict upgrade from existing stock speed or servo firmware and does not obsolete those styles of firmware.** This is a parallel firmware style option that is being made available 
    for evaluation and feedback for customers who may benefit from these specific changes. **This is not simply a drop-in upgrade for any systems using the current stock speed or servo firmware as parameter and 
    tuning changes may be required to achieve identical performance.**

******************************************************
Is VSpin Meant To Replace The Existing G2 Firmwares?
******************************************************
As noted above, for the G2 modules, VSpin style firmware is not intended as a direct replacement or upgrade for the existing stock firmware. It is a separate style of firmware being developed in parallel along with continued support 
of our existing stock speed and servo firmwares. VSpin firmware is being made available to gather feedback and help customers who may potentially benefit from its unique features, but the existing speed and servo firmware styles are 
and will continue to be the primary firmware styles for the G2 modules. VSpin does not yet have the extensive amount of customer field testing that our pre-existing stock firmware styles have, so it is a less mature system. 
The existing stock speed and servo firmwares can and should continue to be used by existing and new customers who are not interested in exploring the features offered by VSpin. Those styles of firmware are not obsoleted or replaced by VSpin.

******************************************************
What Has Changed?
******************************************************
VSpin style firmware goes beyond just adding new features, it fundamentally changes the underlying approach to motor control. While our stock speed and servo firmwares ultimately control the drive with a focus on producing a voltage command, 
VSpin is focused on producing a motor current command. It can still accept voltage commands in the same way as existing firmwares, but how it executes that command in the back-end has changed significantly.

This means that a module’s response to the same command may be different on VSpin when compared to our existing stock firmware. **Because of this, it is important to realize that swapping to VSpin style firmware is not necessarily a drop-in replacement 
if you have a system that you have previously tuned on our other firmware. This applies to all** :ref:`Modes <throttle_mode>`, **including Voltage and PWM, not just Velocity mode, since the underlying drive control is what has changed, not just the closed loop velocity controller.**

Beyond potentially changing the module’s response, these back-end modifications lead to some other changes in available features and module performance. These are covered in more detail in other parts of this documentation, but a brief overview is provided below:

* The :ref:`Brushless Drive client <brushless_drive>` is no longer supported. The functionality of this client has been split among various new clients. More information can be found :ref:`here <vspin_transition_guide>`.
* Automatic Field Weakening can now be enabled on all modules. This makes it possible to spin faster than would typically be possible given your supply voltage at the cost of decreased efficiency. This can be useful for bursting to more aggressive setpoints, but must be used with care due to the decreased efficiency. More information can be found :ref:`here <vspin_automatic_field_weakening>`. 
* Torque and motor current commands are now available in addition to the :ref:`voltage and velocity <throttle_mode>` commands that were previously available.These are used for specifying a specific motor current or torque the module should attempt to maintain. Motor current is directly proportional to torque based on the motor’s Kt, so the only difference between torque and motor current commands is the command’s units. This can be useful for applications where controlling torque specifically is more important than speed. More information can be found :ref:`here <vspin_command_current_and_torque>`. 
* The :ref:`closed loop velocity controller <manual_velocity_control_mechanisms>` and :ref:`closed loop position controller <manual_angle_control_mechanisms>` now output a motor current command instead of a voltage command. This means that the units of the controllers’ gains have changed, and so the specific gains used for any tuning will be different. More information can be found :ref:`here <vspin_tuning_changes>`. 
* The performance of the module’s various safeties have changed. Safeties that cause derates now limit the maximum allowed motor current when derating instead of immediately reducing the incoming command. A motor current slew rate limiter is now available as well instead of a voltage slew rate limiter. More information can be found :ref:`here <vspin_safeties>`. 

******************************************************
What Has Not Changed?
******************************************************
The majority of features and the high-level interface for controlling Vertiq modules is not altered by this new style of firmware. Unless explicitly specified, features that have existed previously can be assumed to be unchanged between the existing speed firmware and the VSpin firmware. 

The modules can be controlled from a flight controller in the same way, and the interface for controlling modules using the :ref:`Vertiq APIs <getting_started_with_apis>` with :ref:`Propeller Motor Control <propeller_motor_controller>` or :ref:`Multi Turn Angle Control <multi_turn_angle_control>` has also not changed. For the majority of users, how they command setpoints to their module will not need to change. 

.. note::
    If using velocity or position control, you may need to change the values of your gains when commanding velocity or position setpoints. See :ref:`here <vspin_tuning_changes>` for more information.

The only case where the interface for commanding the modules will need to change significantly is if the :ref:`Brushless Drive client <brushless_drive>` was previously used to interact directly with the client through the API. That client no longer exists on the VSpin firmware, so it cannot be used to command the module. Details on transitioning to using new API clients are provided :ref:`here <vspin_api_transition>`.

******************************************************
Where Can I Get VSpin Firmware?
******************************************************
VSpin firmware can be downloaded from your module's page on our `website <https://www.vertiq.co/>`_.

******************************************************
How Do I Flash A Module With VSpin Firmware?
******************************************************
If you wish to evaluate the VSpin firmware, you can program your module with the VSpin firmware using the :ref:`Control Center <updating_firmware_control_center>` in the same way as all of our other firmware releases. The Control Center supports connecting to and flashing modules with the VSpin firmware.

.. note::
    If you are evaluating transitioning from the stock firmware to VSpin firmware, it is recommended to save your stock firmware configuration using the :ref:`Control Center <user_generated_control_center_defaults>` so you can easily swap back to your original configuration.
