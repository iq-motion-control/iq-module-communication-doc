.. include:: ../text_colors.rst
.. toctree::

.. _correcting_underactuated_torque:

.. |propeller_name| replace:: UP12
.. |motor_name| replace:: Vertiq 2306

***********************************************
Underactuated Propeller Torque Angle Correction
***********************************************

Underactuated propellers can vector force, torque, or a combination of the two. The flapping blades on the |propeller_name| vectors both a force and torque vector. These two vectors are coupled and always point the same direction relative to each other. In the case of the |propeller_name| they are approximately 82 degrees apart. This means that the torque vector and torque induced by the force vector happen at slightly different angles. Depending on the height of the propeller above the Center of Mass (CoM), the resulting torque will point in different directions. Some plots showing different heights above the center of mass show how the final torque direction and magnitude can change.

.. figure:: ../_static/manual_images/pulsing_propeller/torque_angle_0.png
    :align: center
    :scale: 50
    :alt: CCW |propeller_name| 0mm above CoM

    CCW |propeller_name| 0mm above CoM

.. figure:: ../_static/manual_images/pulsing_propeller/torque_angle_50.png
    :align: center
    :scale: 50
    :alt: CCW |propeller_name| 50mm above CoM

    CCW |propeller_name| 50mm above CoM

.. figure:: ../_static/manual_images/pulsing_propeller/torque_angle_100.png
    :align: center
    :scale: 50
    :alt: CCW |propeller_name| 100mm above CoM

    CCW |propeller_name| 100mm above CoM

The graphs show that as the propeller is moved higher above the CoM, the more the force contributes to the final torque. Therfore a correction factor must be introduced.

The defaults files for each underactuated propeller include calibrated a parameter called 'propeller_torque_offset_angle'. This parameter is found in the :ref:`Voltage Superposition Client<vsp_message_table>`. This default calibrated value assumes that the propeller is at the CoM of the aircraft. This is most likely incorrect for the vast majority of aircraft, but there is no standard height and most aircraft will be different. To correct for this the default calibrated parameter must be offset using the equation below:

.. math::

   -tan^{-1}\biggl(\genfrac{}{}{}{}{7.6h}{h+1.709}\biggr)

In this equation 'h' is the height of the propeller blades above the aircraft's CoM in meters. If the propeller is above the vehicle's center of mass the calculation should result in a negative number. This value must be calculated in radians and then added to the calibrated 'propeller_torque_offset_angle' value which is also in radians. The blades are 14.81mm above the base of the propeller. A more complete drawing is shown below to allow for proper calculation with CAD models of your aircraft.

.. figure:: ../_static/manual_images/pulsing_propeller/propeller_drawing.png
    :align: center
    :scale: 50
    :alt: |propeller_name| height drawing

    |propeller_name| height drawing