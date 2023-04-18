.. include:: ../text_colors.rst
.. toctree::

.. _manual_gpio_interface_:

********************************************
GPIO Interface
********************************************

Module Support
================

The user GPIO interface is supported by only the Fortiq-42 modules.

Speed Modules
**************

.. table:: Speed Module Support for GPIO Interface

	+-------------+------------------------------------+
	| Module      | User GPIO Support                  |
	+-------------+------------------------------------+
	| Vertiq 8108 | .. centered:: |:x:|                |
	+-------------+------------------------------------+
	| Vertiq 4006 | .. centered:: |:x:|                |
	+-------------+------------------------------------+
	| Vertiq 2306 | .. centered:: |:x:|                |
	+-------------+------------------------------------+

Servo Modules
**************

.. table:: Servo Module Support for GPIO Interface

	+-----------------+------------------------------------------+
	| Module          | User GPIO Support                        |
	+-----------------+------------------------------------------+
	| Vertiq 8108     | .. centered:: |:x:|                      |
	+-----------------+------------------------------------------+
	| Vertiq 4006     | .. centered:: |:x:|                      |
	+-----------------+------------------------------------------+
	| Vertiq 2306     | .. centered:: |:x:|                      |
	+-----------------+------------------------------------------+
	| Vertiq Fortiq42 | .. centered:: |:white_check_mark:|       |
	+-----------------+------------------------------------------+

Description
===============
Vertiq’s GPIO interface provides a flexible method of interacting with a module’s user-specific GPIO pins. Each GPIO can be set to input or output, which can be switched on-the-fly, if desired. Each pin set as an input may choose whether or not to use an internal pull resistor (up or down), as well as the type of pull used. Each pin set as an output may choose whether to output in a Push-Pull or Open-Drain configuration. 

All pins set as an input can read the input value, and all pins set to output can read, write, and save the outgoing value. Access to these behaviors can be done either through registered or addressable methods. Both registered and addressable interactions occur via IQUART commands, and will be covered more below. Vertiq's GPIO interface has flexibility to change any GPIO parameter via the registers or addressable format. Any value set through the IQ Control Center will be saved to the module’s persistent memory, and will be retained through resets and power-cycles. Using Vertiq’s Python API (see :ref:`Vertiq Python API - GPIO Interface`), values can be written without saving, written with saving, and read.

Registered GPIO Access
=======================
The GPIO registers use one-hot encoding to specify which GPIO to affect. All registers excluding the Input Values (read only) have Read/Write/Save permissions. 
As an example, to set the bottom 3 GPIOs to output using Push-Pull, and output high on GPIO 3: set *GPIO Mode* to 7, set *PP/OD* to 0, and *Output Values* to 4.

The GPIO registers are summarized below:
	
	.. table:: GPIO Registers
		:widths: 16 20 20 20 20 20 20 20 20 6 50
		:class: tight-table

		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
		| Register Name | Bit 7            | Bit 6            | Bit 5            | Bit 4            | Bit 3            | Bit 2            | Bit 1            | Bit 0            | Access | Description                                                                                                                                                                       |
		+===============+==================+==================+==================+==================+==================+==================+==================+==================+========+===================================================================================================================================================================================+
		| GPIO Mode     | GPIO 8 Mode      | GPIO 7 Mode      | GPIO 6 Mode      | GPIO 5 Mode      | GPIO 4 Mode      | GPIO 3 Mode      | GPIO 2 Mode      | GPIO 1 Mode      | R/W/S  | Sets the GPIO pin to a specified mode, input or output. 0 indicates input, 1 indicates output                                                                                     |
		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
		| Input Values  | GPIO 8 Input     | GPIO 7 Input     | GPIO 6 Input     | GPIO 5 Input     | GPIO 4 Input     | GPIO 3 Input     | GPIO 2 Input     | GPIO1 Input      | RO     | A read only value that indicates the value of each input pin when set to input mode                                                                                               |
		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
		| Output Values | GPIO 8 Ouput     | GPIO 7 Ouput     | GPIO 6 Ouput     | GPIO 5 Ouput     | GPIO 4 Ouput     | GPIO 3 Ouput     | GPIO 2 Ouput     | GPIO 1 Ouput     | R/W/S  | The value to send/being sent on an output GPIO line                                                                                                                               |
		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
		| Use Pull      | GPIO 8 Use Pull  | GPIO 7 Use Pull  | GPIO 6 Use Pull  | GPIO 5 Use Pull  | GPIO 4 Use Pull  | GPIO 3 Use Pull  | GPIO 2 Use Pull  | GPIO 1 Use Pull  | R/W/S  | Determines whether or not to use the internal pull-up or pull-down resistor on input mode. If 0, use no pull, if 1 use the pull type specified by the corresponding Pull Type bit |
		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
		| Pull Type     | GPIO 8 Pull Type | GPIO 7 Pull Type | GPIO 6 Pull Type | GPIO 5 Pull Type | GPIO 4 Pull Type | GPIO 3 Pull Type | GPIO 2 Pull Type | GPIO 1 Pull Type | R/W/S  | Determines the type to use on an input pin, if Use Pull is set high. 0 indicates pull down, 1 indicates pull up.                                                                  |
		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
		| PP/OD         | GPIO 8 PP/OD     | GPIO 7 PP/OD     | GPIO 6 PP/OD     | GPIO 5 PP/OD     | GPIO 4 PP/OD     | GPIO 3 PP/OD     | GPIO 2 PP/OD     | GPIO 1 PP/OD     | R/W/S  | Determines the output mode, push-pull or open-drain. 0 indicates push-pull, 1 indicates open-drain                                                                                |
		+---------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+------------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


