.. include:: ../text_colors.rst
.. toctree::

.. _motor_noise_debugging:

***********************************************
Communication Errors From Motor Noise
***********************************************

Where does motor noise come from?
============================
Electric motors produce torque through interactions between the motor’s magnetic field and the electric current flowing through wire windings, which produce their own 
variable magnetic fields. Current is directed through the motor windings by applying a voltage across the windings and can be switched by changing the voltage polarity. 
In a three phase brushless DC motor like those produced by Vertiq, the three windings are connected between each of the three phases. Current is directed through the coils 
by applying either a positive voltage, zero/ground voltage, or by floating the phase by applying no voltage. The process of directing the current through the coils of a 
three-phase motor is known as commutation.

However, motors are not perfect or closed systems. The same switching voltages that occur during commutation to spin the motor may also cause electromagnetic interference 
(EMI) in nearby electronic systems, including those used to drive and communicate with the motor. EMI originating from an electric motor is what we call motor noise. EMI 
can be radiated, coupled, or conducted and can occur at different frequencies and their harmonics. Therefore, it is important to understand the source and impacts of EMI 
when designing mitigation techniques. 

This document will specifically address the mitigation of motor noise that couples to the motor’s body through stray capacitances in the motor itself. These stray 
capacitances are very small, typically in the picofarad range. However, they can cause significant problems for communication buses if they are conducted beyond the motor 
itself to, for example, the frame of an unmanned vehicle. Similar frequencies may also be radiated from the motor and its coils, but the impacts of radiated noise on 
unmanned vehicles tend to be less significant and the same techniques can be used to mitigate them.  


Why are internal ESCs more susceptible to noise?
=================================
Drones and similar electric vehicles using electric motors with integrated ESCs tend to have longer communication wire lengths than vehicles using external ESCs. 
This is because external ESCs are typically installed close to a drone’s power distribution unit with the three phase power cables extending along the arm to the motor. 
Whereas a drone utilizing motors with internal ESCs will have communications cables extending the entire length of the drone arm to the motor’s ESC connectors. Generally 
speaking, a communication network’s susceptibility to EMI will increase the longer its wire lengths get. Therefore it is especially important to reduce EMI and build 
robust communication buses on a vehicle utilizing motors with internal ESCs. 


How to minimize noise on communication lines
=====================================================
One very important EMI consideration for any vehicle design using electric motors is how to mitigate or eliminate motor noise. This problem becomes especially apparent on 
drone’s constructed with conductive frame materials such as carbon fiber. When an electric motor is mounted to a conductive frame with metal or conductive screws the motor 
body and vehicle frame will be electrically connected. In this scenario, it is likely that any motor noise that couples to the motor’s aluminum body via parasitic 
capacitances will conduct onto the vehicle’s frame. A noisy vehicle frame can produce EMI that may cause dropped packets or communication failures on signal buses. 
An example of AC coupled motor noise interfering with a CAN bus network can be seen below if figure 1 where channels two and three (pink and light blue) are a CAN bus high 
and low signals and channel four (blue) is measuring coupled noise on the carbon fiber drone arm that the CAN bus is routed through. Motor noise can be insulated from a 
vehicle’s frame by mounting the motor using nylon or nonconductive spacers and/or screws.

.. figure:: ../_static/tutorial_images/motor_noise/noise_diagram.PNG
    :align: center

    Figure 1: Coupled Motor Noise 

However, designers of heavy duty vehicles often want to avoid using non conductive plastic fasteners as they are relatively weaker mechanically and act as thermal 
insulators. Fortunately, motor noise coupled to a motor’s body can also be eliminated or greatly diminished by connecting the motor body to power ground. It is common for 
electronics with both AC and DC power supplies to have their frame or case grounded for user safety and EMI considerations. Whether or not to ground an unmanned vehicle 
frame is a decision with many application specific considerations. 

