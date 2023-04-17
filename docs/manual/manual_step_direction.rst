.. include:: ../text_colors.rst
.. toctree::

.. _manual_step_direction:

**************************************
Step/Direction
**************************************

Module Support
===================

Step/Direction Modules
************************

.. table:: Step/Direction Modules

	+-----------------+--------------------------------------------------+
	| Module          |     Notes                                        |
	+-----------------+--------------------------------------------------+
	| Vertiq 2306     | Available by request only                        |
	+-----------------+--------------------------------------------------+
	| Vertiq Fortiq42 | Available only while using the step/dir firmware |
	+-----------------+--------------------------------------------------+

Description
============
Step/Direction control is a two wire interface with two inputs: step and direction. On each received step pulse, the module will move a set radial distance in the direction specified by the direction line (clockwise or counter clockwise). 

The distance traveled on each received step pulse is defined by the *Step Angle Size* parameter. This value is user configurable. 

.. note::
	
	Unlike :ref:`manual_hobby`, while using Step/Direction control with a Fortiq-42 module, serial communication through IQUART remains available. 

.. note::
	By using step/dir firmware, the motor can only be controlled via step/dir instructions. Attempts to spin, stop, or otherwise control the motor via other protocols will fail. You can still communicate with the module via IQUART or CANOpen (if available), but cannot control the module. Hobby protocols are disabled when using step/dir firmware.

IQ Control Center
==================
The IQ Control Center provides the easiest way to configure step/direction control on your module. To do so:

#. Open IQ Control Center. If you have not installed the program, please follow the instructions in `Getting Started with Speed Motors Using IQ Control Center <https://iqmotion.readthedocs.io/en/latest/tutorials/testing_with_control_center.html>`_. 

#. Connect your module to IQ Control Center

#. Click the General tab on the left side

#. To configure the step size, adjust the *Step angle* parameter

Vertiq Python API - Step/Direction Interface
==============================================
.. note::
	Please note that the following *step/dir* testing was performed with a Fortiq-42 module. Your exact commands may change depending on the module in use.

The step/direction interface can also be accessed through Vertiq’s Python API's *step_direction_input* client, and is summarized by the following table:
	
	.. table:: Step/Direction Input Client 

		+-----------------+------------------+--------+------------------------------------------------------------------+
		| Entry           | Access           | Format | Description                                                      |
		+-----------------+------------------+--------+------------------------------------------------------------------+
		|angle            | Read             | Float  | The module's current position in radians                         |
		+-----------------+------------------+--------+------------------------------------------------------------------+
		|angle_step       | Read/Write/Save  | Float  | The distance, in radians, to rotate on each received step signal |
		+-----------------+------------------+--------+------------------------------------------------------------------+



#. Set up your local computer to use the Python API using the instructions found at `Getting Started with Python <https://iqmotion.readthedocs.io/en/latest/langs/python.html>`_

#. After completing the walkthrough, you can interact with the *step_direction_input* client described above

#. To read a value

	.. code-block::

		print(fortiq.get("step_direction_input", "<entry_name>"))

	For example, to read the value stored in *angle*:

	.. code-block::  
	
		print(fortiq.get("step_direction_input", "angle"))

#. To write a value

	.. code-block::
	
		fortiq.set("step_direction_input", "<entry_name>", value)
	
	For example, to set the *angle_step*

	.. code-block:: 

		fortiq.set("step_direction_input", "angle_step", 0.001)

#. To save a value to persistent memory

	.. code-block::

		fortiq.save("step_direction_input", "<entry_name>")

	For example, to save the value in *angle_step*

	.. code-block::
		
		fortiq.save("step_direction_input", "angle_step")