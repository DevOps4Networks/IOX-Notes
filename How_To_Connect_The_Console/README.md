#How To Connect to the Console

Console connectivity, with the mini-USB console cable, or a serial/USB adaptor, is the first step in getting a device booted with a suitable IOS image and
configured for IP connectivity. If all else fails, the console connection will practically always work.

The blue mini-USB console cable that is included with the IR829/809 (and other 800 series devices) connects the mini-USB console port, under the screwed down plate, to a USB port on your machine. The stumbling blocks that I encountered were having the right USB drivers, and identifying the specific device to use for the COM port from within a given console application. 

I explain how to create console connections for Windows, OS X and Linux below.

##How to use the IR829/809 Mini-USB Console Cable with Windows 10

The two main aspects to think about when using Windows as a client for a console connection via the mini-USB cable are determining which COM port to connect to, and which terminal emulation software to use.
 
Windows used to be shipped with [HyperTerminal](http://www.hilgraeve.com/hyperterminal/), so many people are accustomed to using that as a console client. HyperTerminal is licenced software ($65), so that may not be what you want. A free alternative, and arguably simpler to use, is [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), which I describe below.
 
Either tool, and others like them, will connect to a COM port. The trick is knowing which. In Windows 10, before you insert the USB console cable, you can open the "Device Manager" to see the available COM ports (type "Device Manager" into the Cortana field). The Device Manager will display a tree of devices on your PC. Open "Ports (COM & LPT)" and you may see some ports already (what you will see depends on your PC's configuration). Then, plug in the USB console cable and additional ports will appear. In my case COM4 and COM5, specifically "Silicon Labs Dual CP210x USB to UART Bridge: Standard COM Port (COM4)", and the same for COM5.
 
One of those ports, COM4 or COM5 or whichever appears for you, will work for serial connection purposes with PuTTY, or other terminal emulators.
 
You can run PuTTY by typing "Putty" in Cortana, and then selecting the "PuTTY Desktop app" icon that should appear. The PuTTY Configuration window will appear, defaulting to "SSH" settings. Select the "Serial" radio button and the fields will change to show a "Serial line" field and a "Speed" field. The Serial line field will have a default value, typically, of "COM1". You will need to change that to one of the COM ports that appeared when you were looking at the Device Manager as described above, e.g. COM4. The Speed value should stay at the default of 9600.
 
After changing the Serial line value, press return and a console window should appear with a prompt. If there is no prompt, try again with a different COM port value, COM5 say.
 
It is possible that the drivers required to make this work are not present in your copy of Windows. In which case you may need to install them. The drivers [here](https://software.cisco.com/download/release.html?mdfid=282867574&softwareid=282855122&release=3.1) may work for you.

##How to use the IR829/809 Mini-USB Console Cable with Linux

These [Cisco Console](https://help.ubuntu.com/community/CiscoConsole) instructions worked for me with `/dev/ttyUSB1`. Though the instructions are from the Ubuntu site, they should work just the same with pretty much any version of Linux. I tested them with the Ubuntu Desktop 14.04 LTS version of Linux.
 
The [Minicom](https://en.wikipedia.org/wiki/Minicom) application used for illustration purposes is quite basic as terminal emulators go, so you might want to look at other Linux terminal emulators. There are plenty to choose from.

##How to use the IR829/809 Mini-USB Console Cable with OSX El Capitan 10.11

The typical command to use with OSX is [screen](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/screen.1.html). For that to work properly on OSX El Capitan 10.11 I needed to install new [CP210x USB to UART Bridge VCP Drivers](https://www.silabs.com/products/mcu/Pages/USBtoUARTBridgeVCPDrivers.aspx#mac). This was probably due to [SIP](https://en.wikipedia.org/wiki/System_Integrity_Protection), which was introduced in 10.11. 

Once I had updated the drivers, I was able to:
 
screen /dev/cu.SLAB_USBtoUART4 9600
 
The screen command is fine, built in and so free. It does have the flaw that, when one exits a console session with Ctrl-a-d, the session is still attached, and so access to that console port can be blocked. If that does happen, you can do this:

 ```bash       
 lsof | grep UART
 screen    2786          <username>    5u      CHR              17,13      0t189             725 /dev/cu.SLAB_USBtoUART6
 kill -9 2786
 lsof | grep UART
 $ 
 ```   
  
A very good alternative is the [Serial](https://www.decisivetactics.com/products/serial/) application, and it was the Serial support team who helped me figure the driver issue out. Yes, it does cost $30, but it just works (TM) as it bundles the required drivers, which are run in user space.
 
When using Serial you will need to open (Command-o) a terminal window, whereupon you will be presented with an "Open Port" window, which will allow you to select one of, in my case, two USB to UART bridges. In my case, it was #2 that worked. When then terminal window opens, you will need to open the settings (Command-;) and set the "Terminal Settings->Configuration" option to be "Cisco".

##How to use a Serial-USB Console Cable with the IR829/809 and OSX El Capitan 10.11

A serial console cable can be used to connect to the IR829 (and other 800 series devices). Whilst I don't have this working yet, I have made some slight progress, and someone else might be able to fill in some blanks.
 
The IR829 has two serial cable ports next to the Ethernet switch ports. These are typically used for remote console access, whereas the mini-USB port would be used for local console access. It is perfectly possible that the serial ports are not enabled or configured by default, so you might first need to use local access via the mini-USB console port to configure the serial ports. I'll revisit this aspect later.
 
With many Cisco devices, but not the IR829, a blue serial console cable is/used to be supplied. It is perfectly possible that you will have some of the old serial cables lying about. In order to use those serial cables in a world where few laptops have serial ports, you would need a USB to Serial Adaptor, which you might also have lying about.
 
What I have discovered, about OS X El Capitan specifically, is that the drivers for the Prolific USB adaptor need to be updated. Updated drivers are available from here: [PL2303 Mac OS X Driver Download](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=229&pcid=41). Note that the installer is unsigned, so you might not want to install those drivers at all. If you do, you will need to try the install once, see the warning that indicates the installer is unsigned, and then go to Settings->Security & Privacy->General, and select the option to install anyway.
 
The main difference that the new drivers make is that, when the serial to USB cable arrangement is plugged into the Mac USB port, appropriate devices appear in /dev (whereas they did not before):
 
ls /dev/*usb*
cu.usbserial tty.usbserial
 
In theory, given these devices, one should be able to:
 
screen /dev/cu.usbserial 9600
 
But that is not working for me yet, at least not on an IR829 straight out of the box.