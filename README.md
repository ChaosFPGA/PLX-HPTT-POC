# 🔧 Private Budget HPTT Build – NiteFury + PCIe PLX M.2

> ⚠️ Private project – for educational and security research purposes only.  
> ❌ Not intended for production, resale, or public deployment.

## Overview

This repository documents a **budget HPTT (Hybrid PCIe Trace Tool)** build using:

- 🎯 **NiteFury FPGA Board** (Xilinx Kintex-7)
- 🔌 **PCIe PLX M.2 Carrier Card** (for desktop interposer)
- 🧠 Custom Verilog Firmware & PCIe IP Core
- 🔍 DMA Memory Access + Stealth NVMe Spoofing

### Goals

- 💸 **Low-cost hardware-based DMA injection**
- 🛠️ Designed for testing PCIe host behavior
- 🎭 Emulates legitimate NVMe endpoint
- 🕵️ Stealth integration with real traffic (HPTT style)
- ✅ Passes DRVScan, no Device Manager errors

---

## 🧱 Hardware

| Component            | Details |
|----------------------|---------|
| FPGA Board           | [NiteFury](https://knjn.com/NiteFury.html) (Kintex-7 XC7K160T) |
| Carrier Interface    | PCIe M.2 PLX 8725/8747 adapter (x4/x8 bifurcation) |
| PSU (Optional)       | 5V DC via barrel jack or riser |
| PCIe Riser           | x4 or x8 flexible extender (optional) |
| Host PC              | Any PCIe Gen3/Gen4 compatible desktop (VMD off) |

---

## 💾 Firmware Features

- ✔️ **PCIe Endpoint IP Core** (Xilinx Vivado)
- 🧬 1:1 NVMe config space emulation (spoofed Samsung 980 Pro)
- 📦 BAR-mapped DMA engine (PCILeech-compatible)
- 🧩 Optional stealth mode (invisible namespace)
- 🛡️ MMIO activation trigger (for stealth or DMA mode)
- 🎛️ Support for speedtest and DRVScan-safe enumeration

---

## 🧰 Tools Used

- 🛠️ **Xilinx Vivado** 2020.2+
- 🧠 **Verilog HDL**
- 💉 **PCILeech** (for testing)
- 🪟 **Windows 10/11** (for host testing)
- 🧪 **DRVScan / Ekknod tools** (for stealth validation)

---
