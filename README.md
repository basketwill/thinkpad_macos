# thinkpad_macos
It is best to have Sandy Bridge, Ivy Bridge, Haswell, or Broadwell, Skylake or Kaby Lake.
Many people post here asking "Will my laptop work." I will try to address some of the general issues with laptops in this post.

First off, I should mention that getting a laptop close to 100% functionality is always difficult (unless it is a laptop already with a well written guide) and may not be possible or to the level you want. That's just because of the nature of laptops: you don't get to choose every hardware component (for OS X compatibility) like you can a desktop build.

That said, most newer laptops are generally compatible with macOS/OS X mostly due to the fact that the base components are provided by Intel CPUs and the Intel 6-series or 7-series chipset. If your laptop has Intel graphics with an Intel CPU, chances are you can attempt to make the laptop work with OS X. Some dedicated Nvidia/AMD devices are also supported, but tend to be more problematic.

Where the difficulty comes in is with all the other hardware and variables...

Refer to the FAQ for additional information: http://www.tonymacx86.com/yosemite-...faq-read-first-laptop-frequent-questions.html


General Notes:

- CPU and chipset series: You need to know what exact CPU your laptop has. If it is Sandy Bridge it will have a model designation iX-2xxx*. If it is Ivy Bridge it will have a model designation iX-3xxx*. Haswell (supported starting with 10.8.5) is iX-4xxx*. Broadwell iX-5xxx, Skylake iX-6xxx, and Kaby Lake iX-7xxx. You should also note your chipset version, either 6-series, 7-series, 8-series, 9-series, 100-series. A mixed configuration (eg. Sandy Bridge CPU on 7-series chipset) is more difficult to configure than one that isn't mixed.

- Discrete graphics: Many laptops that have AMD Radeon or Nvidia GeForce discrete graphics utilize a switching mechanism to switch from integrated graphics (Intel HD) to the discrete card when the situation demands. This type of switching does not work in OS X and generally it means disabling the discrete capability in BIOS. If your BIOS does not allow it to be disabled, it can complicate the installation procedure (you may have to delete the related kexts). Also, it is possible that the discrete card is still drawing power even though you are not using it in OS X, so try to find a laptop without discrete graphics.

- WiFi: There are a limited number of WiFi chips supported by OS X drivers. In addition, OS X's WiFi driver architecture is not documented, so there are not very many WiFi driver "Linux ports" to OS X. There are some useful links for OS X WiFi in this post: http://www.tonymacx86.com/hp-probook/97099-wi-fi-bluetooth-cards-laptops-mac-os-os-x.html. In addition, some BIOS implementations have implemented a whitelist where only certain cards can be installed into the laptop, perhaps only cards branded specifically by the manufacturer of the laptop. Sometimes, the whitelist can be disabled via a hacked BIOS, but sometimes the BIOS is encrypted. The service manual for your laptop may give you a list of compatible cards and if one of those happens to also be compatible with OS X, then you can search for such a card and try replacing it. Some of the more compact laptops (Ultrabooks) have WiFi soldered onto the motherboard or combined with another componenent (mSATA SSD), making replacement more difficult or expensive. In many cases, the specific WiFi chip included with the laptop is not specified when you purchase.

Note: Compatible WiFi chipsets and the procedures for making them work are covered here: http://www.tonymacx86.com/network/104850-guide-airport-pcie-half-mini-v2.html. Any WiFi chips not listed are not supported and must be replaced. Also read the laptop FAQ (linked above).

- Ethernet: Ethernet is generally built-in to the laptops motherboard. And since OS X Ethernet driver interface is documented there are quite a few open source drivers available. Usually you can find one that works, but not always. Read the laptop FAQ.

