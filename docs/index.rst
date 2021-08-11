.. Python API documentation master file, created by
   sphinx-quickstart on Wed Mar 17 12:58:20 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


.. toctree::
   :caption: API Documentation
   :titlesonly:
   :hidden:

   langs/python
   langs/arduino
   langs/cpp
   langs/matlab
   

.. toctree::
   :caption: Modules
   :maxdepth: 2
   :hidden:
   
   modules/vertiq_2306_2200
   modules/vertiq_2306_220
   modules/vertiq_8108_150
   modules/fortiq

.. toctree::
   :caption: Messaging Protocols
   :hidden:

   intro


Welcome to the IQ Motion Control Serial API Guide
=================================================

This guide is meant to help you get the most out of your IQModule. 
We currently support 4 different languages for controlling your IQModule:

* Python
* Arduino 
* C++
* Matlab

The intention of these guides is to teach you how to quickly communicate with an IQModule using our API.
You may proceed with a `Getting Started` guide on the left


Reporting Documentation Issues
******************************

We try our best to keep this documentation as clear, concise, and updated as possible.
If you come across any typos, bugs in example code, confusing verbiage, or simply have a question on how to do something, 
Please report it on our public github issues page!

`Reporting Issues or Questions about the Documentation <https://github.com/iq-motion-control/iq-module-communication-doc/issues>`_


Identifying Your Module's Firmware Style
****************************************

.. sidebar:: IQ Control Center Info Pane

   .. image:: _static/Control_Center_Info_Pane.png
         :alt: Control Center Information Pane

Our motors are unique in that they can be configured to run as a speed or position based (servo/stepper) module based on the firmware you upload to it.

To verify which firmware is on your module, connect to the IQ Control Center Desktop Application and check the Info Pane:

`Download IQ Control Center <https://github.com/iq-motion-control/iq-control-center/releases>`_ 


.. Indices and tables
.. ==================

.. * :ref:`genindex`
.. * :ref:`modindex`
.. * :ref:`search`
