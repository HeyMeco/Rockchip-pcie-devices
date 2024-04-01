---
layout: card
title: "Nvidia GeForce RTX 3090"
functionality: "Detected | HW Limit see Issue"
kernel: "6.1.62"
driver_required: "Yes"
github_issue: "[Issue #1](https://github.com/HeyMeco/Rockchip-pcie-devices/issues/1)"
buy_link: "ToDo"
---
The RTX 3090 is supported by both Nvidia's proprietary driver and the open source Nouveau driver in the Linux Kernel.

It requires a 16x slot. You should also supply _at least_ 5A of 12V power to whatever PCIe riser you use to connect it, in addition to external power to the card's PCIe power inputs.

There are two ways to try installing the driver:

### Proprietary Nvidia driver

After flashing 64-bit Ubuntu Desktop with Pyavitz Debian Image Builder, run upgrades , so the Nvidia driver will compile:

```
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

After rebooting, you need to exit the graphical user interface before installing the Nvidia drivers.

Now, download Nvidia's latest [AARCH64 Driver for ARM 64-bit processors](https://www.nvidia.com/en-us/drivers/unix/linux-aarch64-archive/), and run it with `sudo`:

```
sudo ./NVIDIA-Linux-aarch64-535.129.03.run
```

After a reboot, the card would not initialize and fail with the message: 
```
[Nov18 10:51] nvidia: loading out-of-tree module taints kernel.
[  +0.000040] nvidia: module license 'NVIDIA' taints kernel.
[  +0.000003] Disabling lock debugging due to kernel taint
[  +0.046946] nvidia: module verification failed: signature and/or required key missing - tainting kernel
[  +0.059404] nvidia-nvlink: Nvlink Core is being initialized, major device number 237
[  +0.000050] NVRM: CPUID: unknown implementer/part 0x41/0xd05.
[  +0.006673] The NVIDIA GPU driver for AArch64 has not been qualified on this CPU
              and therefore it is not recommended or intended for use in any production
              environment.
[  +0.001653] nvidia 0002:21:00.0: enabling device (0000 -> 0003)
[  +0.000060] NVRM: This PCI I/O region assigned to your NVIDIA device is invalid:
              NVRM: BAR1 is 0M @ 0x0 (PCI:0002:21:00.0)
[  +0.000008] NVRM: This PCI I/O region assigned to your NVIDIA device is invalid:
              NVRM: BAR2 is 0M @ 0x0 (PCI:0002:21:00.0)
[  +0.000005] NVRM: This PCI I/O region assigned to your NVIDIA device is invalid:
              NVRM: BAR3 is 0M @ 0x0 (PCI:0002:21:00.0)
[  +0.000005] NVRM: This PCI I/O region assigned to your NVIDIA device is invalid:
              NVRM: BAR4 is 0M @ 0x0 (PCI:0002:21:00.0)
[  +0.000029] nvidia 0002:21:00.0: vgaarb: changed VGA decodes: olddecodes=io+mem,decodes=none:owns=none
[  +0.000033] NVRM: The NVIDIA GPU 0002:21:00.0
              NVRM: (PCI ID: 10de:2204) installed in this system has
              NVRM: fallen off the bus and is not responding to commands.
