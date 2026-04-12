## Explenation

Openwrt firmware for Fritz!Box 7412.<br>
For flashing back the original AVM firmware an initramfs-image is additionaly included.

### Credits to:

<pre>

 .-----.
|       |.-----.-----.-----.|  |  |  |.----.|  |_
|   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
|_______||   __|_____|__|__||________||__|  |____|
        |__| W I R E L E S S   F R E E D O M
-----------------------------------------------------
</pre>
OpenWrt binarys:<br>
Release: *24.10.4* (2025-10-22)<br>
Source: *[Official OpenWrt releases](https://downloads.openwrt.org/releases/24.10.4/targets/lantiq/xrx200/openwrt-24.10.4-lantiq-xrx200-avm_fritz7412-initramfs-kernel.bin)*
<br>


<pre>
  A)aa   V)    vv  M)mm mmm 
 A)  aa  V)    vv M)  mm  mm 
A)    aa V)    vv M)  mm  mm 
A)aaaaaa  V)  vv  M)  mm  mm 
A)    aa   V)vv   M)      mm 
A)    aa    V)    M)      mm 
</pre>          

AVM binary:<br>
Release: *FRITZ!OS 6.85*<br>
Source: *[InfinityMod](https://github.com/InfinityMod)*


## Usage

### Requirements

#### Hardware:
- AVM FRITZ!Box 7412
- PC / Laptop with a wired Ethernet port

#### Software

| Tool | Windows | Linux |
|---|---|---|
| Shell | PowerShell 5.1+ (built-in) | Bash (built-in) |
| Python 3 | [Download at python.org](https://www.python.org/downloads/) | `apt install python3` |
| SSH client | Built-in since Windows 10 (OpenSSH) | Built-in |

### Install OpenWrt

#### Flash OpenWrt into Router Ram

1) Directly connect the router to lan.<br>
Set ethernet adapter to static:<br>
&nbsp; &nbsp; &nbsp; IP-Address: 192.168.178.10<br>
&nbsp; &nbsp; &nbsp; Subnetmask: 255.255.255.0<br>

2) Open shell and navigate to the project folder of this repository.

3) Unplug and plug in power connector of the router to reset the device.

4) Flash OpenWrt to the routers ram after the connection is initialy established:
    ~~~
    bash ./flash_initramfs_openwrt.sh
    ~~~
    
5. The flashing process is successful if the router restarts automatically and no errors are shown. Otherwise, repeat the process starting from step 3.

   > **! Warning !**<br>
   > Do not power off the device while flashing! 

#### Persistant install OpenWrt

1) After router has restarted change your ethernet adapter to:<br>
IP-Address: 192.168.1.70<br>
Subnetmask: 255.255.255.0<br>

2) Connect in shell to OpenWrt router system:
    ~~~
    ssh 192.168.1.1 -l root
    ~~~

3) Run via the router shell:
    ~~~
    fritz_tffs_nand -d /dev/mtd1 -n linux_fs_start
    ~~~

4) If command responses "0" proceed with the next step. Otherwise power off the router and perform a router update via the webinterface. Restart at 1) to load OpenWrt again into ram.

5) Open the web interface: http://192.168.1.1/cgi-bin/luci/admin/system/flash

   Then upload the file `/openwrt/openwrt-24.10.4-lantiq-xrx200-avm_fritz7412-squashfs-sysupgrade.bin`
   using the **"Flash new firmware image"** option at the bottom of the page.

   > **! Warning !**<br>
   > Do not power off the device while flashing! 


### Flash original AVM Firmware back

1) Directly connect to the router via lan.
Set ethernet adapter to static:<br>
IP-Address: 192.168.178.10<br>
Subnetmask: 255.255.255.0<br>

2) Open shell and navigate to the project folder of this repository.

3) Unplug and plug in power connector of the router to reset the device.

4) Flash the AVM firmware to the routers ram after the connection is established:
    ~~~
    bash ./flash_initramfs_avm.sh
    ~~~

5. The flashing process is successful if the router restarts automatically and no errors are shown. Otherwise, repeat the process starting from step 3.

   > **! Warning !**<br>
   > Do not power off the device!<br>
   > Wait until the connection is re-established and the AVM web interface becomes accessible again.
