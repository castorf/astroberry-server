# Astroberry Server
Astroberry Server is a ready to use system for Raspberry Pi for controlling all your astronomy equipment.
It handles all astronomy equipment supported by INDI server.

Visit [www.astroberry.io](https://www.astroberry.io) for details and system image.

Visit [Facebook profile](https://www.facebook.com/astroberryserver) to meet other users.

Visit [YouTube channel](https://www.youtube.com/channel/UCQfeJZsRZQrm7O1Gl8yAt5A) for featured videos.

Visit [INDI forum](https://indilib.org/forum/astroberry.html) to share your experience.

![alt img2](https://raw.githubusercontent.com/rkaczorek/astroberry-server/master/files/sneakpreview.jpg)


The system features:
- Support for Raspberry Pi 3 and 4, Pi Zero and... probably any other Raspberry Pi version released so far
- Raspberry Pi OS Desktop
- APT repository for Raspberry Pi OS (yes, now any Raspberry Pi OS user can install Astroberry Server with 'apt install')
- Web interface featuring GPS Panel and Astro Panel (celestial almanac for your localization)
- Astroberry Wireless Hotspot allowing to access the system directly i.e. without external wireless network eg. in the field
- Remote desktop accessible over VNC at astroberry.local:5900 or a web browser at http://astroberry.local/desktop
- INDI framework with all available device drivers
- KStars planetarium software and Ekos with all available device drivers plus custom astroberry drivers
- SkyChart / Cartes du Ciel planetarium program (only in precooked image)
- Hallo Northern SKY planetarium program (only in precooked image)
- CCDciel capture software (only in precooked image)
- Astrometry for field solving
- ASTAP, the Astrometric STAcking Program (only in precooked image)
- PHD2 for autoguiding
- Gnome Predict for satellite tracking
- oaCapture for planetary imaging
- FireCapture for planetary imaging
- SER Player for watching captured video streams (only in precooked image)
- Astroberry DIY drivers for focuser and relay board
- Astroberry PiFace drivers for focuser and relay board
- Astroberry Motor HAT for focuser based on Adafruit Motor HAT
- Virtual GPS for users who do not have GPS device
- File sharing server allowing for network access to captured images
- Support for raspi-config (console) and rc_gui (graphical UI) for easy configuration of Raspberry Pi options

# How to start?
Starting from version 2.0.0 you can install Astroberry Server using two modes:

Download the image file from https://www.astroberry.io/distro/

Verify SHA256 checksums of downloaded image in [SHA256SUMS](https://github.com/rkaczorek/astroberry-server/blob/master/SHA256SUMS) to ensure that it is authentic and not corrupted.

Unpack the image file and flash your microSD card (minimum 16GB required) using [etcher.io](https://etcher.io/) or running the below commands in your terminal:
```
unzip astroberry-server_2.0.4.img.zip
sudo dd if=astroberry-server_2.0.4.img of=/dev/sdX bs=8M status=progress
```
Note 1: **Replace sdX with your microSD card identifier**. Make sure it is correct before running the above command!

After flashing your microSD card, boot your Raspberry Pi and enjoy! It is recommended to update your system after first boot. Run 'sudo apt update && sudo apt upgrade' to keep your system up to date.

OR

Download official [Raspberry Pi OS with desktop](https://www.raspberrypi.org/downloads/raspberry-pi-os/) image and flash your microSD card with it.
After the first boot, connect your Raspberry Pi to a screen, setup your system with the first boot wizard and run the following commands in your terminal:
```
wget -O - https://www.astroberry.io/repo/key | sudo apt-key add -
sudo su -c "echo 'deb https://www.astroberry.io/repo/ buster main' > /etc/apt/sources.list.d/astroberry.list"
sudo apt update
sudo apt upgrade
sudo apt install astroberry-server-full 
```
Note: You should not run this procedure over network (i.e. ssh) as the network connection will be reset during installation procedure.

# How to use it?
It's as simple as this:
- Start your Raspberry Pi with the flashed microSD card.
- Connect to an Astroberry Wireless Hotspot (default password is astroberry) 
- Point your browser to http://astroberry.local or http://IP_ADDRESS
- Click Connect button to access Astroberry Server
- Connect to Astroberry desktop (default password is astroberry)

Note 1: If you connect via Hotspot default IP_ADDRESS is 10.42.0.1, if you connect via wire or your home wireless network, IP_ADDRESS will be assigned by your router/access point.

Note 2: Astroberry Server is accessible via insecure at http://astroberry.local or http://IP_ADDRESS or
secure https://astroberry.local or https://IP_ADDRESS. If you use the latter you need to trust provided certificate or install your own. Otherwise your browser will warn you of security risk.

Note 3: If your display cannot handle FullHD resolution (1920x1080) you need to either connect via web browser and set Local in sliding menu Settings / Scaling OR you need to change the screen resolution by running raspi-config in terminal or Raspberry Pi Configuration from Prefferences menu.
      
# How to upgrade?
Starting from version 2.0.0 you can upgrade all system components using regular system upgrade using apt, apt-get, aptitude or Software Updater. If you use terminal just run:
```
sudo apt update
sudo apt upgrade
```
There is no need to reflash your microSD card with the latest image file to upgrade the system anymore!

# How to reconfigure it?
You can use it as any Raspberry Pi OS system, however there are some mission critical packages installed for you. Don't uninstall them if you want to
keep your Astroberry Server in good shape. These are:
- astroberry-server-full
- astroberry-server-sysmod
- astroberry-server-wui
- astroberry-server-artwork

# What is default username and password?
It's (almost) always **astroberry**:
- For SSH access run: ssh astroberry@astroberry.local and use 'astroberry' for password
- For VNC access connect to astroberry.local:5900 and use astroberry for username and password
- For secure browser access use https://astroberry.local/ or https://IP_ADDRESS then click Connect button and use 'astroberry' for password
- For insecure browser access use http://astroberry.local/ or http://IP_ADDRESS then click Connect button and use 'astrober' for password (up to 8 characters)

# Installing your own certificates
Using secure connection requires a security certificate installed on your Astroberry Server. Basic certificate is provided with the system.
However, you need to trust this certificate at the first conenction to use it! Due to security constraints of modern browsers default certificate
configuration might not work for you. In such a case just use unencrypted connection or install commercial certificates.
To do this you need to get your certificate issued by a public certification authority recognized by your browser.
As soon as you get your own certificate you can install it by replacing the content of the file /opt/noVNC/server.pem. To activate your changes run:
```
sudo systemctl restart novnc.service
sudo systemctl restart nginx.service
```

# Where is the source code of the system?
The core of the system is based on [Raspberry Pi OS with desktop](https://www.raspberrypi.org/downloads/raspberry-pi-os/) and is maintained by Raspberry Foundation. The core is is enriched with bunch of tools and configurationscoming from the following subprojects:

- [astroberry-server-wui](https://github.com/rkaczorek/astroberry-server-wui)
- [astroberry-server-artwork](https://github.com/rkaczorek/astroberry-server-artwork)
- [astroberry-server-sysmod](https://github.com/rkaczorek/astroberry-server-sysmod)
- [astroberry-server-wiz](https://github.com/rkaczorek/astroberry-server-wiz)
- [astroberry-server-hotspot](https://github.com/rkaczorek/astroberry-server-hotspot)
- [astroberry-server-full](https://github.com/rkaczorek/astroberry-server-full)

Plus it is enhanced by these projects:
- [astroberry-diy](https://github.com/rkaczorek/astroberry-diy)
- [astroberry-amh](https://github.com/rkaczorek/astroberry-amh)
- [astroberry-piface](https://github.com/rkaczorek/astroberry-piface)
- [virtualgps](https://github.com/rkaczorek/virtualgps)
- [gpspanel](https://github.com/rkaczorek/gpspanel)
- [astropanel](https://github.com/rkaczorek/astropanel)
- [indi-mqtt](https://github.com/rkaczorek/indi-mqtt)

# FAQ
**Q: How can I update the system?**

A: You can upgrade all system components using regular system upgrade using apt, apt-get, aptitude or Software Updater.

**Q: The image is too large for my microSD card**

A: If the image appears to be too big shrink it according to [this example](https://softwarebakery.com//shrinking-images-on-linux)

**Q: How to connect to my wireless home network?**

A: Wireless connection is predefined for you. Just edit it and change network name and password.
   - Right-click wireless icon on the taskbar
   - Select Edit connections
   - Double-click Wireless connection
   - Enter your network name in SSID field
   - Go to Wi-Fi Security tab
   - Enter your network password in Password field
   - Reboot

**Q: I cannot login to astroberry HotSpot**

A: Note that default keyboard layout used in the image is [QWERTY](https://en.wikipedia.org/wiki/QWERTY). If you use other keyboard layout the password you type in might be different than you think e.g. for French keyboard it may become astroberrz (instead astroberry). Change your keyboard layout using raspi-config or gui_rc to aligh system configuration and your keyboard.

**Q: How can I change my regional settings or add support for my language?**

A: The easiest way is to run raspi-config (console) and rc_gui (graphical UI). The latter is accessible in Menu / Preferences / Raspberry Pi Configuration

**Q: Screen resolution does not match my display. How can I fix it?**

A: If your display cannot handle FullHD resolution (1920x1080) you need to either connect via web browser and set Local in sliding menu Settings / Scaling OR you need to change the system resolution by running raspi-config in terminal or Raspberry Pi Configuration from Prefferences menu.

**Q: What is the source of location data in GPS Panel and Astro Panel accessible in sliding panel (when connected with web browser)?**

A: The panels use GPS readings for your location. If you don't have GPS the panel uses virtualgps provided with Astroberry Server. You can set your static location by editing /etc/location.conf file or using Preferences/Geographic Location menu. After the change reboot or restart virtual GPS by running: sudo systemctl restart virtualgps.service

**Q: How can I login to default pi user account?**

A: Pi user account is disabled for security reasons. You can reenable it anytime by running: sudo passwd -u pi

# Issues
File any issues on https://github.com/rkaczorek/astroberry-server

