Texas Instruments, Inc.

ZStack-CC2530 Release Notes

-------------------------------------------------------------------------------
-------------------------------------------------------------------------------

ZigBee 2007 Release
Version 2.4.0-1.4.0
December 03, 2010


Notices:
 - ZStack-CC2530 has been certified for ZigBee/ZigBee-PRO compliance.

 - Z-Stack supports the ZigBee 2007 Specification, including features such
   as PanID Conflict Resolution, Frequency Agility, and Fragmentation. The
   ZigBee 2007 Specification (www.zigbee.org) defines two ZigBee stack
   profiles, ZigBee and ZigBee-Pro. ZStack-2.4.0 provides support for both
   of these profiles. See the Z-Stack Developer's Guide for details.

 - Z-Stack now supports an IAR project to build "ZigBee Network Processor"
   (ZNP) devices. CC2530-based ZNP devices can be used with any host MCU
   that provides either an SPI or UART port to add ZigBee-Pro communication
   to existing or new designs. See the "CC2530ZNP Interface Specification"
   and "Z-Stack User's Guide for CC2530 ZigBee-PRO Network Processor -
   Sample Applications" documents for details and using the ZNP.

 - The library files have been built and tested with EW8051 version 7.60.5
   (7.60.5.40066) and may not work with other versions of the IAR tools.
   You can obtain the 7.60 installer and patches from the IAR website.

 - Z-Stack has been built and tested with IAR's CLIB library, which provides
   a light-weight C library which does not support Embedded C++. Use of DLIB
   is not recommended since Z-Stack is not tested with that library.

 - When programming a target for the first time with this release, make sure
   that you select the "Erase Flash" in the "Debugger->Texas Instruments->
   Download" tab in the project options. When programming completes, it is
   recommended that the "Erase Flash" box gets un-checked so that NV items
   are retained during later re-programming.

 - Please review the document "Upgrading To Z-Stack v2.4." for information
   about moving existing v2.3.1 applications to v2.4.0.


Changes:
 - Added support for the ZigBee OTA Upgrade Cluster as specified in ZigBee
   Alliance document 095264r00. OTA provides an inter-operable mechanism
   for wirelessly upgrading the firmware on Z-Stack devices. Refer to the
   "Z-Stack OTA Upgrade User's Guide" for details on this feature. [3457]

 - Modified all IAR sample application project files to eliminate the "non-Pro"
   configurations. The ZIGBEEPRO compile flag was deleted from project files
   and placed into the f8wConfig.cfg file. Non-Pro devices can still be built by
   disabling this compile flag and using the provided non-Pro libraries. [3446]

 - Modified all IAR library and sample application project files to work with the
   EW8051-7.60 toolchain. Optimization has been set to "high, size" with the
   "Code Motion" option disabled. [3444]

 - Added capability to control the "OSAL task processing loop" from an external
   process. A new function, osal_run_system(), runs one pass of the OSAL task
   processor, being called from the forever loop in osal_start_system(). [3437]

 - Implemented the ZCL Basic and BACNet (regular & extended) Input, Output,
   and Value clusters of the ZCL specification version 03, sections 3.14 and 9.4:
   075123r03ZB_AFG-ZigBee_Cluster_Library_Specification. [3226]

 - Updated the ZCL 11073 Protocol Tunnel Cluster implementation to rev15
   specifications: 075360r15ZB_ZHC_PTG-PHHC-Profile. [3122]


