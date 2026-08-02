# Diagnostic Findings

| Observation | What it supports | What it does not prove |
|---|---|---|
| RX 580 disappeared from Device Manager, Task Manager, DDU and AMDVBFlash | Severe enumeration/initialization failure occurred | GPU was permanently dead |
| RX 580 fans still spun | Card received at least some electrical power | Power delivery was completely healthy |
| Basic Display Adapter could start the device | PCIe device could enumerate in some states | AMD software was the only cause |
| Event ID 411 / `amdwddmg` / `0xC00000E5` | AMD device startup failed | Exact physical/software root cause |
| DDU recovered earlier states | Driver state mattered | Every failure was software-only |
| PCIe contact cleaning/reseat restored detection | Physical connection quality mattered in at least one recovery | Contacts were the only cause |
| Sleep/wake repeatedly triggered failure | Power-state transition was a strong trigger | Sleep itself was the root cause |
| 60 Hz improved stability at one point | Display state affected stability margin | High refresh rate was the root cause |
| 120 Hz later worked for months | High refresh rate could be stable | Hardware was fully healthy |
| `VIDEO_MEMORY_MANAGEMENT_INTERNAL` occurred | Graphics memory/driver stack was involved | Physical VRAM failure was proven |
| RAM test found no errors | Ordinary RAM failure became less likely | RAM could never contribute |
| Buzzing increased with load/FPS | Load-dependent electrical noise was present | GPU or PSU failure was proven |
| Keyboard RGB/USB sometimes stopped | Some events may have been whole-system hangs | PSU failure was proven |
