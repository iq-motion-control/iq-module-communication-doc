.. include:: ../text_colors.rst
.. toctree::

.. _protect_against_regen:

*************************************************************************
Configuring Your Module to Protect Against Regeneration Voltage Spikes
*************************************************************************

What are Regenerative Voltage Spikes
=======================================
Regeneration voltage occurs when the commanded module voltage is less than the module's back-EMF voltage. When this occurs, the motor's kinetic 
energy is converted directly to electrical energy, and produces a current flowing out of the motor and into the power source.

The commanded module voltage may be less than the back-EMF voltage in cases where the module is slowing down or braking. The faster that the commanded voltage 
changes (for example, jumping from a 20V to a 0V command), the greater the magnitude of the regeneration current.

How Can Regeneration Affect Your Module
=========================================
If the module's power source can absorb the regenerated current without allowing the voltage to spike, regeneration protection isn't needed. This is often true with a battery as the power source.
When connected to a power source that cannot absorb this regenerated current, which is generally the case with benchtop power supplies, the output voltage of the source will often increase during regeneration. 
These spikes can result in voltages above the module's and your supply's maximum rated operating voltage. When this happens, you can permanently damage the module, the power supply, or both.

Protecting Against Dangerous Regeneration Voltage Spikes
===========================================================
Regenerative voltage spikes can pose a threat to your modules and power supply if not properly controlled. As such, all Vertiq modules can limit their regeneration current to limit the magnitude of voltage spikes on power supplies. 
Vertiq modules have two main parameters to protect against dangerous regeneration voltage spikes. They are available through IQ Control Center's advanced tab as *Volts Limit* and *Volts Limit Starting Voltage*.

For example, by default on modules rated to 14S, you will see:

.. image:: ../_static/control_center_pics/regen_params.png
        :alt: Regeneration Parameters in IQ Control Center

In short, these two values work together in order to limit the rate at which your module will slow down or apply a braking force. In doing so, the module limits the amount of 
negative supply current produced, reducing the voltage spike produced by the power supply.

.. note::
    These configuration parameters define **system voltages**. For example, if your module has a maximum voltage rating of 14S (or 58.8V), but your power supply has a 
    maximum rating of 30V, the values of Volts Limit Starting Voltage and Volts Limit must be set according to the power supply's absolute 30V maximum.

    As another example, suppose you want to power your module rated up to 12S with a 6S equivalent power supply. In this case, you would set your voltage limits based on the
    6S equivalent power supply.

    By default, Vertiq modules have these values set to protect the module's circuits while operating with the supplied voltage at its maximum rating. In order to protect your power supply and module, 
    please ensure that your configured voltage limits are set according to the **system's lowest maximum voltage rating**.

* **Volts Limit Starting Voltage** defines the value (in volts) at which the module will begin checking if any limits on regeneration need to be applied. A 0V starting voltage means 
  that regeneration voltage protection is always enabled. A starting voltage configured just below the Volts Limit enables maximum braking under normal conditions, and enables protection 
  only during voltage spikes.
  
* **Volts Limit** defines the absolute maximum supply voltage the module will apply. In some extreme instances, your module may apply a voltage higher than the configured Volts Limit. In order to handle this properly, ensure that there is a buffer between the absolute maximum allowable system voltage and the value of Volts Limit. 
  For example, if your system can handle a maximum of 30V, your Volts Limit may be 27V. Never set Volts Limit below the maximum supply voltage. For example, if you're using a 6S 
  battery with a maximum voltage of 25.2V, never set Volts Limit at or below 25.2V. Violating this rule may cause the motor to run away to its maximum speed.