.. _control_center_start_guide:

#################################
IQ Control Center Manual
#################################

************************************
About IQ Control Center
************************************

IQ Control Center is the easiest and fastest way to configure your Vertiq modules. It provides simple mechanisms for 
changing module parameters, updating firmware, and testing basic module functionality. This page is meant to help guide you from first 
installation to a full understanding of using the Control Center software.

Please note that not all module features and parameters are accessible through the Control Center. If you are looking for a more specialized 
interaction with your module(s), you must use Vertiq's APIs (:ref:`Arduino<getting_started_arduino_api>`, :ref:`Python<getting_started_python_api>`, :ref:`C++<getting_started_cpp_api>`, :ref:`Matlab<getting_started_matlab_api>`)

.. note::
    All examples in this document use instructions and images for IQ Control Center v1.5.2. Beyond the Downloading and Installing section, 
    all examples will be shown using the Windows version of IQ Control Center.

************************************
Downloading and Installing
************************************
Downloads for all operating systems can be found on the Control Center's `GitHub <https://github.com/iq-motion-control/iq-control-center/releases>`_.

Windows
============
1. Navigate to the most recent release on Control Center's `GitHub <https://github.com/iq-motion-control/iq-control-center/releases>`_
2. Click on the Windows installer to download it

.. image:: ../_static/control_center_pics/windows/windows_install.png
    :scale: 50%

3. Unzip the installer, and inside of the unzipped folder, you will see

.. image:: ../_static/control_center_pics/windows/win_unzipped.png

4. Run the application
5. You will see the IQ Control Center Setup window appear

.. image:: ../_static/control_center_pics/windows/win_installer_pg1.png

6. Complete the installation as instructed by the installer wizard
7. If successful, you will see the following after selecting Show Details

.. image:: ../_static/control_center_pics/windows/windows_installer_done.png

8. Click Next, then Finish
9. Now, if you hit the Windows key, and search for IQ Control Center, you should see the Application listed. Click it to start the application

.. image:: ../_static/control_center_pics/windows/windows_key_search.png

10. Alternatively, navigate to the location where you installed the application
11. Open the folder, then open IQ Control Center

.. image:: ../_static/control_center_pics/windows/windows_installed_folder.png

12. Inside of that folder, you will find the IQ Control Center executable. Double click it to start the application

.. image:: ../_static/control_center_pics/windows/win_executable.png

13. After opening IQ Control Center, you should see the following

.. image:: ../_static/control_center_pics/windows/win_control_center_open.png

Congratulations! You have successfully installed IQ Control Center, and are ready to start communicating with your modules.

Linux (Ubuntu)
=================
1. Navigate to the most recent release on Control Center's `GitHub <https://github.com/iq-motion-control/iq-control-center/releases>`_
2. Click on the Linux installer to download it
   
.. image:: ../_static/control_center_pics/linux/linux_installer_github.png
    :scale: 75%

3. Once downloaded, unzip the folder, and you will see the installer

.. image:: ../_static/control_center_pics/linux/linux_installer_icon.png

4. Double click to start the installer 
   
.. note::
    If your installer does not open, please perform the following steps.

    a. Open a new terminal window
    b. Navigate to the location storing the Control Center Installer

    .. image:: ../_static/control_center_pics/linux/linux_terminal_location.png

    c. Use ./ to run the installer
    d. If you see the following error You MUST run `sudo apt-get install libxcb-xinerama0`

    .. image:: ../_static/control_center_pics/linux/xinerama_error.png

    e. Once libxcb-xinerama0 is installed, you should be able to open the installer

5. Once open, you will see the following window

.. image:: ../_static/control_center_pics/linux/linux_installer_open.png

6. Complete the wizard in order to install the IQ Control Center application
7. Once the installation is complete, and after clicking Show Details, you will see

.. image:: ../_static/control_center_pics/linux/linux_finished_install.png

8. Click Next, and Finish to complete installation
9. Navigate to your installation location, and find the IQ Control Center folder
10. Enter the IQ Control Center folder, and run IQ Control Center
11. Once open you will see the application

.. image:: ../_static/control_center_pics/linux/linux_control_center_open.png

Congratulations! You have successfully installed IQ Control Center, and are ready to start communicating with your modules.

