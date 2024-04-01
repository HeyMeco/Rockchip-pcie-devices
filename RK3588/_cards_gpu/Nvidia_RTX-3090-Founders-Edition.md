---
layout: card
title: "Nvidia GeForce RTX 3090"
functionality: "Detected & Bar Space Assigned | Probably HW Limit see Issue"
kernel: "6.8.2"
driver_required: "Yes"
github_issue: "[Issue #2](https://github.com/HeyMeco/Rockchip-pcie-devices/issues/2)"
buy_link: "ToDo"
---
The RTX 3090 is supported by both Nvidia's proprietary driver and the open source Nouveau driver in the Linux Kernel.

It requires a 16x slot. You should also supply _at least_ 5A of 12V power to whatever PCIe riser you use to connect it, in addition to external power to the card's PCIe power inputs.

There are two ways to install the driver:

### Proprietary Nvidia driver

After flashing Armbian Ubuntu Jammy Desktop with kernel headers and having a custom Patch that fixed a bug in the PCIe Root Controller that claimed all the memory allocation you can install the Nvidia driver:

After the setup, you need to exit the graphical user interface `sudo systemctl stop gdm3` before installing the Nvidia drivers.

Now, download Nvidia's latest [AARCH64 Driver for ARM 64-bit processors](https://www.nvidia.com/en-us/drivers/unix/linux-aarch64-archive/), and run it with `sudo`:

```
sudo ./NVIDIA-Linux-aarch64-550.40.07.run
```

After a reboot, the card would not initialize and fail with the message: 
```
[    8.600582] NVRM: GPU 0000:01:00.0: RmInitAdapter failed! (0x25:0x65:1468)
[    8.600674] NVRM: GPU 0000:01:00.0: rm_init_adapter failed, device minor number 0
[    8.600817] [drm:nv_drm_load [nvidia_drm]] *ERROR* [nvidia-drm] [GPU ID 0x00000100] Failed to allocate NvKmsKapiDevice
[    8.601012] [drm:nv_drm_probe_devices [nvidia_drm]] *ERROR* [nvidia-drm] [GPU ID 0x00000100] Failed to register device
```

### Nouveau Driver
1. Uninstall the proprietary Nvidia Driver first.
2. `sudo modprobe nouveau` to activate it. (Make sure it was being built in the kernel configuration)

