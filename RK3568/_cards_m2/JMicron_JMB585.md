---
layout: card
title: "JMB585 PCIe Gen3 SATA Controller"
functionality: "Full"
driver_required: "Mainline AHCI"
kernel: "vendor 5.10.160"
github_issue: "Not needed"

---
The card shows up as `SATA controller: JMicron Technology Corp. JMB58x AHCI SATA controller (prog-if 01 [AHCI 1.0])` using `lspci`, and I successfully tested two drives connected to it (I didn't have any more to test, otherwise I would've plugged in more!).

The card was able to saturate the 2.5GBe Port for SMB File Shares.

Any kind of RAID setup will require software RAID, which relies on the slow(ish) A55 Cores. So parity calculations can make RAID writes fairly slow, even on fast disks.

I successfully tested this card with Open Media Vault and had no issues.