Role Name
=========

**⚠ NOT RELEASED YET - VERY INCOMPLETE & NOT TESTED ⚠**

This role makes the system sound available as an audio stream through DLNA.

That means that you will be able to listen to the sound playing on the targeted machine from any DLNA renderer.
This is used for instance to listen to the music played by a Raspberry Pi with Kodi on your Hi-Fi DLNA speakers.

Under the hood it :

- installs Rygel and GstLaunch (including PulseAudio and gstreamer, as required dependencies)
- creates a PulseAudio sink that appears as an audio stream on DLNA renderers

By default FLAC and MPEG streams are available ; you may customize / change this by updating the "Configure Rygel" step in `tasks/main.yml`.
FLAC is recommended to reduce audio delay if you have a decent network speed.
See https://wiki.gnome.org/Projects/Rygel/Pulseaudio#gst-launch.



Requirements
------------

It is aimed at [Raspberry Pi](https://www.raspberrypi.org/) and not tested on other platforms.



Role Variables
--------------

Please look at the self-documented `defaults/main.yml` to see what variables you need to set.



Dependencies
------------

None.



Example Playbook
----------------

    - hosts: servers
      roles:
         - role: nicolabs.dlna_audiostream
           vars:
               pulseaudio_users:
                - pi
                - me
               dlna_audiostream_expose_system_audio: no


A note on PulseAudio configuration for DLNA
-------------------------------------------

[All sources I've found](https://wiki.gnome.org/Projects/Rygel/Pulseaudio) give the graphic way to configure the DLNA server : by checking two boxes in the **paprefs** utility.

However this didn't allow for automatic configuration so this role implements it by adding the corresponding lines in `/etc/pulse/default.pa`.
I relied on https://gitlab.freedesktop.org/pulseaudio/paprefs/-/tree/master/src to understand how to replicate the _paprefs_ behaviour.

The effect is the same but, as stated in `default.pa`, please note that changes in this file will not be seen in _paprefs_ and playing with both might generate conflicts (well I guess one will just override the other...).



TODO
----

- redirect the default system sound to this sink (may be ignored if you just want to plug a given application into the sink)
- start Rygel at boot
- create more / allow customizing audio stream (e.g. a compressed one when the network is not reliable enough)



References
----------

- [PulseAudio main documentation](https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/)
- [github.com/BaReinhard/Super-Simple-Raspberry-Pi-Audio-Receiver-Install](https://github.com/BaReinhard/Super-Simple-Raspberry-Pi-Audio-Receiver-Install/blob/master/init.d/pulseaudio)



License
-------

MIT



Author Information
------------------

https//nicolabs.net
