.. include:: ../text_colors.rst
.. toctree::

.. _manual_adc_interface:

***********************************************
ADC Interface
***********************************************

Module Support
================

The user ADC interface is supported by only the Fortiq-42 modules.

Speed Modules
**************

.. table:: Speed Module Support for User ADC

	+-------------+---------------------+
	| Module      | User ADC Support    |
	+-------------+---------------------+
	| Vertiq 8108 | .. centered:: |:x:| |
	+-------------+---------------------+
	| Vertiq 4006 | .. centered:: |:x:| |
	+-------------+---------------------+
	| Vertiq 2306 | .. centered:: |:x:| |
	+-------------+---------------------+

Servo Modules
**************

.. table:: Servo Module Support for User ADC

	+-----------------+-----------------------------------+
	| Module          | User ADC Support                  |
	+-----------------+-----------------------------------+
	| Vertiq 8108     | .. centered:: |:x:|               |
	+-----------------+-----------------------------------+
	| Vertiq 4006     | .. centered:: |:x:|               |
	+-----------------+-----------------------------------+
	| Vertiq 2306     | .. centered:: |:x:|               |
	+-----------------+-----------------------------------+
	| Vertiq Fortiq42 | .. centered:: |:white_check_mark:||
	+-----------------+-----------------------------------+

Description
=============
Vertiq's ADC Interface provides access to an application-blind, on-board Analog to Digital Converter (ADC). The ADC handles voltages from 0.0V to 3.6V with a 12-bit resolution. 

The ADC interface provides read-only access to both the voltage read and the raw ADC value.

Usage
========
IQ Control Center
**********************
The IQ Control Center provides the easiest way to test reading the voltage input on your module's ADC. To do so:

#. Open IQ Control Center. If you have not installed the program, please follow the instructions in `Getting Started with Speed Motors Using IQ Control Center <https://iqmotion.readthedocs.io/en/latest/tutorials/testing_with_control_center.html>`_. 

#. Connect your module to IQ Control Center

#. Select the Testing Tab on the left side:
	
	.. image:: ../_static/manual_images/fortiq/other/control_center_testing.png

#. Scroll down in the Testing Tab until you find *Read ADC Voltage*

	.. image:: ../_static/manual_images/fortiq/adc/read_adc_tab.png

#. Click the refresh button to read the ADC voltage keeping in mind that the ADC is limited to [0, 3.6]V


Vertiq Python API - ADC Interface
*************************************
.. note::
	Please note that the following *ADC Interface* testing was performed with a Fortiq-42 module. Your exact commands may change depending on the module in use.

The ADC interface can also be accessed through Vertiq’s Python API. Through the API you gain read access to both the ADC’s read voltage and raw ADC value. To use the API, please use the following steps:

#. If you have never used Vertiq's Python API, you must first set up your local computer to use the Python API using the instructions found at `Getting Started with Python <https://iqmotion.readthedocs.io/en/latest/langs/python.html>`_

#. After completing the walkthrough, you can interact with the *adc_interface* client

#. To read the voltage, use:

	.. code-block::
		
		print(fortiq.get("adc_interface", "adc_voltage"))

#. To read the raw value, use: 

	.. code-block::

		print(fortiq.get("adc_interface", "raw_value"))

ADC Interface Client - Entry Summary
-----------------------------------------
.. table:: *adc_interface* Entries

	+---------------+--------+----------+
	| Entry Name    | Access | Format   |
	+===============+========+==========+  
	| adc_voltage   | Read   | float    |
	+---------------+--------+----------+
	| raw_value     | Read   | uint16   |
	+---------------+--------+----------+

