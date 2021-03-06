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


Disable firewall
----------------

    systemctl stop firewalld.service
    systemctl disable firewalld.service


Disable automatic updates
-------------------------

Start the **Software** application, and chose "Software Sources".  In the
"Update Settings" there is an option called "Check for updates" which can be
set to never.


SElinux
-------

To check the current status of SELinux on your host run `sestatus` or
`getenforce`.  You can change the policy by editing the `SELINUX` variable in
`/etc/selinux/config` and rebooting.

    setenforce 0
    sed -i -e 's/SELINUX=enforcing/SELINUX=permissive/g' /etc/selinux/config


Install Debian's "build-essential" equivalent
---------------------------------------------

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


Find out which package contains a certain command
-------------------------------------------------

    yum provides <command>


Enable git-prompt
-----------------

Add this to `~/.bash_profile`:

    # Show git branch and state in prompt
    export PS1='[\u@\h \W$(declare -F __git_ps1 &>/dev/null && __git_ps1 " (%s)")]\$ '
    export GIT_PS1_SHOWDIRTYSTATE=yes
    export GIT_PS1_SHOWSTASHSTATE=yes
    export GIT_PS1_SHOWUNTRACKEDFILES=yes

If you don't have bash completion active, add this to `~/.bashrc`:

    source /usr/share/git-core/contrib/completion/git-prompt.sh


Run commands at startup
-----------------------

There is no `rc.local` file by default in Fedora 18 and above.  In order to
run some extra commands at startup, you need to create `/etc/rc.d/rc.local`,
make it executable, and enable the `rc-local` service.  Note that the
`rc.local` file is a shell script that must end with `exit 0`.

    vim /etc/rc.d/rc.local
    chmod +x /etc/rc.d/rc.local
    systemctl start rc-local


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


Allow core dumps when abort() is called
---------------------------------------

Set `ProcessUnpackaged = yes` in
`/etc/abrt/abrt-action-save-package-data.conf`


Fully disable IPv6
------------------

Some times you just want to figure out if an issue is protocol family related.
Preivously IPv6 was compiled as a kernel module, and by blacklisting it or
simply removing it disabled the protocol.  Now it is compiled into the kernel,
and it can be disabled by setting the `ipv6.disable` option to 1.  Edit
`/etc/default/grub` and find the line containing `GRUB_CMDLINE_LINUX`.  Add
`ipv6.disable=1` to the rest of the boot options.  Then update the Grub 2
configuration, by running:

    grub2-mkconfig -o /boot/grub2/grub.cfg

Some tutorials suggest turning IPv6 off using `sysctl`:

    net.ipv6.conf.all.disable_ipv6=1
    net.ipv6.conf.default.disable_ipv6=1

However, this still allows the interfaces to get the default link-local
addresses, and so it doesn't completely disable IPv6.
