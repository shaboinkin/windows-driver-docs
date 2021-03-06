---
title: Order of Resources in Start-Device IRP
author: windows-driver-content
description: Order of Resources in Start-Device IRP
MS-HAID:
- 'pcidg\_fb942a42-adf3-4b15-82f4-dc78b893e2c3.xml'
- 'PCI.order\_of\_resources\_in\_start\_device\_irp'
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: df55105e-3da3-40cc-9f57-05632cb2d043
keywords: ["order of resources WDK PCI", "start-device IRP WDK PCI"]
---

# Order of Resources in Start-Device IRP


The order of the resources that are reported in the start-device I/O request packet (IRP) should match the order of the resources that are listed in the PCI base address registers (BARs). There are two types of resource lists, raw and translated. Each resource list has resource descriptors. The resource descriptors in the resource lists are in the order of base address registers (BARs) on PCI devices. The order of resources in the raw and translated lists is the same. There is device-private descriptor data between two consecutive resource descriptors. The resource descriptors for BARs are followed by one or more descriptors for extended message-signaled interrupt (MSI-X) messages, or one descriptor for MSI, or one or more descriptors for hardware-based interrupts. In some cases, such as with video devices, for example, the descriptors for BARs are followed by descriptors for legacy video resources. The ordering of descriptors for BARs in a resource list is guaranteed to match the BARs on a PCI device on all hardware platforms.

 

 


--------------------


