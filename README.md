# 🔧 True Hybrid PCIe Trace Tool (HPTT) – NiteFury + PLX M.2

> ⚠️ Private project – for **educational and security research** only  
> ❌ Not intended for production, resale, or malicious use  
> 🧠 Purpose: Hardware-based analysis of PCIe host behavior via **stealth DMA injection**

---

## 📘 Overview

This repository documents a **1:1 functional HPTT (Hybrid PCIe Trace Tool)** implementation using:

- 🎯 **NiteFury FPGA Board** (Xilinx Kintex-7 XC7K160T)
- 🔌 **PCIe PLX M.2 Carrier Card** (PLX8725/8747 switch)
- 🟢 **Real PCIe Device** (e.g. AX210 Wi-Fi 6E)
- 🧠 Custom Verilog firmware with DLL-level PCIe traffic injection
- 🔍 Hidden DMA channel merged with real device’s traffic

The system leverages **a real PCIe device as a front** while the FPGA injects memory access traffic, maintaining total stealth. The host OS only detects the real device, and DMA is dynamically activated via MMIO or passive traffic.

---

## 🎯 Goals

- 🛠️ Build a **true hardware-based HPTT clone**
- 🕵️ Inject DMA traffic without spoofed devices or config space exposure
- 🧩 Use real PCIe device for full operational cover (AX210/USB/NVMe)
- 💉 Enable PCILeech-compatible memory reads/writes invisibly
- 🛡️ Pass DRVScan and avoid detection by Device Manager or anti-cheat systems
- 🧬 Maintain 100% device functionality (e.g., AX210 stays connected and working)

---

## 🧱 Hardware

| Component              | Details |
|------------------------|---------|
| **FPGA Board**         | [NiteFury](https://knjn.com/NiteFury.html) – Kintex-7 XC7K160T |
| **Real PCIe Device**   | AX210 Wi-Fi 6E, ASMedia USB controller, or any PCIe x1 endpoint |
| **Carrier Interface**  | PLX8725/8747 M.2 PCIe switch adapter (bifurcation-capable) |
| **Power Supply**       | 5V DC barrel jack or M.2 riser power |
| **PCIe Riser (Optional)** | x4 or x8 extender to desktop slot |
| **Host PC**            | PCIe Gen3/Gen4-capable desktop (BIOS: VMD **disabled**) |

---

## 🔧 Architecture & Functionality

### 🎭 Real Device Enumeration
- The system only detects the **real PCIe device** (e.g., AX210)
- The FPGA is connected through the PLX switch but **does not enumerate**
- The real device handles all BARs, interrupts, drivers, and config space

### 🧠 Hidden DMA Injection
- FPGA uses **GTP or PIPE interface** to monitor traffic from the PLX switch
- A custom Verilog **Data Link Layer (DLL) engine** merges PCILeech-style TLPs into the stream
- DMA read/write responses are extracted and buffered without disrupting the real device
- Triggered by:
  - MMIO write to real device’s BAR (from Activator.exe), or
  - Passive signal handshake

### 🛡️ Total Stealth
- No PCIe enumeration of the FPGA itself
- No spoofed or emulated endpoints
- No new device in Device Manager
- No warnings, no driver conflicts
- DRVScan shows only the original PCIe device

---

## 💾 Firmware Features

| Feature | Description |
|--------|-------------|
| ✅ DLL-layer TLP injector | PCIe DMA requests inserted at low level |
| ✅ TLP sniffer and response parser | Reads host memory from injected PCILeech packets |
| ✅ MMIO trigger | DMA activated only after specific value written to AX210 BAR |
| ✅ Passive fallback | Auto-activation via handshake on known traffic pattern |
| ✅ Dynamic DMA mode | On/off toggle without reboot |
| ✅ Traffic isolation | FPGA keeps DMA traffic from interrupting device function |
| ✅ PCILeech compatible | Standard memory read/write tools work via backdoor |

---

## 🛠 Tools Used

| Tool | Usage |
|------|-------|
| **Xilinx Vivado 2020.2+** | Firmware development for NiteFury |
| **Verilog HDL** | Custom PCIe DLL and DMA engine logic |
| **PCILeech** | Memory access testing |
| **DRVScan / Ekknod tools** | Stealth validation |
| **Activator.exe** | MMIO trigger loader (optional) |
| **Windows 10/11** | Target OS for behavior testing |

---

## ⚙️ Setup Instructions

### 1. Assemble Hardware
- Insert **NiteFury** and **real PCIe device** into PLX M.2 switch adapter
- Install adapter into host PCIe x4/x8 slot
- Power FPGA via 5V barrel jack or M.2 riser

### 2. Load FPGA Firmware
- Build Vivado project
- Flash `.bit` file to NiteFury using JTAG
- Ensure FPGA does **not respond to PCIe enumeration**

### 3. Boot Host System
- Confirm only **real PCIe device** is visible in Device Manager
- Use Wi-Fi, USB, or NVMe normally

### 4. Activate DMA Mode
- Run `Activator.exe` to write a trigger value into real device's MMIO BAR
- FPGA detects trigger and begins injecting PCILeech TLPs
- Run PCILeech to read/write host memory via concealed channel

---

## ✅ Verification Checklist

| Test | Expected Result |
|------|------------------|
| Device Manager | Only real PCIe device appears |
| DRVScan | Passes (clean, valid vendor ID + BARs) |
| Wi-Fi/USB/NVMe | Fully functional during DMA injection |
| PCILeech | Can read/write memory post-trigger |
| Host behavior | No crashes, no PCIe errors |

---

## 🔐 Notes

- This is **not a spoof**: a real PCIe device is used as cover
- FPGA is fully hidden from the host
- Device model used must match firmware’s merge profile (e.g., AX210)
- PLX adapter must support **multi-function endpoint passthrough**
- Timing closure and signal integrity matter: use proper constraints

---

## 📁 Future Additions

- 🔜 Support for more PCIe device profiles (USB controller, NVMe SSD)
- 🔜 Automatic traffic-based activation
- 🔜 Internal memory logging for forensics

---

## 📞 Contact

For firmware requests, preconfigured Vivado project files, or help building your own Activator loader, contact the maintainer privately for educational access.

---
