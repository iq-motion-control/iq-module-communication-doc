
.. include:: ../text_colors.rst
.. toctree::

.. _manual_buzzer_control:

**************************************
Buzzer Controller and Status Songs
**************************************

What is the Buzzer
====================
Your module is capable of inducing specific vibration frequencies in order to create audible tones. These buzzer tones are used to indicate several module statuses. 
Here, we will discuss all possible “songs” your module may play during normal operation and their meanings. We will also discuss how to use the 
:ref:`Buzzer Control client <buzzer_control>` in order to create and play your own songs.

Standard Songs
==================

Startup
-----------
On a successful power-up and initialization, your module will play a 5-tone startup song. After this song completes, your module is ready for use.

The complete startup song is played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/startup_song.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

In the event that your module is programmed with improper firmware, its calibration data becomes corrupted, or its product key becomes corrupted your module will only 
play a 3-tone startup song. If :ref:`reprogramming your module <updating_firmware_control_center>` does not resolve this issue, please contact us at support@vertiq.co.

The failed startup song is played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/bad_startup.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

.. _arming_song:

Arming
-------
If you are using the :ref:`Advanced Arming feature <manual_advanced_arming>`, then, on arming, your module will perform its :ref:`advanced_arming_behavior` which includes the arming song played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/arming_song.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

Disarming
------------
If you are using the :ref:`Advanced Arming feature <manual_advanced_arming>`, then, on disarming, your module will perform its :ref:`advanced_disarming_behavior`. 
If the song playback option is configured to either Once or Continuously, you will hear the disarming song played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/disarm.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

If the song playback option is set to Continuously, the disarm song will repeat until the disarm state is broken.

Analog Hobby Protocol Calibration
-------------------------------------
While :ref:`performing analog hobby protocol calibration <hobby_calibration_tutorial>`, you will hear two separate song portions indicating the calibration's progress. 
The first portion of the song indicates that the calibration has started and is looking for the highest possible throttle signal. This is followed by tones played twice as fast indicating that the calibration is looking for the lowest possible throttle signal.
An example of a full calibration song is played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/hobby_calibration.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

Timeout
----------
When a :ref:`timeout <manual_timeout>` is reached, your module will play the timeout song as specified by its :ref:`timeout_behavior`. 
When the Song Playback is set to play either once or continuously, you will here the timeout song played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/timeout.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

If the song playback option is set to Continuously, the timeout song will repeat until the timeout is ended.

Watchdog
-----------
Watchdog timers are used to indicate that there has been a blocking error in the firmware. When a watchdog error occurs, the module restarts, and plays the watchdog error song played here:

.. raw:: html

    <audio controls>
        <source src="../_static/manual_images/buzzer_song_mp3s_pics/watchdog.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>

If your module plays the watchdog error song, please reach out to our support team at support@vertiq.co with the details surrounding the watchdog. 
Any flight logs, support files, etc. that can be provided will help our team best diagnose and fix the issue in a new firmware release.

Buzzer Song Volume
====================
If the audio feedback from your module is either too loud or quiet, you can adjust its volume with the Volume Max parameter available in the Control Center's Advanced tab.

.. image:: ../_static/manual_images/buzzer_song_mp3s_pics/volume_max.png

To increase the volume, increase the maximum voltage, to decrease the volume, decrease maximum voltage.

Custom Songs with the Buzzer Control Client
===================================================
Since the buzzer controller is a :ref:`standard IQUART client <buzzer_control>`, you can communicate with it via IQUART. The examples presented here will be performed using the :ref:`Python API <getting_started_python_api>`.

In order to play a note of your choosing, you must set, in order, a frequency, duration, and volume. Only then can you trigger playback.

Suppose you want your module to play a middle C (261.6 Hz) for 3 seconds. 

.. code-block:: python

    import iqmotion as iq
    import time

    # Module Communication - This must be updated with the correct parameters for your module's serial communication
    com = iq.SerialCommunicator("COM3", baudrate=115200)

    # Using the Vertiq2306
    module = iq.Vertiq2306(com, firmware="speed")

    #Play a middle C for 3 seconds
    module.set("buzzer_control", "hz", 261.6)
    module.set("buzzer_control", "duration", 3000)
    module.set("buzzer_control", "volume", 100)
    module.set("buzzer_control", "ctrl_note")

    #Let it finish
    time.sleep(5)

    #Release the module
    module.set("buzzer_control", "ctrl_coast")

While the module is playing a note, its reported ``ctrl_mode`` is note (2). This value is polled in order to expand our basic application to play basic songs. 
The following can be used as a basis to create custom songs, and provides an example of playing the C-Major scale.

.. code-block:: python

    import iqmotion as iq

    # Module Communication - This must be updated with the correct parameters for your module's serial communication
    com = iq.SerialCommunicator("COM3", baudrate=115200)

    # Using the Vertiq2306
    module = iq.Vertiq2306(com, firmware="speed")

    # Information about how loud to make each note as well as the frequency of each note in the C-Major scale
    volume = 100
    c = 261.6
    d = 293.7
    e = 329.7
    f = 349.2
    g = 392
    a = 440
    b = 493

    # Fill in our song information
    duration = 750  # ms
    c_scale = [c, d, e, f, g, a, b, c * 2]
    c_scale_durations = [duration]*len(c_scale)


    def play_note(hz, duration):
        module.set("buzzer_control", "hz", hz)
        module.set("buzzer_control", "duration", duration)
        module.set("buzzer_control", "volume", volume)
        module.set("buzzer_control", "ctrl_note")

    def play_song(notes, durations):
        for i in range(len(notes)):
            play_note(notes[i], durations[i])

            # Wait until the current note is done
            while (module.get("buzzer_control", "ctrl_mode") == 2):
                pass

    if __name__ == "__main__":
        play_song(c_scale, c_scale_durations)
        module.set("buzzer_control", "ctrl_coast")
