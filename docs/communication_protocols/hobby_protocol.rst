.. include:: ../text_colors.rst
.. toctree::

.. _hobby_protocol:

***********************************************
Hobby Protocols
***********************************************

Hobby protocols refers to an array of different protocols commonly used by drone hobbyists for controlling ESCs. Vertiq modules support a selection of the 
most popular of these protocols. Some details on these protocols and how Vertiq modules support them is provided below.

.. note::

    Please note that no two hobby protocols can be used together as :ref:`redundant throttle sources <redundant_throttle_manual>`. Only one 
    hobby protocol can be accepted at any time.

Module Support
===============

Speed Modules
**************

.. include:: ../manual/all_checked_table.rst

Servo Modules
**************

.. include:: ../manual/all_checked_table.rst

Supported Hobby Protocols
==========================

.. _hobby_standard_pwm:

Standard PWM
*************
Standard PWM refers to a very commonly used analog protocol that uses a 1000 microsecond to 2000 microsecond pulse to send throttle commands. This pulse encodes a number 
from 0.0 to 1.0, where typically a 1000 microsecond pulse represents 0.0, a 1500 microsecond pulse represents 0.50, and a 2000 microsecond pulse represents 1.0. Exactly how Vertiq modules convert this number into a command 
depends on the configurations of the module. Refer to the :ref:`throttle_mapping` section for more details on that mapping in speed modules and 
the :ref:`throttle_def` section for more details on how a throttle command is defined by speed modules.

These endpoints can be calibrated to change how incoming pulse widths are interpreted, e.g. you could calibrate 1200 microseconds to map to 0.0 and 2000 microseconds to map to 1.0 instead. See 
the `Analog Protocol Calibration`_ section for more details on calibration.

.. _hobby_dshot:

DSHOT
******
DSHOT is a digital protocol that is gaining in popularity and is supported by a wide range of flight controllers. It makes it possible to quickly send throttle commands to ESCs 
as well as a host of other special messages. The specification for DSHOT is fairly complex, but a good overview can be found in the `“D-SHOT - The Missing Handbook” article from 
Chris Landa <https://brushlesswhoop.com/dshot-and-bidirectional-dshot/>`_. Similarly to Standard PWM, the 11 bit throttle command included in a DSHOT message is interpreted by a Vertiq module as encoding a number from 0.0 to 1.0. 
Exactly how Vertiq modules convert this number into a throttle command depends on the configurations of the module. Refer to the :ref:`throttle_mapping` section for 
more details on that mapping in speed modules and the :ref:`throttle_def` section for more details on how a throttle command is defined by speed modules.

**Vertiq modules support DSHOT, but they do not support Bidirectional DSHOT.** See the linked article from Chris Landa above for more details on the difference between the two protocols. 

DSHOT has various different speeds, currently Vertiq modules support:

* DSHOT150
* DSHOT300
* DSHOT600
* DSHOT1200

.. _hobby_other_protocols:

Additional Protocols
*********************
Vertiq modules also support several other less commonly used hobby protocols:

* OneShot125
* OneShot42
* Multishot

Similarly to `Standard PWM`_, the commands sent by these protocols are interpreted by a Vertiq module as encoding a number between 0.0 to 1.0. Exactly how Vertiq modules convert 
this number into a throttle command depends on the configurations of the module. Refer to the :ref:`throttle_mapping` section for more details on that mapping for speed modules and 
the :ref:`throttle_def` section for more details on how a speed module defines a throttle command.

Interaction with Serial Communication
======================================
The connector used for inputting hobby protocols on Vertiq modules is also used for serial communication. Serial communication is used for connecting to the 
modules with IQ Control Center or interacting with them using the Vertiq APIs. 

**Because the connector is shared between hobby protocols and serial communication, only one of them can be active at any time.** On startup, the module will 
look for valid messages of either the currently configured hobby protocol or the Vertiq serial protocol. **When it detects the first valid message of 
either type, it will lock-on to that type of communication, and stop listening for any other types of messages on that physical interface. This behavior is slightly different 
when using the** *Autodetect* **setting for the** *Communication* **parameter, see the note below.** Also note that for modules that support DroneCAN, DroneCAN uses a separate 
physical interface from hobby protocols and serial communication, so DroneCAN communication can still be used when using hobby protocols.

.. note:: When using *Autodetect* as the setting for the module's *Communication* parameter, the module will not recognize incoming hobby protocol messages as valid until it
    receives approximately 100 of them. So if using *Autodetect*, the module will not lock-on to using hobby protocols instead of serial communication on the first hobby protocol
    message as described above. It will take over 100 hobby protocol messages before the module locks-on to using the hobby protocol and shuts off serial communication. When using any other
    setting for the *Communication* parameter, the first hobby protocol message will trigger the lock-on. If you know which hobby protocol you want to control the module with,
    it is recommended to explicitly set the module to use that hobby protocol and avoid using the *Autodetect* setting.

For example, if after startup a DSHOT message is the first thing sent to the module, and the module is not set to use *Autodetect* (see the note above), it will then only listen for 
additional DSHOT messages, ignoring any serial communication. The module must be rebooted before you can switch which protocol it will listen to. **This means that you cannot connect to 
the IQ Control Center after controlling the module with a hobby protocol unless you reboot, and vice versa.** 

Module Configuration
======================
The :ref:`hobby_fc_tutorial` tutorial details important configuration parameters to set on speed modules when using PWM and DSHOT. 
Though that tutorial is focused on integration with flight controllers, the information on module configuration for using hobby protocols is applicable for any kind of setup.

Analog Protocol Calibration
=============================
The :ref:`hobby_calibration_tutorial` tutorial on analog protocol calibration covers all of the steps of the Vertiq calibration procedure for speed modules and describes how to trim and reset 
the calibrated endpoints.

Arming
========
Hobby Protocols use the same advanced arming procedure as all other throttle sources on speed modules. The details of this arming procedure are covered in the :ref:`manual_advanced_arming` section.