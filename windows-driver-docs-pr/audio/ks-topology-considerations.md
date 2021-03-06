---
title: KS Topology Considerations
description: KS Topology Considerations
ms.assetid: 81a2a41a-2a10-4de0-9a64-2c8f86a0c96d
keywords: ["non-PCM audio formats WDK , KS topology", "bridge pins WDK audio", "mixer API WDK audio", "KS filters WDK audio , non-PCM wave formats"]
---

# KS Topology Considerations


## <span id="ks_topology_considerations"></span><span id="KS_TOPOLOGY_CONSIDERATIONS"></span>


The [WDMAud system driver](user-mode-wdm-audio-components.md#wdmaud_system_driver) (Wdmaud.sys) translates the KS-filter topology into the legacy mixer lines that are exposed through the **mixer** API. A non-PCM pin corresponds to a SRC line (MIXERLINE\_COMPONENTTYPE\_SRC\_*XXX*) in the mixer API. If this pin is in a data path that eventually flows into a bridge pin (the physical connection at the endpoint of a graph) that is dedicated to non-PCM data, the **mixer** API exposes the bridge pin as an additional DST line (MIXERLINE\_COMPONENTTYPE\_DST\_*XXX*), separate from the DST line for PCM data. This can add unnecessary complexity to the controls that are visible through a **mixer**-API client such as a replacement for the SndVol32 utility.

If you prefer not to expose a non-PCM pin in this manner, one approach is to make sure that the data path containing the pin eventually feeds into a SUM node that is shared by the PCM data path. That is, join the non-PCM DST line to the main DST line. Unfortunately, this workaround misrepresents the true hardware topology and might lead to future problems with clients that attempt to control the non-PCM data stream through nodes downstream from the SUM node. A better approach is to modify the **mixer**-API client to simply ignore SRC and DST lines that have no controls.

If you use the [KsStudio utility](ksstudio-utility.md) to view your wave filter in KSCATEGORY\_AUDIO, you should expect to see a separate pin for non-PCM data. When viewing the composite system audio graph under KSCATEGORY\_AUDIO\_DEVICE, you should see your non-PCM data ranges on the main wave-output pin, alongside any PCM data ranges.

SysAudio (Sysaudio.sys) is the system audio device in Windows Server 2003, Windows XP, Windows 2000, and Windows Me/98. Note that SysAudio generates KSCATEGORY\_AUDIO\_DEVICE automatically--a driver should not register itself manually in this category.

You are not required to connect a non-PCM data path to the Topology miniport driver. This connection is of benefit only if the non-PCM data path interacts with the rest of the device's topology; for instance, if it feeds into a common mixer or sample-rate converter. Connecting a streaming pin to a bridge pin, where both pins are on the wave miniport driver, forms a valid, complete topology for a non-PCM data stream that flows directly to an S/PDIF port, for example.

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[audio\audio]:%20KS%20Topology%20Considerations%20%20RELEASE:%20%287/18/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")


