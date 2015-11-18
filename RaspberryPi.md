Enable HDMI screen blanking with DPMS
-------------------------------------

Add this to `/boot/config.txt`:

    hdmi_blanking=1

Completely turn off the video module
------------------------------------

Add this to `/etc/rc.local`:

    /opt/vc/bin/tvservice -o