Firstly, please consider everywhere the frame will be grounded. Electronic enclosures, frames, and cases are commonly grounded at a single point close to the power source 
ground. This is an easily applicable standard for a single motor with an integrated ESC where a mounting screw can be used to ground the chassis. Grounding through a 
mounting screw is particularly effective in reducing EMI as it provides a very low impedance path for coupled noise to drain back to ground. However, a vehicle with 
multiple motors will have multiple grounding points on its frame. One must also consider whether or not the other electronic devices mounted to the vehicle’s frame have 
their mounting positions tied to ground or potentially even a positive voltage. Unfortunately, there is no one standard for electronic enclosure or PCB mounting hole 
grounding or isolation. Therefore, one should perform a continuity check between the power connectors and enclosure/mounting position of each device being mounted to a 
vehicle frame so that all ground return paths can be considered and short circuits can be avoided. 

Also, consider that a grounded frame will introduce new potential ground faults. For example, if a cable carrying a positive voltage like battery voltage is damaged and 
makes contact with a grounded frame it will likely cause a vehicle wide power failure and potentially damage on board electronics. The mechanical reliability of power and 
signal cables should always be considered in high vibration environments such as vehicle bodies. 

The simplest grounding scheme is to connect the drone frame to ground at a single point near the power source ground which will usually be near the center of a drone. Any 
remaining motor noise or EMI on the signal buses can be further mitigated by using shielded twisted pair cable. The cable shields should have their drains connected to 
ground at the same location as the frame itself for maximum impact. 

However, a single grounding point is not always possible when integrating multiple electronics with grounded enclosures or PCBs. A single point of chassis ground may also 
not be sufficient in eliminating EMI spikes if there is enough impedance between the EMI source and grounding point. Please consider the following when designing a vehicle 
with multiple points of frame ground.

Having multiple grounding points on a vehicle frame may cause the frame itself to conduct currents, which are sometimes referred to as parasitic currents or ground loops. 
Parasitic currents will flow when there is a voltage difference between two points that are nominally ground referenced and may exacerbate communication issues. Ground 
loops are a problem typically found in applications with large cable lengths where small currents can produce large voltage offsets like the AC mains in buildings. Ground 
loops may not be an observable problem on relatively smaller scales such as unmanned vehicle frames. However, if communication errors are occurring on a vehicle with 
multiple frame grounding locations that do not occur when the frame is isolated or only has one connection to ground, ground loops may be the cause. When designing a v
ehicle where it is necessary to ground the vehicle frame at multiple points, please consider the following to help keep the vehicle frame at a common potential:

1. Make a frame ground connection point as close as possible to the power supply/battery ground.
2. Make a frame ground connection point close to the power ground input of each electronic device on a communication bus.
3. Use the shortest possible cable length when connecting each electronic device’s power ground to the power supply/battery ground. This should result in the power 
   network having a star topology with the power supply/battery connection in the center.
4. Use a sufficiently thick wire gauge for the current application when making ground connections.  
5. In general, frame ground paths between an electronic device and the power supply/battery should have a single parallel power ground path through the power ground cable. 

Using multiple frame ground locations also introduces potential ground faults that one should consider protecting against. For example, if a ground power connection is 
broken the ground current will have a return path through the vehicle frame. Special care should be taken to avoid turning a carbon fiber vehicle frame into a ground power 
return bus. Carbon fiber is a bad material to use as a power bus because it has a relatively high resistivity compared to aluminum or copper. A material with a higher 
resistivity will produce more waste heat at the same current load than a material with a lower resistivity. Put simply, intentionally conducting power ground through 
standard carbon fiber at currents typically associated with drone motors may create a fire hazard. To prevent ground faults associated with cabling, cable harnesses should 
be designed with mechanical features for use in a high vibration environment like a vehicle frame. Additionally, consider making redundant ground connections through a fuse 
or polymeric positive temperature coefficient device (PTC) to prevent high current flows through a carbon fiber frame.
