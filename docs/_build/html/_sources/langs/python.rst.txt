**********
Python API
**********

The Python API library contain everything required to open a serial port, send and receive messages on that
serial port, and parse the results. 

Prerequisites
=============

Windows and Mac Users:
  * `Installing Pipenv (Win & Mac) <https://medium.com/@mahmudahsan/how-to-use-python-pipenv-in-mac-and-windows-1c6dc87b403e>`_
  
Linux Users:
  * `Installing Pipenv (Linux) <https://github.com/pypa/pipenv>`_
 
Getting Started
===============

First, import iqmotion library and create a ``SerialCommunicator``, which opens 
a serial port and is responsible for the transmission and reception of messages, by typing:  

.. code-block:: python
    
    import iqmotion as iq

    com = iq.SerialCommunicator('COM PORT')


Replace the ``COM PORT`` string with the port string for your serial device (FTDI or similar). In Windows,
this string has the form ’COM1’, ’COM2’, etc. In a Unix based OS, this string has the form ’/dev/ttyUSB0’
or similar and depends on the device. The default serial baud rate for the motor controller is 115200.

Communicating With Motor Controller
===================================

Python interacts with the motor controller via IQ Module Objects. Currently IQ Python API 
supports 2 motors: Vertiq 2306 & Vertiq 8108

Creating a Vertiq Module
------------------------

Here's how you create a Vertiq Module:  

.. code-block:: python

    import iqmotion as iq

    serial_port = "/dev/ttyUSB0" # Machine dependent
    com = iq.SerialCommunicator(serial_port)

    # Create a vertiq2306 module with default firmware settings ("speed")
    vertiq2306 = iq.Vertiq2306(com, 0)

    # Create a vertiq8108 module with default firmware settings ("speed")
    vertiq8108 = iq.Vertiq8108(com, 0) 

Choosing the firmware for the Vertiq Module
-------------------------------------------

If you have flashed different firmware onto the module, then you'll need to reflect that change in the API as well.  
    
* The Vertiq2306 currently supports:  
    ``firmware = ["stepdir", "speed", "servo"]``

* The Vertiq8108 currently supports:  
    ``firmware = ["speed"]``

Here's how you change the firmware in the API:

.. code-block:: python

    # Create a vertiq module with different firmare settings
    vertiq2306 = iq.vertiq2306(com, 0, firmware="stepdir") 

Load new clients on top of your Vertiq Module
---------------------------------------------

To add clients on top of a Vertiq2306 or Vertiq8108, 
create a folder that holds all of your custom client json 
files and pass the folder name to the module.

.. code-block:: python
    
    # This folder should contain custom client jsons
    clients_path = "clients/" 

    vertiq2306 = iq.vertiq2306(com, 0, clients_path=clients_path)
    vertiq2306.list_clients() # Displays loaded clients for the module


Create a Base Module containing only essential clients
------------------------------------------------------

A Base Module is a module that contains all the essentials 
clients needed to interact with a motor. It does not have any 
control modules so it's a blank slate.

To start with a base module and add clients on top, 
create a folder that holds all of your custom client 
json files and pass the folder name to the module.

.. code-block:: python
    
    # This folder should contain custom client jsons
    clients_path = "custom_clients/"

    FlyDronePro = iq.BaseIqModule(com, 0, clients_path=clients_path)
    FlyDronePro.list_clients() # Displays loaded clients for the module

Modifying Motor Controller Clients with Get/Set/Save
====================================================

Each module is capable of forming packets to send ``get, set, and save`` messages to a
motor controller. 

**To form a** ``get`` **message:**

``module.get("client", "client_entry")``

.. code-block:: python

    vertiq = iq.vertiq8108(com, 0)
    uc_temp = vertiq.get("temperature_monitor_uc", "uc_temp")
    print(f"The temperature of the UC is {uc_temp}")


**To form a** ``set`` **message with a value:**

``module.set("client", "client_entry", value)``

.. code-block:: python

    vertiq = iq.vertiq8108(com, 0)

    volts = 5  # Set motor power to 5 Volts 
    vertiq.set("propeller_motor_control", "ctrl_volts", volts)

**To form a** ``set`` **message with no value:**

``module.set("client", "client_entry")``

.. code-block:: python

    vertiq = iq.vertiq8108(com, 0)

    # Reboots motor with saved values
    vertiq .set("system_control", "reboot_program")


**Finally, to form a** ``save`` **message, use**

Saves the client and client entry values already set on the module

``module.save("client", "client_entry")``

.. code-block:: python

    vertiq = iq.vertiq8108(com, 0)
    vertiq.set("propeller_motor_control", "velocity_kp", 10)
    vertiq.save("propeller_motor_control", "velocity_kp")

These commands form serialized ``get/set/save`` packets and store them into a ``com`` SerialCommunicator
object which sends the packet request to the motor via Serial Communication.