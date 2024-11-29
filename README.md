# HowToSetup5G
Notes on how to setup 5G using a B200 using srsRAN and open5GS. 

# Equipment List
 - [x] SDR: B200
 - [x] HW: Compute Resource
   - A least 6 threads
   - gen 10 i5 or greater
   - 8Gb or more of RAM.
   - 32GB of RAM, 12 thread HW reference:
     - <img width="1254" alt="image" src="https://github.com/user-attachments/assets/eaeefb45-8a5c-4b05-96a0-216ee51cccd8">
     - ```txt
       Architecture:             x86_64
       CPU op-mode(s):         32-bit, 64-bit
       Address sizes:          39 bits physical, 48 bits virtual
       Byte Order:             Little Endian
       CPU(s):                   12
       On-line CPU(s) list:    0-11
       Vendor ID:                GenuineIntel
       Model name:             Intel(R) Core(TM) i5-10500T CPU @ 2.30GHz
       CPU family:           6
       Model:                165
       Thread(s) per core:   2
       Core(s) per socket:   6
       ```

 - [x] External 10MHz Clock: [LBE-1420 GPSDO locked clock source](https://www.leobodnar.com/shop/index.php?main_page=product_info&cPath=107&products_id=393)
 - [x] UE: Pixel 7 Pro ... _Your Milage May Vary_:
   -  [srsRAN COTS Tutorial](https://docs.srsran.com/projects/project/en/latest/tutorials/source/cotsUE/source/index.html).
   -  [srsRAN tested UEs List](https://docs.srsran.com/projects/project/en/latest/knowledge_base/source/cots_ues/source/index.html#cots-ues)
   -  [srsRAN Tested UE Thread](https://github.com/srsran/srsRAN_Project/discussions/219)
 - [x] SIM Cards: [sysmoISIM-SJA5-9FV](https://shop.sysmocom.de/sysmoISIM-SJA5-9FV-SIM-USIM-ISIM-Card-10-pack-with-ADM-keys-9FV-chip/sysmoISIM-SJA5-9FV-10p-adm)
 - [x] SIM Card Reader: ive had luck using something as silly as a [keyboard with a reader](https://www.newegg.com/p/0GA-0012-00GA0?srsltid=AfmBOoqnXhbK0DdsKeG1P9-U5BoYgsWzew4s3nObur69v0E4h9-1XAA8) and the professional [HID readers](https://www.amazon.com/HID-OMNIKEY-R31210320-01-Smart-Reader/dp/B06XY2XLWF/ref=sr_1_6?dib=eyJ2IjoiMSJ9.MgDG35J3Y8yUQ9pErhzBjTa14ttzDOxe-eac5CDJKkBsVQ-b49WWksMf6FCOj_zuFofY-FRRhDvFpMitIQVnpXqNm2A613GAMs0Nj0lbe8QgX7CBIVldxdAPib9xMyG1jOfdcxXQHI5u84K7QWTr-c23FPtHLxH_HqXLofPHm8HHfSxy8t2bEhNplFTzoa6GWxAo3BSk-jqEUGK9H7RdF-Q464qmOMsYHrd3qT4oz9Q.nrwXiLMPbL9-1qiGcw07y4z1na0dL1-Ug4siJ8J0l7Y&dib_tag=se&keywords=hid+card+reader&qid=1732484152&sr=8-6). Find whatever works shouldn't matter too much. 
 - [x] You will also need something for the nano sim card to interface with the reader like [this from osmocom](https://shop.sysmocom.de/Professional-SIM-card-adapter-plug-in-micro-nano-SIM-to-full-size/sim-adapter-pcb) if you are buying thier sim cards too.
  - A note on [gialer sims](https://www.gialer.com/collections/writable-sim-card?srsltid=AfmBOopnxaRaEf3ZiOZJkSau9cXRLsgZazs4cQkggr8iBu_cgaa3Q4SE). They are much cheaper than osmocom sims, and reading the srsRAN open5GS forums people apparently have made them work. But from my experience paying the extra $$ for the osmocom sim cards and using pysim worked, and i have not been able to get the gialer sims working with open5Gs to date. (They work fine for 4G/LTE srsRAN though!) 

# 1. OS install and setup
double check you have ubuntu 22.04 
$ uname -a 
 - Install ubutnu 22.04 (GUI or No GUI doesn't matter. )
 - Setup internet. << Varies for each setup.
Internet working? `ping 8.8.8.8`   
after the install check if you can update. 
`apt update`
If not you might need to set your clock. ( `date -s "2 June 2023 14:05:00"`)
```bash
apt update
apt upgrade
```

# 2. Setting Up UHD
make sure the device is connected:
```bash
macc@macc-desktop:~/srsRAN_Project/configs$ uhd_find_devices 
[INFO] [UHD] linux; GNU C++ version 11.2.0; Boost_107400; UHD_4.1.0.5-3
--------------------------------------------------
-- UHD Device 0
--------------------------------------------------
Device Address:
    serial: 31C039C
    name: MyB210
    product: B210
    type: b200

```


# 3. Install srsRAN_Project
[follow this for "Vannila" install](https://docs.srsran.com/projects/project/en/latest/user_manuals/source/installation.html)

when done check that it works:
```bash
$ gnb -v 

--== srsRAN gNB (commit 9d5dd742a) ==--

srsRAN 5G gNB version 24.10.0 (9d5dd742a)
```
# 4. Install open5GS
Follow [THIS GUIDE](https://open5gs.org/open5gs/docs/guide/01-quickstart/)
Don't use the Source Install, its hard, and weird, i've had little sucess with it. If you do , remember that the configuration file is actually `example.yaml` or something like that... 
we can ignored anything _zypper_ related because thats a openSUSE thing and not Ubuntu 22.04 thing. 
if open5GS is installed there should be a file here: 
`$ cat /etc/open5gs/amf.yaml `

There is an external interface that is made. This is the N6. its called ogstun! lol 
```bash
$ ip a
26: ogstun: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1400 qdisc fq_codel state UP group default qlen 500
    link/none 
    inet 10.45.0.1/16 brd 10.45.255.255 scope global ogstun
       valid_lft forever preferred_lft forever
    inet6 2001:db8:cafe::1/48 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::d7ce:8025:5642:773c/64 scope link stable-privacy 
       valid_lft forever preferred_lft forever

```
Skip the nightly builds
Skip the openSUSE



## Setting UP the webgui
its MUCH easier to change some settings for if you tend or want to access the gNB / Core remotely that you can also access the GUI without ssh tunnels for the port. 
[reference](https://www.sharetechnote.com/html/OpenRAN/OR_open5gs_webui.html)

SKIP the _install Node.js on openSUSE_ cuz thats not our OS. 

to validate that the webgui is working go to :
http://localhost:9999
login in admin/4123

#5. Configurations gNB / Core configurations
## /srsRAN_Project/configs



## /etc/open5GS/
if you get permission folders 
`$ sudo chown -R $USER:$USER /etc/open5gs/`
`$ chmod 777 /etc/open5GS/`
`code /etc/open5GS/`
### `/etc/open5GS/amf.yml`
in this file you want to change the MCC and MNC to match whatever you want. 001 - 01 works pretty well for the most part so we'll use that. 
there are 3 MCC/MNC in this file.
```bash
$ amf.yaml
     guami:
       - plmn_id:
-          mcc: 999 # << set this to 001
-          mnc: 70 # <<< set this to 01
         amf_id:
           region: 2
           set: 1
     tai:
       - plmn_id:
-          mcc: 999 # << set this to 001
-          mnc: 70 # <<< set this to 01
         tac: 1
     plmn_support:
       - plmn_id:
-          mcc: 999 # << set this to 001
-          mnc: 70 # <<< set this to 01
         s_nssai:
           - sst: 1
     security:
```
### `/etc/open5GS/nrf`
```bash
nrf:
  serving:  # 5G roaming requires PLMN in NRF
    - plmn_id:
        mcc: 999 # << set to 001
        mnc: 70 # << set to 01
```
when you are done w/ configurations restart the services:
```bash
sudo systemctl stop open5gs-mmed
sudo systemctl stop open5gs-sgwcd
sudo systemctl stop open5gs-smfd
sudo systemctl stop open5gs-amfd
sudo systemctl stop open5gs-sgwud
sudo systemctl stop open5gs-upfd
sudo systemctl stop open5gs-hssd
sudo systemctl stop open5gs-pcrfd
sudo systemctl stop open5gs-nrfd
sudo systemctl stop open5gs-scpd
sudo systemctl stop open5gs-seppd
sudo systemctl stop open5gs-ausfd
sudo systemctl stop open5gs-udmd
sudo systemctl stop open5gs-pcfd
sudo systemctl stop open5gs-nssfd
sudo systemctl stop open5gs-bsfd
sudo systemctl stop open5gs-udrd
sudo systemctl stop open5gs-webui

sudo systemctl restart open5gs-mmed
sudo systemctl restart open5gs-sgwcd
sudo systemctl restart open5gs-smfd
sudo systemctl restart open5gs-amfd
sudo systemctl restart open5gs-sgwud
sudo systemctl restart open5gs-upfd
sudo systemctl restart open5gs-hssd
sudo systemctl restart open5gs-pcrfd
sudo systemctl restart open5gs-nrfd
sudo systemctl restart open5gs-scpd
sudo systemctl restart open5gs-seppd
sudo systemctl restart open5gs-ausfd
sudo systemctl restart open5gs-udmd
sudo systemctl restart open5gs-pcfd
sudo systemctl restart open5gs-nssfd
sudo systemctl restart open5gs-bsfd
sudo systemctl restart open5gs-udrd
sudo systemctl restart open5gs-webui
```
i like to copy the above into `/usr/local/sbin/restartOpen5GS` to its easier than copy-pasta

# verify that srsRAN and Open5GS are configured and can talk properly:
```bash
sudo gnb -c ./00101gnb_rf_b200_tdd_n78_20mhz\ copy.yml 

--== srsRAN gNB (commit 9d5dd742a) ==--


The PRACH detector will not meet the performance requirements with the configuration {Format B4, ZCZ 0, SCS 30kHz, Rx ports 1}.
Lower PHY in dual executor mode.
Available radio types: uhd.
[INFO] [UHD] linux; GNU C++ version 11.2.0; Boost_107400; UHD_4.1.0.5-3
[INFO] [LOGGING] Fastpath logging disabled at runtime.
Making USRP object with args 'type=b200,num_recv_frames=64,num_send_frames=64'
[INFO] [B200] Detected Device: B210
[INFO] [B200] Operating over USB 3.
[INFO] [B200] Initialize CODEC control...
[INFO] [B200] Initialize Radio control...
[INFO] [B200] Performing register loopback test... 
[INFO] [B200] Register loopback test passed
[INFO] [B200] Performing register loopback test... 
[INFO] [B200] Register loopback test passed
[INFO] [B200] Setting master clock rate selection to 'automatic'.
[INFO] [B200] Asking for clock rate 16.000000 MHz... 
[INFO] [B200] Actually got clock rate 16.000000 MHz.
[INFO] [MULTI_USRP] Setting master clock rate selection to 'manual'.
[INFO] [B200] Asking for clock rate 23.040000 MHz... 
[INFO] [B200] Actually got clock rate 23.040000 MHz.
Cell pci=1, bw=20 MHz, 1T1R, dl_arfcn=632628 (n78), dl_freq=3489.42 MHz, dl_ssb_arfcn=632256, ul_freq=3489.42 MHz

N2: Connection to AMF on 127.0.0.5:38412 completed
==== gNB started ===
Type <h> to view help

```

#6. Sim Configuration
## pysim

## open5GS WebGUI

# Comments on scripts and permissions:
`sudo chmod o-rwx foldername to add permissions.`

to see ip address assigned is in the SMF. (Session management function):
``` macc@macc-desktop:~/srsRAN_Project/configs$ tail -f /var/log/open5gs/smf.log 
11/26 14:19:58.951: [sbi] INFO: [PCF] (SCP-discover) NF registered [621a452a-ac2b-41ef-ba1a-71f88b864831] (../lib/sbi/path.c:212)
11/26 14:19:58.951: [sbi] INFO: [621a452a-ac2b-41ef-ba1a-71f88b864831] NF Instance setup [type:PCF validity:0s] (../lib/sbi/path.c:227)
11/26 14:19:58.951: [smf] INFO: NF EndPoint(addr) setup [127.0.0.13:7777] (../src/smf/npcf-handler.c:367)
11/26 14:19:58.951: [smf] INFO: UE SUPI[imsi-001010000138080] DNN[internet] IPv4[10.45.0.2] IPv6[] (../src/smf/npcf-handler.c:578)
```
IN this case its 10.45.0.2





# Working
![Uploading image.png…]()

# RANDOM References:
[5G Tool Calcs](https://5g-tools.com/5g-nr-throughput-calculator/)
[GSMA_Arena](https://www.gsmarena.com/)
[NR ARFCNS to Freq](https://www.sqimway.com/nr_band.php)
