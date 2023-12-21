``` bash
Active_gNBs = ( "gNB-OAI");
# Asn1_verbosity, choice in: none, info, annoying
Asn1_verbosity = "none";

gNBs =
(
 {
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

    ////////// Physical parameters:

    pdsch_AntennaPorts_XP = 2; #thamizh change
    pusch_AntennaPorts    = 2; #thamizh change
    do_CSIRS              = 1;
    do_SRS                = 0 ;
    sib1_tda			  = 15;

    pdcch_ConfigSIB1 = (
      {
        controlResourceSetZero = 11; #10; #thamizh change
        searchSpaceZero = 0;
      }
    );

    servingCellConfigCommon = (
    {
 #spCellConfigCommon

      physCellId                                                    = 0;
     # n_TimingAdvanceOffset                                         = 0;
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
       initialDLBWPlocationAndBandwidth                             = 1099; #38.101-1 Table 5.3.2-1
       #
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
        initialDLBWPsubcarrierSpacing                               = 1;
      #pdcch-ConfigCommon
        initialDLBWPcontrolResourceSetZero                          = 11; #thamizh change
        initialDLBWPsearchSpaceZero                                 = 0;

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
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
        initialULBWPsubcarrierSpacing                               = 1;
      #rach-ConfigCommon
        #rach-ConfigGeneric
          prach_ConfigurationIndex                                  = 159; #thamizh change
#prach_msg1_FDM
#0 = one, 1=two, 2=four, 3=eight
          prach_msg1_FDM                                            = 0;
          prach_msg1_FrequencyStart                                 = 22;
          zeroCorrelationZoneConfig                                 = 15; #thamizh change
          preambleReceivedTargetPower                               = -100; #-84 #Luis -70; #thamizh change
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
# restrictedSetConfig
# 0=unrestricted, 1=restricted type A, 2=restricted type B
        restrictedSetConfig                                         = 0,

        msg3_DeltaPreamble                                          = 6; #thamizh change
        p0_NominalWithGrant                                         = -96 #-86; #Luis-70; #thamizh change

# pucch-ConfigCommon setup :
# pucchGroupHopping
# 0 = neither, 1= group hopping, 2=sequence hopping
        pucchGroupHopping                                           = 0;
        hoppingId                                                   = 0; #thamizh change
        p0_nominal                                                  = -96; #Luis-70; #thamizh change
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

# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
      subcarrierSpacing                                             = 1;


  #tdd-UL-DL-ConfigurationCommon
# subcarrierSpacing
# 0=kHz15, 1=kHz30, 2=kHz60, 3=kHz120
      referenceSubcarrierSpacing                                    = 1;
      # pattern1
      # dl_UL_TransmissionPeriodicity
      # 0=ms0p5, 1=ms0p625, 2=ms1, 3=ms1p25, 4=ms2, 5=ms2p5, 6=ms5, 7=ms10
      dl_UL_TransmissionPeriodicity                                 = 5; #thamizh change
      nrofDownlinkSlots                                             = 3; #thamizh change
      nrofDownlinkSymbols                                           = 6; #thamizh change
      nrofUplinkSlots                                               = 1; #thamizh change
      nrofUplinkSymbols                                             = 4; #thamizh change
      #luis config
      #dl_UL_TransmissionPeriodicity                                 = 6; #thamizh change
      #nrofDownlinkSlots                                             = 7; #thamizh change
      #nrofDownlinkSymbols                                           = 6; #thamizh change
      #nrofUplinkSlots                                               = 2; #thamizh change
      #nrofUplinkSymbols                                             = 4; #thamizh change

  ssPBCH_BlockPower                                                 = 0;
  }

  );


    # ------- SCTP definitions
    SCTP :
    {
        # Number of streams to use in input/output
        SCTP_INSTREAMS  = 2;
        SCTP_OUTSTREAMS = 2;
    };


    ////////// AMF parameters:
    amf_ip_address      = ( { ipv4       = "192.168.70.132";
                              ipv6       = "192:168:30::17";
                              active     = "yes";
                              preference = "ipv4";
                            }
                          );

    NETWORK_INTERFACES :
    {
        GNB_INTERFACE_NAME_FOR_NG_AMF            = "demo-oai";
        GNB_IPV4_ADDRESS_FOR_NG_AMF              = "192.168.1.191/24";
        GNB_INTERFACE_NAME_FOR_NGU               = "demo-oai";
        GNB_IPV4_ADDRESS_FOR_NGU                 = "192.168.1.191/24";
        GNB_PORT_FOR_S1U                         = 2152; # Spec 2152
    };
  }
);

MACRLCs = (
{
  num_cc                      = 1;
  tr_s_preference             = "local_L1";
  tr_n_preference             = "local_RRC";
  pusch_TargetSNRx10          = 200;
  pucch_TargetSNRx10          = 200;
#  pusch_FailureThres          = 1000;
  ulsch_max_frame_inactivity  = 0;
  ul_max_mcs                  = 20;
}
);

L1s = (
{
  num_cc = 1;
  tr_n_preference = "local_mac";
  prach_dtx_threshold = 130
  pucch0_dtx_threshold = 80;
  pusch_dtx_threshold = 10;
#  max_ldpc_iterations = 6;
#thread_pool_size = 8;
  tx_amp_backoff_dB = 20; #36; #6;
  L1_rx_thread_core = 8;
  L1_phase_comp = 1;
}
);

RUs = (
{
  local_rf       = "no"; #thamizh change
  nb_tx          = 2; #thamizh change
  nb_rx          = 2; #thamizh change 
  att_tx         = 0
  att_rx         = 0;
  bands          = [78];
  max_pdschReferenceSignalPower = -27;
  max_rxgain                    = 75;
  sf_extension                  = 0;
  eNB_instances  = [0];
  ru_thread_core = 6;
  sl_ahead       = 5;
  ##beamforming 1x2 matrix: 1 layer x 2 antennas
  bf_weights = [0x00007fff, 0x0000,0x00007fff, 0x0000];
  #clock_src = "internal";
  sdr_addrs = "dummy --usecasefile /home/seven/Liteon-ngKore_E/cmake_targets/ran_build/build/usecase_du_3450.cfg --num_eth_vfs 2 --vf_addr_o_xu_a \"0000:8a:02.0,0000:8a:02.1\""
  clock_src = "internal"; #thamizh change
  tr_preference  = "raw_if4p5"; #thamizh change
  do_precoding = 0; #thamizh change
  
}
);

THREAD_STRUCT = (
{
  #three config for level of parallelism "PARALLEL_SINGLE_THREAD", "PARALLEL_RU_L1_SPLIT", or "PARALLEL_RU_L1_TRX_SPLIT"
  parallel_config    = "PARALLEL_SINGLE_THREAD";
  #two option for worker "WORKER_DISABLE" or "WORKER_ENABLE"
  worker_config      = "WORKER_ENABLE";
}
);

rfsimulator :
{
  serveraddr = "server";
  serverport = "4043";
  options = (); #("saviq"); or/and "chanmod"
  modelname = "AWGN";
  IQfile = "/tmp/rfsimulator.iqs";
};

security = {
  # preferred ciphering algorithms
  # the first one of the list that an UE supports in chosen
  # valid values: nea0, nea1, nea2, nea3
  ciphering_algorithms = ( "nea0" );

  # preferred integrity algorithms
  # the first one of the list that an UE supports in chosen
  # valid values: nia0, nia1, nia2, nia3
  integrity_algorithms = ( "nia2", "nia0" );

  # setting 'drb_ciphering' to "no" disables ciphering for DRBs, no matter
  # what 'ciphering_algorithms' configures; same thing for 'drb_integrity'
  drb_ciphering = "yes";
  drb_integrity = "no";
};

log_config :
{
  global_log_level                      ="info";
  global_log_verbosity                  ="medium";
  hw_log_level                          ="info";
  hw_log_verbosity                      ="medium";
  phy_log_level                         ="info";
  phy_log_verbosity                     ="medium";
  mac_log_level                         ="info";
  mac_log_verbosity                     ="medium";
  rlc_log_level                         ="info";
  rlc_log_verbosity                     ="medium";
  pdcp_log_level                        ="info";
  pdcp_log_verbosity                    ="medium";
  rrc_log_level                         ="info";
  rrc_log_verbosity                     ="medium";
  ngap_log_level                        ="info";
  ngap_log_verbosity                    ="medium";
  f1ap_log_level                        ="info";
  f1ap_log_verbosity                    ="medium";
};
```
