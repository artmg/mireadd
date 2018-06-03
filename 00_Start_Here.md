
If you have never done an NCS project before then hopefully Staring Here 
will get you on your way. 

## From Windows or MacOS

We're assuming that you're not lucky enough to use an ubuntu derivative 
as your day to day working system, so let's see how you can start from a Windows or Mac.
Neither of those operating systems are directly supported by the Intel Movidius NCS 
Software Development Kit (SDK), so let's use Ubuntu or Lubuntu on them anyhow, 
through the power of virtualisation. 

Here are simple step by step instructions for setting up 
a guest virtual machine to run ?ubuntu 
so you can use the Movidius Neural Compute Stick (NCS)

Use Oracle VirtualBox to host a linux guest OS

These instructions have been tested on macOS High Sierra and on Windows 10.

## Install Virtual Box

* or you could use vmWare workstation if you prefer
* you will also need Virtual Box Extension Pack to use USB2/3
  * https://www.virtualbox.org/wiki/Downloads

## download Ubuntu ISO

* Current [support](https://ncsforum.movidius.com/discussion/100/product-faq) only for Ubuntu 16.04
* coming from Windows you may find it easier to use Lubuntu derivitive
* http://cdimage.ubuntu.com/lubuntu/releases/16.04.4/release/lubuntu-16.04-desktop-amd64.iso

## create the VM

* use the wizard to create a new Linux/Ubuntu VM
* you can set 2048 MB RAM for now, and this can be increased later if needed
* 20GB should be more than ample for most projects
* once you have created the VM...
* start up the VM
* browse to the above ISO image to mount it in the virtual optical cdrom drive
  * if you're not prompted for the above then 
    * use the 'Host key' e.g. Right-Alt to leave the VM window
    * Click the title of the VM window to see the Virtual Box menu
    * Devices / Optical Drives / Choose disk image
* choose Try Lubuntu 
* once the desktop opens use the Install Lubuntu icon
* use install type: Erase disk and install Lubuntu
  * this is safe because you're in a VM and the VMDK was just created empty :)
* answer install steps as you prefer
  * you will find it easiest to 'Log in automatically'
  * you are unlikely to need store sensitive data so no need to encypt home drive
* follow through until reboot
* restart now
* if the VM does not restart by itself
  * use the Host Key to leave the VM window 
  * from the Virtual Box menu choose Machine / Reset
* unmount the ISO from the VM


## Install guest tools

Before setting up to pass through the USB below, you must install the Guest Additions. 
These tools are mainly drivers that make the guest function well inside Virtual Box. 

* Insert/ Mount **Guest Additions Cdrom**
  * in the VBox guest menu: Devices / Insert Guest Additions CD image
* If prompted, Open in file manager
  * otherwise double-click on the VBox_GAs desktop icon
* press F4 to open a terminal in this folder
* Install the guest additions with the commands below...

**Hint:** if you can't paste these commands into your VM from your host, 
then browse to this page _inside_ your VM :)

If you are going to paste in code blocks, you should start with this one
whcih asks for your password to 'sudo'

```
sudo echo
```

Otherwise, if you just type them in manually, 
then start below and don't bother with the lines begining with `#`

```
# install the dependency 'dkms' to get guest tools installed
sudo apt install -y git dkms

sudo ./VBoxLinuxAdditions.run
```

This may take a minute or so. 
If it installed without errors then reboot (`sudo reboot`)

You may now enable copy and paste...

* Enable copy & paste
  * Devices / Shared Clipboard / Bidirectional

As a bonus, you can now dynamically resize the desktop area by dragging the window edges

## Pass through USB

The most important reason for the guest additions above, though, is to make USB 2 & 3 work.
These will be used to pass through the Movidius Stick for the VM to be able to use.

