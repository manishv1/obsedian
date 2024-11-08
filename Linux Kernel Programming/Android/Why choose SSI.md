## Common Android Partitions

**Here are some common Android partitions:**

- ### Boot Partition
    

The boot partition in Android contains the bootloader, recovery image, and the kernel, which are crucial for starting and managing the device's operating system.

- ### System Partition
    

The Android operating system and core system files are stored in this partition. It's mounted as read-only to ensure the integrity of the OS.

- ### Vendor Partition
    

This partition contains proprietary files and drivers provided by the device manufacturer. It helps ensure proper hardware functionality.

- ### Recovery Partition
    

The recovery partition houses a minimal Android environment used for system updates, maintenance, and recovery operations like factory resets or installing custom recoveries.

- ### Data Partition
    

This is where user data and app data are stored, including user-installed apps, photos, videos, and user preferences. This partition is typically the largest on the device.

- ### Cache Partition
    

Temporary system and app data are stored here to improve system performance. It can be cleared to resolve certain issues or free up space.

- ### Bootloader Partition
    

The bootloader partition contains the bootloader itself, which is the software responsible for initializing the device's hardware and loading the operating system.

-
## Partitions around SSI

Figure 1 shows partitions around SSI, and the versioned interfaces across the partitions and policies on the interfaces. This section explains each of the partitions and interfaces in detail.

![[Pasted image 20241105213528.png]]


- **Vendor:** A collection of SoC-specific components.
    
- **ODM:** A collection of board-specific components that aren’t provided by the SoC. Typically the SoC vendor owns the vendor image, while the device maker owns the ODM image. When there is no separate `/odm` partition, both the SoC vendor and ODM images are merged together in the `/vendor` partition.

- **Product:** A collection of product- or device-specific components that represent OEM customizations and extensions to the Android OS. Put SoC-specific components in the `/vendor` partition. SoC vendors can also use the `/product` partition for appropriate components, such as SoC-independent ones. For example, if an SoC vendor provides an SoC-independent component to their OEM customers (that’s optional to ship with the product), the SoC vendor can place that component in the product image. The location of a component isn’t determined by its _ownership_, it’s dictated by its _purpose_.



![[Pasted image 20241105215829.png]]

**Figure 1.** Maintaining ABI between partitions.

- There's no ABI stability between `odm` and `vendor` partitions. Both partitions must be upgraded at the same time.
- The `odm` and `vendor` partitions can depend on each other, but the `vendor` partition **must** work without an `odm` partition.
- The ABI between `odm` and `system` is the same as the ABI between `vendor` and `system`.

Direct interaction between the `product` partition and the `vendor` or `odm` partition **isn't allowed**. (This is enforced by SEpolicy.)


The vendor manifest **specifies HALs, SELinux policy versions, etc.** **common to an SoC**. It is recommended to be placed in the Android source tree at device/ VENDOR / DEVICE /manifest. xml , but multiple fragment files can be used.