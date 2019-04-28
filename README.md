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
OpenWrt binarys are build from:<br>
&nbsp; *commit-id:* 9af2735734fce61eaf9bdb49fff218e6548af6f2 (27.04.2019)<br>
&nbsp; *repo:* https://git.openwrt.org/openwrt/openwrt.git
 
<br>


<pre>
  A)aa   V)    vv  M)mm mmm 
 A)  aa  V)    vv M)  mm  mm 
A)    aa V)    vv M)  mm  mm 
A)aaaaaa  V)  vv  M)  mm  mm 
A)    aa   V)vv   M)      mm 
A)    aa    V)    M)      mm 
</pre>          

AVM binarys are from firmare release:<br>
&nbsp; *product:* FRITZ!Box 7412<br>
&nbsp; *version:* FRITZ!OS 6.85


## Usage

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
    
5) Flashing will be proceed if the router restarts from its own and no exception is shown, otherwise begin from 3)


#### Persistant install OpenWrt

(Credits to: [patch](https://patchwork.ozlabs.org/patch/883954/), [sysupgrade](https://openwrt.org/docs/guide-user/installation/sysupgrade.cli))
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

5) In a new shell window run this command to transfer the sysupgrade binary to the router:
    ~~~
    scp /path/to/openwrt-lantiq-xrx200-avm_fritz7412-squashfs-sysupgrade.bin root@192.168.1.1:/root/openwrt-lantiq-xrx200-avm_fritz7412-squashfs-sysupgrade.bin 
    ~~~

6) Install OpenWrt on the NAND flash via your routers shell:
    ~~~
    sysupgrade -v ./openwrt-lantiq-xrx200-avm_fritz7412-squashfs-sysupgrade.bin
    ~~~

#### Install Luci on OpenWrt

1) Modify *distfeeds.conf* at
    ~~~
    vi /etc/opkg/distfeeds.conf
    ~~~
    to:
    ~~~
    src/gz reboot_core http://downloads.openwrt.org/snapshots/targets/lantiq/xrx200/packages
    src/gz reboot_base http://downloads.lede-project.org/snapshots/packages/mipsel_24kc/base
    src/gz reboot_telephony http://downloads.lede-project.org/snapshots/packages/mipsel_24kc/telephony
    src/gz reboot_packages http://downloads.lede-project.org/snapshots/packages/mipsel_24kc/packages
    src/gz reboot_routing http://downloads.lede-project.org/snapshots/packages/mipsel_24kc/routing
    src/gz reboot_luci http://downloads.lede-project.org/snapshots/packages/mipsel_24kc/luci
    ~~~
2) Perform a package update: 
    ~~~
    opkg update
    ~~~

3) Install luci packages:
    ~~~
    opkg install luci
    opkg install luci-ssl
    ~~~

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

5) Flashing will be proceed if the router restarts from its own and no exception is shown, otherwise begin from 3).

5) Do not power off the device! and wait until the connection is established and the AVM webinterface of the router shows up.



