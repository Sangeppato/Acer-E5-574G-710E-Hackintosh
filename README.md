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
* [MaciASL]
* [Kext Utility]
* [IASL]
* [Clover Configurator] (this is what I use, but you can just choose a plist editor)

### Kext installation
Kext are kernel extensions in macOS and they are required to get components working properly.
To install a kext, put the .kext file in /Library/Extensions, in terminal:
    
	sudo cp -R /path/to/kext/kextname.kext /Library/Extensions/    
And then change its permissions and owner:

    sudo chmod -R 755 /Library/Extensions/kextname.kext
    sudo chown -R root:wheel /Library/extensions/kextname.kext    
After that, rebuilt the kext cache:

    sudo kextcache -i /    
If you prefer, instead of changing permissions and ownership + rebuilding the cache in the terminal, you can just run Kext Utility and enter your password, it will do everything for you (just wait for the "Quit" button to appear).

## Specific components 
### Audio
To get audio working, the best way is probably to use AppleALC (VoodooHDA works as well), or at least this is what I use.
Install [Lilu] and [AppleALC] kexts and open your `config.plist` file in `/EFI/CLOVER` with Clover Configurator.
In the "Devices" section set "Inject Audio" to 'No' and in "Properties" look for `#layout-id` in the properties keys, then remove the hash character ('#') in it.

[Lilu]: https://github.com/acidanthera/Lilu
[AppleALC]: https://github.com/acidanthera/AppleALC
