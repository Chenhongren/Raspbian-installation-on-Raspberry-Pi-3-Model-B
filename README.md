# Raspbian-installation-on-Raspberry-Pi-3-Model-B
 *Raspbian Jessie with Pixel*
 
# REFERENCE

http://www.pyimagesearch.com/2016/04/18/install-guide-raspberry-pi-3-raspbian-jessie-opencv-3/

# INTRODUCTION

The Raspberry Pi 3 is the third-generation Raspberry Pi. It replaced the Raspberry Pi 2 Model B in February 2016. This latest model (Raspberry Pi 3) includes 802.11n WiFi, Bluetooth 4.0, and a quad-core 64-bit ARM Cortex A53 running at 1.2 GHz. It’s a usable desktop computer. 

And the goal of this manual is to thus guide you step-by-step through the compile and installation process. There are ten parts in this document. If you follow these steps, you can operate image processing using OpenCV on the Raspberry Pi 3 Model B. Anyway, let’s go ahead and get started installing OpenCV 3 on your brand-new Raspberry Pi 3 running Raspbian Jessie.

# VERSION INFO
- __Raspberry Pi3 Model B__：Raspbian JESSIE WITH PIXEL

  Image with PIXEL desktop based on Debian Jessie
 
  Version：January 2017
 
  Release date：2017-01-11
 
  Kernel version：4.4
 
- __OpenCV__：3.1.0
- __Python__：3.4



# Raspbian Installation Step
- __Windows(Recommend)__:

   - __Step1__： Download the “SD Card formatter” from the URL below 
  
       https://www.sdcard.org/cht/downloads/formatter_4/
       
   - __Step2__： Download the Win32DiskImager utility from the Sourceforge Project page as a zip file; you can run this from a USB drive.
  
       https://sourceforge.net/projects/win32diskimager/

   - __Step3__：Extract the executable from the zip file and run the Win32DiskImager utility; you may need to run this as administrator. Right-click on the file, and select Run as administrator.
  
   - __Step4__： Select the image file you extracted earlier.
 
   - __Step5__：Select the drive letter of the SD card in the device box. Be careful to select the correct drive; if you get the wrong one you can destroy the data on your computer's hard disk! If you are using an SD card slot in your computer and can't see the drive in the Win32DiskImager window, try using an external SD adapter.
  
   - __Step6__：Click Write and wait for the write to complete.
  
   - __Step7__：Exit the imager and eject the SD card.


- __Mac(Not Recommend)__:

  Please refer the information from the URL below：
  
  https://www.raspberrypi.org/documentation/installation/installing-images/mac.md

- __Linuux__:

  Please refer the information from the URL below：
  
  https://www.raspberrypi.org/documentation/installation/installing-images/linux.md

# OpenCV Installation Step
- __Step 1__ : Expand filesystem

	The prepared images for the Raspberry Pi are created for SD cards of the size of 2GB. The SD card can be resized or restructured to use the full size of a SD card that is greater than 2GB.
	```
 	$ sudo raspi-config
	$ sudo reboot
	$ df –h
 	```
	And you’ll see Raspbian filesystem has been expanded to include all 8GB of the micro-SD card.

	OpenCV, along with all its dependencies, will need a few gigabytes during the compile, so you should delete the Wolfram engine to free up some space on your Raspberry Pi 3:
	```
	$sudo apt-get purge wolfram-engine
	```
- __Step 2__ : Install Dependencies

	The first step is to update and upgrade any existing packages:
	```
	$ sudo apt-get update
	$ sudo apt-get upgrade
	```
	We then need to install some developer tools, including CMake, which helps us configure the OpenCV build process:
	```
	$sudo apt-get install build-essential cmake pkg-config
	```
	Next, we need to install some image I/O packages that allow us to load various image file formats from disk. Examples of such file formats include JPEG, PNG, TIFF, etc.:
	```
	$ sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
	```
	Just as we need image I/O packages, we also need video I/O packages. These libraries allow us to read various video file formats from disk as well as work directly with video streams:
	```
	$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
	$ sudo apt-get install libxvidcore-dev libx264-dev
	```
	The OpenCV library comes with a sub-module named highgui which is used to display images to our screen and build basic GUIs. In order to compile the highgui module, we need to install the GTK development library:
	```
	$ sudo apt-get install libgtk2.0-dev
	```
	Many operations inside of OpenCV (namely matrix operations) can be optimized further by installing a few extra dependencies:
	```
	$ sudo apt-get install libatlas-base-dev gfortran
	```
	These optimization libraries are especially important for resource constrained devices such as the Raspberry Pi. Lastly, let’s install both the Python 2.7 and Python 3 header files so we can compile OpenCV with Python bindings:
	```
	$ sudo apt-get install python2.7-dev python3-dev
	```
- __Step 3__ : Download the OpenCV source code

	Now that we have our dependencies installed, let’s grab the 3.1.0 archive of OpenCV from the official OpenCV repository. (Note: As future versions of OpenCV are released, you can replace 3.1.0 with the latest version number):
	```
	$ cd ~
	$ wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.1.0.zip
	$ unzip opencv.zip
	```
	We’ll want the full install of OpenCV 3 (to have access to features such as SIFT and SURF, for instance), so we also need to grab the opencv_contrib repository as well:
	```
	$ wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.1.0.zip
	$ unzip opencv_contrib.zip
	```
	*【NOTE】Make sure your OpenCV  and OpenCV_contrib  versions are the same (in this case, 3.1.0 ). If the versions numbers do not match up, then you’ll likely run into either compile-time or runtime.*

- __Step 4__ : Python3 Installation

	Before we can start compiling OpenCV on our Raspberry Pi 3, we first need to install pip, a Python package manager:
	```
	$ wget https://bootstrap.pypa.io/get-pip.py
	$ sudo python get-pip.py
	```
	Then, you should need the virtual environments (virtualenv and virtualenvwrapper):
	```
	$ sudo pip install virtualenv virtualenvwrapper
	$ sudo rm -rf ~/.cache/pip
	```
	Now that both virtualenv and virtualenvwrapper have been installed, we need to update our ~/.profile file to include the following lines at the bottom of the file:
	```
	$ echo -e "\n# virtualenv and virtualenvwrapper" >> ~/.profile
	$ echo "export WORKON_HOME=$HOME/.virtualenvs" >> ~/.profile
	$ echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.profile
	$ source ~/.profile
	```
	Next, let’s create the Python virtual environment that we’ll use for computer vision development:
	```
	$ mkvirtualenv cv -p python3
	```
	This command will create a new Python virtual environment named cv using Python 3.4. After that, you can use workon and you’ll be dropped down into your virtual environment:
	```
	$ source ~/.profile
	$ workon cv
	```
	Assuming you’ve made it this far, you should now be in the cv virtual environment (which you should stay in for the rest of this tutorial). Our only Python dependency is NumPy, a Python package used for numerical processing:
	```
	$ pip install numpy
	```

- __Step 5__ : Expand filesystem
- __Step 6__ : Expand filesystem
- __Step 7__ : Expand filesystem
