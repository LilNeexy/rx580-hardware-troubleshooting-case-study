# Troubleshooting Timeline

## Initial black-screen failures

The first recurring symptom was a frozen frame followed by loss of DisplayPort signal. The problem became repeatable during game launches, restarts, and later sleep/wake transitions.

## AMD driver failure

AMD Bug Report Tool appeared after a restart and reported a graphics/display-driver failure.

## System instability

The PC later froze outside gaming. A BIOS reset temporarily restored normal behavior.

## RX 580 disappeared from Windows

During a severe failure state:

- Device Manager did not detect the RX 580
- Task Manager did not detect it
- DDU did not detect it
- AMDVBFlash did not detect it
- Vega 8 remained available
- RX 580 fans still spun

## VBIOS investigation

GPU-Z information appeared consistent with an ASUS RX 580 8 GB. No obvious mining-modified VBIOS evidence was found. No successful VBIOS flash was performed.

## PCIe cleaning/reseat recovery

I removed the RX 580, lightly cleaned the PCIe edge contacts, reseated the card, and restarted the system.

The RX 580 reappeared and returned to load operation.

## Load-dependent buzzing

Electrical buzzing became obvious under GPU load and increased with FPS. Approximate recorded load was 100% GPU utilization, ~142 W, ~1360 MHz GPU clock, ~2000 MHz VRAM clock, and ~67 °C.

## Sleep/wake failures

Sleep and display sleep became strong triggers. A full power cycle was often required after a failed wake.

## Refresh-rate testing

60 Hz improved stability at one stage. Later, 120 Hz operated normally for several months under a known-good driver/update configuration.

## Event Viewer evidence

Kernel-PnP repeatedly recorded the RX 580 as:

```text
PCI\VEN_1002&DEV_67DF&SUBSYS_05211043
```

AMD device start failures included:

```text
Event ID: 411
Service: amdwddmg
Problem Status: 0xC00000E5
```

## BSOD

A `VIDEO_MEMORY_MANAGEMENT_INTERNAL` BSOD occurred.

System RAM testing reported no errors, and Windows integrity/repair checks were completed.

## Long stable period

A known-good older AMD driver, 120 Hz, and controls preventing unwanted update changes produced several months of stable operation.

## Later system-level crash clue

A later Valorant launch caused a frame freeze and black-screen event. Keyboard RGB/USB also appeared to stop, indicating that at least some incidents may have involved a broader system or bus-level hang rather than only DisplayPort signal loss.
