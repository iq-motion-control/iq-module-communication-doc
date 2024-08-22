.. include:: ../text_colors.rst
.. toctree::

.. _getting_started_with_apis:

***************************************
Getting Started with Vertiq's APIs
***************************************

What is the Purpose of Vertiq's APIs
===========================================
Vertiq's APIs provide a simple mechanism for communicating with your modules via the IQUART protocol. IQUART is Vertiq's proprietary serial 
protocol meant to allow configuration and control of all Vertiq modules. To learn more about IQUART, please see :ref:`the protocol's documentation <uart_messaging>`. 
Our APIs provide support for Python, Matlab, C++, and Arduino. These options provide flexibility to work easily on both PC-based and embedded solutions.

What Can the APIs Do
========================
Essentially, each API is a language specific implementation of the IQUART protocol with access to information about each of the IQUART clients. 
Each IQUART client contains a set of IQUART entries which are parameters that allow you to directly configure and control your module. 
For example, through the :ref:`propeller_motor_controller` Client, you find PID tuning parameters as well as control commands 
such as velocity and voltage. For a full list of the clients supported by your module, please see your module's family page. 
For a full listing of all IQUART clients please refer to the :ref:`iquart_client_reference_tables`. 

.. _api_interactions:

How to Interact with IQUART Clients and Entries
--------------------------------------------------
IQUART entries can have up to three interactions: getting, setting, and saving. The interactions available are specific to each entry, and are specified in 
the :ref:`iquart_client_reference_tables`. A Get command requests the value of a given parameter, and returns the module's reply. A Set command tells the module to 
change a parameter's value. A Save command tells the module to store the current parameter value into persistent memory so that it is retained through power cycles.

The specifics of getting, setting, and saving are dependent on your specific API. To learn more, please read the manual for your API.

* :ref:`getting_started_python_api`
* :ref:`getting_started_arduino_api`
* :ref:`getting_started_cpp_api`
* :ref:`getting_started_matlab_api`