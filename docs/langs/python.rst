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

Creating a Module
=================

Python interacts with the motor controller via IQ Module Objects. Currently IQ Python API 
supports 3 types of motors: Vertiq 2306, Vertiq 8108, and Fortiq M42BLS

Creating a Vertiq Module
------------------------

Here's how you create a Vertiq Module:  

.. code-block:: python

    import iqmotion as iq

    serial_port = "/dev/ttyUSB0" # Machine dependent
    com = iq.SerialCommunicator(serial_port)

    # Create a Vertiq2306 module with default firmware settings ("speed")
    vertiq2306 = iq.Vertiq2306(com, 0)

    # Create a Vertiq8108 module with default firmware settings ("speed")
    vertiq8108 = iq.Vertiq8108(com, 0) 

Creating a Fortiq Module
------------------------

Here's how you create a Vertiq Module:  

.. code-block:: python

    import iqmotion as iq

    serial_port = "/dev/ttyUSB0" # Machine dependent
    com = iq.SerialCommunicator(serial_port)

    # Create a Vertiq2306 module with default firmware settings ("speed")
    fortiq = iq.Fortiq(com, 0)

    # Create a Vertiq8108 module with default firmware settings ("speed")
    fortiq = iq.Fortiq(com, 0) 

Create a Base Module 
--------------------

A Base Module is a module that contains all the essentials 
clients needed to interact with a motor. It does not have any 
control modules so consider it a blank slate.

To start with a base module and add clients on top, 
create a folder that holds all of your custom client 
json files and pass the folder name to the module.

.. code-block:: python
    
    # This folder should contain custom client jsons
    clients_path = "custom_clients/"

    FlyDronePro = iq.BaseIqModule(com, 0, clients_path=clients_path)
    FlyDronePro.list_clients() # Displays loaded clients for the module


API Options
===========

.. contents:: 
    :local:

Changing the API firmware to match the Firmware
-----------------------------------------------

Each module comes loaded with default firmware.

+----------------+----------------------+---------------------+---------------------+-----------+
| Firmware Style | `Vertiq2306 2200Kv`_ | `Vertiq2306 220Kv`_ | `Vertiq8108 150Kv`_ | `Fortiq`_ |
+================+======================+=====================+=====================+===========+
| ="speed"       | DEFAULT              | x                   | DEFAULT             | x         |
+----------------+----------------------+---------------------+---------------------+-----------+
| ="servo"       | x                    | DEFAULT             |                     | DEFAULT   |
+----------------+----------------------+---------------------+---------------------+-----------+
| ="stepdir"     | x                    | x                   |                     | x         |
+----------------+----------------------+---------------------+---------------------+-----------+


.. note:: 
        The Vertiq2306 220Kv and Vertiq2306 2200Kv share the same API call. However the 220Kv was designed as a servo motor 
        while the 2200Kv was designed as a speed motor.
        
        The API call to Vertiq2306 defaults to speed, therefore if using a 220Kv module, you need to specify `firmware="servo"`

Example of changing the API firmware:

.. code-block:: python

    # Create a vertiq module for the 220Kv Module
    vertiq2306 = iq.Vertiq2306(com, 0, firmware="servo") 

    # Create API Fortiq Module intended for a Fortiq with Speed Firmware
    fortiq = iq.Fortiq(com, 0, firmware="speed") 

Available Firmware to Download
------------------------------

.. _Vertiq2306 2200Kv: https://www.iq-control.com/vertiq-2306-2200kv
.. _Vertiq2306 220Kv: https://www.iq-control.com/vertiq-2306-220kv
.. _Fortiq: https://www.iq-control.com/fortiq-bls42
.. _Vertiq8108 150Kv: https://www.iq-control.com/vertiq-8108-150kv

+----------------------+------------------------------+------------------------------------------+
|        Module        | Standard on Motor Controller |          API Default Selection           |
+======================+==============================+==========================================+
| `Vertiq2306 2200Kv`_ | Speed                        | iq.Vertiq2306(com, 0)                    |
+----------------------+------------------------------+------------------------------------------+
| `Vertiq2306 220Kv`_  | Servo                        | iq.Vertiq2306(com, 0, firmware="servo")* |
+----------------------+------------------------------+------------------------------------------+
| `Vertiq8108 150Kv`_  | Speed                        | iq.Vertiq8108(com, 0)                    |
+----------------------+------------------------------+------------------------------------------+
| `Fortiq`_            | Servo                        | iq.Fortiq(com, 0)                        |
+----------------------+------------------------------+------------------------------------------+


Adding New Clients to your IQModule
-----------------------------------

There are 2 options avaiable for adding extra clients

 
1.  clients_path {str: directory path}: 

    Create a folder that holds all of your custom client json 
    files and pass the folder name to the module.

    Note: this option will add every client entry in the folder

    .. code-block:: python

        import iqmotion as iq
        import os

        # extra_clients = <dirname to client jsons>
        path_to_clients = os.path.join(os.path.dirname(__file__), ("extra_clients"))

        com = iq.SerialCommunicator("/dev/ttyUSB0")
        fortiq = iq.Fortiq(com, 0, clients_path=path_to_clients)
        fortiq.list_clients()
        
    
2.  extra_clients {list: [dir path, dir path, ...]}:

    Contains a list of paths to each cleint entry you want to include
    Note: you need to pass in an absolute paths

    .. code-block:: python

        import iqmotion as iq
        import os

        # extra_clients = <dirname to client jsons>
        anticogging = os.path.join(os.path.dirname(__file__), ("extra_clients/anticogging.json"))
        buzzer = os.path.join(os.path.dirname(__file__), ("extra_clients/buzzer_control.json"))
        extra_clients = [anticogging, buzzer]

        com = iq.SerialCommunicator("/dev/ttyUSB0")
        fortiq = iq.Fortiq(com, 0, extra_clients=extra_clients)
        fortiq.list_clients()

Using Get/Set/Save Commands
===========================

Each module is capable of forming packets to send ``get, set, and save`` messages to a
motor controller. 

**To form a** ``get`` **message:**

``module.get("client", "client_entry")``

.. code-block:: python

    vertiq = iq.Vertiq8108(com, 0)
    uc_temp = vertiq.get("temperature_monitor_uc", "uc_temp")
    print(f"The temperature of the UC is {uc_temp}")


**To form a** ``set`` **message with a value:**

``module.set("client", "client_entry", value)``

.. code-block:: python

    vertiq = iq.Vertiq8108(com, 0)

    volts = 5  # Set motor power to 5 Volts 
    vertiq.set("propeller_motor_control", "ctrl_volts", volts)

**To form a** ``set`` **message with no value:**

``module.set("client", "client_entry")``

.. code-block:: python

    vertiq = iq.Vertiq8108(com, 0)

    # Reboots motor with saved values
    vertiq .set("system_control", "reboot_program")


**Finally, to form a** ``save`` **message, use**

Saves the client and client entry values already set on the module

``module.save("client", "client_entry")``

.. code-block:: python

    vertiq = iq.Vertiq8108(com, 0)
    vertiq.set("propeller_motor_control", "velocity_kp", 10)
    vertiq.save("propeller_motor_control", "velocity_kp")

These commands form serialized ``get/set/save`` packets and store them into a ``com`` SerialCommunicator
object which sends the packet request to the motor via Serial Communication.