* Open the Device Settings to add Pass through PNP Device filters
  * Ports / USB 
  * Enable USB: USB3 (xHCI controller)
  * Device Filters / Add Empty / Edit
  * Name: NCS USB2 - Venor ID: 03e7 - Product ID: 2150
  * Name: NCS USB3 - Venor ID: 03e7 - Product ID: f63b
  * credit https://ncsforum.movidius.com/discussion/406/ncs-on-windows-10-with-virtualbox 

## test you can pass the USB stick through to the VM

* yes, you _may_ feel excited because you are pluging in your new blue wonder
* no, it's **not** going to start doing anything awesome just yet
* plug your NCS into a USB interface on your PC
* check you can see the device in ubuntu
  * from a terminal run the following command
  * Terminal is in the start menu / System Tools / LXTerminal
  * or use the keyboard shorctut CTRL - ALT - T

```    
lsusb
```

## updates and snapshot

As a final couple of steps, you can install any recent updates to the old OS version, 
and then take a snapshot so you can revert to this clean install any time you want. 

* open the Terminal with CTRL ALT T
* if you are going to paste the following commands as a block start with `sudo echo`

```    
sudo apt update

sudo apt upgrade -y
```

Now take a snapshot of the current VM state:

* Shut down
* In the Virtual Box Manager, look at the top of the right hand pane
* Look for Machine Tools and Details
* near here you will see a button maked 'Snapshots' - click it
* click Take + 
* Enter a name, e.g. Fresh install with tools and updates
* click OK

Now you will be able to undo anything you do after this point, 
and return to this clean fresh install, any time you want.

Snapshots are especially useful if you want to try out a whole variety of projects 
one after another. 
They avoid clashing when multiple versions and libraries are installed for each, 
and they avoid your disk filling up as you clean it down each time.
Also if you always go back to a clean system, 
you can be sure that instructions you write will work consistently for someone else.


## Now you have ?ubuntu...

* Install the Movidius™ Neural Compute (MvNC) Software Development Kit (SDK)
  * NB: this is using the source code branch for the SDK VERSION 2  
  * open a Terminal window (CTRL-ALT-T)
  * paste in the following commands

```
# install the dependencies 
sudo apt install -y git make

# credit https://developer.movidius.com/start
mkdir -p ~/workspace
cd ~/workspace
# using branch for new v2 - see https://github.com/movidius/ncsdk
git clone -b ncsdk2 https://github.com/movidius/ncsdk.git
cd ~/workspace/ncsdk
make install

# Test installation by running built-in examples
cd ~/workspace/ncsdk
make examples
```

If you want you may choose to take another snapshot here


### Troubleshooting

If you get odd errors when playing with the install, 
you can uninstall the SDK to try again as follows

```
cd ~/workspace/ncsdk
./uninstall.sh
cd ..
rm –rf ncsdk
# credit https://ncsforum.movidius.com/discussion/670/caffe-build-error-during-ncsdk-installation
```

## Next Steps

* Try the NCAppZoo stream_infer to recognise from a webcam
 * https://github.com/intel-aero/meta-intel-aero/wiki/07-Movidius-Neural-Compute-Stick#ui-example
 * To pass through the webcam on VirtualBox use the guest Devices Menu
* learn about how the image classifier is created
 * https://movidius.github.io/blog/ncs-image-classifier/
* Introduction to the Development workflow
 * https://movidius.github.io/ncsdk/
* More detail on developing beyond the simple stream_infer sample earlier
 * how to train a network specifically for the kind of application you need, 
    based on a data set you want to use
 * https://movidius.github.io/blog/deploying-custom-caffe-models/ 

### Robotics 

Intel also have a Robotic Open Source (ROS) project, 
http://wiki.ros.org/IntelROSProject, 
that includes 2D and 3D camera modules. 


### Advanced usage with docker containers


see: 

* https://github.com/kwierman/docker_ncsdk
* https://stackoverflow.com/questions/49181354/run-ncsdk-movidius-neural-stick-on-macos
* https://ichbinblau.github.io/2017/12/19/Running-the-NCSDK-examples-in-Docker/


