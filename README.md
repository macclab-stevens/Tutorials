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
`apt update`
`apt upgrade`

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

# 4. Install open5GS
Follow [THIS GUIDE](https://open5gs.org/open5gs/docs/guide/01-quickstart/)
Don't use the Source Install, its hard, and weird, i've had little sucess with it. If you do , remember that the configuration file is actually `example.yaml` or something like that... 

if open5GS is installed there should be a file here: 
`$ cat /etc/open5gs/amf.yaml `

## Setting UP the webgui
its MUCH easier to change some settings for if you tend or want to access the gNB / Core remotely that you can also access the GUI without ssh tunnels for the port. 
[reference](https://www.sharetechnote.com/html/OpenRAN/OR_open5gs_webui.html)

#5. Configurations gNB / Core configurations
## /srsRAN_Project/configs



## /etc/open5GS/
`/etc/open5GS/amf.yml`
`/etc/open5GS/nrf`


#6. Sim Configuration
## pysim

## open5GS WebGUI

# Comments on scripts and permissions:
`sudo chmod o-rwx foldername to add permissions.`







# Working
<img width="1258" alt="image" src="https://github.com/user-attachments/assets/001f5eba-4b8e-4925-be1f-46c8bd821113">

# RANDOM References:
[5G Tool Calcs](https://5g-tools.com/5g-nr-throughput-calculator/)
