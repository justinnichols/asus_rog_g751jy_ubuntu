[Faster Boot Time](#fasterboot)<br/>
[Faster Shutdown](#fastershutdown)<br/>
[NVIDIA Drivers](#nvidia)<br/>
[GNOME3 - Window Decoration Button Order/Location](#windowdecoration)<br/>
[GNOME3 - Close Lid Actions](#closelid)<br/>
[GNOME3 - Sort Directories First](#sortdirs)<br/>
[GNOME3 - Set Mouse Acceleration Properties](#setmouseaccel)<br/>
<hr>

<a name="fasterboot"></a>
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

<a name="fastershutdown"></a>
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

<a name="nvidia"></a>
## NVIDIA Drivers
```
$> sudo add-apt-repository ppa:graphics-drivers/ppa
$> sudo update
```
Load the Additional Drivers app from the Dash and pick nvidia-378 (or latest) and Apply (this may take a while).  Then reboot.

## GNOME 3
<a name="windowdecoration"></a>
### CHANGE WINDOW DECORATION BUTTON ORDER/LOCATION
```
gsettings set org.gnome.desktop.wm.preferences button-layout "close,minimize,maximize:"
```
<a name="closelid"></a>
### CLOSE LID
```
gsettings set org.gnome.settings-daemon.plugins.power lid-close-ac-action 'nothing'
gsettings set org.gnome.settings-daemon.plugins.power lid-close-battery-action 'nothing'
```

<a name="sortdirs"></a>
### SORT DIRECTORIES FIRST
```
gsettings set org.gtk.Settings.FileChooser sort-directories-first true
gsettings set org.gnome.nautilus.preferences sort-directories-first true
```

<a name="setmouseaccel"></a>
### SET MOUSE ACCELERATION PROPERTIES
```
#!/bin/bash

SEARCH="Razer Razer Naga Chroma"

if [ "$SEARCH" = "" ]; then
    exit 1
fi

ids=$(xinput --list | awk -v search="$SEARCH" \
    '$0 ~ search {match($0, /id=[0-9]+/);\
                  if (RSTART) \
                    print substr($0, RSTART+3, RLENGTH-3)\
                 }'\
     )

for i in $ids
do
    xinput set-prop $i 'Device Accel Profile' -1
    xinput set-prop $i 'Device Accel Constant Deceleration' 2.5
    xinput set-prop $i 'Device Accel Velocity Scaling' 1.0
done
```
