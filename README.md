# PYNQ_USB_WIFI
In this Github we are going to enable USB Wi-Fi settings for pynq-z1 board.
Software: Vivado 2019.1, Petalinux 2019.1, Serial Terminal. I'm using Ubuntu 18.04.6 on VMware.
Hardware: PYNQ-Z1 board, USB WI-FI Dongle (I'm using DWA-131 from D-link)
This usb wifi Dongle requires rtl8xxx driver for Linux which is easily available.

Steps:
1. First Download Pynq-Z1 git repo by using following command **git clone --recurse-submodule https://github.com/Xilinx/PYNQ.git**
2. Now change this repo for 2.5 version by this command **git checkout origin/image_v2.5**
3. Change directory to sdbuilds **cd <Pynq repo dir>/sdbuild**
4. Run this command **source ./scripts/setup_host.sh**
5. Source vivado, SDK and petalinux.
6. run the make file in sdbuild directory to create petalinux project. **make BOARDDIR=<pynq dir>/boards**
7. When everything is done go to <pynq>/sdbuild/build/Pynq-Z1/petalinux_project
8. Run petalinux-config -c kernel for kernel configuration
9. Networking Support ---> Wireless
    1. --- Wireless                                                      
      **<*>**   cfg80211 - wireless configuration API                            
      **[*]**     nl80211 testmode command                            
      **[ ]**     enable developer warnings                            
      **[ ]**     cfg80211 certification onus                            
      **[*]**     enable powersave by default                            
      **[ ]**     cfg80211 DebugFS entries                              
      **[*]**     support CRDA                                         
      **[*]**     cfg80211 wireless extensions compatibility             
      **<*>**   Generic IEEE 802.11 Networking Stack (mac80211)          
      **[*]**   Minstrel                                                  
      **[*]**     Minstrel 802.11n support                               
      **[ ]**       Minstrel 802.11ac support                               
            Default rate control algorithm (Minstrel)  --->          
      **[*]**   Enable mac80211 mesh networking (pre-802.11s) support    
      **-*-**   Enable LED triggers                                        
      **[ ]**   Export mac80211 internals in DebugFS                      
      **[ ]**   Trace all mac80211 debug messages                        
      **[ ]**   Select mac80211 debugging features  ----                  

11. Device Drivers  ---> Network Device Support ---> Wireless LAN
    Make sure all Realtek devices are selected
    1. **[*]**   Realtek devices                                                           
          **<*>**     Realtek 8180/8185/8187SE PCI support                                     
          **<**M**>**     Realtek 8187 and 8187B USB support                                       
          **<**M**>**     Realtek rtlwifi family of devices  --->                                 
          **<**M**>**     RTL8723AU/RTL8188[CR]U/RTL819[12]CU (mac80211) support                     
          **[*]**       Include support for untested Realtek 8xxx USB devices (EXPERIMENTAL)    

13. Save and Exit. build the kernel by **petalinux-build** command.
14. Create BOOT.BIN and boot the board.
15. Connect USB WI-FI dongle to Pynq-Z1 USB port and run **lsusb** command to see if it detects the device or not.
    In my case, it shows this. **Bus 001 Device 002: ID 2001:3319 D-Link Corp**
16. Add module **insmod /lib/modules/4.19.0-xilinx-v2019.1/kernel/drivers/net/wireless/realtek/rtl8xxxu/rtl8xxxu.ko**
17. Install Network manager
    1. **sudo apt-get update**
    2. **sudo apt-get install network-manager**
18. Run the following commands.
    1. **nmcli dev status**
    2. **nmcli radio wifi**
    3. **nmcli radio wifi on**
    4. **nmcli dev wifi list**
    5. **sudo nmcli dev wifi connect <network-ssid> password "network-password"**
    6. link: https://www.makeuseof.com/connect-to-wifi-with-nmcli/
19. After successfully connecting run ifconfig command to see your IP address.
