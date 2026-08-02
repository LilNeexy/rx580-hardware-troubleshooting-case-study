# ASUS RX 580 Hardware Troubleshooting Case Study

## Overview

This repository documents my troubleshooting of an intermittent ASUS Dual Radeon RX 580 OC 8 GB graphics-card problem in a Windows 11 desktop.

The system experienced black screens, loss of DisplayPort signal, GPU detection failures, sleep/wake failures, AMD driver-start errors, and occasional system-wide freezes. I worked through the problem by separating software, driver, PCIe, power-state, and physical hardware possibilities instead of replacing parts immediately.

The RX 580 was recovered multiple times and returned to normal load operation. A single permanent root cause was not proven, so the project focuses on the diagnostic process, recoveries, and evidence collected.

## System Configuration

| Component | Configuration |
|---|---|
| CPU | AMD Ryzen 3 3200G |
| Integrated GPU | Radeon Vega 8 |
| Dedicated GPU | ASUS Dual Radeon RX 580 OC 8 GB |
| Motherboard | ASUS PRIME A320M-K |
| PSU | Corsair CV450, 450 W, 80 PLUS Bronze |
| RAM | 16 GB Corsair Vengeance DDR4-3200 |
| OS | Windows 11 |
| Display | DisplayPort |

## Main Symptoms

- Frame freeze followed by **No DisplayPort Signal**
- Failures during game launch and fullscreen transitions
- RX 580 intermittently disappearing from Windows
- Repeated failures after sleep/wake or display power-state transitions
- AMD graphics-driver startup failures
- `VIDEO_MEMORY_MANAGEMENT_INTERNAL` BSOD
- Some crashes also affected keyboard RGB/USB

## Troubleshooting Work

### 1. Driver isolation
<img width="360" height="217" alt="game-keeps-crashing-v0-gvahd88e9dff1" src="https://github.com/user-attachments/assets/9e8805c1-d617-4f34-9e52-fc4bf1b599f5" />

I used Display Driver Uninstaller (DDU) in Safe Mode to remove AMD graphics drivers and establish a clean driver baseline.

During troubleshooting, the RX 580 appeared in several different states:

- fully detected with the AMD driver
- Microsoft Basic Display Adapter
- greyed out in Device Manager
- completely absent from Device Manager, Task Manager, DDU, and AMDVBFlash

The Ryzen 3 3200G integrated Vega 8 graphics remained available during some RX 580 failure states, which helped keep the system bootable for diagnosis.

### 2. Event Viewer analysis
<img width="250" height="250" alt="Event_Viewer" src="https://github.com/user-attachments/assets/41cc40b8-d156-4f9c-b429-ea11020d2d66" />

The RX 580 was recorded as:

```text
PCI\VEN_1002&DEV_67DF&SUBSYS_05211043
```

A recurring Kernel-PnP error was:

```text
Event ID: 411
Service: amdwddmg
Problem Status: 0xC00000E5
```

The AMD Polaris driver package could be configured successfully, but the device could still fail during driver startup.

Microsoft Basic Display Adapter also successfully started the device in at least one state. This helped separate PCIe device enumeration from full AMD-driver initialization.

### 3. Physical PCIe inspection and recovery
<img width="500" height="500" alt="41lEA3EIsgL" src="https://github.com/user-attachments/assets/8f37d9bb-4325-425b-b071-62dca1d30334" />

I removed the RX 580, inspected the PCIe interface, lightly cleaned the PCIe edge contacts, and reseated the card.

After reinstalling it:

- the RX 580 became detectable again
- Windows recognized the card
- the GPU returned to gaming/load operation

This was one of the strongest hardware-side observations in the troubleshooting process because physical contact/reseating directly correlated with recovery.

### 4. Sleep and power-state testing
<img width="313" height="268" alt="sysstate" src="https://github.com/user-attachments/assets/7f683cf3-62e0-4836-8738-2e86c26d0000" />

Sleep/wake became one of the most repeatable triggers.

Typical failure sequence:

```text
System enters sleep
        ↓
System wakes
        ↓
No DisplayPort Signal
        ↓
RX 580 may fail to initialize
        ↓
Full shutdown / cold boot required
```

During testing I disabled PC sleep and display sleep and used full shutdown instead.

PCI Express Link State Power Management and BIOS power-related settings were also investigated.

### 5. Refresh-rate testing
<img width="810" height="402" alt="windows-11-advanced-display" src="https://github.com/user-attachments/assets/9ebdf8fa-d1fa-4f8f-9f92-857b7e5eb022" />

At one stage, 60 Hz provided better stability.

Later, the system operated at 120 Hz for several months with a known-good older AMD driver and update controls.

This showed that refresh rate affected the stability margin but was not enough to explain the problem on its own.

### 6. GPU load verification
<img width="1280" height="720" alt="WCCFamdadrenalin20221" src="https://github.com/user-attachments/assets/b64cd14d-9f96-4274-892e-958930fea15d" />

After recovery, the GPU was able to reach full utilization.

Recorded values were approximately:

```text
GPU utilization: 100%
GPU power:       ~142 W
GPU clock:       ~1360 MHz
VRAM clock:      ~2000 MHz
GPU temperature: ~67 °C
Fan speed:       ~1217 RPM
VRAM usage:      ~2.1 GB
```

The card also produced load-dependent electrical buzzing. The noise increased with GPU load/FPS and stopped after leaving the game. I treated this as an observation rather than proof of a failing GPU or PSU.

### 7. BSOD investigation
<img width="679" height="452" alt="images" src="https://github.com/user-attachments/assets/6307a15a-01ab-4361-bd55-0b6fcc90f85b" />

The system produced:

```text
VIDEO_MEMORY_MANAGEMENT_INTERNAL
```

I then completed:

- system RAM testing — no memory errors detected
- Windows integrity/system repair checks

This reduced the likelihood of ordinary system RAM corruption or basic Windows file corruption.

### 8. Driver/update stability testing
<img width="250" height="250" alt="images" src="https://github.com/user-attachments/assets/2d51d0cb-9fdc-4153-80d4-1a9de2e7a220" />

A known-good older AMD driver configuration was stable for several months at 120 Hz while unwanted graphics-driver update interference was blocked.

Black-screen behavior could return after allowing update-related changes.

This made Windows/driver state a significant part of the investigation, while physical PCIe, power delivery, motherboard, and GPU aging remained possible contributors.

## Result

The RX 580 was not permanently dead. It repeatedly returned to normal operation and was able to run games at full load.

The most defensible conclusion from the evidence is an intermittent RX 580 initialization / PCIe / power-state stability problem with a strong Windows 11 and AMD Polaris-driver interaction.

A final hardware root cause would require controlled A/B testing with:

1. the RX 580 in another known-good PC
2. another known-good GPU in this motherboard
3. another known-good PSU in this system

## Skills Demonstrated

- PC hardware troubleshooting
- GPU and PCIe fault isolation
- Windows Device Manager
- Windows Event Viewer / Kernel-PnP analysis
- AMD driver troubleshooting
- Display Driver Uninstaller (DDU)
- BIOS and power-state configuration
- GPU monitoring and load verification
- BSOD investigation
- physical component inspection, cleaning, and reseating
- technical documentation
- evidence-based troubleshooting

## Evidence

Supporting records are stored in [`evidence/`](evidence/).

Exact-model reference material is stored separately in [`references/`](references/) so reference photographs are not mixed with troubleshooting evidence.
