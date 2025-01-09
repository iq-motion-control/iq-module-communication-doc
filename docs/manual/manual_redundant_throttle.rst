##############################
Redundant Throttle Support
##############################

***************************
About Redundant Throttle
***************************

Vertiq's redundant throttle support is a safety feature that allows your module to accept multiple throttle sources' commands simultaneously, and to switch between them should 
the primary source fail. For example, suppose you would like to control your module using the :ref:`DroneCAN protocol <dronecan_protocol>`. To do so, you connect your module's 
CAN connections with your flight controller's, and connect no other throttle sources. 

.. image:: ../_static/manual_images/redundant_throttle/basic_can_connection.png
    :width: 50%

Now, in flight, your CAN connection comes loose, and the module and flight controller lose DroneCAN communication. In this case, your vehicle will lose control as your module no longer 
reacts to new throttle commands, and the module will eventually :ref:`manual_timeout`.

Now, suppose that you connect DroneCAN as before, but now, also connect your module to a DSHOT output on your flight controller.

.. image:: ../_static/manual_images/redundant_throttle/dshot_and_dronecan_connection.png
    :width: 50%

Under normal operating conditions, the module will apply only throttle commands received via DroneCAN (as determined by your redundant throttle configuration discussed :ref:`below <redundant_throttle_config>`). 
If we once again sever the DroneCAN connection, however, the module will seamlessly transition to accepting DSHOT commands. By connecting multiple throttle sources, and 
leveraging Vertiq's redundant throttle feature, you robustify your vehicle's ability to fly safely even after communication failures.


.. _redundant_throttle_config:

***************************************
Redundant Throttle Configurations
***************************************

There are two types of configurations available with the redundant throttle. The first defines the amount of time that the module waits before switching to another throttle 
source if the active source goes offline. The second set of parameters defines the priorities of each supported throttle source. You can find the specific parameters 
at :ref:`throttle_source_manager`.

Vertiq's supported throttle sources are :ref:`DroneCAN <dronecan_protocol>`, :ref:`IQUART <uart_messaging>`, and :ref:`hobby protocols <hobby_protocol>`. Please note that only priority values corresponding to protocols supported by 
your module have any effect. See your module's family page to see what throttle sources are supported.

Priorities can be valued from 0 to 3. Setting a priority of 0 indicates that the module will ignore all throttle messages received from the configured source. 
Priority values [1, 3] define the priority of each protocol against the others where 3 defines the highest priority.

.. note:: 
    Due to Vertiq modules' hardware, it is not possible to use both IQUART and hobby protocols simultaneously. As such, it is only possible to use DroneCAN with one of IQUART 
    or hobby as redundant throttle sources.

Configuration Example 1
##########################

Suppose your module supports DroneCAN, IQUART, and hobby inputs. 

You configure the following:

- IQUART's priority to 3 and DroneCAN's to 1 (the module will automatically set hobby's to 2 ensuring that there are no two matching priorities)
- Flight controller outputting both :ref:`IQUART Flight Controller Interface commands <controlling_ifci>` as well as DroneCAN commands
- Throttle source timeout configured to 0.5 seconds

The sequence diagram below illustrates how your module reacts to received throttle commands as well as how it deals with switching between sources.

.. image:: ../_static/manual_images/redundant_throttle/iquart_prio_example.png
    :width: 50%

Note that the first DroneCAN throttle is applied to the motor since no IQUART messages had been received before. Then, when both an IQUART and DroneCAN message are received, 
the IQUART throttle is applied. Last, DroneCAN throttles are only applied again once the throttle timeout is reached after IQUART disconnects.

Configuration Example 2
##########################

Suppose your module supports DroneCAN, IQUART, and hobby inputs. 

You configure the following:

- DroneCAN's priority to 3, IQUART to 0, hobby to 0
- Flight controller outputting both :ref:`hobby_dshot` as well as DroneCAN commands
- Throttle source timeout configured to 0.1s

.. image:: ../_static/manual_images/redundant_throttle/dshot_ignored_ex.png
    :width: 70%

In this example, the module is configured to ignore all hobby and IQUART throttle messages. So, all received DSHOT throttles are dropped, and throttles are only applied once DroneCAN 
throttles are received. An important note from this example is that the module will reach the module's :ref:`propeller motor control timeout <manual_timeout>` even if the 
flight controller continues to send DSHOT throttles. As they are not processed by the throttle source manager, DSHOT throttles cannot be used to reset the timeout timer.

********************************
Redundant Throttle and Arming
********************************

All throttle commands received by the throttle source manager are sent to your module's :ref:`arming handler <manual_advanced_arming>`, and are subject to all constraints defined by the arming handler. 
For example, suppose you have configured IQUART to be the highest priority source, and are sending IQUART throttle commands via the :ref:`Vertiq Testing Tool <vertiq_testing__guide>`. At the same 
time, you are transmitting DroneCAN throttles with the :ref:`DroneCAN GUI tool <dronecan_gui_basics>`. In this example, your arming handler is set to arm on throttle in the range 0-7.5% on 10 
consecutive throttles.

VTT outputs IQUART throttles at 10%, and the DroneCAN GUI transmits 3% throttles. In this case, your module will not arm. Since IQUART has a higher priority, its throttles take 
precedence over DroneCAN, so the arming handler will only receive the 10% throttles. If you pause the VTT output, however, your module will arm as the throttle source manager will 
have transitioned to accepting the DroneCAN commands within the arming region. Now, if you unpause the VTT output, your module will start spinning at 10% throttle.

The example is demonstrated below:

.. raw:: html

    <style type="text/css">
    .center_vid {   margin-left: auto;
                    margin-right: auto;
                    display: block;
                    width: 75%; 
                }
    </style>
    <video class='center_vid' controls><source src="../_static/manual_images/redundant_throttle/redundant_arming_example.mp4" type="video/mp4"></video>

.. _config_values: