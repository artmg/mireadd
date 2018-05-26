
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

These instructions have been tested on macOS High Sierra, but should work fine in Windows too.

## Install Virtual Box

* or you could use vmWare workstation if you prefer
* you will also need Virtual Box Extension Pack to use USB2/3
  * https://www.virtualbox.org/wiki/Downloads

## download Ubuntu ISO

* Current [support](https://ncsforum.movidius.com/discussion/100/product-faq) only for Ubuntu 16.04
* coming from Windows you may find it easier to use Lubuntu derivitive
* http://cdimage.ubuntu.com/lubuntu/releases/16.04.4/release/lubuntu-16.04-desktop-amd64.iso

## create the VM

* 20GB should be more than ample
* Open the Device Settings to add Pass through PNP Device filters
  * Ports / USB 
  * Enable USB: USB3 (xHCI controller)
  * Device Filters / Add Empty / Edit
  * Name: NCS USB2 - Venor ID: 03e7 - Product ID: 2150
  * Name: NCS USB3 - Venor ID: 03e7 - Product ID: f63b
  * credit https://ncsforum.movidius.com/discussion/406/ncs-on-windows-10-with-virtualbox 
* mount the ISO in the virtual cdrom
* start up the VM
* choose Try Lubuntu 
* once the desktop opens use the Install Lubuntu icon
* use install type: Erase disk and install Lubuntu
  * this is safe because you're in a VM and the VMDK was just created empty :)
* answer install steps as you prefer
* follow through until reboot
* unmount the ISO from the VM
* reboot

## Install guest tools

This is the other part of getting the USB working properly

**Note:** when you use sudo in ubuntu you are asked for a password. 
You may find it easier to type 

```
sudo echo
```

so that you are prompted for that password (which is cached for 5 minutes) 
before you paste in code blocks

**Hint:** if you can't paste these commands into your VM from your host, 
then browse to this page _inside_ your VM :)

* Insert/ Mount **Guest Additions Cdrom**
  * in the VBox guest menu: Devices / Insert Guest Additions CD image
* Open in file manager
* press F4 to open a terminal in this folder
* Install the guest additions with the command...

```
# install the dependency 'dkms' to get guest tools installed
sudo apt install -y git dkms

sudo ./VBoxLinuxAdditions.run
```

if it installed without errors then reboot (`sudo reboot`)

* Enable copy & paste
  * Devices / Shared Clipboard / Bidirectional

As a bonus, you can now dynamically resize the desktop area by dragging the window edges


## test you can pass the USB stick through to the VM

* yes, you _may_ feel excited because you are pluging in your new blue wonder
* no, it's **not** going to start doing anything awesome just yet
* plug your NCS into a USB interface on your PC
* check you can see the device in ubuntu
  * from a terminal run the command 

```    
lsusb
```

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


