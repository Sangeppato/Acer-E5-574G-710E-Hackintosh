# Acer-E5-574G-710E-Hackintosh
The Laptop works fairly well with macOS Mojave 10.14.1, I still have to get the touchpad working with VoodooI2C (I'm using ApplePS2SmartTouchpad right now) and to replace the Wifi card with a Broadcom BCM94352Z.
### What works
* All the basic components work without issues (CPU, RAM, Intel GPU, Display)
* Display brightness
* Battery indicator
* Keyboard (Even the FN keys)
* Touchapd 
* HDMI, even with the GT920M disabled
* Audio
* Suspension (Only if you close the Lid, if you select "sleep" from the menu it wakes up immediately)

## Getting started
You should follow [RehabMan's great guide](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/) to prepair your USB stick and install the OS. Huge thanks to RehabMan for his great job.
For the config.plist file, you should choose "config_HD515_520_530_540.plist".

### Software you will need
* [Clover Configurator] (this is what I use, but you can just choose a plist editor)
* [MaciASL](https://github.com/RehabMan/OS-X-MaciASL-patchmatic)
* [Kext Utility](https://mac.softpedia.com/get/System-Utilities/Kext-Utility.shtml)
* [IASL](https://bitbucket.org/RehabMan/acpica/downloads/): After unzipping the file, you may want to copy the binary to your path:
    ```
    sudo cp /path/to/iasl /usr/bin/
    ```

[Clover Configurator]: https://mackie100projects.altervista.org/download-clover-configurator/

### Kext installation
Kext are kernel extensions in macOS and they are required to get components working properly.
To install a kext, put the .kext file in /Library/Extensions. In terminal:
    
	sudo cp -R /path/to/kext/kextname.kext /Library/Extensions/    
And then change its permissions and owner:

    sudo chmod -R 755 /Library/Extensions/kextname.kext
    sudo chown -R root:wheel /Library/extensions/kextname.kext    
After that, rebuilt the kext cache:

    sudo kextcache -i /    

If you prefer, instead of changing permissions and ownership + rebuilding the cache in the terminal, you can just run Kext Utility and enter your password, it will do everything for you (just wait for the "Quit" button to appear, it means that the process is complete).

## Specific components 
### Audio
To get audio working, the best way is probably to use AppleALC (VoodooHDA works as well), or at least this is what I use.
Install [Lilu] and [AppleALC] kexts and open your `config.plist` file in `/EFI/CLOVER` with Clover Configurator.
In the "Devices" section set "Inject Audio" to 'No' and in "Properties" look for `#layout-id` in the properties keys, then remove the hash character ('#') from it.

[Lilu]: https://github.com/acidanthera/Lilu
[AppleALC]: https://github.com/acidanthera/AppleALC

### Battery
Download the latest Kext from [Here](https://github.com/RehabMan/OS-X-ACPI-Battery-Driver), it should just work.
More info [here](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/).

### Display brightness control
Download the latest zip file from [Here](https://bitbucket.org/RehabMan/applebacklightfixup/downloads/). Inside it you will find a kext and a file called "SSDT-PNLF.aml": you have to copy it to `EFI/CLOVER/ACPI/patched`, while you should already know what to do with the kext.

NOTE: This with just allow you to change the brightness in Settings->Display, setting the keyboard shortcuts is a different story.
More info [here](https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightfixup-kext.218222/).

### Keyboard and Touchpad
For the Keyboard and Touchpad I personally use the ApplePS2SmartTouchpad kext from [this] repository.
You could stick with VoodooPS2Controller, but the touchpad will only have a basic behaviour.
The best option should be to install VoodooI2C (see [here]), since the trackpad is a precision one, but unfortunately I couldn't get it to work yet.

[this]: https://github.com/gunslinger23/XPS15-9560-High-Sierra
[here]: https://voodooi2c.github.io/#Installation/Installation

### Disabling the Nvidia GT920M
Since macOS doesn't support Nvidia Optimus (actually, the only System which currently supports it properly is Windows), you will probably want to disable the Nvidia GPU in order to get a better battery life.
RehabMan obviousty made a guide about it and you can find it [here](https://www.tonymacx86.com/threads/guide-disabling-discrete-graphics-in-dual-gpu-laptops.163772/).

NOTE: On this Laptop the `_OFF` method is in the same SSDT as the `_INI` one (It should be SSDT8) and it doesn't access the `EC`, so everything should work just applying the `Disable from _INI (SSDT)` patch properly, way easier than the case in the guide.
After patching the SSDT you should save it as .aml and put in `EFI/CLOVER/ACPI/patched`.
