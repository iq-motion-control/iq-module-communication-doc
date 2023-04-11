.. _uart_messaging:

***************
UART Messaging
***************

Vertiq uses a fully featured, serial based protocol for communicating with motor controllers. This communication
protocol is broken into classes of related functionality. As such, Vertiq supplies libraries for communicating with
the motor controllers in various object-oriented languages.

**The Vertiq API currently supports the following languages:**

* Arduino
* C++
* Matlab
* Python

Message Packetization
=====================

General Packets
----------------
Packets are structured such that a valid packet is identifiable even when surrounded by random bytes. The
start byte (dec 85, hex 0x55, bin 0b01010101) indicates the start of a new packet. This particular start byte
is simple to identify using an oscilloscope by its alternating 01 pattern. The ’length’ byte indicates length of
the ’payload’ segment, which in turn points to the location of the two CRC bytes. The ’type’ byte indicates
the source and destination client for this packet. ’type’ is required and comes immediately before ’payload’.
The ’payload’ segment is a variable length section, little-endian, with a practical limit of 59 bytes due to
the common 64 byte buffer size for many serial protocols. Finally, a little-endian CRC-16-CCITT cyclic
redundancy check on ’length’+’type’+’payload’ is appended. See Table 1 a visual representation.

Standard Packets
----------------
A common use for communication is to get values, set values, save values, and reply to a get request. The
General Packet is used to make a Standard Packet capable of performing these four functions. The ’payload’
segment of the General Packet is broken into three new segments: subType, obj/access, and ’data’. The
’subType’ byte refers to the specific value within the ’type’ that is being get/set/saved. The ’obj’ and ’access’
are packed into a single byte, where ’obj’, using the high 6 bits, refers to the Module ID (settable in the
System Control class) and the ’access’, the lowest 2 bits, indicate the message intention. ’access’ can be one
of four values: ”get” = 0, ”set” = 1, ”save” = 2, ”reply” = 3. ”get” access requests a ”reply” of ’subType’.
”set” access places –data– into the ’subType’. ”save” access saves the ’subType’ value in flash. ”reply” is
the response from a ”get”, usually with a value loaded into –data–.


Table 1: Message Packetization

+----------+------+--------+------+-----------+------------+--------+------+------+
| General  | 0x55 | Length | Type |  .. centered:: -payload-        | crcL | crcH |
+----------+------+--------+------+-----------+------------+--------+------+------+
| Standard | 0x55 | Length | Type | subType   | obj/access | -data- | crcL | crcH |
+----------+------+--------+------+-----------+------------+--------+------+------+