Addressable GPIO Access
==========================
User GPIOs allow for individual write access to each GPIO. Addressable access is a write only protocol. To set a value via addressable access, send one byte that represents the value to write as well as the GPIO to write to. The MSB represents the value, while the bottom 7 bits represent the GPIO number. For example, to set GPIO 3 to be an output, write 131\ :sub:`10` \ (0b10000011) to the GPIO Addressable - Mode entry (see :ref:`Initial GPIO Setup and Testing with IQ Control Center`)


Usage
============
.. _Initial GPIO Setup and Testing with IQ Control Center:

Initial GPIO Setup and Testing with IQ Control Center
*********************************************************
The IQ Control Center provides the easiest way to interact with your module’s GPIO peripherals. To do so: 

#. Open IQ Control Center. If you have not installed the program, please follow the instructions in `Getting Started with Speed Motors Using IQ Control Center <https://iqmotion.readthedocs.io/en/latest/tutorials/testing_with_control_center.html>`_. 

#. Connect your module to IQ Control Center

#. Select the Tuning Tab on the left side

	.. image:: ../_static/manual_images/fortiq/other/control_center_tuning.png

#. Scroll down in the Tuning Tab until you find *GPIO Addressable - Mode*
	
	.. image:: ../_static/manual_images/fortiq/gpio/control_center_gpio_tuning.png
		:height: 400

	This is where you will find access to both registered and addressable GPIO control. In order to read the current GPIO state, you must read from the register values, as addressable access is Write-Only. 

#. To test your GPIOs: 
	
	#. Create the following circuit

	.. image:: ../_static/manual_images/fortiq/gpio/gpio_sample_circuit.png
	
	|

	2. Write 7 to *GPIO Mode Register* to set all 3 GPIOs to output

	#. Make sure *GPIO Push Pull - Open Drain Register* is set to 0, indicating that all outputs are configured as push-pull

	#. Open the Testing Tab in the Control Center, and find *GPIO Addressable - Output*

		.. image:: ../_static/manual_images/fortiq/gpio/control_Center_gpio_testing.png

	#. Write 7 to *GPIO Output Register* to set all outputs high. You should see your LEDs light up
	#. Write 1 to *GPIO Addressable - Output* to turn off only GPIO 1
	#. Write 6 to GPIO Mode Register to set GPIO 1 to input and GPIOs 2 and 3 to output
	#. Write 129 to GPIO Addressable - Use-Pull to enable pull on GPIO 1
	#. Write 129 to GPIO Addressable - Pull-Type to set GPIO 1 to pull up
	#. Select the *Update Button* on GPIO Inputs. You should read 1
	#. Write 6 to GPIO Pull-Type Register to set GPIO1 to pull down. You should now read 0 on GPIO Inputs

.. _Vertiq Python API - GPIO Interface:

Vertiq Python API - GPIO Interface
***************************************

.. note::
	Please note that the following *GPIO Interface* testing was performed with a Fortiq-42 module. Your exact commands may change depending on the module in use.

The GPIO interface can also be used through Vertiq’s Python API. To use the API, please use the following steps:

#. Set up your local computer to use the Python API using the instructions found at `Getting Started with Python <https://iqmotion.readthedocs.io/en/latest/langs/python.html>`_

#. After completing the walkthrough, you can interact with the *gpio_controller* client summarized below

	+----------------------------------+-----------------+------------------------------------------------------------+
	| Entry                            | Access          | Description                                                |
	+==================================+=================+============================================================+
	| modes_register                   | Read/Write/Save | Access to the GPIO Modes register                          |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| inputs_register                  | Read            | Access to the GPIO Inputs register                         |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| outputs_register                 | Read/Write/Save | Access to the GPIO Outputs register                        |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| use_pull_register                | Read/Write/Save | Access to the GPIO Use-Pull Register                       |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| pull_type_register               | Read/Write/Save | Access to the GPIO Pull Type Register                      |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| push_pull_register               | Read/Write/Save | Access to the GPIO PP/OD register                          |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| addressable_gpio_modes           | Write           | Addressable write access to a single GPIO's Mode           |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| addressable_outputs              | Write           | Addressable write access to a single GPIO's Output         |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| addressable_use_pull             | Write           | Addressable write access to a single GPIO's Use Pull Value |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| addressable_pull_type            | Write           | Addressable write access to a single GPIO's Pull Type      |
	+----------------------------------+-----------------+------------------------------------------------------------+
	| addressable_push_pull_open_drain | Write           | Addressable write access to a single GPIO's PP/OD Mode     |
	+----------------------------------+-----------------+------------------------------------------------------------+

#. To read a value

	.. code-block::

		print(fortiq.get("gpio_controller", "<entry_name>"))

	For example, to read the value stored in the *modes_register*:

	.. code-block::  
	
		print(fortiq.get("gpio_controller", "modes_register"))

#. To write a value

	.. code-block::
	
		fortiq.set("gpio_controller", "<entry_name>", value)
	
	For example, to set the bottom three GPIOs to outputs

	.. code-block:: 

		fortiq.set("gpio_controller", "modes_register", 7)

#. To save a value to persistent memory

	.. code-block::

		fortiq.save("gpio_controller", "<entry_name>")

	For example, to save the value in the *modes_register*

	.. code-block::
		
		fortiq.save("gpio_controller", "modes_register")