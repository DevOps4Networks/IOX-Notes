#How to Boot to IOS from rommon-2, and Not Have to do That Again

If, via the console, you see that your device is booting to the "rommon-2" prompt, and so not to IOS directly, you can use these instructions to fix that. This may happen after you have reset the device, for example. For instructions on how to reset, see these [instructions](https://github.com/DevOps4Networks/IOX-Notes/blob/master/How_To_Reset_To_Factory_Settings/README.md).
 
If you do not have a console connection working, see How to use the IR829 Mini-USB Console Cable with OSX El Capitan 10.11, How to use the IR829 Mini-USB Console Cable with Windows 10, How to use the IR829 Mini-USB Console Cable with Linux or How to use a Serial-USB Console Cable with the IR829/809 and OSX El Capitan 10.11, depending on your choice of development environments.
 
After powering on the device, you will see various boot messages eventually resulting in the rommon-2 prompt as below. The "dir" command will show you what is in the flash memory in the device. Importantly, you should expect to see the IOS image file, which is "ir800-universalk9-mz.SPA.155-3.M" in the example below.
 
 ```bash
rommon-2> dir
 
flash:
vlan.dat
ir800-universalk9-mz.SPA.155-3.M
managed
eem
```
 
You can then use the "boot" command to boot from the IOS image, as shown below. There will be many messages shown in the console after you boot, the bulk of which are elided below. After 1-2 minutes, the device should be booted to the stage where you are asked: "Would you like to enter the initial configuration dialog? [yes/no]:". Enter "no", and the device will continue to boot and eventually display a "IR800>" prompt (you may need to press return to see that prompt appear).
 
 ```bash
rommon-2> boot flash:/ir800-universalk9-mz.SPA.155-3.M                       
Booting image: flash:/ir800-universalk9-mz.SPA.155-3.M....  [Multiboot-elf, <0
x110000:0x96f41a4:0x48b644>, shtab=0x9c90500
Signature verification was successful, entry=0x110240]
[CU:0]
Jumps to: 0x110240
 
Smart Init is enabled
smart init is sizing iomem
                TYPE      MEMORY_REQ
    Onboard devices &
        buffer pools      0x02E44000
-----------------------------------------------
              TOTAL:      0x02E44000
 
Rounded IOMEM up to: 47MB.
Using 10 percent iomem. [47MB/448MB]
 
              Restricted Rights Legend
...
%Error opening tftp://255.255.255.255/network-confg (Timed out)
%Error opening tftp://255.255.255.255/cisconet.cfg (Timed out)
 
        --- System Configuration Dialog ---
 
Would you like to enter the initial configuration dialog? [yes/no]: no
Press RETURN to get started!
...
*Feb 23 11:05:50.895: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1, changed state to up
*Feb 23 11:06:16.235: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
*Feb 23 11:06:25.153: %CELLULAR-2-MODEM_UP: Cellular0 modem is now UP.
```

Once the device has booted to this stage you should see the prompt as below. If you do not, press return once or twice.
```bash
IR800>
```

Because you skipped the initial configuration dialog earlier, the device has a default configuration at this stage. You can enter "enable" (en) mode, and then configuration mode (conf t | configure terminal), as shown below, without entering any passwords (this is NOT safe for operations!). Then you can configure the boot image, which is the same as you used when booting from rommon-2, write the configuration to memory (wr mem | write memory) and reload the device. This time it will boot directly to IOS.

```bash 
IR800>en
IR800#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IR800(config)#boot system flash:/ir800-universalk9-mz.SPA.155-3.M
IR800(config)#exit
IR800#wr mem
*Feb 23 15:18:48.056: %SYS-5-CONFIG_I: Configured from console by consolemem
Building configuration...
 
  [OK]
IR800#reload
 
Do you want to reload the internal AP ? [yes/no]: yes
 
Do you want to save the configuration of the AP? [yes/no]: yes
Proceed with reload? [confirm]
``` 

With the last prompt above, to reload, press return to continue.

That's it. Good luck, and please open an issue if this does not work for you, or you can help improve these 
instructions.
