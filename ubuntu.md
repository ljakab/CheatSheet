NOTE: Many recipes should work unchanged on Debian, but since I don't use/test
      it, I just named the file 'ubuntu.txt'. And some will work on other
      distros as well.


Find Ubuntu version
-------------------

    lsb_release -a


Disable console screen saver
----------------------------

Update `/etc/kbd/config` to have:

    BLANK_TIME=0
    POWERDOWN_TIME=0

Alternatively, in the current console, run `setterm -blank 0`.


Disable framebuffer console on Ubuntu 10.04
-------------------------------------------

Add `blacklist vga16fb` to `/etc/modprobe.d/blacklist.conf`.


Globally enable core dumps
--------------------------

In `/etc/security/limits.conf`:

    *               soft    core            unlimited

The root user ignores the wildcard, so you have to specify it explicitly.


Disable upstart job
-------------------

Edit /etc/init/service.conf


Disable Debian style job
------------------------

Use either rcconf, sysv-rc-conf, or update-rc.d


Fix problems due to persistent interface naming
-----------------------------------------------

Edit `/etc/udev/rules.d/70-persistent-net.rules
