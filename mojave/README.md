# Hackintosh-On-MSI-GE72-6QD
Hackintosh tricks on MSI GE72 6QD Apache Pro Laptop for Mojave

![](https://github.com/fliot/Hackintosh-On-MSI-GE72-6QD/blob/master/msi-ge72-6qd.jpg?raw=true)

## Configuration

| Specifications | Detail | Mojave | Catalina |
| ------------------- | ------------------------------------------- | ------ | -------- |
| Computer model      | MSI GE72 6QD Apache Pro                     |        |          |
| CPU                 | Latest 6th Gen. Intel® Core™ i7 processor   |      X |          |
| CHIPSET             | Intel® HM170                                |      X |          |
| MEMORY              | DDR4-2133                                   |      X |          |
| DISPLAY             | 17.3" Full HD (1920 x 1080)                 |      X |          |
| GRAPHICS            | GeForce® GTX 960M + Intel integrated 530    | Only Intel 530, no Nvidia | |
| GRAPHICS VRAM       | 4 / 2GB GDDR5                               |      - |          |
| STORAGE             | 1 x 2.5" HDD + 1 x NVMe M.2 SSD by PCIe Gen3 X4 / SATA-SSD Combo | X | |
| OPTICAL DRIVE       | BD Writer / DVD Super Multi                 |      X |          |
| AUDIO               | Intel audio integrated C230                 |      X |          |
| WEBCAM              | HD type (30fps@720p)                        |      X |          |
| CARD READER         | SD (XC/HC) Realtek RTS5129                  | Not working |     |
| LAN                 | Killer E2400 Gigabit Ethernet               |      X |        X |
| WIRELESS LAN        | 802.11ac Intel Wireless 3165                |      X |          |
| BLUETOOTH           | Bluetooth Intel integrated                  |      X |          |
| HDMI                | HDMI output                                 |        |          |
| USB                 | USB ports 2.0, 3.0 & 3.1 Type-C             |      X |          |
| KEYBOARD/Trackpad   | Full-color backlight SteelSeries keyboard   |      X |          |
| BATTERY             |                                             |      X |          |


# What's not working
- SD Card (but I have still good hopes to find it out)
- GTX 960M(Disabled)
- Screen Brightness (but I have still good hopes to find it out)
- halt/idle management (I advise you to short the issue, disabling idle https://davidpierron.com/2018/10/11/hackintosh-sluggish-after-brief-idle-period-solved/ )

The rest is all good (including Intel Wireless 3165 adapter).


# 1) Bios

Prepare your Bios with Apple hardware limits

```
Fn/Win key swap: disabled
SATA Mode Selection: AHCI
Intel(R) SpeedStep(tm): Enabled
Super Charger works at S3/S4/S5: disabled
ERP lot 3 support: disabled
Wake up On Lan S5 support: disabled
Network Stack: disabled
Intel Virtualization Technology: disabled
VT-d: disabled
Hyper-threading: enabled
CPU C states: enabled
And configure UEFI Boot
```


# 2) UniBeast

Perform a simple installation from Unibeast Mojave,

Once installer partition set, in clover efi boot, dive into options
```
Remove the « nv_disable=1 » flag
PCI devices->USB Ownership
PCI devices->USB Injection
CPU tuning->EnableC7
Graphics Injector->InjectIntel
Before starting, on selected EFI boot target, remove « Set Nvidia to vest » selection.
```

Go though regular installation process.

Once installation done, boot from hard disk, with the very same clover refi option than earlier (it supposes to be already set this way, but it worths to check).


# 3) From your Mojave system,
mount your EFI partition, and copy the .zip attached files into /Volumes/EFI/EFI/CLOVER/kexts

(You can also delete unused « patched » kext), at the end, only these kext are required in /Volumes/EFI/EFI/CLOVER :
```
./ACPI/patched/AppleBacklightFixup.kext
./kexts/Other/AppleALC.kext
./kexts/Other/Lilu.kext
./kexts/Other/VirtualSMC.kext
./kexts/Other/USBInjectAll.kext
./kexts/Other/WhateverGreen.kext
./kexts/Other/VoodooPS2Controller.kext
./kexts/Other/AtherosE2200Ethernet.kext
./kexts/Other/VoodooSDHC.kext
./kexts/Other/AppleIntelWiFi.kext
./kexts/Other/FakePCIID.kext
```

Edit the file /Volumes/EFI/EFI/CLOVER/kexts/Other/AppleIntelWiFi.kext/Contents/Info.plist

Change BSSID and PWD section, to match your wifi params


Copy /Volumes/EFI/EFI/CLOVER/kexts/Other/AppleIntelWiFi.kext into /System/Library/Extensions/AppleIntelWiFi.kext

(I know, being in EFI Clover should be enough, and maybe we are doing double use, but it seems not work if this drivers is not duplicated in these 2 spaces…)


# 4) Done !

Just reboot with these changes, and you will have:
```
BSD process name corresponding to current thread: kernel_task
Boot args: dart=0 

Mac OS version:
18G103

Kernel version:
Darwin Kernel Version 18.7.0: Tue Aug 20 16:57:14 PDT 2019; root:xnu-4903.271.2~2/RELEASE_X86_64

Model: iMac14,2, BootROM 142.0.0.0.0, 4 processors, Intel Core i7, 2,59 GHz, 16 GB, SMC 2.15f7
Graphics: kHW_IntelHDGraphics530Item, Intel HD Graphics 530, spdisplays_builtin
Graphics: spdisplays_display, spdisplays_pcie_device
Memory Module: BANK 0/DIMM0, 8 GB, DDR4, 2133 MHz, Samsung, M471A1K43BB0-CPB
Memory Module: BANK 1/DIMM0, 8 GB, DDR4, 2133 MHz, Samsung, M471A1K43BB0-CPB
AirPort: Third-Party Wireless Card
Bluetooth: Version 6.0.14d3, 3 services, 26 devices, 1 incoming serial ports
Network Service: Wi-Fi, AirPort, en2
PCI Card: display, 3D Controller, Slot-1
Serial ATA Device: HFS256G39MND-3510A, 256,06 GB
Serial ATA Device: HL-DT-ST DVDRAM GUD0N
Serial ATA Device: HGST HTS721010A9E630, 1 TB
USB Device: USB 3.0 Bus
USB Device: USB2.0-CRW
USB Device: BisonCam, NB Pro
USB Device: Bluetooth HCI
USB Device: MSI EPF USB
USB Device: USB 3.1 Bus
```
