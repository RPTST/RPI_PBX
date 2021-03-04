# RPI_PBX

# Download ISO
    http://www.raspberry-asterisk.org/downloads/

# Download, install and run Raspberry Pi Imager.
    https://www.raspberrypi.org/downloads/

# Write an empty text file named "ssh" (no file extension) to the root of the directory of the card. When it sees the "ssh" on its first boot-up, Raspberry Pi OS will automatically enable SSH (Secure Socket Shell), which will allow you to remotely access the Pi command line from your PC.
# To setup a Wi-Fi connection on your headless Raspberry Pi, create a text file called wpa_supplicant.conf, and place it in the root directory of the microSD card. You will need the following text in the file.

    country=US
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
    scan_ssid=1
    ssid="your_wifi_ssid"
    psk="your_wifi_password"
    }

# Put the sd card into your Raspberry Pi and apply power

# ssh into freepbx box using raspberry and perform the following:
    $ passwd (change your password)
    $ raspi-config (go to Advanced and expand your filesystem)
    $ reboot

# ssh into freepbx box

# Setup Static IP address in /etc/dhcpcd.conf add the following lines
    $ sudo nano -w /etc/dhcpcd.conf
    
    interface wlan0
    static ip_address=192.168.2.2/24
    static routers=192.168.2.1
    static domain_name_servers=192.168.2.1 8.8.8.8

# update the reset of the system
    $ raspbx-upgrade
    $ configure-timezone
    $ install-fail2ban
    $ fwconsole ma --edge upgradeall
    $ fwconsole ma --edge downloadinstall ucp
    $ fwconsole reload
    $ reboot

# Backup to USB flash drive (temperarly)
    $ mount /dev/sda1 /media/usb -o uid=asterisk,gid=asterisk

# or to permenatly

    $ ls -l /dev/disk/by-uuid/

# using the uuid Open /etc/fstab
    $ nano /etc/fstab
    UUID=2014-3D52  /mnt/usb        fat32    uid=asterisk,gid=asterisk   0       0

# Replace the UUID by your own UUID you get in the prerequisites. Replace fat32 by your own file system if needed (ntfs or ext4 for example)

# If you run into any issue try running
    $ fwconsole chwon

# Restore from backup
    $ fwconsole backup --restore /media/usb/,Directory>/<filename>
