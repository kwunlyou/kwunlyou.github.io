+++
title = "Raspberry Pi Zero Video Monitor"
subtitle = ""

# Add a summary to display on homepage (optional).
summary = ""

date = 2016-12-23T00:00:00+01:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = []

# Is this a featured post? (true/false)
featured = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["Raspberry Pi"]
categories = ["DIY"]

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

### Materials:
- [Raspberry Pi Zero](https://thepihut.com/products/raspberry-pi-zero)
- [Raspberry Pi Zero camera adapter](https://thepihut.com/products/raspberry-pi-zero-camera-adapter)
- [Raspberry Pi NoIR camera module V2](https://thepihut.com/products/raspberry-pi-noir-camera-module)
- [USB to microUSB OTG converter shim](https://thepihut.com/products/usb-to-microusb-otg-converter-shim)
- [USB Wifi adapter for the Raspberry Pi](https://thepihut.com/products/usb-wifi-adapter-for-the-raspberry-pi)
- [SanDisk Ultra 16 GB Memory Card](https://www.amazon.co.uk/gp/product/B010Q57SEE/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1)
- [The official Raspberry Pi Zero case](https://www.raspberrypi.org/products/raspberry-pi-zero-case/)
- USB charger plug with USB to micro USB (originally for LG Nexus 5)
- Mini camera tripod

### Raspberry Pi Zero Headless Setup:
1. **Install the operating system Raspbian:** 
<br> Download the image [RASPBIAN JESSIE LITE](https://www.raspberrypi.org/downloads/raspbian/), and write the image to the SD card following the online [instruction](https://www.raspberrypi.org/documentation/installation/installing-images/README.md). 

2. **Conifgure SSH access via USB:**
<br> Modify `config.txt` and `cmdline.txt` respectively in the `/boot/` dicrectory based on this [guide](https://www.thepolyglotdeveloper.com/2016/06/connect-raspberry-pi-zero-usb-cable-ssh/). <font color="red">Note</font>, SSH is disabled on the lastest Raspbian by default as stated in [this security update](https://www.raspberrypi.org/blog/a-security-update-for-raspbian-pixel/). To enable SSH,  we have to create a file named as `ssh` in the `/boot/` directory.

3. **SSH to Raspberry Pi Zero**
<br> Insert the SD card to Raspberry Pi Zero and connect it to your PC or laptop via the the USB cable. <font color="red">Note</font>, the micro USB side must to be connected with the data port (not power port) of Raspberry Pi Zero. Type `ssh pi@raspberrypi.local` in your terminal to SSH to Raspberry Pi Zero, the default password is raspberry.

4. **Configure the WIFI**
<br> Basically, `/etc/network/interfaces` and `/etc/wpa_supplicant/wpa_supplicant.conf` have to be modified. You can follow this [article](https://davidmaitland.me/2015/12/raspberry-pi-zero-headless-setup/) to finish the configuration. 

### Raspberry Pi Zero Camera Setup:
1. **Enable the connection to the Raspberry Pi camera**
<br> `sudo raspi-conf`, go to `interfacing options`, and enable the camera. Reboot the Raspberry Pi Zero. Install the python package `picamera` and type the command `raspistill -o image.jpg` to make sure everything works well.

2. **Intsall RPi-Cam-Web-Interface**
<br> Follow the official [document](http://elinux.org/RPi-Cam-Web-Interface) to install the package which provides a convenient web interface to manage and configure Pi camera. It also supports motion detection.

Done. Enjoy!

<img alt="raspberry_pi_video_monitor" style="width:50%" src="/img/raspberry_pi_video_monitor.jpg" />

