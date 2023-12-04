# config
## Identification parameter
``` bash
 ////////// Identification parameters:
    gNB_ID    =  0xe00;
    gNB_name  =  "gNB-OAI";

    // Tracking area code, 0x0000 and 0xfffe are reserved values
    tracking_area_code  =  0x0001;
    plmn_list = ({
                  mcc = 001;
                  mnc = 01;
                  mnc_length = 2;
                  snssaiList = (
                    {
                      sst = 1;
                    }
                  );

                  });

    //nr_cellid = 12345678L;
    nr_cellid = 1;
```
- **mcc (mobile country code)** : An MCC is used in combination with an MNC (a combination known as an "MCC/MNC tuple") to uniquely identify a mobile network operator (carrier) using the 5G public land mobile networks
   - [To check mcc](https://en.wikipedia.org/wiki/Mobile_country_code)
 - **mnc (mobile network code)** : A unique identifier used in conjunction with a mobile country code (MCC) to identify a mobile network operator.
   - [To check mnc](https://en.wikipedia.org/wiki/Mobile_country_code)
 - **snssaiList** : To uniquely identify a  Network Slice, the 5G system defines the S-NSSAI (Single – Network Slice Selection Assistance Information).S-NSSAI is made up of two field SST (Slice/Service Type) and SD (Service Differentiator). SD is an optional field. SST has 8 bit field length implying that it can indicates a total of 255 different slice types.
  - **SST** : this will define the expected behaviour of the Network Slice in terms of specific features and services , such as [V2X](https://www.google.com/search?q=v2x+services+provided+by+5g+technology+is+for&oq=v2x+ser&gs_lcrp=EgZjaHJvbWUqDAgBEAAYQxiABBiKBTIGCAAQRRg5MgwIARAAGEMYgAQYigUyBwgCEAAYgAQyBwgDEAAYgAQyCAgEEAAYFhgeMggIBRAAGBYYHjIICAYQABgWGB4yCAgHEAAYFhgeMg0ICBAuGAoYFhgeGOUEMg0ICRAAGIYDGIAEGIoF0gEINTcxM2owajeoAgCwAgA&sourceid=chrome&ie=UTF-8)
![Screenshot from 2023-12-04 18-54-27](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/09c4a637-184b-4c8a-8b38-d8aa263f7486)

  - **SD (slice differentiator)** : this is optional information that complements the Slice/Service type and is used as an additional differentiator if multiple Network Slices carry the same SST value


















