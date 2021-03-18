# Acer E5 771G - Big Sur OpenCore 0.6.7

CFG Lock disable not possible on this device.

Hold Fn + Tab at Boot then release and spam F2 to toggle Advanced Menu in Bios.

## Requirements / Values to look out for

Framebuffer hack has been removed from the EFI as has the dGPU disable ACPI file as I have found the combination of BIOS values that work. Please keep in mind if you get any of the following steps wrong and you get a black screen after restart you're going to have to reset your BIOS by opening your laptop. When you do just disconnect the battery, disconnect the coin battery that keeps the BIOS values saved and hold the Power button for at least 15 seconds before reconnecting everything and powering your laptop back on.

### Disable dGPU:

Under the Advanced tab -> Video Configuration set Special Features to Disabled; Primary Display should be set to IGFX.

These values depend on each other so don't change them individually. Enabling the dGPU is as easy as setting these values back to default.


### iGPU DVMT:

Again under the Advanced tab > Video Configuration -> Internal Graphic Device set the IGD - Aperture Size to 256 MB; IGD - DVMT Pre-Allocated to 128 MB; IGD - DVMT Size to 128 MB.

It seems like any other combination breaks when having only MacOS installed (ex. I could set Apeture Size to whatever value I wanted under Windowsbut 256 MB is maximum under this configuration) and it seems like DVMT Size is stuck at 128 MB if we want to be able to touch Pre-Allocated value at all(which we need to change to at least 64 MB remove the framebuffer hack)


### Fixing Sleep:

After weeks of endless repetition and ambiguous errors the fix was actually really simple and I just kept overlooking it...
Anyway - in case you're coming from another device and looking for ways to fix your sleep you should first always start with creating a manual USBMap.kext. Map Webcam, bluetooth and anything that's inside of the laptop as 255 aka internal component so it doesn't mess with sleep. This is the first clue I had after I went from instant wakes/reboots to making it enter sleep but instead of resuming from sleep it would just reboot the system. I turned off S3(Sleep) with pmset commands and tried with hibernatemode 25 only and it worked but it broke my wifi, bluetooth and instead of giving me a pulsing orange sleep indication the LED would completely turn off. The system would resume but not before going through the entire boot process once again. Now if you have similar issues and pmset log returning "EFI/Bootrom failure after last point of entry to sleep." the laptops firmware is to blame and you can only fix it by changing how your board interacts with sleep events in BIOS.


Now to fix it for Acer boards/Insyde BIOS firmwares (and anyone else that can find these/similar options):

Under Advanced tab -> Chipset Configuration -> set RTC Lock to Disabled (Afaik makes no difference but can't hurt); After G3 On to Last State; Board Capability to DeepSx; DeepSx Power Policies to Enable in S3-S4-S5.


And just like that sleep is fixed. Minor issues include mouse clicking and touchpad not waking the device so use the keyboard. Also feel free to add "pmset -a lidwake 1" if you're into that functionality but keep in mind sleep isn't instant (up to 10 seconds) and opening your lid back up doesn't auto resume from sleep so you still have to press a key.
I'll look into it eventually but I hope this helps.


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

FileVault

Sleep


## Work in progress:

Touchpad (Works with Basic driver set in BIOS with PS2 kext. When I tried switching to HID it would freeze on boot at "HID: Legacy SHIM 2")

Fn + Brightness keys (Every Fn combination works besides this one. Brightness up is at Pause/Break. Needs patching)

Battery (Doesn't look accurate, requires extensive patching so no guarantees I'll ever get around to it)


## Outright doesn't work:

840m dGPU (Disabled in Bios)

SD Card Reader (Not gonna lose any sleep over this one)



## Not tested in a Windows or Linux dual boot setup so no guarantees there.

Refer to OpenCore's Apple Secure Boot post install guide on how to set ApECID, otherwise Wifi won't load and you might have issues booting.

![BigSur](./bigsur.png)
