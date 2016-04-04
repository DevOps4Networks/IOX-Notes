#How to reset the IR829 to factory settings

There are two ways to manually reset an IR829 or an IR809 to factory settings, depending on the version of IOS installed. The reset button for the IR829 is under the screwed down plate that covers the mini-USB console port. The reset button for the IR809 is next to the power socket and mini-USB port. You will need a paperclip or similar to press the reset button.

The basic concepts using the reset button are discussed in the [Release Notes for Cisco IOS Release 15.6(1)T for the Cisco IR800 Industrial Integrated Services Routers](http://www.cisco.com/c/en/us/td/docs/routers/access/800/829/15-6-1TIR8xx-Release-Notes.html). 

The alternative to the reset button are the `clear start` and `reload` commands, which are the CLI equivalent of a hard reset. Note that, when using these commands, the configuration should NOT be saved. 

If you use the CLI commands, or effect a reset with the reset button, your device will boot back to the `rommon-2` prompt, and you will need to follow these instructions to boot to IOS again.
 
##15.5(3)M2 or later (including 15.6 releases)
 
If the IOS version is 15.5(3)M2 or later (including 15.6 releases), use the method as described in the documentation explained below, as resetting when IOS is running no longer works.
 
The instructions from this document: http://www.cisco.com/c/dam/en/us/td/docs/routers/access/800/829/829-HIG-7-15.pdf say:
 
"The Reset button resets the router configuration to the default configuration set by the factory. To restore the router configuration to the default configuration set by the factory, use a standard size #1 paper clip with wire gauge 0.033 inch or smaller and simultaneously press the reset button while applying power to the router."
 
What worked for me with the IR829 was holding the reset button down for a count of ~20 seconds, until the PoE light came on, after which the device was reset to factory settings.

With the IR809, the power plug is screwed in, so the only way to be able to press the reset button and apply power would be to unplug the PSU itself, which may not be possible.

 
##15.5(3)M1 or earlier (including 15.4 releases)
 
If the version of IOS is 15.5(3)M1, or earlier (including 15.4 releases), then the device must have booted to IOS before it can be reset. Note that this does not mean that the command prompt has to be available. The device could have partially booted, and then hung (which is likely why you want to reset it in the first place).
 
If the device boots to rommon-2, and is not booting IOS automatically, then you need to manually boot into IOS from the rommon-2 prompt like this (where the image name and version may differ):
 
 ```bash
rommon-2> boot flash:/ir800-universalk9-mz.SPA.155-3.M
 ```
 
Once the IOS boot process starts and you start seeing the initial copyright legend and messages like the one below, you can reset:
 
 ```bash
*Jan  2 00:00:07.963: %IOS_LICENSE_IMAGE_APPLICATION-6-LICENSE_LEVEL: Module name = ir800 Next reboot level = ipbasek9 and License = ipbasek9 ...
 ```
 
To effect a reset, press the reset button for 10 seconds until this message appears:
 
```bash
%FCPLMGR-5-CFG_BUTTON_LONG_PRESSED: Config reset button was held for more than 10 seconds. NVRAM filesystem erased.
```

That will reset nvram, which is where any previous non-factory configuration will have ben saved. Then the device will reboot. If it reboots to rommon-2, you can manually boot from there to IOS as shown above.
 
 