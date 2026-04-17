.. include:: ../text_colors.rst
.. toctree::

.. _manual_pulsing:

***********************************************
Pulsing Based Control Mechanisms
***********************************************

Vertiq offers pulsing firmware available for the :ref:`23-XX family <vertiq_23xx_family>` which can be used with the following propellers: UPT-23-10, `UPF-23-12 <https://www.vertiq.co/upf-23-12>`_

There are two styles of Vertiq’s pulsing control firmware:

#. Velocity based (all pulsing firmware versions 0.1.0 and later)
#. Voltage based (all pulsing firmware 0.0.27 and earlier)

We highly recommend using velocity based pulsing firmware. You can find all pulsing firmware on the Vertiq website on your module's `product page <https://www.vertiq.co>`_ under firmware.

Velocity Based Pulsing
======================

For more of a background on velocity control, please refer to :ref:`Velocity and Voltage Based Control Mechanisms <manual_velocity_control_mechanisms>`.

Velocity based pulsing works by commanding the module to a baseline velocity and then adding a sine wave of a specified pulsing velocity that increases and decreases the velocity at a moment in time which tilts the propeller.

.. figure:: ../_static/manual_images/pulsing/components.png
    :align: center
    :scale: 75
    :alt: The module and pulsing velocities

    The module and pulsing velocities

.. figure:: ../_static/manual_images/pulsing/result.png
    :align: center
    :scale: 75
    :alt: The resulting velocity with reference to the module velocity

    The resulting velocity with reference to the module velocity

.. note:: When using velocity based pulsing, only velocities can be used to command a pulse.

Voltage Based Pulsing
=====================

For more of a background on voltage control, please refer to :ref:`Velocity and Voltage Based Control Mechanisms <manual_velocity_control_mechanisms>`.

Voltage based pulsing works by commanding the module to a baseline voltage and then adding a sine wave of a specified pulsing voltage that increases and decreases the voltage at a moment in time which tilts the propeller.

.. note:: When using voltage based pulsing, only voltage can be used to command a pulse.

Velocity Pulsing Demo
=====================

    .. warning::
        Please remove all propellers from any module you plan on testing. Failure to do so can result in harm to you or others around you. Further, please ensure that your 
        module is secured to a stationary platform or surface before attempting to spin it. 

For this example, we are using 23-06 pulsing firmware v0.2.0 which can be found on the `23-06 product page <https://www.vertiq.co/23-06-g1>`_. To observe the difference with the module spinning with velocity based pulsing, connect your module without a propeller to Control Center. Go to the Testing tab and set Velocity to 100 rad/s. Then in the same tab, set Pulsing Velocity to 30 rad/s. This can be seen in the screenshot below.

.. figure:: ../_static/manual_images/pulsing/control_center_pulse_example.png
    :align: center
    :scale: 75
    :alt: Control Center pulsing example

    Control Center pulsing example

You should now hear a difference in the module as it adjusts its velocity to pulse. To compare, you can set Pulsing Velocity to 0 to spin the module normally. This can also be seen in the video below.

.. raw:: html

    <style type="text/css">
    .center_vid {   margin-left: auto;
                    margin-right: auto;
                    display: block;
                    width: 75%; 
                }
    </style>
    <video class='center_vid' controls><source src="../_static/manual_images/pulsing/control_center_pulse_example.mp4" type="video/mp4"></video>

Next steps
==========

To continue setting up a pulsing module follow the next steps:

* :ref:`Setting Up PX4 and ArduPilot Firmware with IFCI Intregration <ifci_px4_flight_controller>`
* :ref:`IFCI Integration with PX4 and ArduPilot <ifci_integration>`
