# Kismet Linux WiFi capture tool for OpenWRT routers

Compiling Kismet for OpenWRT
Sources:

https://github.com/kismetwireless/kismet-docs/blob/master/howto/remote-capture-build.md

Prerequisites for OpenWRT compiler:

https://oldwiki.archive.openwrt.org/doc/howto/buildroot.exigence

Equipment needed:

-Ubuntu machine

-Internet access

Setting up dev environment:

-Ensure you have an Ubuntu machine with internet and root access.  It can be a minimal install but you’ll need at least 50MB of hard drive space.

-Ensure the necessary dependencies are installed:

*sudo apt-get install git make build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex git-core gettext libssl-dev unzip python3-distutils mercurial

 *sudo apt- get update
 


Pull Kismet and OpenWRT packages:

-In this example I’ll go under the root directory and make a directory called kismet:  mkdir kismet

-Navigate to that directory:   

-In a terminal session in the root directory run the following to pull the most recent Kismet package with capture:  git clone https://github.com/kismetwireless/kismet-packages

-For OpenWRT, also run git clone https://github.com/openwrt/openwrt.git

-Now copy the Kismet package over:  cp -R /kismet/kismet-packages/openwrt/kismet-openwrt/kismet-capture-linux-wifi /kismet/openwrt/package/network

-Also copy the kismet.mk file over:  cp -R /kismet/kismet-packages/openwrt/kismet-openwrt/kismet.mk /kismet/openwrt/package/network

-Navigate to the directory where you moved the kismet folder over to:  cd /kismet/openwrt/package/network

-Open the Makefile and add a version and revision number under PKG_NAME.  The actual number shouldn’t matter.  It should look something like this:

PKG_NAME:=kismet-capture-linux-wifi

PKG_VERSION:=8

PKG_RELEASE:=1

-Save the file.


OpenWRT Config:

-Navigate to the OpenWRT directory you just cloned – cd openwrt

-Start the config tool – make menuconfig

-Confirm the correct platform is selected – ‘Atheros AR7xxx/AR9xxx’ and ‘Subtarget (Generic)’ in this case.

-Navigate to Image Configuration.

-Navigate to Separate Feed Repositories.

-Select ‘Enable feed packages.’

-Exit the config tool and save when prompted.

-Now we need OpenWRT to pull feeds and update the build system.  Run the following two commands:

./scripts/feeds update -a

./scripts/feeds install -a

-Make sure you are in the OpenWRT directory and again run 'make menuconfig'

-Navigate to ‘Network’.  Scroll down until you see ‘kismet’ and select hit <enter>.  Select ‘M’ for ‘kismet-capture-linux-wifi’ to enable the module.

-Exit and save when prompted.


Compile OpenWRT:

-Note:  if running as root, you will first need to type 'export FORCE_UNSAFE_CONFIGURE=1'

-Under the OpenWRT directory, type 'make -j4  V=sc'

-If everything went well you should now have two packages to copy over to your OpenWRT router.

-Under your OpenWRT folder navigate to  /bin/packages/<architecture>/base and see if it is listed in there.

-If you see both packages you are good to go!

-At this point you have the package you need.  First install the prereq packages from my repository on the router, then you should be able to install the package created from this 
process.
