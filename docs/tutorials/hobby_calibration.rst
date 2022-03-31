.. include:: ../text_colors.rst
.. toctree::

.. _hobby_calibration_tutorial:

***********************************************
Calibrating Modules With Analog Hobby Protocols
***********************************************

IQ modules can be controlled with analog hobby protocols, such as PWM and OneShot. These protocols send throttle commands to the module using a square wave or pulse of variable duration,
where the length of the wave determines the throttle input. For example, when using PWM, by default IQ modules interpret a pulse of 1000 us indicates as 0% throttle, and a pulse of 2000 us
as 100% throttle.

However, the exact pulse duration used to represent the endpoints of the throttle range can vary between different controllers. One controller may consider 1000 us to 2000 us to be the 0%
to 100% range, but another may consider 950 us to 1950 us to be the range. For a controller to properly control an IQ module, they must agree on this input range. In some instances you
can edit the range used by the controllerm as discussed for Ardupilot and PX4 based flight controllers in this tutorial: :ref:`_hobby_fc_tutorial`. 

Another option is to adjust the input range of the IQ module so that it matches the controller. This is commonly done on a range of ESCs, and is known as "calibration". This tutorial
will cover the calibration process for an IQ module.

..
    Now how the heck do I go about structuring this. Hmm. Do I Just talk about the process at a high level and then demonstrate with something like a Maestro? Or on a flight controller?
    Definitely a video of the calibraton process would be good
    Should also definitely talk about the calibration tools included in the Advanced tab, very useful.

Title
=====