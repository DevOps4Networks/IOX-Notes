#How to Install a New GOS

The version of the GOS for your device will be updated during its 
operational lifetime. Indeed, this is likely to happen a few times as you work through developing and 
integrating your applications with IOx.
 
That means you will need to be able to install a GOS images onto the IR829 or IR809 as they are made 
available, for example from [here] 
(https://software.cisco.com/portal/pub/download/portal/select.html?&mdfid=286287074&flowid=75322&softwareid=286306224). 

The install process itself is perfectly straightforward, and I illustrate that directly 
below. The trick is how to get the image onto the device in the first place. At the time of writing, it seems 
that the USB port is not usable for copying images to IOS, though see 
[How to use a USB Drive to Upgrade IOx on an IR829/809](https://communities.cisco.com/thread/63704) in case 
that changes.
 
The typical mechanism to get IOS images and bundles onto an IR829/809 (or, indeed, any other Cisco device) is 
to use the Trivial File Transfer Protocol (TFTP). It also possible to use FTP, HTTP(S), SCP, RCP and others, 
but TFTP is the most common choice. See 
[How to Set up a TFTP Server on OS X, Windows or Linux](https://github.com/DevOps4Networks/IOX-Notes/blob/master/How_To_Setup_TFTP/README.md)
 
See 
[How to Install IOx Bundles on the IR829 or IR809](https://github.com/DevOps4Networks/IOX-Notes/tree/master/How_To_Install_IOx_Bundles)
for details of how to use the `copy tftp flash` command to copy the GOS image from a TFTP server into flash memory.

```bash
IR829-DevTest3>en
Password: 
IR829-DevTest3#guest-os 1 stop
 Stopping Guest OS ...... Done!
 
IR829-DevTest3#
*Apr  5 10:31:44.783: %IR800_INSTALL-6-SUCCESS_GOS_OPERATION: Successfully performed STOP operation for GOS.
IR829-DevTest3#guest-os 1 image uninstall
 Uninstalling Guest OS image ..............
*Apr  5 10:32:06.449: %IOX-6-HEARTBEAT_TIMER_EXPIRED: IOX Heartbeat timer expired..... Done!
 
IR829-DevTest3#
*Apr  5 10:32:12.079: %IR800_INSTALL-6-SUCCESS_GOS_OPERATION: Successfully performed UNINSTALL operation for GOS.
IR829-DevTest3#dir
Directory of flash:/
 
    ...
    7  -rw-    87576215  Mar 17 2016 12:16:54 +00:00  ir800-ioxvm-1.0.0.4-T.bin
    ...
 
994918400 bytes total (179896320 bytes free)
IR829-DevTest3#$image install flash:/ir800-ioxvm-1.0.0.4-T.bin verify        
 Verifying Guest OS image: /ir800-ioxvm-1.0.0.4-T.bin ...
 Installing Guest OS image: /ir800-ioxvm-1.0.0.4-T.bin ...........................................
..... Done!
 
IR829-DevTest3#
*Apr  5 10:33:56.549: %IR800_INSTALL-6-SUCCESS_GOS_OPERATION: Successfully performed INSTALL opera
tion for GOS.
IR829-DevTest3#guest-os 1 start
 Starting Guest OS ...... Done!
 
IR829-DevTest3#
*Apr  5 10:34:25.935: %IR800_INSTALL-6-SUCCESS_GOS_OPERATION: Successfully performed START operati
on for GOS.
IR829-DevTest3#show platform guest-os
Guest OS status:
Installation: Cisco-GOS,version-1.0.0.4
State: RUNNING
 
IR829-DevTest3#show iox host list detail
 
IOX Server is running. Process ID: 323
Count of hosts registered: 1
 
Host registered:
===============
    IOX Server Address: FE80::662:73FF:FEBB:8EEC; Port: 22222
 
    Link Local Address of Host: FE80::1FF:FE90:8B05
    IPV4 Address of Host:       10.44.2.2
    IPV6 Address of Host:       fe80::1ff:fe90:8b05
    Client Version:             0.4
    Session ID:                 1
    OS Nodename:                IR829-DevTest3-GOS-1
    Host Hardware Vendor:       Cisco Systems, Inc.
    Host Hardware Version:      1.0
    Host Card Type:             not implemented
    Host OS Version:            1.0.0.4
    OS status:                  UNKNOWN
 
    Interface Hardware Vendor:  None
    Interface Hardware Version: None
    Interface Card Type:        None
 
    Applications Registered:
    =======================
        Count of applications registered by this host: 0
 
IR829-DevTest3#
```

That's it. Good luck, and please open an issue if this does not work for you, or you can help improve these 
instructions.
