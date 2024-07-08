.. include:: ../text_colors.rst
.. toctree::

.. _getting_started_python_api:

******************************
Getting Started with Python
******************************

Installation
==================

Information on how to install the ``iqmotion`` library can be found on our `PyPi page <https://pypi.org/project/iqmotion/>`_.

Python API Basics
=========================
Hardware Setup
----------------
Information about required hardware for API communication can be found in the :ref:`Control Center documentation <connection_guide>`. The requirements for API communication 
are the same as those for the IQ Control Center.

Opening a Serial Connection
--------------------------------
In order to connect with your USB-to-UART bridge, the Python API provides the ``SerialCommunicator`` class.

The ``SerialCommunicator`` class is initialized with a serial port name and a baud rate. The name of your serial port is dependent on your operating system. 
For example, in the hardware example provided in the Control Center documentation linked above, the serial port name is “COM3,” and in Linux, it may be "/dev/ttyUSB0."

In order to create a new ``SerialCommunicator`` object on a Windows operating system where your module is set to 115200 baud, and your FTDI reports on COM9, you would do the following:

.. code-block:: python
    
    import iqmotion as iq
    com = iq.SerialCommunicator("COM9", baudrate=115200)

In order to communicate with a Vertiq module through the Python API, you must create a ``SerialCommunicator`` object.

Creating a Module Object
-----------------------------
Vertiq's module objects are preconfigured to communicate with all accessible IQUART clients for your module's firmware. All module objects are created with the same 
initialization.

1. A serial communicator object
2. Optionally, a module ID (``module_idn``)

  * This represents the module ID of the device you would like to communicate with. If your module has its module ID configured to 26, you must set ``module_idn`` to 26 in object instantiation. 
    If no module ID is specified, the module object is created with an ID of 0.

3. Optionally, a firmware style (``firmware``)

  * This value is a string, and can be “speed”, “servo”, or “pulsing”. To learn more about what firmware styles are available for your module, please refer to your module's family page. 
    If no firmware style is specified, the module object is created with a style of ``speed``.

4. Optionally, a path to additional clients (``clients_path``)

  * There are two options available for adding extra clients. First, in order to add all client files in a folder, you can pass a path to a folder that contains all additional, client json files. Second, 
    you can pass an array of paths to custom client json files in order to only add those you are interested in rather than an entire folder. In general, you will not need to include additional clients.

Please use the following to create the correct object for your module type:

.. csv-table:: Module Families and Python Object Names
        :header: "Module Family", "Object Name"

        "Vertiq 23-XX", "``Vertiq2306``"
        "Vertiq 40-XX", "``Vertiq4006``"
        "Vertiq 60-XX", "``Vertiq6008``"
        "Vertiq 81-XX", "``Vertiq8108``"

Additionally, there are generic ``SpeedModule`` and ``ServoModule`` objects with access to the most basic IQUART endpoints for the associated firmware style.

Suppose you want to interact with a Vertiq 40-06 using speed firmware whose module ID is 42, and has extra client files located in a folder with the path “clients.” To do so:

1. Create a ``SerialCommunicator`` object as described above (in this case the USB-to-UART is on port COM3)
2. Create a variable ``client_files`` with the path to your additional client json files
3. Create a variable module as a ``Vertiq4006`` module with module ID 42, speed firmware, and extra clients path client_files

.. code-block:: python

    import iqmotion as iq

    # Module Communication
    com = iq.SerialCommunicator("COM3", baudrate=115200)

    # Clients to Load
    client_files = "clients/"

    # Using the Vertiq4006 with additional custom client files
    module = iq.Vertiq4006(com, module_idn=42, firmware="speed", clients_path=client_files)

At this point, you can communicate with, configure, and control your connected module through its available clients.

Interacting with Clients and Endpoints
-------------------------------------------------
As mentioned in :ref:`getting_started_with_apis`, all Vertiq clients contain endpoints that can accept get, set, and save commands. This section discusses 
how to perform gets, sets, and saves through the Python API.

Before moving forward, please familiarize yourself with the clients available for your module's family and firmware style. You can find this information on your 
module's family page in the Supported IQUART Clients section. For these examples, we will continue to use the ``Vertiq4006`` object created above.

.. note::
    In all instances, the value of ``client_entry`` is a value specified by the *Short Name* column of the associated IQUART client table.

Get
^^^^^^^^^^
All Python get commands have the format ``module.get("client", "client_entry")``. The get function returns the value of a single client entry returned by the module through IQUART.

Suppose we want to monitor the voltage read at the module's input. We can do this through the volts entry of the :ref:`Power Monitor <power_monitor>` client.

.. image:: ../_static/api_pics/volts_entry.png

In order to get and view the value of the volts parameter, we can add the following to our example

.. code-block:: python

    print(module.get("power_monitor", "volts"))

You can also treat the returned value as a normal parameter, and store it in a variable.

Get All
^^^^^^^^^^
All Python get_all commands have the format ``module.get_all("client”)``. The get_all function returns the value of all client entries returned by the module through IQUART.

Suppose we want to see the current state of all parameters in the :ref:`System Control <system_control>` client.

To do so, we can do the following:

.. code-block:: python

    print(module.get_all("system_control"))

Again, you can also treat the returned value as a normal parameter, and store it in a variable. In this case the data is stored as an array.

Set
^^^^^^^^^^
Most Python set commands have the format ``module.set("client", "client_entry", value)``. The set function changes the value of the target ``client_entry`` to ``value``. 
A value set and not saved will not be retained after a power cycle.

Suppose we want to change the :ref:`Propeller Motor Controller's <propeller_motor_controller>` ``timeout`` parameter to 3 seconds.

.. image:: ../_static/api_pics/timeout_entry.png

To do so:

.. code-block:: python

    module.set("propeller_motor_control", "timeout", 3)

Some client entries (such as :ref:`System Control's <system_control>` ``reboot_program``) accept sets without a ``value``. Simply calling ``module.set("client", "client_entry")`` is enough to 
trigger the desired behavior. So, to reboot your module's program:

.. code-block:: python

    module.set("system_control", "reboot_program")

Set Verify
^^^^^^^^^^^^
Additionally to the standard set, you can use the ``set_verify`` function in order to set a new value, and confirm that the value has been set correctly on the module. Set verify 
calls have the same format as standard set commands, but provide additional optional parameters ``get_values``, ``time_out``, ``retries``, and ``save``. 

* ``get_values``: Specifies values to add in the get message (such as index for some client entries). By default, this is ``None``
* ``time_out``: A blocking timeout while verifying the set is successful in seconds. By default, this is 0.1s
* ``retries``: The number of times you would like to retry the set before giving up. By default, this is 5
* ``save``: Allows you to save the value once the set is confirmed. By default this is ``False``

Save
^^^^^^^^^^^^
All Python save commands have the format ``module.save("client", "client_entry")``. The save function takes the currently set entry value, and stores it in the module's persistent memory. 
Values that are saved are retained on power cycles.

Suppose we want to save the timeout value set above. To do so:

.. code-block:: python

    module.save("propeller_motor_control", "timeout")

Next Steps
=========================
As the get, set, and save commands are the basis of all IQUART configuration and control, you now posess all of the base knowledge necessary to begin development 
with the Vertiq Python API. More specific examples of using the Python API exist throughout our Feature Reference Manual, such as a basic example 
for commanding your module to :ref:`spin with the Propeller Motor Controller Client <spin_with_speed_demo>`.