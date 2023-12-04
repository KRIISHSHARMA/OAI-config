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

- **nr_cellid** : Uniquely identifies a cell within a PLMN. It is often constructed from gNodeB ID + Physical Cell ID. 36-bit string identifying an NR Cell Id
  - NCI = gNB_ ID * 2^(36-gNB_ID_bit_length) + CI
Example: gNB_ ID_ When the length is 24 bits, the CI length is 12 bits, NCI=gNBID * (2 ^ 12)+CI=gNBID * 4096+CI. Because CI occupies the 12th place, there are a total of 4096 communities [here](https://www.telecomhall.net/t/what-is-the-formula-for-cell-id-nci-in-5g-nr-networks/12623/8)

## 
``` bash
 ////////// Physical parameters:

    pdsch_AntennaPorts_XP = 2; #thamizh change
    pusch_AntennaPorts    = 2; #thamizh change
    do_CSIRS              = 1;
    do_SRS                = 0 ;
    sib1_tda			  = 15;
```

``` bash 
    pdcch_ConfigSIB1 = (
      {
        controlResourceSetZero = 11; #10; #thamizh change
        searchSpaceZero = 0;
      }
    );
``` 
- **PDCCH_ConfigSIB1**  = Value of this IE is a 8 bits length , for example **178** , converting to binary will give `1011 0010`
  - First 4 bits (MSB) will determine the "controlResourceSetZero" Index , this will indicate the Number of RB/symbols used to determine the CORESET seize of the type0 PDCCH Common Seasrch Space. In the above example **11** i.e `1011`
  - Last 4 Bits (LSB) will determine the "searchSpaceZero" Index, this will indicate the PDCCH Monitoring Occasions , meaning the System Frame Number & Slot Index that the UE need to monitor for the Type0-PDCCH common search space . In above example **2** i.e `0010`
  - therefore **PDCCH_ConfigSIB1** of the give command ( not the example ) = `1011 0000` = **176**

- **ControlResourceSetZero** : Number of Consecutive RB's in the Coreset & Number of OFDM symbols assigned for the PDCCH are determined.
  - example : for n41 , SCS of SSB = 30 , min BW (Mhz) = 5/10 following tables from TS 38.213 13-1 to 13-10 are used to determine the Corset RB',Symbols & the RB Offset (Starting point of the Coreset).
    
    ![1559797783424](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/90cbdc46-9b45-42af-80a2-e4ec15637402)
    
  - ControlResourceSetZero Index # 11 is looked up from below 3GPP 38.213, 13-4 Table
    
    ![1559798980178](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/9a8ab6b0-6668-4f99-a953-1e147530c88a)

- The following are determined from the above table
  1. **OFFSET** = 14 RBs from "offsetTocarrier" this will be the starting point to the Coreset#0
  2.  **Number of RBs** = There will be 48 consecutive RBs that will make up the Coreset#0 (Number of RBs) 
  3. **number of symbols** = 1 symbol is occupied by PDCCH
  5.**Multiplexing Pattern** = 1,  is used to determine the Type 0 PDCCH Common Search space Monitoring occasion, This is used in **SearchSpaceZero**

- **SearchSpaceZero** : used to determine the Type 0 PDCCH Search Space Monitoring occasion, meaning it determines the System Frame Number (SFNc) & Slot Index where Type0-PDCCH (Carrying the SI RNTI) can be monitored
Based on the CORSET **Multiplexing pattern** & Frequency Range following 38.213 13-11 to 13-14 tables are used to determine the System Frame Number (SFNc) and Slot Index for monitoring PDCCH.

![1559734615320](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/14d63747-d9b2-4222-91d9-b46e0db91c8b)
![1559735068706](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/31b3ebcd-7052-44e7-9b89-ead902e1d6c5)

- looking up the **SSZ** index = 2 that we decoded from **PDCCH_ConfigSIB1** we get the following info
  1. O: It specifies a slot offset relative to the start of the frame
  2. M: it variable is related to the Number of Search Space Sets per Slot
  3. First Symbol Index: specifics the starting symbol for the Search Space Set within the slot
- have to look more on SearchSpaceZero
- [reference](https://www.linkedin.com/pulse/5g-nr-controlresourcesetzero-corseset0-search-space-zero-chelikani/)









