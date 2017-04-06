## Faster Boot Time
```
$> sudo vi /etc/default/grub
```
Set the following:
```
GRUB_CMDLINE_LINUX_DEFAULT="usbhid.quirks=0x0b05:0x17fd:0x0004"
```

Then
```
$> sudo update-grub
```

## Faster Shutdown
```
$> sudo systemctl edit cups-browsed.service
```
Add the following:
```
[Service]
TimeoutStopSec=1
```
Then restart computer.

## NVIDIA Drivers
```
$> sudo add-apt-repository ppa:graphics-drivers/ppa
$> sudo update
```
Load the Additional Drivers app from the Dash and pick nvidia-378 (or latest) and Apply (this may take a while).  Then reboot.

## CLOSE LID - DO NOTHING - Gnome3
```
gsettings set org.gnome.settings-daemon.plugins.power lid-close-ac-action 'nothing'
gsettings set org.gnome.settings-daemon.plugins.power lid-close-battery-action 'nothing'
```