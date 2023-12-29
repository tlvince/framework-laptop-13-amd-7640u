# Framework Laptop 13 AMD Ryzen 5 7640U

Device notes and configuration under Linux for the [Framework Laptop 13 AMD Ryzen 7040 Series](https://frame.work/gb/en/products/laptop-diy-13-gen-amd?tab=overview) Ryzen 5 7640U variant, DIY edition.

Everything works of the box as of Linux v6.5 with firmware version 03.03.

## Specs

- [AMD Ryzen 7640U](https://www.amd.com/en/product/13196) (Phoenix, Zen 4)
- [SK hynix Platinum P41](https://ssd.skhynix.com/platinum_p41/) 1TB SSD
- [Crucial CT2K16G56C46S5](https://uk.crucial.com/memory/DDR5/CT2K16G56C46S5) 32GB DDR5-5600 SODIMM
- [Matte display](https://frame.work/gb/en/products/display-kit?v=FRANGX0001) 13.5", 3:2, 2256x1504, 204 ppi, 400 nits
- 55Wh battery
- RZ616/MT7922 WiFi adapter
- 4x [USB-C Expansion Cards](https://frame.work/gb/en/products/usb-c-expansion-card)
- [Capella CM3218](https://github.com/FrameworkComputer/Framework-Laptop-13/blob/b3872f334810103c758e39a33572b42a5e4d67e0/Webcam/README.md#pinout) ambient light sensor

## Opinion

- [Notebookcheck review](https://www.notebookcheck.net/Framework-Laptop-13-5-Ryzen-7-7840U-review-So-much-better-than-the-Intel-version.756613.0.html)
- 204 ppi display - [not ideal for HiDPI](https://github.com/cassidyjames/dippi/blob/1f5c8a7c80b0a81fc9e9313496dd72530a599dda/dpi.md#dpi-calculationsranges), needs 1.5x fractional scaling

## Bugs

- [x] System freezes: update to [BIOS version 03.03](https://knowledgebase.frame.work/en_us/framework-laptop-bios-and-driver-releases-amd-ryzen-7040-series-r1rXGVL16)
- [x] Fingerprint reader fails to register: update [fingerprint reader firmware](https://knowledgebase.frame.work/en_us/updating-fingerprint-reader-firmware-on-linux-for-13th-gen-and-amd-ryzen-7040-series-laptops-HJrvxv_za)
- [x] Graphical glitches: [increase iGPU RAM](https://knowledgebase.frame.work/en_us/allocate-additional-ram-to-igpu-framework-laptop-13-amd-ryzen-7040-series-BkpPUPQa)
- [ ] Further [AMDgpu instability and performance issues](https://community.frame.work/t/active-upstream-amdgpu-issues-affecting-ryzen-7840u-igpu-780m/41053)
- [x] MT7922 WiFi limited to 802.11n (WiFi 4) and 2.4GHz: configure the [regulatory domain](https://wiki.archlinux.org/index.php?title=Network_configuration/Wireless&oldid=791904#Respecting_the_regulatory_domain)
- [ ] `ectool` [unsupported](https://community.frame.work/t/what-ec-is-used/38574/2): pending [kernel patches](https://lore.kernel.org/chrome-platform/20231005160701.19987-1-dustin@howett.net/), [workaround via fork](https://community.frame.work/t/exploring-the-embedded-controller/12846/122)
- [ ] system wakes if AC is connected during sleep: pending firmware update or [kernel patch](https://lore.kernel.org/platform-driver-x86/20231212045006.97581-1-mario.limonciello@amd.com/), [workaround via udev rules](https://community.frame.work/t/tracking-framework-amd-ryzen-7040-series-lid-wakeup-behavior-feedback/39128/45)
- [ ] Bluetooth LE Audio unsupported by MT7922: see [MediaTek MT7922 controller crashes after LE Setup Isochronous Data Path](https://lore.kernel.org/linux-bluetooth/38cb99f2b63dc55763e9e2c8ae4d4cb14afc6770.camel@tlvince.com/)
- [ ] [Systemd suspend-then-hibernate wakes up after 5 minutes](https://community.frame.work/t/resolved-systemd-suspend-then-hibernate-wakes-up-after-5-minutes/39392): pending [kernel patch](https://lore.kernel.org/linux-kernel/20231106162310.85711-1-mario.limonciello@amd.com/), workaround: `rtc_cmos.use_acpi_alarm=1`
- [ ] power-profiles-daemon does not set EPP: pending [patch](https://gitlab.freedesktop.org/upower/power-profiles-daemon/-/merge_requests/123), workaround: use TLP
- [ ] HDMI and DisplayPort expansion cards do not autosuspend: pending [systemd patch](https://github.com/systemd/systemd/pull/30131)
- [ ] [ambient light sensor fails to init on Linux 6.7-rc3](https://bugzilla.kernel.org/show_bug.cgi?id=218223)
- [ ] [high power use when decoding H264 & vp09 with vaapi (software decoding is more efficient)](https://gitlab.freedesktop.org/mesa/mesa/-/issues/10223)
- [ ] [Headset microphone not selectable as an input source](https://community.frame.work/t/resolved-headset-mic-on-amd-fw13-running-fedora-39/38847): pending [kernel patch](https://lore.kernel.org/alsa-devel/87h6kvwgfq.wl-tiwai@suse.de/T/#mdfbd5f9fb7901e4ea870821bc828f71c884108ae), [workaround via kernel params](https://github.com/NixOS/nixos-hardware/blob/fa194fc484fd7270ab324bb985593f71102e84d1/framework/13-inch/common/default.nix#L7-L13)
- [ ] [ucsi_acpi errors](https://community.frame.work/t/tracking-anyone-else-seeing-usbcon-pd-error-spam-in-dmesg-user-request/40072) (USB Power Delivery): pending [firmware update](https://community.frame.work/t/amd-framework-usb-c-charger-compatibility-issues/39323/36)

## Enhancements

- [ ] [AMD P-State Preferred Core](https://www.phoronix.com/news/AMD-P-State-Preferred-Core) patches
- [x] [Adaptive Backlight Management (ABM)](https://community.frame.work/t/adaptive-backlight-management-abm/41055)

## BIOS

- GUI-based UEFI
- _can_ set custom charge limit
- no "legacy" S3 deep sleep option

## Software

- [getUserMedia / getDisplayMedia Test Page](https://mozilla.github.io/webrtc-landing/gum_test.html) - useful for testing webcam/screensharing
- [Hardware video acceleration](https://wiki.archlinux.org/title/Hardware_video_acceleration) VA-API support via `libva-mesa-driver`
  - Firefox requires [`media.ffmpeg.vaapi.enable=true`](https://wiki.archlinux.org/title/firefox#Hardware_video_acceleration)

## Sleep

- S3 sleep unsupported
- consumed ~4.7% battery in ~12 hours

## Battery

Test environment

- 40% brightness (~200 nits)
- WiFi connected
- Bluetooth disabled
- webcam and microphone disabled (via hardware switches)
- keyboard backlight disabled
- power button LED lowest brightness
- ambient light sensor disabled (`hid_sensor_hub`)
- Firmware: 03.03
- GNOME with 150% scaling
- 4x USB-C expansion cards

```sh
powerstat -d 0 -c -H 1 480
```

### Idle benchmarks

One `foot` terminal running `powerstat`

#### 2023-11-07

- Linux 6.5.9-arch2-1
- GNOME 45.1
  - `gnome-shell --no-x11` (XWayland disabled)
  - `gdm3`
  - `gnome-settings-daemon`
- MT7922

| State                     | C3%     | Power (W) |
| ------------------------- | ------- | --------- |
| idle (kernel: no TLP/ppd) | 92.268% | 3.92      |
| idle (ppd balanced)       | 85.5%   | 3.86      |
| idle (ppd power saver)    | 86.521% | 3.67      |

#### 2023-11-12

- 6.6.0 #1-NixOS
- GNOME 44.5
- MT7922

| State                     | C3%     | Power (W) |
| ------------------------- | ------- | --------- |
| idle (kernel: no TLP/ppd) | 99.586% | 4.27      |
| idle (ppd balanced)       | 98.32%  | 4.43      |
| idle (TLP defaults)       | 97.899% | 3.93      |
| idle (TLP power saver)    | 98.256% | 3.89      |

#### 2023-11-30

- 6.6.2 #1-NixOS
- GNOME 45.1
- `amdgpu.abmlevel=3`
- MT7922

| State                                             | C3%     | Power (W) |
| ------------------------------------------------- | ------- | --------- |
| idle (TLP power saver, balanced platform profile) | 99.451% | 3.10      |

#### 2023-12-29

- 6.6.8 #1-NixOS
- GNOME 45.2
- [patched](https://community.frame.work/t/tracking-ppd-v-tlp-for-amd-ryzen-7040/39423/137) power-profiles-daemon (multiple drivers)
- AX210

| State                                              | C3%     | Power (W) |
| -------------------------------------------------- | ------- | --------- |
| idle (ppd balanced)                                | 99.338% | 4.51      |
| idle (ppd power saver)                             | 99.368% | 4.32      |
| idle (TLP power saver, low power platform profile) | 99.376% | 3.84      |
| idle (TLP power saver, balanced platform profile)  | 99.391% | 3.80      |

### Video benchmarks

- One `foot` terminal running `powerstat`
- Firefox (in Wayland mode with hardware-video acceleration) playing 1080p vp9 YouTube video
- both windows evenly split (vertically)
- other baseline settings [as above](#battery)

#### 2023-11-12

- 6.6.0 #1-NixOS
- GNOME 44.5
- MT7922

| State                                              | C3%     | Power (W) |
| -------------------------------------------------- | ------- | --------- |
| video (kernel: no TLP/ppd)                         | 85.867% | 10.25     |
| video (ppd balanced)                               | 86.230% | 10.36     |
| video (ppd power saver)                            | 85.531% | 10.50     |
| video (TLP defaults)                               | 86.122% | 10.20     |
| video (TLP power saver)                            | 84.004% | 8.59      |
| video (TLP power saver, balanced platform profile) | 84.842% | 8.13      |

#### 2023-11-30

- 6.6.2 #1-NixOS
- GNOME 45.1
- `amdgpu.abmlevel=3`
- MT7922

| State                                              | C3%     | Power (W) |
| -------------------------------------------------- | ------- | --------- |
| video (TLP power saver, balanced platform profile) | 83.483% | 7.99      |

#### 2023-12-29

- 6.6.8 #1-NixOS
- GNOME 45.2
- [patched](https://community.frame.work/t/tracking-ppd-v-tlp-for-amd-ryzen-7040/39423/137) power-profiles-daemon (multiple drivers)
- AX210

| State                                              | C3%     | Power (W) |
| -------------------------------------------------- | ------- | --------- |
| idle (ppd balanced)                                | 84.705% | 9.35      |
| idle (ppd power saver)                             | 83.443% | 8.82      |
| idle (TLP power saver, low power platform profile) | 83.390% | 8.11      |
| idle (TLP power saver, balanced platform profile)  | 84.495% | 8.26      |

### Browsing benchmarks

- One `foot` terminal running `powertop`
- Firefox, light websites (no videos)
- both windows evenly split (vertically)
- `brightnessctl set 0%`
- TLP power saver ([config](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-balanced-platform-profile-config.txt))
- other baseline settings [as above](#battery)

#### 2023-11-16

- 6.6.1 #1-NixOS
- GNOME 44.5
- RZ616 WiFi adapter: 1.84-2.19W

| State                   | Power (W) |
| ----------------------- | --------- |
| browsing, WiFi disabled | 4-5       |
| browsing, WiFi enabled  | 5-6       |

### Device benchmarks

- One `foot` terminal running `powertop`
- TLP power saver ([config](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-balanced-platform-profile-config.txt))
- other baseline settings [as above](#battery)

#### 2023-11-16

- 6.6.1 #1-NixOS

| Device  | State                     | Power (W) |
| ------- | ------------------------- | --------- |
| Display | 0% brightness             | 3.14      |
| Display | 40% brightness            | 3.52      |
| MT7922  | Idle (0.0 pkts/s)         | 1.67      |
| MT7922  | YouTube 4K (3461 pkt/s)   | 4.05      |
| MT7922  | YouTube 4K (4690 pkt/s)   | 4.98      |
| MT7922  | Speed test (16557 pkts/s) | 9.16      |

#### 2023-11-25

- 6.6.2 #1-NixOS

| Device  | State                    | Power (W) |
| ------- | ------------------------ | --------- |
| Display | 40% brightness           | 3.52      |
| AX210   | Idle (0.0 pkts/s)        | 1.55      |
| AX210   | YouTube 4K (3601 pkts/s) | 5.06      |
| AX210   | Download (10400 pkt/s)   | 6.40      |
| AX210   | Speed test (13539 pkt/s) | 7.14      |

## Kernel defaults

- CPU: `/sys/devices/system/cpu/cpu*/cpufreq/*`
- Platform: `/sys/firmware/acpi/platform_profile`

[ppd](https://gitlab.freedesktop.org/hadess/power-profiles-daemon/-/blob/1b18784f12e863b9c2aae5c0ff6aa08379cb896e/README.md#operations-on-amd-based-machines) (unmaintained) only sets `platform_power` and does not change the EPP, see [Sacaling governor and EPP is not set if amd_pstate=active is activated](https://gitlab.freedesktop.org/hadess/power-profiles-daemon/-/issues/124).

Addtionally, for Energy Performance Preference (EPP) to work the `powersave` governer must be used:

> If the AMD P-State scaling driver is used in `active` mode, the P-State scaling governor will be changed to `powersave` as it is the only P-State scaling governor that allows for the "Energy vs Performance Hints" to be taken into consideration.

### v6.6.0

| Param                           | Value                     |
| ------------------------------- | ------------------------- |
| `scaling_governer`              | `performance`             |
| `scaling_driver`                | `amd-pstate-epp` (active) |
| `energy_performance_preference` | `performance`             |
| `platform_profile`              | `balanced`                |

## Links

- [tlvince/nixos-config](https://github.com/tlvince/nixos-config)
- [lhl/linuxlaptops - Framework Laptop](https://github.com/lhl/linuxlaptops/wiki/2022-Framework-Laptop-DIY-Edition-12th-Gen-Intel-Batch-1)
- [A Developer's Experience: Framework 13 AMD 7040 Series](https://zach.codes/p/a-developers-review-framework-13)