[  +0.000046] nvidia: probe of 0002:21:00.0 failed with error -1
[  +0.000074] NVRM: The NVIDIA probe routine failed for 1 device(s).
[  +0.000005] NVRM: None of the NVIDIA devices were initialized.
[  +0.000539] nvidia-nvlink: Unregistered Nvlink Core, major device number 237
[  +0.133392] zram0: detected capacity change from 0 to 2097152
[  +0.064756] nvidia-nvlink: Nvlink Core is being initialized, major device number 237.
```

Lspci -vvvv shows:
```
0002:21:00.0 VGA compatible controller: NVIDIA Corporation GA102 [GeForce RTX 3090] (rev a1) (prog-if 00 [VGA controller])
	Subsystem: NVIDIA Corporation GA102 [GeForce RTX 3090]
	Control: I/O+ Mem+ BusMaster- SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx-
	Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast >TAbort- <TAbort- <MAbort- >SERR- <PERR- INTx-
	Interrupt: pin A routed to IRQ 40
	Region 0: Memory at f1000000 (32-bit, non-prefetchable) [virtual] [size=16M]
	Region 1: Memory at <unassigned> (64-bit, prefetchable) [virtual]
	Region 3: Memory at <unassigned> (64-bit, prefetchable) [virtual]
	Region 5: I/O ports at 200000 [virtual] [size=128]
	Expansion ROM at f0800000 [virtual] [disabled] [size=512K]
	Capabilities: [60] Power Management version 3
		Flags: PMEClk- DSI- D1- D2- AuxCurrent=0mA PME(D0+,D1-,D2-,D3hot+,D3cold-)
		Status: D0 NoSoftRst+ PME-Enable- DSel=0 DScale=0 PME-
	Capabilities: [68] MSI: Enable- Count=1/1 Maskable- 64bit+
		Address: 0000000000000000  Data: 0000
	Capabilities: [78] Express (v2) Legacy Endpoint, MSI 00
		DevCap:	MaxPayload 256 bytes, PhantFunc 0, Latency L0s unlimited, L1 <64us
			ExtTag+ AttnBtn- AttnInd- PwrInd- RBE+ FLReset+
		DevCtl:	CorrErr- NonFatalErr- FatalErr- UnsupReq-
			RlxdOrd+ ExtTag+ PhantFunc- AuxPwr- NoSnoop+ FLReset-
			MaxPayload 128 bytes, MaxReadReq 512 bytes
		DevSta:	CorrErr+ NonFatalErr- FatalErr- UnsupReq+ AuxPwr- TransPend-
		LnkCap:	Port #0, Speed 16GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <512ns, L1 <4us
			ClockPM+ Surprise- LLActRep- BwNot- ASPMOptComp+
		LnkCtl:	ASPM Disabled; RCB 64 bytes, Disabled- CommClk-
			ExtSynch- ClockPM- AutWidDis- BWInt- AutBWInt-
		LnkSta:	Speed 2.5GT/s (downgraded), Width x1 (downgraded)
			TrErr- Train- SlotClk+ DLActive- BWMgmt- ABWMgmt-
		DevCap2: Completion Timeout: Range AB, TimeoutDis+ NROPrPrP- LTR+
			 10BitTagComp+ 10BitTagReq+ OBFF Via message, ExtFmt- EETLPPrefix-
			 EmergencyPowerReduction Not Supported, EmergencyPowerReductionInit-
			 FRS-
			 AtomicOpsCap: 32bit- 64bit- 128bitCAS-
		DevCtl2: Completion Timeout: 50us to 50ms, TimeoutDis- LTR- OBFF Disabled,
			 AtomicOpsCtl: ReqEn-
		LnkCap2: Supported Link Speeds: 2.5-16GT/s, Crosslink- Retimer+ 2Retimers+ DRS-
		LnkCtl2: Target Link Speed: 16GT/s, EnterCompliance- SpeedDis-
			 Transmit Margin: Normal Operating Range, EnterModifiedCompliance- ComplianceSOS-
			 Compliance De-emphasis: -6dB
		LnkSta2: Current De-emphasis Level: -6dB, EqualizationComplete- EqualizationPhase1-
			 EqualizationPhase2- EqualizationPhase3- LinkEqualizationRequest-
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
			   T_CommonMode=0us LTR1.2_Threshold=0ns
		L1SubCtl2: T_PwrOn=10us
	Capabilities: [128 v1] Power Budgeting <?>
	Capabilities: [420 v2] Advanced Error Reporting
		UESta:	DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
		UEMsk:	DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
		UESvrt:	DLP+ SDES+ TLP- FCP+ CmpltTO- CmpltAbrt- UnxCmplt- RxOF+ MalfTLP+ ECRC- UnsupReq- ACSViol-
		CESta:	RxErr+ BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+
		CEMsk:	RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+
		AERCap:	First Error Pointer: 00, ECRCGenCap- ECRCGenEn- ECRCChkCap- ECRCChkEn-
			MultHdrRecCap- MultHdrRecEn- TLPPfxPres- HdrLogCap-
		HeaderLog: 00000000 00000000 00000000 00000000
	Capabilities: [600 v1] Vendor Specific Information: ID=0001 Rev=1 Len=024 <?>
	Capabilities: [900 v1] Secondary PCI Express
		LnkCtl3: LnkEquIntrruptEn- PerformEqu-
		LaneErrStat: LaneErr at lane: 0
	Capabilities: [bb0 v1] Physical Resizable BAR
		BAR 0: current size: 16MB, supported: 16MB
		BAR 1: current size: 256MB, supported: 64MB 128MB 256MB 512MB 1GB 2GB 4GB 8GB 16GB 32GB
		BAR 3: current size: 32MB, supported: 32MB
	Capabilities: [c1c v1] Physical Layer 16.0 GT/s <?>
	Capabilities: [d00 v1] Lane Margining at the Receiver <?>
	Capabilities: [e00 v1] Data Link Feature <?>
	Kernel modules: nvidia_drm, nvidia
```
See the linked GitHub issue for more details.
