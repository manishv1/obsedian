
- **Before Project Treble**

![[Pasted image 20241105215100.png]]

 An ambitious redesign of Android, known as Project Treble, separated the OS framework from device-specific low-level software (referred to as vendor implementation) using a clear, reliable vendor interface.
    
 The Android framework was tightly coupled with HAL interfaces, which were defined in the hardware/libhardware folder. Hardware vendors had to update their HALs to match new Android interfaces, which was time-consuming and complex.

Project Treble separated the Android OS framework from the vendor implementation, which is the hardware-specific part of Android. The two parts are installed in separate partitions, and a versioned interface called the vendor interface (VINTF) is defined across the partitions. This allows the system partition to be modified without modifying the vendor partition, and vice versa.

For app developers, API consistency is important for the app to work on various devices. It can be verified by CTS (Compatibility Test Suite), and the developer APIs are specified in CDD (Compatibility Definition Document).

The introduction of a new vendor interface between the Android OS framework and the vendor implementation achieves this. A Vendor Test Suite (VTS), analogous to the CTS validates the new vendor interface, to ensure its forward compatibility. Versioning is mandatory for all stable HALs. The goal is to make the new framework compatible with the old vendor implementation.

![[Pasted image 20241105212821.png]]


![[Pasted image 20241105212756.png]]![[Pasted image 20241105213037.png]]![[Pasted image 20241105213204.png]]

## Treble Changes - Build System

Android Framework will reside in the system partition, and Vendor HAL Implementation will be in a newly defined partition (vendor.img).

![[Pasted image 20241105213228.png]]