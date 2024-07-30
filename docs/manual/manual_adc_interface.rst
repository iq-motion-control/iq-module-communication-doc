.. include:: ../text_colors.rst
.. toctree::

.. _manual_adc_interface:

***********************************************
Analog-to-Digital (ADC) Interface
***********************************************

Module Support
================

The user ADC interface is supported by only the Fortiq-42 modules.

Speed Modules
**************

.. include:: none_checked_table.rst

Servo Modules
**************

.. table:: Module Support

	+-------------------+-----------------------------------+
	| Module            | Feature Supported                 |
	+-------------------+-----------------------------------+
	| Vertiq 81-XX      | .. centered:: |:x:|               |
	+-------------------+-----------------------------------+
	| Vertiq 60-XX      | .. centered:: |:x:|               |
	+-------------------+-----------------------------------+
	| Vertiq 40-XX      | .. centered:: |:x:|               |
	+-------------------+-----------------------------------+
	| Vertiq 23-XX      | .. centered:: |:x:|               |
	+-------------------+-----------------------------------+
	| Vertiq Fortiq-42  | .. centered:: |:white_check_mark:||
	+-------------------+-----------------------------------+
Description
=============
Vertiq's ADC Interface provides access to an on-board Analog to Digital Converter (ADC). An ADC makes it possible for your module to read input analog voltages. The ADC handles voltages from 0.0V to 3.3V with a 12-bit resolution. For example, if you input 1V to the ADC interface, reading the voltage would return 1V, and reading the "raw value" would return 1241 (:math:`\frac{V_{\text{in}} * 4096}{3.3}`). 

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

#. Click the refresh button to read the ADC voltage keeping in mind that the ADC is limited to 0 to 3.3V


Vertiq Python API - ADC Interface
*************************************
.. note::
	Please note that the following *ADC Interface* testing was performed with a Fortiq-42 module. Your exact commands may change depending on the module in use.

The ADC interface can also be accessed through Vertiq’s Python API. Through the API you can read both the ADC’s read voltage and raw ADC value. To use the API, please use the following steps:

#. If you have never used Vertiq's Python API, you must first set up your local computer to use the Python API using the instructions found at `Getting Started with Python <https://iqmotion.readthedocs.io/en/latest/api_docs/python.html>`_

#. After completing the walkthrough, you can interact with the *adc_interface* client

#. To read the voltage, use:

	.. code-block::
		
		print(fortiq.get("adc_interface", "adc_voltage"))

#. To read the raw value, use: 

	.. code-block::

		print(fortiq.get("adc_interface", "raw_value"))

