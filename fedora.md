The below "recipes" have been tested on Fedora 18 only, they may or may not
work on other versions.

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

The OpenSSH server is installed, but not enabled by default. To start on boot:

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
