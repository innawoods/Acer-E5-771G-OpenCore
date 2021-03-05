# Acer E5 771G - Big Sur OpenCore 0.6.7

Hold Fn + Tab at Boot then release and spam F2 to toggle Advanced Menu in Bios.

## What works:

HD 5500 iGPU

Realtek 8168/8111 Ethernet

Intel AC 3160 Wifi (with AirportItlwm limited functionality)

Bluetooth (Integrated in AC 3160)

HDMI

Webcam

Microphone

Realtek ALC 283 Audio

Samsung 860 Evo SATA SSD


## Work in progress:

Touchpad (Works with Basic driver set in Bios)

Sleep (Managed to get it to sleep by setting Webcam to internal in Usbmap but it reboots when you try to wake it)

FileVault (Clicking does nothing atm, maybe it's my install?)

Fn + Brightness keys (Used to work with THLIVSQAZ/ACER-E5-471G-OpenCore setup. Probably going to need some patching)

Battery (Doesn't look accurate, requires extensive patching so no guarantees I'll ever get around to it)


## Outright doesn't work:

840m dGPU (Disabled in Bios)

SD Card Reader (Not gonna lose any sleep over this one)



## Not tested in a Windows or Linux dual boot setup so no guarantees there.

Refer to OpenCore's Apple Secure Boot post install guide on how to set ApECID, otherwise Wifi won't load and you might have issues booting.

![BigSur](./bigsur.png)