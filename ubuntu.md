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

Or

Create /etc/init/service.override, and put "manual" inside.


Disable Debian style job
------------------------

Use either rcconf, sysv-rc-conf, or update-rc.d


Fix problems due to persistent interface naming
-----------------------------------------------

Edit `/etc/udev/rules.d/70-persistent-net.rules`


Build a custom kernel "the Debian way"
--------------------------------------

    apt-get install kernel-package fakeroot
    tar xvf linux-x.y.z.tar.xz
    cd linux-x.y.z
    make menuconfig
    scripts/config --disable DEBUG_INFO

Disabling `DEBUG_INFO` significantly decreases disk space requirements for the
build.

Two options to actually build the .deb packages:

    make-kpkg clean
    fakeroot make-kpkg --initrd --revision=1 kernel_image kernel_headers

or

    make clean
    make deb-pkg


Add PPA
-------

    sudo add-apt-repository <PPA>


I typically install the following packages on a base (server) install
---------------------------------------------------------------------

System utilities:

  * openssh-server
  * ntp
  * tshark (or wireshark if there is are X libraries installed)
  * traceroute
  * tcpreplay
  * python-scapy
  * elinks/lynx
  * fish
  * htop
  * iotop

Generic development tools:

  * build-essential
  * autoconf
  * libtool
  * clang
  * git
  * git-review
  * subversion
  * cvs
  * rcs
  * cscope
  * pkg-config
  * bison
  * flex
  * debhelper
  * dh-autoreconf
  * dkms
  * oracle-java7-installer (ppa:webupd8team/java)
  * maven

Build dependencies of more than one package I typically install from source:

  * libssl-dev

Build dependencies for LISPmob:

  * libconfuse-dev
  * libzmq3-dev
  * libxml2-dev
  * gengetopt

Setting up proxies
------------------

APT specific: `/etc/apt/apt.conf`

    Acquire::http::proxy "http://myproxy.server.com:8080/";
    Acquire::ftp::proxy "ftp://myproxy.server.com:8080/";
    Acquire::https::proxy "https://myproxy.server.com:8080/";

Global: `/etc/environment`

    http_proxy=http://myproxy.server.com:8080/
    https_proxy=http://myproxy.server.com:8080/
    ftp_proxy=http://myproxy.server.com:8080/
    no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
    HTTP_PROXY=http://myproxy.server.com:8080/
    HTTPS_PROXY=http://myproxy.server.com:8080/
    FTP_PROXY=http://myproxy.server.com:8080/
    NO_PROXY="localhost,127.0.0.1,localaddress,.localdomain.com"
