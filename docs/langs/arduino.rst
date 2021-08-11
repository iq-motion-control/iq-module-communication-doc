****************************
Getting Started with Arduino
****************************

The Arduino libraries allow simple ``get/set/save`` access to all parameters on the modules, which includes the
various control methods to make the modules spin. Please see the official Arduino library installation guide at
https://www.arduino.cc/en/guide/libraries. Using the Library Manager is preferred. Search for “IQ
Module Communication”.  A .zip of the library can also be found at http://iq-control.com/support.

To use the IQ Module Communication library, you must include the library header file (done automatically
by Arduino if you use their Sketch →Include Library →IQ Module Communication), by typing

``#include <iq_module_communicaiton.hpp>``

Next make an IqSerial object. You have two options. Use the default constructor to use the default
Serial (also called Serial0) on Arduino by typing 

``IqSerial ser;`` 

or select the Serial peripheral of your choice by passing in the Serial object by typing 

``IqSerial ser(Serial2);``

Next, create a client object to communicate with the motor. Here we’ll talk with the Brushless Drive
Client by typing

``BrushlessDriveClient mot(0);``

0 indicates the Module ID. See the System Control Client documentation for more details.

Now we can setup the Arduino and communication. Initialize the IqSerial object by calling begin. The
default begin function will set the baud rate to 115200. If you have changed the motor module’s baud rate, 
pass in the non-default baud rate into the begin function.

.. code-block:: arduino

   void setup() {
      // Initialize the IqSerial object
      ser.begin();
      // Initialize Serial (for displaying information on the terminal)
      Serial.begin(115200);
   }

Finally, use the client to talk to the motor. Here we’ll repeatedly send it a low level spin command.

.. code-block:: arduino 
   
   void loop() {
      static float velocity = 0.0f;
      // Send a voltage command of 0.1 volts
      ser.set(mot.drive_spin_volts_,0.1f);
      // Get the velocity. If a response was received...
      if(ser.get(mot.obs_velocity_,velocity))
      {
         // Display the reported velocity
         Serial.println(velocity);
      }
   }

The ``get, set, and save`` functions take in a client object.sub object as the first parameter.

* In ``get``, the second parameter is a reference to a value. The get function will put the response from the motor in this value variable. The get function returns a bool, which indicate if it received a response from the motor.

* The ``set`` function takes in the value to be sent as the second parameter. This value must be the datatype expected by the client object.sub object.

* The ``save`` function requires just the client object.sub object.

For a complete list of client object.sub object and their datatypes, please see the documentation for the
client classes.


