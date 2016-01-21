# Adaptive Mesos
Adaptive Cluster based on Apache Mesos

## Requirements

Mesos runs on Linux (64 Bit) and Mac OS X (64 Bit). For full support of process isolation under Linux a recent kernel >=3.10 is required. 

Make sure your hostname is resolvable via DNS or via ```/etc/hosts``` to allow full support of Docker’s host-networking capabilities, needed for some of the Mesos tests. When in doubt, please validate that ```/etc/hosts``` contains your hostname.

## Downloading

1. Download the latest stable release from [Apache](http://mesos.apache.org/downloads/), or

	```
	$ wget http://www.apache.org/dist/mesos/0.26.0/mesos-0.26.0.tar.gz
	$ tar -zxf mesos-0.26.0.tar.gz
	```
2.  Clone the Mesos git [repository](https://git-wip-us.apache.org/repos/asf/mesos.git).

	```
	# Official Repo
	$ git clone https://git-wip-us.apache.org/repos/asf/mesos.git (Official)
	
	# Mirror Repo (Faster but delayed  ~4hrs)
	$ git clone https://github.com/apache/mesos (Mirror)
	```

**NOTE:** The official tar.gz distro 0.26.0 fails to fully complete ```make check``` on Mac OS X 10.11.2, it works, however, with the cloned repository. 
 
## Preparation

### Mac OS X (=> 10.9)

```
# Install Command Line Tools.
$ xcode-select --install

# Install Homebrew.
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# Install libraries.
$ brew install autoconf automake libtool subversion maven Caskroom/cask/java
```

### Ubuntu (=> 14.04)

```
# Update the packages.
$ sudo apt-get update

# Install the latest OpenJDK.
$ sudo apt-get install -y openjdk-7-jdk

# Install autotools (Only necessary if building from git repository).
$ sudo apt-get install -y autoconf libtool

# Install other Mesos dependencies.
$ sudo apt-get -y install build-essential python-dev python-boto libcurl4-nss-dev 	libsasl2-dev maven libapr1-dev libsvn-dev
```

### CentOS (=> 7.1)
	
```
# Install a few utility tools
$ sudo yum install -y tar wget

# Fetch the Apache Maven repo file.
$ sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo

# 'Mesos > 0.21.0' requires 'subversion > 1.8' devel package, which is
# not available in the default repositories.
# Add the WANdisco SVN repo file: '/etc/yum.repos.d/wandisco-svn.repo' with content:

      [WANdiscoSVN]
      name=WANdisco SVN Repo 1.9
      enabled=1
      baseurl=http://opensource.wandisco.com/centos/7/svn-1.9/RPMS/$basearch/
      gpgcheck=1
      gpgkey=http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco

# Install essential development tools.
$ sudo yum groupinstall -y "Development Tools"

# Install other Mesos dependencies.
$ sudo yum install -y apache-maven python-devel java-1.7.0-openjdk-devel zlib-devel libcurl-devel openssl-devel cyrus-sasl-devel cyrus-sasl-md5 apr-devel subversion-devel apr-util-devel
```
	
### CentOS (6.6)

```
	# Install a recent kernel for full support of process isolation.
    $ sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
    $ sudo rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
    $ sudo yum --enablerepo=elrepo-kernel install -y kernel-lt

    # Make the just installed kernel the one booted by default, and reboot.
    $ sudo sed -i 's/default=1/default=0/g' /boot/grub/grub.conf
    $ sudo reboot

    # Install a few utility tools. This also forces an update of `nss`,
    # which is necessary for the Java bindings to build properly.
    $ sudo yum install -y tar wget which nss

    # 'Mesos > 0.21.0' requires a C++ compiler with full C++11 support,
    # (e.g. GCC > 4.8) which is available via 'devtoolset-2'.
    # Fetch the Scientific Linux CERN devtoolset repo file.
    $ sudo wget -O /etc/yum.repos.d/slc6-devtoolset.repo http://linuxsoft.cern.ch/cern/devtoolset/slc6-devtoolset.repo

    # Import the CERN GPG key.
    $ sudo rpm --import http://linuxsoft.cern.ch/cern/centos/7/os/x86_64/RPM-GPG-KEY-cern

    # Fetch the Apache Maven repo file.
    $ sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo

    # 'Mesos > 0.21.0' requires 'subversion > 1.8' devel package, which is
    # not available in the default repositories.
    # Add the WANdisco SVN repo file: '/etc/yum.repos.d/wandisco-svn.repo' with content:

      [WANdiscoSVN]
      name=WANdisco SVN Repo 1.8
      enabled=1
      baseurl=http://opensource.wandisco.com/centos/6/svn-1.8/RPMS/$basearch/
      gpgcheck=1
      gpgkey=http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco

    # Install essential development tools.
    $ sudo yum groupinstall -y "Development Tools"

    # Install 'devtoolset-2-toolchain' which includes GCC 4.8.2 and related packages.
    $ sudo yum install -y devtoolset-2-toolchain

    # Install other Mesos dependencies.
    $ sudo yum install -y apache-maven python-devel java-1.7.0-openjdk-devel zlib-devel libcurl-devel openssl-devel cyrus-sasl-devel cyrus-sasl-md5 apr-devel subversion-devel apr-util-devel

    # Enter a shell with 'devtoolset-2' enabled.
    $ scl enable devtoolset-2 bash
    $ g++ --version  # Make sure you've got GCC > 4.8!
	
```	

## Building

```
# Change working directory.
$ cd mesos

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

# Configure.
$ mkdir build
$ cd build
$ ../configure

# Build.
# Use make -j <NUMBER OF CORES> for faster builds.
$ make

# Test.
$ make check

# Install (optional).
$ make install
```