You would then see that this driver also can't setup the GPU
```
[    2.955213] nouveau 0000:01:00.0: enabling device (0000 -> 0003)
[    2.955312] nouveau 0000:01:00.0: NVIDIA GA102 (b72000a1)
[    3.317492] nouveau 0000:01:00.0: bios: version 94.02.4b.00.0b
[    3.606146] nouveau 0000:01:00.0: bios: M0203E type 0a
[    3.606160] nouveau 0000:01:00.0: fb: 24576 MiB of unknown memory type
[    4.468637] nouveau 0000:01:00.0: DRM: VRAM: 24576 MiB
[    4.468672] nouveau 0000:01:00.0: DRM: GART: 536870912 MiB
[    4.468680] nouveau 0000:01:00.0: DRM: BIT table 'A' not found
[    4.468686] nouveau 0000:01:00.0: DRM: BIT table 'L' not found
[    4.468690] nouveau 0000:01:00.0: DRM: TMDS table version 2.0
[    4.469967] nouveau 0000:01:00.0: DRM: MM: using COPY for buffer copies
[    4.473861] [drm] Initialized nouveau 1.4.0 20120801 for 0000:01:00.0 on minor 1
[    6.610314] nouveau 0000:01:00.0: DRM: core notifier timeout
[    8.611900] nouveau 0000:01:00.0: DRM: core notifier timeout
[   10.612000] nouveau 0000:01:00.0: DRM: wndw-0: timeout
[   10.621007] nouveau 0000:01:00.0: [drm] fb0: nouveaudrmfb frame buffer device
[   11.433062] snd_hda_intel 0000:01:00.1: bound 0000:01:00.0 (ops nouveau_drm_exit [nouveau])
[   99.239911] nouveau 0000:01:00.0: Xwayland[1902]: failed to idle channel 2 [Xwayland[1902]]
```
---
### lspci -vvvv
```
0000:01:00.0 VGA compatible controller: NVIDIA Corporation GA102 [GeForce RTX 3090] (rev a1) (prog-if 00 [VGA controller])
	Subsystem: NVIDIA Corporation GA102 [GeForce RTX 3090]
	Control: I/O+ Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+
	Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast >TAbort- <TAbort- <MAbort- >SERR- <PERR- INTx-
	Latency: 0
	Interrupt: pin A routed to IRQ 108
	Region 0: Memory at 918000000 (32-bit, non-prefetchable) [size=16M]
	Region 1: Memory at 900000000 (64-bit, prefetchable) [size=256M]
	Region 3: Memory at 910000000 (64-bit, prefetchable) [size=32M]
	Region 5: I/O ports at 100000 [size=128]
	Expansion ROM at 919000000 [virtual] [disabled] [size=512K]
	Capabilities: [60] Power Management version 3
		Flags: PMEClk- DSI- D1- D2- AuxCurrent=0mA PME(D0+,D1-,D2-,D3hot+,D3cold-)
		Status: D0 NoSoftRst+ PME-Enable- DSel=0 DScale=0 PME-
	Capabilities: [68] MSI: Enable+ Count=1/1 Maskable- 64bit+
		Address: 00000000fe670040  Data: 0000
	Capabilities: [78] Express (v2) Legacy Endpoint, MSI 00
		DevCap:	MaxPayload 256 bytes, PhantFunc 0, Latency L0s unlimited, L1 <64us
			ExtTag+ AttnBtn- AttnInd- PwrInd- RBE+ FLReset+
		DevCtl:	CorrErr+ NonFatalErr+ FatalErr+ UnsupReq+
			RlxdOrd+ ExtTag+ PhantFunc- AuxPwr- NoSnoop+ FLReset-
			MaxPayload 128 bytes, MaxReadReq 512 bytes
		DevSta:	CorrErr+ NonFatalErr- FatalErr- UnsupReq+ AuxPwr- TransPend-
		LnkCap:	Port #0, Speed 16GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <1us, L1 <4us
			ClockPM+ Surprise- LLActRep- BwNot- ASPMOptComp+
		LnkCtl:	ASPM Disabled; RCB 64 bytes, Disabled- CommClk+
			ExtSynch- ClockPM- AutWidDis- BWInt- AutBWInt-
		LnkSta:	Speed 8GT/s (downgraded), Width x4 (downgraded)
			TrErr- Train- SlotClk+ DLActive- BWMgmt- ABWMgmt-
		DevCap2: Completion Timeout: Range AB, TimeoutDis+ NROPrPrP- LTR+
			 10BitTagComp+ 10BitTagReq+ OBFF Via message, ExtFmt- EETLPPrefix-
			 EmergencyPowerReduction Not Supported, EmergencyPowerReductionInit-
			 FRS-
			 AtomicOpsCap: 32bit- 64bit- 128bitCAS-
		DevCtl2: Completion Timeout: 50us to 50ms, TimeoutDis- LTR+ OBFF Disabled,
			 AtomicOpsCtl: ReqEn-
		LnkCap2: Supported Link Speeds: 2.5-16GT/s, Crosslink- Retimer+ 2Retimers+ DRS-
		LnkCtl2: Target Link Speed: 16GT/s, EnterCompliance- SpeedDis-
			 Transmit Margin: Normal Operating Range, EnterModifiedCompliance- ComplianceSOS-
			 Compliance De-emphasis: -6dB
		LnkSta2: Current De-emphasis Level: -6dB, EqualizationComplete+ EqualizationPhase1+
			 EqualizationPhase2+ EqualizationPhase3+ LinkEqualizationRequest-
			 Retimer- 2Retimers- CrosslinkRes: unsupported
	Capabilities: [b4] Vendor Specific Information: Len=14 <?>
	Capabilities: [100 v1] Virtual Channel
		Caps:	LPEVC=0 RefClk=100ns PATEntryBits=1
		Arb:	Fixed- WRR32- WRR64- WRR128-
		Ctrl:	ArbSelect=Fixed
		Status:	InProgress-
		VC0:	Caps:	PATOffset=00 MaxTimeSlots=1 RejSnoopTrans-
			Arb:	Fixed- WRR32- WRR64- WRR128- TWRR128- WRR256-
			Ctrl:	Enable+ ID=0 ArbSelect=Fixed TC/VC=ff
			Status:	NegoPending- InProgress-
	Capabilities: [250 v1] Latency Tolerance Reporting
		Max snoop latency: 0ns
		Max no snoop latency: 0ns
	Capabilities: [258 v1] L1 PM Substates
		L1SubCap: PCI-PM_L1.2+ PCI-PM_L1.1+ ASPM_L1.2+ ASPM_L1.1+ L1_PM_Substates+
			  PortCommonModeRestoreTime=255us PortTPowerOnTime=10us
		L1SubCtl1: PCI-PM_L1.2- PCI-PM_L1.1- ASPM_L1.2- ASPM_L1.1-
			   T_CommonMode=0us LTR1.2_Threshold=271360ns
		L1SubCtl2: T_PwrOn=10us
	Capabilities: [128 v1] Power Budgeting <?>
	Capabilities: [420 v2] Advanced Error Reporting
		UESta:	DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
		UEMsk:	DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
		UESvrt:	DLP+ SDES+ TLP- FCP+ CmpltTO- CmpltAbrt- UnxCmplt- RxOF+ MalfTLP+ ECRC- UnsupReq- ACSViol-
		CESta:	RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+
		CEMsk:	RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+
		AERCap:	First Error Pointer: 00, ECRCGenCap- ECRCGenEn- ECRCChkCap- ECRCChkEn-
			MultHdrRecCap- MultHdrRecEn- TLPPfxPres- HdrLogCap-
		HeaderLog: 00000000 00000000 00000000 00000000
	Capabilities: [600 v1] Vendor Specific Information: ID=0001 Rev=1 Len=024 <?>
	Capabilities: [900 v1] Secondary PCI Express
		LnkCtl3: LnkEquIntrruptEn- PerformEqu-
		LaneErrStat: 0
	Capabilities: [bb0 v1] Physical Resizable BAR
		BAR 0: current size: 16MB, supported: 16MB
		BAR 1: current size: 256MB, supported: 64MB 128MB 256MB 512MB 1GB 2GB 4GB 8GB 16GB 32GB
		BAR 3: current size: 32MB, supported: 32MB
	Capabilities: [c1c v1] Physical Layer 16.0 GT/s <?>
	Capabilities: [d00 v1] Lane Margining at the Receiver <?>
	Capabilities: [e00 v1] Data Link Feature <?>
	Kernel driver in use: nouveau
	Kernel modules: nouveau
```
---
### The PCIe patch for mainline that brought us so far
From the amazing @mariobalanica :
```
From 65b504779d8b3e4a5040cefa3a5b2c28adbaf94a Mon Sep 17 00:00:00 2001
From: =?UTF-8?q?Mario=20B=C4=83l=C4=83nic=C4=83?=
 <mariobalanica02@gmail.com>
Date: Mon, 1 Apr 2024 17:04:58 +0300
Subject: [PATCH] PCI: dw-rockchip: Disable BAR 0 and 1 of the root port

---
 drivers/pci/controller/dwc/pcie-dw-rockchip.c | 5 +++++
 1 file changed, 5 insertions(+)

diff --git a/drivers/pci/controller/dwc/pcie-dw-rockchip.c b/drivers/pci/controller/dwc/pcie-dw-rockchip.c
index d6842141d384d9..475c980772a43b 100644
--- a/drivers/pci/controller/dwc/pcie-dw-rockchip.c
+++ b/drivers/pci/controller/dwc/pcie-dw-rockchip.c
@@ -47,6 +47,7 @@
 #define PCIE_CLIENT_LTSSM_STATUS	0x300
 #define PCIE_LTSSM_ENABLE_ENHANCE	BIT(4)
 #define PCIE_LTSSM_STATUS_MASK		GENMASK(5, 0)
+#define PCIE_TYPE0_HDR_DBI2_OFFSET	0x100000
 
 struct rockchip_pcie {
 	struct dw_pcie			pci;
@@ -211,6 +212,10 @@ static int rockchip_pcie_host_init(struct dw_pcie_rp *pp)
 	rockchip_pcie_writel_apb(rockchip, PCIE_CLIENT_RC_MODE,
 				 PCIE_CLIENT_GENERAL_CONTROL);
 
+	/* Disable BAR 0 and 1 of root port to avoid wasting space */
+	dw_pcie_writel_dbi(pci, PCIE_TYPE0_HDR_DBI2_OFFSET + PCI_BASE_ADDRESS_0, 0);
+	dw_pcie_writel_dbi(pci, PCIE_TYPE0_HDR_DBI2_OFFSET + PCI_BASE_ADDRESS_1, 0);
+	
 	return 0;
 }
```
See the linked GitHub issue for more details.
