Raspberry Pi Configuration
==========================

Format SD Card

1. Insert 8GB SD Card into USB
2. Run SDFormatter
3. Run Pi Filler with OctoPi Image

Setup WiFi Settings

4. Attach Ethernet Cable
5. ssh to octopi.local
6. sudo nano /etc/network/interfaces
7. Add wpa-ssid "SSID NAME HERE" and wpa-psk "WPA PASSWORD HERE" below "iface wlan0 inet dhcp"
8. Uncomment "iface wlan0 inet dhcp"

Configure SD Card

9. sudo raspi-config
10. Expand Filesystem
11. Change Password
12. sudo reboot
13. Disconnect Ethernet

Setup TFT Screen

14. curl -SLs https://apt.adafruit.com/add-pin | sudo bash
15. sudo apt-get install raspberrypi-bootloader
16. sudo apt-get install adafruit-pitft-helper
17. sudo adafruit-pitft-helper -t 35r
18. sudo reboot

Setup Arduino Flash

19. sudo apt-get install avrdude
20. sudo apt-get install git-core
21. Create update.sh

Setup Touchscreen GUI

22. wget http://steinerdatenbank.de/software/kweb-1.6.8.tar.gz
23. tar -xzf kweb-1.6.8.tar.gz
24. cd kweb-1.6.8
25. ./debinstall
26. sudo apt-get install matchbox xinit x11-xserver-utils unclutter
27. sudo dpkg-reconfigure x11-common
28. wget -P /home/pi/ https://github.com/BillyBlaze/OctoPrint-TouchUI/archive/autostart.zip
29. unzip -d /home/pi/ /home/pi/autostart.zip
30. sudo cp ~/OctoPrint-TouchUI-autostart/touchui.init /etc/init.d/touchui
31. sudo chmod +x /etc/init.d/touchui
32. sudo cp ~/OctoPrint-TouchUI-autostart/touchui.default /etc/default/touchui
33. sudo update-rc.d touchui defaults
34. sudo reboot
35. sudo nano /etc/X11/xorg.conf.d/99-fbdev.conf
36. Paste:

Section "Device"  
  Identifier "myfb"
  Driver "fbdev"
  Option "fbdev" "/dev/fb1"
EndSection

37. Save and Exit
38. sudo chmod +x /etc/X11/xorg.conf.d/99-fbdev.conf
39. sudo reboot


#Additional Commands
1. Backup SD: "sudo -s" then "dd if=/dev/rdiskx of=/path/to/image bs=1m" 

update.sh
   cd Falcon
   git clone https://github.com/chevellebro1/Update.git
   sudo service octoprint stop
   /usr/bin/avrdude -C/etc/avrdude.conf -v -v -v \
   -patmega2560 -cwiring -P/dev/ttyACM0 -b115200 -D \
   -Uflash:w:./Falcon.cpp.hex:i
   sudo service octoprint start



