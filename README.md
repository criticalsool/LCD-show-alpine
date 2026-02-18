# LCD-show-alpine
3.5" TFT LCD Alpine driver for the Raspberry PI and PI 2 and PI 3 

> Tested on Raspberry Pi 4 model B with 3.5" display

## Installation

After Alpine installation, run as root (git needed)
```ash
git clone https://github.com/criticalsool/LCD-show-alpine.git
cd LCD-show-alpine/
./LCD35-show
```

> `apk add git` to add git

## Adding screen support directly in the Alpine ISO
### Automatically
Just add [**lcd-show.apkovl.tar.gz**]() overlay file *as-is* at the root of Alpine Linux boot media (or onto any custom side-media) and boot-up the Raspberry Pi.

### Manually
- Copy `tft35a.dtbo` to `boot/overlays/`
- Add this to `boot/usercfg.txt`
```
# RPI Screen config

# Enable audio (loads snd_bcm2835)
dtparam=audio=on

[pi4]
# Enable DRM VC4 V3D driver on top of the dispmanx display stack
dtoverlay=vc4-fkms-v3d
max_framebuffers=2

[all]
overscan_scale=1

hdmi_force_hotplug=1
dtparam=i2c_arm=on
dtparam=spi=on
enable_uart=1
dtoverlay=tft35a:rotate=90
```
- Add this to `boot/cmdline.txt`
```
 dwc_otg.lpm_enable=0 console=tty1 console=ttyAMA0,115200 elevator=deadline rootwait fbcon=map:10 fbcon=font:ProFont6x11 logo.nologo
```

## Restore/Delete
```ash
rm /boot/overlays/tft35a.dtbo
rm /boot/usercfg.txt
mv /boot/cmdline.txt.bak /boot/cmdline.txt
```

## TODO
- Adding apkovl tar gz for auto screen support
- Adding touchscreen support in Xorg
- Adding other screen size/type

## Thanks
- [macmpi headless bootstrap](https://github.com/macmpi/alpine-linux-headless-bootstrap) for the bootstrap
- [goodtft LCD-show](https://github.com/goodtft/LCD-show) for the LCD driver and config.txt indications
