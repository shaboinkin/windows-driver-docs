---
title: Handling Installable Features and Options
author: windows-driver-content
description: Handling Installable Features and Options
MS-HAID:
- 'nt5gpd\_28b5fc1f-3170-421e-a03a-c24baa219b16.xml'
- 'print.handling\_installable\_features\_and\_options'
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: b970808f-55bd-4a3a-9464-c9cd3567fa6f
keywords: ["Unidrv, installable features and options", "installable features and options WDK Unidrv", "GPD files WDK Unidrv , installable features and options", "Unidrv WDK print"]
---

# Handling Installable Features and Options


## <a href="" id="ddk-handling-installable-features-and-options-gg"></a>


Some of a printer's features or options might be installable. For example, a printer could accept an optional envelope feeder, which might or might not be currently attached. This envelope feeder must be described within a GPD file in two ways:

-   As an option for the InputBin feature.

-   As an installable "feature" (even though it is really an option), which allows the user to indicate whether it is actually installed.

First, to specify the envelope feeder, along with an automatic feeder, as options for the InputBin feature, the following GPD entries could be used.

```
*Feature: InputBin
{
    *Name: "Input Bin"
    *Option: AUTO
    {
        *Name: "Automatic Feeder"
        *Command: CmdSelect {Command Attributes}
    }
    *Option: ENVFEED
    {
        *Name: "Envelope Feeder"
        *Command: CmdSelect {Command Attributes}
    }
}
```

To make the envelope feeder installable, additional GPD entries are needed, as follows:

```
*InstalledOptionName: "Installed"
*NotInstalledOptionName: "Not installed"
*Feature: InputBin
{
    *Name: "Input Bin"
    *Option: AUTO
    {
        *Name: "Automatic Feeder"
        *Command: CmdSelect {Command Attributes}
    }
    *Option: ENVFEED
    {
        *Name: "Envelope Feeder"
        *Command: CmdSelect {Command Attributes}
        *Installable?: TRUE
        *InstallableFeatureName: "Optional Envelope Feeder"
    }
}
```

Within the \*Option entry for the envelope feeder, two attributes have been added:

-   The \*Installable? attribute indicates that the option is installable.

-   The \*InstallableFeatureName attribute specifies a text string that Unidrv displays so that the user can indicate whether the option is actually installed.

Whenever **\*Installable?** is set to **TRUE** for a feature or option, Unidrv creates an additional feature for the property sheet display. (Note that even if the installable item is an option, Unidrv creates a feature representation for it in the property sheet.) This Unidrv-synthesized feature is identified by the string supplied with **\*InstallableFeatureName**. The feature provides two options, "Installed" and "Not installed", and allows the user to select one of these options. The strings "Installed" and "Not installed" are specified with the \*InstalledOptionName and \*NotInstalledOptionName attributes so that you can modify them if other text is more appropriate.

So, for our example, the property sheet would include an InputBin feature, labeled **Input Bin**, that includes two options, labeled **Automatic Feeder** and **Envelope Feeder**. The property sheet would also include an additional feature, labeled **Optional Envelope Feeder**, with two options, labeled **Installed** and **Not installed**. The user can only select **Envelope Feeder** under **Input Bin** if he or she first selects **Installed** under **Optional Envelope Feeder**.

Sometimes, it is necessary to indicate that certain installable options cannot be installed simultaneously, or that a certain noninstallable option cannot be selected if some other installable option is installed. To handle these situations, use GPD entries that specify [option constraints](option-constraints.md).

You cannot use the \*Installable? attribute with optional features that require a \*DisabledFeatures entry. For these features, you must explicitly specify the optional feature with "Installed" and "Not installed" options. For example, suppose a printer has an optional duplexing unit. The Duplex feature (see [Standard Features](standard-features.md)) must be disabled if the duplexing unit is not installed. You must define an "Optional Duplexing Unit" feature, with "Installed" and "Not Installed" options. Within the "Not Installed" \*Option entry, you would include a \*DisabledFeatures entry for the Duplex feature. The following GPD entries can be used:

```
*Feature: DuplexUnit
{
    *ConflictPriority: 3   *% Make priority higher than Duplex feature
    *Name: "Optional Duplexing Unit"
    *Option: Installed
    {
        *Name: "Installed"
    }
    *Option: NotInstalled
    {
        *Name: "Not Installed"
        *DisabledFeatures: LIST(Duplex)
        *Constraints: LIST (Duplex.LongEdge, Duplex.ShortEdge)
    }
}
```

Be sure to also specify any relevant [option constraints](option-constraints.md), as illustrated.

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bprint\print%5D:%20Handling%20Installable%20Features%20and%20Options%20%20RELEASE:%20%289/1/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")


