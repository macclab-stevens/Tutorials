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

 - [x] External 10MHz Clock: LBE-1420 GPSDO locked clock source: https://www.leobodnar.com/shop/index.php?main_page=product_info&cPath=107&products_id=393
 - [x] UE:Pixel 7 Pro 
 - [x] SIM Cards: sysmoISIM-SJA5-9FV SIM https://shop.sysmocom.de/sysmoISIM-SJA5-9FV-SIM-USIM-ISIM-Card-10-pack-with-ADM-keys-9FV-chip/sysmoISIM-SJA5-9FV-10p-adm

# 1. OS install and setup
 - Install ubutnu 22.04 (GUI or No GUI doesn't matter. )
 - Setup internet. << Varies for each setup.
Internet working? `ping 8.8.8.8`   
after the install check if you can update. 
`apt update`
If not you might need to set your clock. ( `date -s "2 June 2023 14:05:00"`)
`apt update`
`apt upgrade`









# Working
<img width="1258" alt="image" src="https://github.com/user-attachments/assets/001f5eba-4b8e-4925-be1f-46c8bd821113">
