The below "recipes" have been tested on Fedora 18 and 19 only, they may or may
not work on other versions.

Replace NetworkManager with classic networking
----------------------------------------------

Stop and disable the NetworkManager service, enable and start the classic
network service:

    systemctl stop NetworkManager.service
    systemctl disable NetworkManager.service
    systemctl enable network.service
    systemctl start network.service

The 3rd and 4th commands didn't work on the first try, so I used this
temporarily:

    service network start

If you have several interfaces, you may need to set only one of them to use
DHCP, in case separate instances of dhclient overwrite /etc/resolv.conf with
something you don't want. Network configuration files are located under the
following names (if you want to edit manually):

    /etc/sysconfig/network-scripts/ifcfg-ethX


Enable OpenSSH server
---------------------

The OpenSSH server (openssh-server) is installed, but not enabled by default.
To start on boot:

    systemctl enable sshd.service
    systemctl start sshd.service


Install `build-essential' equivalent
------------------------------------

For the very basics, you can install the following packages:
  * make
  * automake
  * gcc
  * gcc-c++
  * kernel-devel

For a more complete development environment, the sofware package group
"Development Tools" should be installed:

    yum groupinstall "Development Tools"

and optionally

    yum groupinstall "Development Libraries"

Obviously, this can be easily done in the UI as well.

Some additional packages, not included in the above, that you may want/need
for development:
  * cmake
  * clang-analyzer
  * wireshark


Install VMware Tools on Fedora 19
---------------------------------

A freshly installed Fedora 19 guest comes preinstalled with open-vm-tools, an
open-source version of the VMware tools for guest operating systems. However,
these tools can't do everything the proprietary version can, at least on
VMware Fusion, so here's the steps required to swap things out.

First, we need to remove open-vm-tools from the system:

    sudo yum remove open-vm-tools open-vm-tools-desktop

Next we need to install the kernel header files required for compiling VMware
tools:

    sudo yum install -y kernel-devel-$(uname -r)

Normally this would be the last step before starting the tools installation,
but Fedora 19 (and RHEL 6.4) have a version.h file that's not where the tools
installer expects it. So, copy it to where it's needed:

    sudo cp /usr/src/kernels/$(uname -r)/include/generated/uapi/linux/version.h \
      /lib/modules/$(uname -r)/build/include/linux/

Now, initiate the VMware tools install - on VMware Fusion this is the menu
item Virtual Machine -> Reinstall VMware Tools. After the virtual CD is
mounted, proceed with the install:

    cd /tmp/
    cp /run/media/bbrowning/VMware\ Tools/VMwareTools-*.tar.gz .
    tar xzf VMwareTools-*.tar.gz
    sudo vmware-tools-distrib/vmware-install.pl

I just accepted all the defaults for every prompt, including allowing the
installer to run vmware-config-tools.pl.

Hope that helps - having to copy the `version.h` file is the biggest thing
that will trip up experienced VMware users.
