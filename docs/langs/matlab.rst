***************************
Getting Started with Matlab
***************************


The Matlab libraries contain everything required to open a serial port, send and receive messages on that
serial port, and parse the results. First, create a MessageInterface, which opens a serial port and is responsible
for the transmission and reception of messages, by typing

``com = MessageInterface(’COM_PORT’,115200);``

Replace the ’COM PORT’ string with the port string for your serial device (FTDI or similar). In Windows,
this string has the form ’COM1’, ’COM2’, etc. In a Unix based OS, this string has the form ’/dev/ttyUSB0’
or similar and depends on the device. The default serial baud rate for the motor controller is 115200.
To communicate to the motor controller, create a client object using

``client_object = ClientClass(’com’,com);``

Then, send and receive messages using this object via the get, set, and save member functions.

``value = client_object.get(’short_name’);``

sends a get request to the motor controller and waits for its response. The responded value is returned.

``client_object.set(’short_name’, value); % with value``
``client_object.set(’short_name’); % without value``

sends a set message. If the message requires a value, the value is stored in the motor controller’s RAM.

``client_object.save(’short_name’);``

sends a save message, which store’s the current RAM value into non-volatile memory. These functions are
blocking and perform all necessary tasks for messaging.


All clients have added member functions list, get all, set all, set verify, and save all.

``client_object.list()``

displays all possible short names, their data types, and their units.

``data_all = client_object.get_all()``

performs a get on all messages in list and stores it in data all.

``client_object.set_all(data_all);``

``data.short_name1 = 0;``

``data.short_name2 = 1;``

``client_object.set_all(data);``

will send set messages for all fieldnames in data.

``client_object.set_verify(’short_name’, value);``

performs the same function as set, but also performs a get to verify transmission. It will retry up to 10
times if transmission fails.

``client_object.save_all()``

saves all values currently in the motor controller’s RAM into non-volatile memory.
For a complete example of usage, please see the documentation for the client classes.