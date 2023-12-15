# config
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

###########################################################################

``` bash
 ////////// Physical parameters:

    pdsch_AntennaPorts_XP = 2; #thamizh change
    pusch_AntennaPorts    = 2; #thamizh change
    do_CSIRS              = 1;
    do_SRS                = 0 ;
    sib1_tda			  = 15;
```

- Query

###########################################################################

``` bash 
    pdcch_ConfigSIB1 = (
      {
        controlResourceSetZero = 11; #10; #thamizh change
        searchSpaceZero = 0;
      }
    );
```
- [reference](https://www.sharetechnote.com/html/5G/5G_CommonSearchSpace_Type0_PDCCH.html)
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


###########################################################################

``` bash

   servingCellConfigCommon = (
    {
```
``` bash 
#spCellConfigCommon

      physCellId                                                    = 0;
```
- PCI range from 0 to 1007
  
![Screenshot from 2023-12-04 22-34-47](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/384706f1-c253-4599-8ab5-347b9a05c095)

- NID(1) = SSS ; NID(2) = PSS 

``` bash 
# n_TimingAdvanceOffset                                         = 0;
```
- query

``` bash 
#  downlinkConfigCommon
    #frequencyInfoDL
      # this is 3450.72 MHz (center frequency)
      absoluteFrequencySSB                                          = 630048;
      dl_frequencyBand                                              = 78;
      # this is 3401.58 MHz
      dl_absoluteFrequencyPointA                                    = 626772;
      #scs-SpecificCarrierList
        dl_offstToCarrier                                           = 0;
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
        dl_subcarrierSpacing                                        = 1;
        dl_carrierBandwidth                                         = 273;
     #initialDownlinkBWP
      #genericParameters
       initialDLBWPlocationAndBandwidth                             = 1099; #38.101-1 Table 5.3.2-1 , TS 38.508-1 V17.5.0 (2022-06) - 4.3.1.0D-1
       
```
![unnamed](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/9552de5a-6f65-4576-bc16-2df16473d4de)
![Screenshot from 2023-12-04 22-50-38](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/375a9f76-cf6c-47f9-9587-e77803181b7f)
![Screenshot from 2023-12-04 22-51-02](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/803fc0ef-eab0-42bc-8413-db02e81e65e1)
![Screenshot from 2023-12-05 10-25-45](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/8175d883-5ded-4c46-b48c-a46f6ff0abec)

- **absoluteFrequencySSB** : It represent the center frequency of SSB Block
- **absoluteFrequencyPointA** : It represents the common reference point A
- **offsetToPointA** = It defines the frequency offset between point A and the lowest subcarrier of the RB overlapping with SSB. The unit for RB is expressed as 15KHz for FR1 and 60 KHz for FR2
- **Kssb** = it defines the frequency of RB#0 of SSB and The unit for RB is expressed as 15KHz for FR1 and 60 KHz for FR2
- **dl_offstToCarrier** : if dl_offstToCarrier is equal to 0, it means that the starting point of the communication at Point A aligns perfectly with the carrier frequency. There is no shift or difference in frequency; they match up directly. It's like saying they are on the same page in terms of where they are operating in the wireless spectrum.
- [Reference](https://www.techplayon.com/5g-nr-ssb-positioning-time-and-frequency-resources/)

- [initialDLBWPlocationAndBandwidth](https://www.linkedin.com/pulse/location-bandwidth-abhishek-ranjan/)  : CORRECTION **Max RBs would be 272 hence end point of the exmaple given above would be 272**

![Screenshot from 2023-12-05 10-09-58](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/983f711d-2f69-44c4-952d-4da22b492294)

- table from 3GPP `TS 38.508-1 V17.5.0 (2022-06) - 4.3.1.0D-1`

``` bash
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
        initialDLBWPsubcarrierSpacing                               = 1;
      #pdcch-ConfigCommon
        initialDLBWPcontrolResourceSetZero                          = 11; #thamizh change
        initialDLBWPsearchSpaceZero                                 = 0;
```
- scroll up 

``` bash

  #uplinkConfigCommon
     #frequencyInfoUL
      ul_frequencyBand                                              = 78;
      #scs-SpecificCarrierList
      ul_offstToCarrier                                             = 0;
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
      ul_subcarrierSpacing                                          = 1;
      ul_carrierBandwidth                                           = 273;
      pMax                                                          = 23;
     #initialUplinkBWP
      #genericParameters
        initialULBWPlocationAndBandwidth                            = 1099;
```

- **ul_frequencyBand** : [n78](https://en.wikipedia.org/wiki/5G_NR_frequency_bands)
- **ul_offstToCarrier** :  If ul_offstToCarrier is equal to 0, it means that there is no frequency offset between the reference point (usually associated with the uplink, denoted by "ul") and the lowest usable subcarrier on the carrier. In other words, Point A aligns with the starting point of the carrier frequency, and there is no need to shift or offset the frequency in the uplink direction.If ul_offstToCarrier is equal to 0, it means that the starting point of the data transmission in the uplink (from your device, like a smartphone) aligns perfectly with the main frequency that the 5G network is using. There is no need for any adjustment or shift in the frequency for the uplink transmission. It's like your device is directly tuned to the right frequency without any offset.

- **ul_carrierBandwidth** = 273 i.e 100MHz
 ![unnamed](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/9552de5a-6f65-4576-bc16-2df16473d4de)

- **pMAX** : The parameter Pmax is usually configured by the network
on a cell level. Pmax was configured to 23 dBm (200 mW)
   - [reference](https://www.techplayon.com/5g-nr-ue-power-classes/#)

## RACH-ConfigCommon

``` bash
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
        initialULBWPsubcarrierSpacing                               = 1;
      #rach-ConfigCommon
        #rach-ConfigGeneric
          prach_ConfigurationIndex                                  = 159; #thamizh change
```
- **prach-ConfigurationIndex** : This value Points to Table for prach-ConfigurationIndex In 38.211. The table defines which PRACH format to use and when to send PRACH in time domain and number of RACH occasions available.

![Screenshot from 2023-12-05 18-47-01](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/921d09c4-17af-4893-9843-b7d9a2278062)
![Screenshot from 2023-12-10 00-53-01](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/37d56c08-d01d-4e28-9a3a-9ee08591e62b)


- [reference](https://www.sharetechnote.com/html/5G/5G_RACH.html) : 3GPP TS 38.211 version 16.2.0 Release 16 Table 6.3.3.2-3
![Screenshot from 2023-12-05 18-51-32](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/4efd7916-a2a0-48b1-8f1b-59e6556aef52)

- Prach-configurationindex = 159 and from the below table we can find out more

![Screenshot from 2023-12-05 18-47-01](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/921d09c4-17af-4893-9843-b7d9a2278062)
![Screenshot from 2023-12-10 00-53-01](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/b3fb138a-7902-4a5f-9fbb-59efd819d1f6)

![1586146953045](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/7b5540cd-fd3a-430b-870a-d589341ed4c3)

- For the purpose of slot numbering in the prach-ConfigurationIndex tables, the following subcarrier spacing is be assumed:
  - 15 kHz for FR1
  - 60 kHz for FR2.
 
![1586148399537](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/c1892136-ce1e-4fe5-ae64-e0753c5ff278)

![calculation](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/8c88ae5f-1506-415b-9574-525c8093ef84)
![illustration](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/c2b0a7b1-8180-4c0a-972e-bc257593241f)





``` bash
#prach_msg1_FDM
#0 = one, 1=two, 2=four, 3=eight
          prach_msg1_FDM                                            = 0; # ENUMERATED {one, two, four, eight}, 
          prach_msg1_FrequencyStart                                 = 22; # INTEGER (0..maxNrofPhysicalResourceBlocks-1), 
          zeroCorrelationZoneConfig                                 = 15; #thamizh change # INTEGER(0..15)
          preambleReceivedTargetPower                               = -100; #-84 #Luis -70; #thamizh change # INTEGER (-202..-60) in db 
#preamblTransMax (0...10) = (3,4,5,6,7,8,10,20,50,100,200)
          preambleTransMax                                          = 7; #thamizh change
#powerRampingStep
# 0=dB0,1=dB2,2=dB4,3=dB6
        powerRampingStep                                            = 2;
#ra_ReponseWindow
#1,2,4,8,10,20,40,80
        ra_ResponseWindow                                           = 5; #thamizh change
#ssb_perRACH_OccasionAndCB_PreamblesPerSSB_PR
#1=oneeighth,2=onefourth,3=half,4=one,5=two,6=four,7=eight,8=sixteen
        ssb_perRACH_OccasionAndCB_PreamblesPerSSB_PR                = 3; #thamizh change
#       oneEighth                                   ENUMERATED {n4,n8,n12,n16,n20,n24,n28,n32,n36,n40,n44,n48,n52,n56,n60,n64}, 
        oneFourth                                   ENUMERATED {n4,n8,n12,n16,n20,n24,n28,n32,n36,n40,n44,n48,n52,n56,n60,n64}, 
        oneHalf                                     ENUMERATED {n4,n8,n12,n16,n20,n24,n28,n32,n36,n40,n44,n48,n52,n56,n60,n64}, 
        one                                         ENUMERATED {n4,n8,n12,n16,n20,n24,n28,n32,n36,n40,n44,n48,n52,n56,n60,n64}, 
        two                                         ENUMERATED {n4,n8,n12,n16,n20,n24,n28,n32}, 
        four                                        INTEGER (1..16), 
        eight                                       INTEGER (1..8), 
        sixteen                                     INTEGER (1..4) 
#oneHalf (0..15) 4,8,12,16,...60,64
        ssb_perRACH_OccasionAndCB_PreamblesPerSSB                   = 15; #thamizh change
#ra_ContentionResolutionTimer
#(0..7) 8,16,24,32,40,48,56,64
        ra_ContentionResolutionTimer                                = 7; #4 #thamizh change
        rsrp_ThresholdSSB                                           = 19; #31; #thamizh change
#prach-RootSequenceIndex_PR
#1 = 839, 2 = 139
        prach_RootSequenceIndex_PR                                  = 2;
        prach_RootSequenceIndex                                     = 1; #thamizh change
        # SCS for msg1, can only be 15 for 30 kHz < 6 GHz, takes precendence over the one derived from prach-ConfigIndex
        #
        msg1_SubcarrierSpacing                                      = 1,

```


- [reference](https://www.nrexplained.com/rrc#RACHConfigGeneric)
- [reference](https://www.linkedin.com/pulse/nr-rach-process-syed-mohiuddin/)
- [reference](https://www.sharetechnote.com/html/5G/5G_RACH.html)
- **prach_msg1_FDM** : The number of PRACH transmission occasions FDMed in one time instance. (see TS 38.211 [16], clause 6.3.3.2).
  - The "FDM" in "msg1-FDM" stands for Frequency Domain Multiplexing. This implies that multiple Physical Random Access Channel (PRACH) transmission occasions are multiplexed in the frequency domain within one time instance. The PRACH is used by the UE to initiate communication with the network. So, "msg1-FDM" indicates that several PRACH transmission occasions are combined or multiplexed in the frequency domain during a specific time instance, likely to improve efficiency and utilize the available spectrum effectively. The exact number of PRACH transmission occasions FDMed in one time instance would be determined by the configuration and requirements of the specific 5G network deployment.
  - If "msg1-FDM = 2(1)," it likely means that, during the Random Access procedure in 5G, two PRACH transmission occasions are multiplexed in the frequency domain within one time instance. In other words, two devices or User Equipments (UEs) are attempting to initiate communication with the network at the same time, and their PRACH transmissions are separated in the frequency domain to avoid interference. This configuration may be part of the network's strategy to efficiently handle multiple devices attempting to connect simultaneously. The exact implications of "msg1-FDM = 2" would depend on the specific design and parameters of the 5G network being used.
    
![1586140220502](https://github.com/KRIISHSHARMA/OAI-config/assets/86760658/2e3cbc4c-1f17-47d9-9b1f-1bddb975eea5)



- **prach_msg1_FrequencyStart** : Offset of lowest PRACH transmission occasion in frequency domain with respective to PRB 0. The value is configured so that the corresponding RACH resource is entirely within the bandwidth of the UL BWP. (see TS 38.211 [16], clause 6.3.3.2).
  - prach_msg1_FrequencyStart refers to the starting point or position of the first transmission occasion for a PRACH (Physical Random Access Channel) message in the frequency domain. The offset is measured with respect to PRB 0 (Physical Resource Block 0), which is a fundamental unit of frequency in 5G.
  - The purpose of configuring this value is to ensure that the resources allocated for the Random Access Channel (RACH) transmission are entirely within the bandwidth of the Uplink Bandwidth Part (UL BWP). This helps in efficient use of the available frequency spectrum in the uplink direction.
  - In other words, prach_msg1_FrequencyStart determines where in the frequency spectrum the initial transmission for a PRACH message begins, and this offset is set to make sure that it falls within the designated bandwidth for uplink communication.

- If prach_msg1_FrequencyStart = 22, it means that the first transmission for a certain type of communication in 5G (PRACH) starts at a specific position in the frequency spectrum, and that position is 22 steps away from the initial reference point (PRB 0). It's like saying, "Start talking at position 22 in the frequency range." This helps organize and manage communication resources efficiently.

- **zeroCorrelationZoneConfig** :  Table 6.3.3.1-5 in TS 38.211
  - **Query**


- **preambleReceivedTargetPower** : The target power level at the network receiver side (see TS 38.213 [13], clause 7.4, TS 38.321 [3], clauses 5.1.2, 5.1.3). Only multiples of 2 dBm may be chosen (e.g. -202, -200, -198, ...)
  - A negative value indicates that the signal is weaker than the reference level.
  - having the value always a multiple of 2 might be a result of the way power levels are quantized or discretized in the system. The use of multiples of 2 could simplify the representation and processing of power values in the underlying technology.
  - In summary, the negative sign likely indicates a weaker signal compared to a reference, and the requirement for multiples of 2 might be a technical choice for the representation and handling of power levels in the specific system or protocol.


- **preambleTransMax** :  Max number of RA preamble transmission performed before declaring a failure . How many times a UE can transmit MSG1 before declaring RACH failure. (see TS 38.321 [3], clauses 5.1.4, 5.1.5).

- **powerRampingStep** : Power ramping steps for PRACH . How many times a UE can transmit MSG1 before declaring RACH failure. (see TS 38.321 [3],5.1.3). 

- **ra_ResponseWindow** : Msg2 (RAR) window length in number of slots. The network configures a value lower than or equal to 10 ms when Msg2 is transmitted in licensed spectrum and a value lower than or equal to 40 ms when Msg2 is transmitted with shared spectrum channel access (see TS 38.321 [3], clause 5.1.4). UE ignores the field if included in SCellConfig. If ra-ResponseWindow-v1610 is signalled, UE shall ignore the ra-ResponseWindow (without suffix).
- Msg2 (RAR) window length in number of slots. UE scans for PDCCH looking for msg2 response from gNB within the ra-ResponseWindow, if UE did not receive msg2 within the window it will consider RACH failure and start msg1 transmission again.
  - In simpler terms, "ra-ResponseWindow" refers to the time duration, measured in the number of slots, during which the network expects a response (Msg2 or RAR) from the user equipment (UE) in a wireless communication system. The specific configuration of this window depends on whether the message (Msg2) is transmitted in licensed spectrum or shared spectrum.
  - if Msg2 is sent in licensed spectrum, the window length is set to a value less than or equal to 10 milliseconds. On the other hand, if Msg2 is sent with shared spectrum channel access, the window length is set to a value less than or equal to 40 milliseconds.
  

- [reference](https://www.nrexplained.com/rrc#RACHConfigGeneric)
- **ssb_perRACH_OccasionAndCB_PreamblesPerSSB_PR** : The meaning of this field is twofold: the CHOICE conveys the information about the number of SSBs per RACH occasion. Value oneEighth corresponds to one SSB associated with 8 RACH occasions, value oneFourth corresponds to one SSB associated with 4 RACH occasions, and so on. The ENUMERATED part indicates the number of Contention Based preambles per SSB. Value n4 corresponds to 4 Contention Based preambles per SSB, value n8 corresponds to 8 Contention Based preambles per SSB, and so on. The total number of CB preambles in a RACH occasion is given by CB-preambles-per-SSB * max(1, SSB-per-rach-occasion). See TS 38.213 [13].
- The meaning of this field is twofold: ssb-perRACH-Occasion conveys the information about the number of SSBs mapped per RACH occasion and CB-PreamblesPerSSB indicates the number of Contention Based preambles available per SSB
  - ssb_perRACH_Occasion = half: This means there is one Synchronization Signal Block (SSB) for every two Random Access Channel (RACH) occasions. So, it associates one SSB with every two instances of the network's random access process.
  - CB_PreamblesPerSSB_PR = n15: This indicates that there are 15 Contention Based (CB) preambles associated with each Synchronization Signal Block (SSB). So, when a device accesses the network using CB procedures related to a particular SSB, there are 15 preambles available for contention.
 

- **ra_ContentionResolutionTimer** : The initial value for the contention resolution timer (see TS 38.321 [3], clause 5.1.5). Value sf8 corresponds to 8 subframes, value sf16 corresponds to 16 subframes, and so on.
  - ra_ContentionResolutionTimer = 7 means s64 which corresponds to 64 subframes 

- **rsrp_ThresholdSSB** : UE may select the SS block and corresponding PRACH resource for path-loss estimation and (re)transmission based on SS blocks that satisfy the threshold (see TS 38.213 [13]).
  - In simpler terms, rsrp_ThresholdSSB is a parameter that helps a User Equipment (UE) decide which Synchronization Signal Block (SSB) to choose for path-loss estimation and potential retransmission. The UE makes this decision by selecting SS blocks that meet a certain signal strength threshold.
  - So, if the signal strength of an SS block (measured in RSRP, Reference Signal Received Power) is equal to or higher than the specified threshold, the UE may decide to use that SS block for activities like estimating how far the signal has traveled (path-loss estimation) or for potential retransmissions. The specific details and calculations related to this threshold are further defined in the technical specification TS 38.213 [13].
  - If rsrp_ThresholdSSB = 19, it means that the threshold for the Reference Signal Received Power (RSRP) associated with Synchronization Signal Blocks (SSB) is set to 19.
  - In simpler terms, when your phone is deciding which SSB to use for tasks like estimating signal strength or retransmitting data, it will choose blocks where the RSRP is 19 or higher. This value essentially sets a standard for what is considered a strong enough signal for certain operations in the wireless communication system.

- **prach_RootSequenceIndex_PR** : PRACH root sequence index (see TS 38.211 [16], clause 6.3.3.1). The value range depends on whether L=839 or L=139 or L=571 or L=1151. The length of the root sequence corresponding with the index indicated in this IE should be consistent with the one indicated in prach-ConfigurationIndex in the RACH-ConfigDedicated (if configured). If prach-RootSequenceIndex-r16 is signalled, UE shall ignore the prach-RootSequenceIndex (without suffix). Explain in detail
  - **Query**
 


 - SCS for msg1, can only be 15 for 30 kHz < 6 GHz, takes precendence over the one derived from prach-ConfigIndex
   - There's a rule that says the frequency spacing between parts of the message (Subcarrier Spacing or SCS) can only be as large as 15 when the frequency is between 30 kHz and 6 GHz. Even if there's another rule that suggests a different spacing based on something called prach-ConfigIndex, the first rule (15 for 30 kHz < 6 GHz) is more important. So, your phone follows the first rule and uses a spacing of 15, ignoring what the other rule might suggest.


## PUSCH-ConfigCommon

``` bash
        msg3_DeltaPreamble                                          = 6; #thamizh change
        p0_NominalWithGrant                                         = -96 #-86; #Luis-70; #thamizh change

# pucch-ConfigCommon setup :
# pucchGroupHopping
# 0 = neither, 1= group hopping, 2=sequence hopping
        pucchGroupHopping                                           = 0;
        hoppingId                                                   = 0; #thamizh change
        p0_nominal                                                  = -96; #Luis-70; #thamizh change

```

- **msg3_DeltaPreamble** : Power offset between msg3 and RACH preamble transmission. Actual value = field value * 2 [dB] (see TS 38.213 [13], clause 7.1)
  - MSG3 transmit power — Most recent PRACH preamble transmit power = 2 * msg3-DeltaPreamble
  - msg3_DeltaPreamble is used to ensure that the power level of a specific message (msg3) is appropriately adjusted relative to the power level of the most recent PRACH preamble. The doubling (2 * msg3_DeltaPreamble) is a way to express this adjustment in decibels. This adjustment is crucial for maintaining effective communication and managing power levels in a 5G network.
  - msg3_DeltaPreamble is like adjusting the volume when sending a special message (msg3) after the initial connection attempt (preamble). It's saying, "Hey, make sure the message after the first one is sent with a power level that is twice the specified value for msg3_DeltaPreamble." This adjustment helps with clear and effective communication in a 5G network.


- [PUSCH POWER](https://www.sharetechnote.com/html/5G/5G_PowerControl.html)
- **p0-NominalWithGrant** : P0 value for PUSCH with grant (except msg3). Value in dBm. Only even values (step size 2) allowed (see TS 38.213 [13], clause 7.1) This field is cell specific
  -  This parameter is a key component in the [formula](https://www.sharetechnote.com/html/5G/5G_PowerControl.html) used for calculating the uplink transmission power on the PUSCH. It serves as a baseline or reference power level from which adjustments are made based on various factors like path loss, UE power capability, and additional power control commands from the network.
  -  Query


- **pucchGroupHopping** : Configuration of group- and sequence hopping for all the PUCCH formats 0, 1, 3 and 4. Value neither implies neither group or sequence hopping is enabled. Value enable enables group hopping and disables sequence hopping. Value disable disables group hopping and enables sequence hopping (see TS 38.211 [16], clause 6.3.2.2).
- [resorce allocation](https://www.sharetechnote.com/html/5G/5G_PUCCH.html)
  - Group Hopping: This is a technique where multiple users (UEs) within a cell share the same set of resources during a certain transmission time. Group hopping helps in spreading the interference caused by multiple users.
  - Sequence Hopping: This is another technique to reduce interference, where a UE changes its transmission pattern from one occasion to the next. It helps avoid consistent interference patterns.
  - Value "neither" (0) : Neither group hopping nor sequence hopping is enabled.
  - Value "enable" (1) : Group hopping is enabled, and sequence hopping is disabled.
  - Value "disable" (2) : Group hopping is disabled, and sequence hopping is enabled.
  - Think of it like a dance floor at a party:
    - "neither": Nobody is doing any special moves. It's a regular dance floor with no special patterns.
    - "enable": Everyone is moving together in a coordinated way. They're not changing their moves individually, but the whole group is doing something special.
    - "disable": People are doing their own dance moves, but each person changes their moves from time to time. It's not coordinated like in the "enable" case, but everyone has their own rhythm.
    - In the context of 5G, these "dance moves" are ways that devices (like your smartphone) send information to the network. The choices about group hopping and sequence hopping help manage how devices share the "dance floor" (communication resources) to avoid interference and communicate more efficiently.


- **hoppingId** : Cell-specific scrambling ID for group hopping and sequence hopping if enabled, see TS 38.211 [16], clause 6.3.2.2.


- **p0_nominal** : Power control parameter P0 for PUCCH transmissions. Value in dBm. Only even values (step size 2) allowed (see TS 38.213 [13], clause 7.2).
  - Query

``` bash
# ssb_PositionsInBurs_BitmapPR
# 1=short, 2=medium, 3=long
      ssb_PositionsInBurst_PR                                       = 2;
      ssb_PositionsInBurst_Bitmap                                   = 0x1; #thamizh change

# ssb_periodicityServingCell
# 0 = ms5, 1=ms10, 2=ms20, 3=ms40, 4=ms80, 5=ms160, 6=spare2, 7=spare1
      ssb_periodicityServingCell                                    = 2;

# dmrs_TypeA_position
# 0 = pos2, 1 = pos3
      dmrs_TypeA_Position                                           = 0;

```

- **ssb_PositionsInBurst_PR** :
  - longbitmap : Bitmap when maximum number of SS/PBCH blocks per half frame equals to 64 as defined in TS 38.213 [13], clause 4.1.
  - shortbitmap : Bitmap when maximum number of SS/PBCH blocks per half frame equals to 8 as defined in TS 38.213 [13], clause 4.1.
  - shortbitmap : Bitmap when maximum number of SS/PBCH blocks per half frame equals to 4 as defined in TS 38.213 [13], clause 4.1.
 

- **ssb_PositionsInBurst_bitmap** : For operation in licensed spectrum, indicates the time domain positions of the transmitted SS-blocks in a half frame with SS/PBCH blocks as defined in TS 38.213 [13], clause 4.1. The first/leftmost bit corresponds to SS/PBCH block index 0, the second bit corresponds to SS/PBCH block index 1, and so on. Value 0 in the bitmap indicates that the corresponding SS/PBCH block is not transmitted while value 1 indicates that the corresponding SS/PBCH block is transmitted. The network configures the same pattern in this field as in the corresponding field in ServingCellConfigCommonSIB. For operation with shared spectrum channel access, only mediumBitmap is used and the UE assumes that one or more SS/PBCH blocks indicated by ssb-PositionsInBurst may be transmitted within the discovery burst transmission window and have candidate SS/PBCH blocks indexes corresponding to SS/PBCH block indexes provided by ssb-PositionsInBurst (see TS 38.213 [13], clause 4.1). If the k-th bit of ssb-PositionsInBurst is set to 1, the UE assumes that one or more SS/PBCH blocks within the discovery burst transmission window with candidate SS/PBCH block indexes corresponding to SS/PBCH block index equal to k – 1 may be transmitted; if the kt-th bit is set to 0, the UE assumes that the corresponding SS/PBCH block(s) are not transmitted. If ssb-PositionQCL is configured, the k-th bit is set to 0, where k > ssb-PositionQCL and the number of actually transmitted SS/PBCH blocks is not larger than the number of 1's in the bitmap. The network configures the same pattern in this field as in the corresponding field in ServingCellConfigCommonSIB.
  1. Licensed Spectrum:
     - There is a pattern (bitmap) indicating when specific SS/PBCH blocks are transmitted within a half frame.
     - Each bit in the pattern corresponds to a different SS/PBCH block. If the bit is 0, that block is not transmitted; if it's 1, the block is transmitted.
     - This pattern is set by the network and is the same across different parts of the network.
    2. Shared Spectrum Channel Access:
       - Only a part of the pattern (mediumBitmap) is used, and it's called ssb-PositionsInBurst.
       - The UE (User Equipment) assumes that certain SS/PBCH blocks may be transmitted during a specific transmission window.
       - The ssb-PositionsInBurst indicates which SS/PBCH blocks might be transmitted, with each bit representing a block. If the bit is 1, the block might be transmitted; if it's 0, the block is assumed not to be transmitted.
       - If ssb-PositionQCL is configured, certain bits in ssb-PositionsInBurst are set to 0, based on the configured position, and the total number of transmitted blocks is limited.

- ssb-PositionsInBurst = 0x1 and considering the mediumBitmap, the network is suggesting that, during a specific transmission burst, there's a possibility that the first SSB block could be transmitted. This information helps the user equipment (UE) in the network anticipate when and where to expect synchronization signals and control information for more efficient communication and connection establishment.








