#How to Install IOx Bundles on the IR829 or IR809

The version of IOx, i.e. IOS and the GOS bundled together, for your device will be updated during its 
operational lifetime. Indeed, this is likely to happen a few times as you work through developing and 
integrating your applications with IOx.
 
That means you will need to be able to install new "bundles" onto the IR829 or IR809 as they are made 
available. The bundle install process itself is perfectly straightforward, and I illustrate that directly 
below. The trick is how to get the bundle onto the device in the first place. At the time of writing, it seems 
that the USB port is not usable for copying images to IOS, though see 
[How to use a USB Drive to Upgrade IOx on an IR829/809](https://communities.cisco.com/thread/63704) in case 
that changes.
 
The typical mechanism to get IOS images and bundles onto an IR829/809 (or, indeed, any other Cisco device) is 
to use the Trivial File Transfer Protocol (TFTP). It also possible to use FTP, HTTP(S), SCP, RCP and others, 
but TFTP is the most common choice. See 
[How to Set up a TFTP Server on OS X, Windows or Linux](https://github.com/DevOps4Networks/IOX-Notes/blob/master/How_To_Setup_TFTP/README.md)
 
The commands to copy a bundle from from a TFTP server and install it are illustrated below. Note that these 
commands are being applied after configuring the device with a version of the configurations discussed 
in [How to Connect Your Laptop to an IR829 or an IR809](https://github.com/DevOps4Networks/IOX-Notes/blob/master/How_To_Connect_Your_Laptop/README.md). 
If you have changed the host name in the configuration, then your prompt will be different. The address of the 
TFTP server, and how that works, is explained in 
[How to Set up a TFTP Server on OS X, Windows or Linux](https://github.com/DevOps4Networks/IOX-Notes/blob/master/How_To_Setup_TFTP/README.md).

```bash 
IR829-DevTest# copy tftp flash
Address or name of remote host []? 10.42.1.2
Source filename []? ir800-universalk9-bundle.SPA.156-1.T0a.bin
Destination filename [ir800-universalk9-bundle.SPA.156-1.T0a.bin]?
Accessing tftp://192.168.1.200/ir800-universalk9-bundle.SPA.156-1.T0a.bin...
Loading ir800-universalk9-bundle.SPA.156-1.T0a.bin from 192.168.1.200 (via Vlan2): !!...!!
[OK - 169211968 bytes]
 
169211968 bytes copied in 911.096 secs (185724 bytes/sec)
 
IR829-DevTest# bundle install flash:/ir800-universalk9-bundle.SPA.156-1.T0a.bin        
Installing bundle image: /ir800-universalk9-bundle.SPA.156-1.T0a.bin................................................................
..................................
 
updating Hypervisor image...
Sending file modes: C0444 23739221 ir800-hv.srp.SPA.0.33
 
    SRP md5 verification passed!
 
updating IOS image...
Sending file modes: C0664 62103912 ir800-universalk9-mz.SPA.156-1.T0a
 
    IOS md5 verification passed!
Done!
 
*Feb 24 16:12:09.426: %SYS-5-CONFIG_I: Configured from console by bundle install command
*Feb 24 16:12:09.426: %IR800_INSTALL-6-SUCCESS_BUNDLE_INSTALL: Successfully installed bundle image.
IR829-DevTest#wr mem
Building configuration...
 
  [OK]

IR829-DevTest#boot system flash:/ir800-universalk9-mz.SPA.156-1.T0a
IR829-DevTest#reload
 
Do you want to reload the internal AP ? [yes/no]: yes
 
Do you want to save the configuration of the AP? [yes/no]: yes
Proceed with reload? [confirm]
```

That's it. Good luck, and please open an issue if this does not work for you, or you can help improve these 
instructions.