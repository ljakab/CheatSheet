VM template customization
-------------------------

 * /etc/sudoers with NOPASSWD
 * static networking in /etc/network/interfaces for private network
 * set up proxies if necessary (/etc/environment; /etc/apt/apt.conf)
 * completely disable automatic updates, including package lists:
   ** /etc/apt/apt.conf.d/10periodic
   ** /etc/apt/apt.conf.d/50unattended-upgrades
 * add ~/bin to PATH
 * clone .files
 * add VM private key and authorized public keys
 * apt-get update
 * apt-get dist-upgrade
 * apt-get install build-essential autoconf libtool rcs cvs subversion \
   git-review clang cscope pkg-config bison flex libssl-dev libconfuse-dev \
   libzmq3-dev libxml2-dev gengetopt debhelper dh-autoreconf dkms virtualenv
 * apt-get install tshark htop iotop tcpreplay traceroute elinks lynx \
   python-scapy python-netifaces python-netaddr speedometer fish npm nmap
 * gpasswd -a <username> wireshark
 * install VMware Tools
 * add-apt-repository ppa:webupd8team/java && apt-get update && \
   apt-get install oracle-java7-installer && \
   apt-get install oracle-java8-installer
 * update-alternatives --config java
 * mkdir opt && cd opt && wget http://mirrors.gigenet.com/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz ...
 * install the ODL controller into opt
 * install the yourkit profiler into opt
 * git clone into src: lig, lispmob, ovs, opendaylight/lispflowmapping,
   opendaylight/integration

Need to change after VM instantiation
-------------------------------------

 * /etc/hostname
 * /etc/hosts
 * /etc/network/interfaces