- Audio: Getting audio to work generally depends on finding or creating a patched AppleHDA (for best results and stability, don't use VoodooHDA except as a last resort). Although patching AppleHDA for any audio codec is possible, it is very involved, requires technical skills you may not have, and is time consuming. Read the laptop FAQ.

- Camera: Most built-in cameras are USB, some are connected to USB2 and some to USB3. Some will work, some won't. There is no pattern, and there is no way to tell until you try.

- Card reader: Some card readers are on the PCI bus and have OS X drivers available from the manufacturer (certain Jmicron devices, for example). Some are on the USB bus and might work using the class driver built-in to OS X. You won't know until you try.

- Bluetooth: Sometimes bluetooth is built-in to the WiFi card (on the USB bus) and sometimes it is somewhere else (on the motherboard?). Bluetooth is always a rough spot on any OS X hack, so it may not work how you would like. Best result is with compatible Broadcom bluetooth hardware. Some features like handoff/hotspot/unlocking with Apple watch remain unreliable.

- Battery status: Battery status is possible to make work by using an ACPI compliant AppleSmartBatteryManager.kext. I generally recommend my own kext: https://github.com/RehabMan/OS-X-ACPI-Battery-Driver. Most DSDTs will need patching to work correctly with the vanilla AppleACPIPlatform.kext (and the latest AppleACPIPlatform.kext is required for Ivy Bridge power management). The patching required is often specific to the laptop family and requires some programming background to accomplish (unless you find a patch has already been done for you). See link in the laptop FAQ.

- Keyboard/Trackpad: Most laptops use the PS/2 interface for the keyboard and trackpad. OS X has no support for the PS/2 interface, so you will need to install drivers for it. What trackpad you actually have in your laptop determines which version you seek out. And most of the time, the trackpad manufacturer is not specified... sometimes varying even with the same model of laptop. There has even been a case of a laptop being sent out for repair and coming back with a trackpad from a different vendor. Newer computers are using I2C for the trackpad, which requires different drivers. Such drivers are a work in progress.

- DSDT: Most laptops will require ACPI edits to get various laptop features working. You should be prepared to learn about DSDT patches, MaciASL, how to install your ACPI files such that the bootloader (eg. Clover) can load them, etc. Don't fall into the trap of downloading a DSDT for another laptop. Find patches and patch your own. Native DSDTs can vary between laptops even that are the same model. My ACPI patching guide is linked from the FAQ.

- BIOS: Some BIOS implementations are picky about booting. For example, some will not boot legacy mode to a GPT drive. OS X generally requires to be installed to a GPT drive, so this can be a problem. There are workarounds, but sometimes this can be difficult for the non-technical to understand.

In short, hacking a laptop is a challenge. You should not expect it will be easy. And there are many different laptops, each with unique configurations, BIOS, hardware combinations, etc. It is always possible that a given laptop will not work even if the hardware mentioned above seems to meet the requirements. 

And if you want 100% compatibility with a discrete graphics capability, perfect bluetooth and no hassles, don't look to a hack... buy a MacBook Pro.


Note on gen1 Core i-series CPUs

These laptops generally use "Intel HD Graphics" which are not supported very well by OS X. But some report getting it to work. There is a relatively extensive write up here: http://www.insanelymac.com/forum/topic/286092-guide-1st-generation-intel-hd-graphics-qeci/. YMMV.


Note on Kaby Lake

Early testing of Kaby Lake CPUs and Intel HD620 graphics reveals good compatibility with the kexts for Skylake. Kaby Lake CPU requires using the FakeCPUID (Clover option) with the appropriate CPUID for similar Skylake CPU. And graphics can be made to work with FakeID/IntelGFX and FakePCIID kexts. Read the laptop FAQ.


Note on Samsung Laptops and eDP displays

It seems many Samsung laptops have an eDP connection to the laptop LCD, instead of the more common LVDS. The drivers in OS X for HD3000 and HD4000 do not support eDP. So avoid such Samsung laptops or at least verify whether it is eDP or not.

Evidently there is support for eDP in the HD5000 (HD4400/HD4600) OS X drivers, so eDP on Haswell (or later?) systems should be better supported. Current experience shows that eDP is problematic.