Bug Fixes:
 - Updated the "CC2530ZNP Interface Specification" document relating to the
   RF Test Mode of the CC2530. Removed the un-implemented SYS_TEST_RF
   command (section 4.1.8) and added section 4.2.4.11 to explain the use of
   ZNP_NV_RF_TEST_PARAMS to control RF testing. [3515]

 - Fixed a problem in ZCL Key Establishment where the fc.direction in the ZCL
   header was always set to ZCL_FRAME_CLIENT_SERVER_DIR regardless of
   whether the KE server or client generated the terminate key establishment
   command. [3496]

  - Fixed an inconsistent problem that could occur when a message was sent
   to the destination address of the local device - the security mode was not
   initialized properly, causing some messages to be dropped. [3495]

 - Fixed an issue which only sporadically allowed the Demo configuration of the
   Z-Stack SampleApp to start as a Corrdinator when a jumper was placed on
   the designated pair of pins of P18 (see "Z-Stack User's Guide"). [3493]

 - Modified the default value of MAC_RADIO_RECEIVER_SENSITIVITY_DBM, to
   match the latest CC2520 data sheet (from -91 to -97). This will improve the
   quality of the calculated LQI. [3488]

 - Updated the defined ZDO Status Values to correct an error and add missing
   items. The value of ZDP_TABLE_FULL was changed from 0x87 to 0x8C. New
   definitions were added for ZDP_NO_DESCRIPTOR, ZDP_NOT_AUTHORIZED,
   ZDP_BINDING_TABLE_FULL. [3484]

 - Modified the Z-Stack SE SampleApp to discover endpoints for CBKE instead
   of assuming fixed endpoints. [3483]

 - Fixed a Smart Energy ZCL problem which would cause a function call to a
   NULL address when a PublishPrice message was received and the callback
   had been registered. [3482]

 - Fixed a problem with the Simple Meter device of the Z-Stack SE SampleApp,
   changing the data type of the InstantaneousDemand attribute from uint24 to
   to int24 because energy can flow in both directions per SE spec. [3481]

 - Updated the defined ESP Historical Consumption Attributes for the Smart
   Energy Simple Metering Cluster. The attributes were changed: [3481]
      - "ATTRID_SE_CURRENT_PARTIAL_PROFILE_INTERVAL_START_TIME",
        "ATTRID_SE_CURRENT_PARTIAL_PROFILE_INTERVAL_VALUE"
   have been replaced by:
      - "ATTRID_SE_CUR_PART_PROFILE_INT_START_TIME_DELIVERED",
        "ATTRID_SE_CUR_PART_PROFILE_INT_START_TIME_RECEIVED",
        "ATTRID_SE_CUR_PART_PROFILE_INT_VALUE_DELIVERED",
        "ATTRID_SE_CUR_PART_PROFILE_INT_VALUE_RECEIVED"

 - Fixed a problem where sleep time was programmed with an incorrect long
   duration instead of the expected short duration. Now, the device will not
   enter IDLE sleep if the sleep request is less than 4 x 320us. [3478]

 - Fixed a memory alignment issue in the dynamic memory manager that was
   discovered on a 32-bit device which caused failure during start-up. [3431]

 - Fixed a problem where the broadcast passive acknowledge mechanism did
   not count neighbors in it's neighbor table when calculating the number of
   retransmits it expects to see. [3425]

 - Fixed a memory alignment issue discovered on a 32-bit device which caused
   a router to fail during energy scanning for frequency agility. [3424]

 - Fixed an obscure problem where the MAC could calculate an incorrect time
   if macMcuPrecisionCount() got interrupted in a small time window. [3423]

 - Fixed a problem where allocated memory (OSAL heap) could get corrupted
   when the ZDO_ProcessMgmtRtgReq() function was used. [3401]

 - Fixed problems in the REFLECTOR feature when using APS security, where
   the reflected message would be sent using NWK, not APS security. [3260]

 - Fixed problems with the Inter-PAN feature where a router could mistakenly
   send Link Status messages on the Inter-PAN channel, and allow another
   device to join on the Inter-PAN channel . [3159]

 - Fixed a problem that limited the maximum time that a device could sleep
   and maintain proper system timing to approximately 20.97 seconds. Sleep
   intervals can now use the full 16-bit range (up to 65.535 seconds). [3004]

Memory Sizes:
 - The CC2530 has 256K bytes of Flash memory to store executable program
   and non-volatile (NV) memory, and 8K bytes of RAM for program stack and
   data variables. Actual usage of Flash and RAM memory is variable, of course,
   and dependent on the specific application. Developers can adjust various
   parameters, such as, program stack size and dynamic memory heap size
   to meet their specific needs.

 - The following table provides a comparison of Flash and RAM sizes for one
   of the sample applications provided with ZStack - SampleApp that is found
   in the installed ..\Projects\zstack\Samples\SampleApp\CC2530DB folder. In
   most ZStack sample applications, generic heap settings are used which have
   been selected to accomodate a wide range of applications. For this example,
   heap settings were: Coordinator/Router = 3K bytes, EndDevice = 2K bytes.
   See the "Heap Memory Management" document for details on profiling heap
   memory usage.

 - Memory sizes are shown below for the 3 ZigBee device types, with/without
   ZigBee-PRO, with/without Security, and compiled to run on the SmartRF05EB
   board with CC2530EM module. See the Z-Stack User's Guide for more details.

                             Coordinator        Router         EndDevice
    PRO   SECURE     Flash/RAM      Flash/RAM      Flash/RAM
   ===  ======   ========   ========   ========
    Off        Off        137.6K/6.6K    136.4K/6.6K    108.7K/5.0K
    Off        On        147.9K/6.6K    147.6K/6.6K    119.7K/5.1K
    On        Off        148.8K/6.7K    147.8K/6.7K    114.3K/5.1K
    On        On        159.2K/6.7K    159.0K/6.7K    126.2K/5.2K


Known Issues:
 - Corruption of the call-stack can occur if the MAX_BINDING_CLUSTER_IDS
   compile option is changed from the default value. Do not change the value
   of this parameter in the f8wConfig.cfg file. [3394]

 - To disable security at build time, use the "SECURE=0" compile option. Do
   not attempt to disable security by setting the SECURITY_LEVEL to zero.

 - SerialApp is not reliable when used for high-speed, large file transfers -
   the receiving application occasionally drops a byte.

 - ZOAD is a simple application to demonstrate Over-Air-Download (OAD)
   and is not intended for manufacturing or production usage.

 - The ZDO Complex Descriptor is not supported.

-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
