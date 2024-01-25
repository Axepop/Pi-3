<h1>Adventures in Rasbian CarPi computer</h1>
How I build a RPi touchscreen to retro connect my phone and old 200SX together.


So I had this idea once I got a mothballed RPi 3B+ that I could fancy up my old Datto 200SX and add modern tech to it. I already had a nice little Kendoow with Aux and USB, But the USB was very limted on file size and I couldn't make nice on the fly song selection changed like I wanted to. So I embarked on a learning adventure. Backgrount to help me get trough this was already there. 27 years in IT, 40+ years on automotive restoration or repairs(pro and hobby) pluss endless curiosity.

<h3>Items I needed for the build<h3>

- Raspberry Pi 3B+ 1GB
- XPT2046 Touchscreen
- USB-C breakout board
- 12V 20mm rocker switch
- AutoCAD2014
- soldering iron, flux and solder
- baby screws


<h2>Getting to business</h2>

Let the joy begine, designing somethign! I cracked open my tool box and make a as built solid model of the Pi in AutoCAD 2014 with my dial calipers. Then came the housing, and after and few version designs and 3d printed by my wife and her lovely printer I how the housing set. Inside it holds the Pi 3B+, a XPT2046 Touchscreen, USB-c breakout board and a 12V rocker switch. I desighned a port for the built in micro USB and audio port for flexability.

<h2>Testing the verious OS flavors out there</h2>
Enter in the brain work and researching the nerd world. I spent a bit of time reviewing Crankshaft NG, OpenAutoPro, and a few other builds out there. ultimatly I settled on 747 Developement's OS build and integration. It provides OpenAuto Android integration, OBD2 scanning(dongle required) and a FM radio interface(not implimented yet...yet). Here is the provess I used to get it all done.

I downloaded Etcher 1.18.1 and burned 20210607_RaspberryPi_with_OA_747.img to the microSD card. I had to modify a few things in code. 

<h2>Preboot edit of config.txt for the XPT2046 5 inch HDMI Capacitive Touch Screen Monitor - 800x480</h2>

1) Access the microSD  and edit config.txt with the following

hdmi_group=2
hdmi_mode=87
hdmi_cvt 800 480 60 6 0 0 0
hdmi_drive=1

2 ) Add

dtparam=i2c_arm=on
dtparam=spi=on
dtoverlay=ads7846,penirq=25,speed=10000,penirq_pull=2,xohms=150

3) Insert microSD into Pi and boot it up.

4) Once the OS is done booting I had to reset the user password at Preferences 

5) Open the terminal and run

$ sudo apt update

$ sudo apt install -y gparted  <-- because I may want to fiddle with atached drives someday

6) Calibrate the touchscreen
xpt2046 LCD driver for the Raspberry PI Installation
Updates:

    v1.2-20170302
        Add xserver-xorg-input-evdev_1%3a2.10.3-1_armhf.deb to support Raspbian-2017-03-02
    v1.1-20160815

<h3>How to Install</h3>

    Install Raspbian OS (If you have installed, you may skip this step)

    Download official Raspbian-Jessie image from Raspberry Pi official website
    Download Etcher or other tools to burn image to your SD card

    Clone this repo into your pi, open LX Terminal and enter following commands:

  $ git clone https://github.com/goodtft/LCD-show.git
  
  $ sudo chmod -R 755 LCD-show
  
  $ cd LCD-show/

  $ sudo ./LCD5-show
  

    According to your LCD's type, execute:

    2.8" LCD - $ sudo ./LCD28-show
    3.2" LCD - $ sudo ./LCD32-show
    3.5" LCD - $ sudo ./LCD35-show
    3.97" LCD - $ sudo ./LCD397-show
    4.3" LCD - $ sudo ./LCD43-show
    5" LCD - $ sudo ./LCD5-show    <-- Use this for the screen.
    7inch(B)-800X480 RPI LCD - $ sudo ./LCD7B-show
    7inch(C)-1024X600 RPI LCD - $ sudo ./LCD7C-show
    Original HDMI display - $ sudo ./LCD-hdmi

    Wait a few minutes, the system will restart automatically.

    If you are using Raspbian image version 2017-03-02 or later, you need to execute these additional 2 commands below after step 4 to allow calibration of resistive touch screen. Then reboot the system.

  $ cd LCD-show
  
  $ sudo dpkg -i -B xserver-xorg-input-evdev_1%3a2.10.3-1_armhf.deb
  
  $ sudo cp -rf /usr/share/X11/xorg.conf.d/10-evdev.conf /usr/share/X11/xorg.conf.d/45-evdev.conf
  
  $ sudo reboot




Now it all works like a charm and looks decent too. Will I try something esleLike a (bigger screen) hell yea. But perhaps I should help the wife who is asking me to build bigger 3D printer with my Pi 1. I hope helps someone but if not I hope you enjoyed the small ride.
