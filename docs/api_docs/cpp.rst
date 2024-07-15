.. include:: ../text_colors.rst
.. toctree::

.. _getting_started_cpp_api:

************************
Vertiq C++ API Manual
************************

Installation
================
You can find Vertiq's C++ library on our `GitHub <https://github.com/iq-motion-control/iq-module-communication-cpp>`_. The code in the repository provides C++ 
representations of all :ref:`IQUART clients <iquart_client_reference_tables>` as well as the logic necessary to communicate over IQUART.

Hardware Setup
================
Information about required hardware for API communication can be found in the :ref:`Control Center documentation <connection_guide>`. The requirements for API communication 
are the same as those for the IQ Control Center.

Serial Connection Information
================================
The method of opening and using a serial connection varies depending on the operating system being used to run the C++ API. We will discuss opening a serial port on Windows and Linux here.

Windows
----------
For more information about serial connections on Windows, please read `Microsoft's documentation <https://learn.microsoft.com/en-us/windows/win32/devio/configuring-a-communications-resource>`_. 

Files to Include
^^^^^^^^^^^^^^^^^^^^
In order to open a serial port, you must include iostream, windows.h, and tchar.h in your C++ project.

.. code-block:: cpp

    #include <iostream>
    #include <windows.h>
    #include <tchar.h>

Configuring the Serial Port
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In order to set up the specific serial port, you must create a HANDLE instance as well as a TCHAR instance set to your FTDI's COM port. 
Suppose your FTDI reports on COM4 you will configure your Windows variables as follows:

.. code-block:: cpp

    HANDLE com_port;
    TCHAR *pcCommPort = TEXT("COM4");
    
Now, we can start up and open the serial port so that we can communicate with the connected module. Suppose we want to open our COM port at 115200 baud with the same 
serial protocol configuration as all Veritq modules. To do so:

.. code-block:: cpp
    
    com_port = CreateFile( pcCommPort,
                    GENERIC_READ | GENERIC_WRITE,
                    0,      //  must be opened with exclusive-access
                    NULL,   //  default security attributes
                    OPEN_EXISTING, //  must use OPEN_EXISTING
                    0,      //  not overlapped I/O
                    NULL ); //  hTemplate must be NULL for comm devices

    cout << com_port << endl;

    if (com_port== INVALID_HANDLE_VALUE)
        cout <<"Error in opening serial port" << endl;
    else
        cout << "opening serial port successful" << endl;

    DCB dcb = {0}; // Device-control block used to configure serial communications
    dcb.DCBlength = sizeof(DCB);
    GetCommState (com_port,&dcb);
    dcb.BaudRate  = CBR_115200; // Set baud rate to 115200
    dcb.ByteSize = 8;
    SetCommState (com_port,&dcb);

    //Set up a read timeout
    COMMTIMEOUTS timeouts;
    GetCommTimeouts(com_port, &timeouts);
    timeouts.ReadIntervalTimeout = 5;
    SetCommTimeouts(com_port, &timeouts);

Reading Data
^^^^^^^^^^^^^^^
Serial data received through the FTDI is stored in the file specified by the ``com_port`` parameter. In order to access the file's data:

.. code-block:: cpp

    uint8_t recv_bytes[64];
    DWORD dwBytesReceived;

    ReadFile(com_port, &recv_bytes, 64, &dwBytesReceived, 0);

Transmitting Data
^^^^^^^^^^^^^^^^^^^^

Serial data to be written to the module can be written to the file specified by the ``com_port`` parameter. In order to write data:

.. code-block:: cpp

    uint8_t packet_buf[64];
    uint8_t length = 0;
    DWORD bytes_written;

    // Fill in packet_buf with data (as described later using the API), and update length

    WriteFile(com_port, packet_buf, length, &bytes_written, NULL);
