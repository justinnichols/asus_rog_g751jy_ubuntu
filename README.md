[Faster Boot Time](#fasterboot)<br/>
[Faster Shutdown](#fastershutdown)<br/>
[NVIDIA Drivers](#nvidia)<br/>
[Full Disk Encryption With NVIDIA Drivers](#fde)<br/>
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

<a name="fde"></a>
## Full Disk Encryption with NVIDIA Drivers
There is an issue with Full Disk Encryption and NVIDIA drivers that prevents Plymouth from allowing keyboard entry for unlocking the disk encryption.  The following stops Plymouth from loading and instead the boot log will show and it will ask for the decryption password.  The keyboard will work in this case.

Edit `/etc/default/grub` to remove `quiet splash` from `GRUB_CMDLINE_LINUX_DEFAULT`.  Then run:
```
$> sudo update-grub
```

## GNOME 3
<a name="windowdecoration"></a>
### CHANGE WINDOW DECORATION BUTTON ORDER/LOCATION
```
# To the left
gsettings set org.gnome.desktop.wm.preferences button-layout "close,maximize,minimize:"
gsettings set org.gnome.settings-daemon.plugins.xsettings overrides "{'Gtk/DecorationLayout':<'close,maximize,minimize:'>}"

# To the right
gsettings set org.gnome.desktop.wm.preferences button-layout ":minimize,maximize,close"
gsettings set org.gnome.settings-daemon.plugins.xsettings overrides "{'Gtk/DecorationLayout':<':minimize,maximize,close'>}"
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
