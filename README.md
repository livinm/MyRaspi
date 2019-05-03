# MyRaspi

## Disable blooetooth
Add dtoverlay=pi3-disable-bt to /boot/config.txt. You need to be on the latest OSMC version for this to work. Then run sudo systemctl disable brcm43xx.service

## Autoconnect bluetooth
~~~~
sudo nano /bin/connect-bluetooth.sh
~~~~

~~~~
#!/bin/bash
sudo bluetoothctl << EOF
power on
agent on
default-agent
pair 0C:A6:94:E2:9B:13
trust 0C:A6:94:E2:9B:13 
connect 0C:A6:94:E2:9B:13
exit
EOF
~~~~

~~~~
sudo chmod +x /bin/connect-bluetooth.sh
~~~~

~~~~
sudo nano /etc/rc.local
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

(sleep 10 ; /bin/connect-bluetooth.sh) &

exit 0
~~~~



Wrong:
sudo nano /etc/systemd/system/connect-bluetooth.service

[Unit]
Description=Connect Bluetooth Maksim

[Service]
Type=oneshot
ExecStart=/bin/connect-bluetooth.sh

[Install]
WantedBy=multi-user.target


sudo systemctl enable /etc/systemd/system/connect-bluetooth.service