Mac
=======
1. Navigate to the most recent release on Control Center's `GitHub <https://github.com/iq-motion-control/iq-control-center/releases>`_
2. Click on the Mac installer to download it

.. image:: ../_static/control_center_pics/mac/mac_installer_link.png
    :scale: 75%

3. Navigate to the folder where the software was downloaded

.. image:: ../_static/control_center_pics/mac/mac_downloaded_installer.png

4. **While holding CTRL**, click the file, and select Open
5. In the pop-up that opens, select Open to start the installer

.. image:: ../_static/control_center_pics/mac/mac_warning.png

6. You will see the following window appear

.. image:: ../_static/control_center_pics/mac/mac_wizard_open.png

7. Follow the instructions in the wizard to complete installation
8. After clicking Show Details and a successful installation, you will see the following

.. image:: ../_static/control_center_pics/mac/mac_install_done.png

9. Click Next and Finish to complete installation
10. Navigate to the installation location and find the IQ Control Center folder. Inside you will find

.. image:: ../_static/control_center_pics/mac/mac_inside_the_folder.png

11. Double click on the IQ Control Center icon to start the application
12. Once open you should see the following

.. image:: ../_static/control_center_pics/mac/mac_control_center_open.png

Congratulations! You have successfully installed IQ Control Center, and are ready to start communicating with your modules.

**************************
Hardware Configuration
**************************

Connection with a Computer
===============================

In order to communicate from your computer with a Vertiq module through the Control Center, you will need some form of USB-to-UART converter.

We recommend using an CP2102 FTDI such as the one found `here <https://www.amazon.com/Ximimark-Module-Serial-Converter-CP2102/dp/B07T1XR9FT?>`_.

Once obtained, plug your USB-to-UART converter into your computer and install any necessary drivers (refer to the manufacturer's documentation for 
your USB-to-UART converter). If installed correctly, a new serial port should be available on your PC. 
Take note of the name of this new serial port. In Windows you can see this under Device Manager->Ports (COM & LPT). The image 
below shows an example where the connected USB-to-UART converter shows up as COM3. This port will be used later when connecting with the Control Center.

.. image:: ../_static/control_center_pics/windows/device_manager.png

Wiring
=============
In order to communicate with your module(s) through IQ Control Center, you must connect serial communication wires. 
For instructions on how to do so for your specific module, visit your module's family page:

* :ref:`Vertiq 23-XX Family Serial Configuration <23xx_comms>`
* :ref:`Vertiq 40-XX Family Serial Configuration <40xx_comms>`
* :ref:`Vertiq 81-XX Family Serial Configuration <81xx_comms>`

On modules with exposed serial pads, we recommend using cabling with one end set into a JR Servo female connector, and the other 
end soldered directly to the module's pads. On the 81-08 G1, we recommend using a cable with a Servo JR female connector on both sides.

Using Servo JR connectors makes it easy to connect with, and disconnect from the USB-to-UART device that you are using to connect with the Control Center. 

Single Module Wiring
---------------------------
To communicate with a single module, simply connect the Servo JR female connector to the FTDI.

**Please ensure that your USB-to-UART device's TX port is connected to your module's RX port, and your module's TX to the USB-to-UART device's RX.**

A module wired for both power and serial communication attached to the aforementioned FTDI looks as follows:

.. image:: ../_static/control_center_pics/module_with_ftdi.png
    :scale: 12%

Multiple Module Wiring
---------------------------
If you plan on connecting several modules to the Control Center at once, you must connect all of the modules' serial ports together. 
How you do this is up to you, but you must ensure that the resulting wiring with your USB-to-UART device looks as follows:

.. image:: ../_static/control_center_pics/multi_module_serial.png

We highly recommend connecting your modules together with a non-permanent, easily detachable method. Some configurations through the Control Center 
must be performed on independently connected modules before multiple can be connected at once. So, having a quick way to attach/detach modules will 
make the process much easier. A simple way to accomplish this is with 3-pin, 100-mil header pins and a breadboard.

In order to connect with several modules at once, you will also have to configure each module's Module ID. 
More information about this can be found <link to the NOTE below JORDAN DON'T FORGET TO DO THIS> here.

***********************************
IQ Control Center GUI Overview
***********************************
.. image:: ../_static/control_center_pics/gui_breakdown.png
