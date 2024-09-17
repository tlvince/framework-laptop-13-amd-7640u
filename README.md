# Framework Laptop 13 AMD Ryzen 5 7640U

Device notes and configuration under Linux for the [Framework Laptop 13 AMD Ryzen 7040 Series](https://frame.work/gb/en/products/laptop-diy-13-gen-amd?tab=overview) Ryzen 5 7640U variant, DIY edition.

Everything works of the box as of Linux v6.5 (>=6.9 recommended) with firmware version 03.03 ([03.05](https://knowledgebase.frame.work/framework-laptop-bios-and-driver-releases-amd-ryzen-7040-series-r1rXGVL16) recommended).

## Specs

- [AMD Ryzen 7640U](https://www.amd.com/en/product/13196) (Phoenix, Zen 4)
- [SK hynix Platinum P41](https://ssd.skhynix.com/platinum_p41/) 1TB SSD
- [Crucial CT2K16G56C46S5](https://uk.crucial.com/memory/DDR5/CT2K16G56C46S5) 32GB DDR5-5600 SODIMM
- [BOE NE135FBM-N41 v8.2 (matte)](https://www.panelook.com/NE135FBM-N41_BOE_13.5_LCM_overview_44979.html) 13.5", 3:2, 2256x1504, 204 ppi, 400 nits, eDP1.4, [40 pins](https://github.com/FrameworkComputer/Framework-Laptop-13/tree/57357b797447b55ec7afcaf31b2fe731e7a48144/Display#pinout), [DC-mode dimming](https://github.com/FrameworkComputer/Framework-Laptop-13/tree/57357b797447b55ec7afcaf31b2fe731e7a48144/Mainboard#display-interface) (not PWM)
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
- [x] [Graphical corruption](https://community.frame.work/t/tracking-graphical-corruption-in-fedora-39-amd-3-03-bios/39073), [blinking/flashing white screen](https://gitlab.freedesktop.org/drm/amd/-/issues/3187): [workaround](https://gitlab.freedesktop.org/drm/amd/-/issues/3187#note_2292242): fixed in [BIOS 03.05](https://knowledgebase.frame.work/framework-laptop-bios-and-driver-releases-amd-ryzen-7040-series-r1rXGVL16) (workaround: `amdgpu.sg_display=0` or [increase iGPU RAM](https://knowledgebase.frame.work/en_us/allocate-additional-ram-to-igpu-framework-laptop-13-amd-ryzen-7040-series-BkpPUPQa) in BIOS)
- [x] [vaapi vp9 decoding glitches](https://gitlab.freedesktop.org/mesa/mesa/-/issues/8044): fixed in [linux-firmware@97733278](https://gitlab.com/kernel-firmware/linux-firmware/-/commit/977332782302476e1c863b09b840f463d0378807)
- [x] Further [AMDgpu instability and performance issues](https://community.frame.work/t/active-upstream-amdgpu-issues-affecting-ryzen-7840u-igpu-780m/41053)
- [x] MT7922 WiFi limited to 802.11n (WiFi 4) and 2.4GHz: configure the [regulatory domain](https://wiki.archlinux.org/index.php?title=Network_configuration/Wireless&oldid=791904#Respecting_the_regulatory_domain)
- [x] `ectool` [unsupported](https://community.frame.work/t/what-ec-is-used/38574/2): pending [kernel patches](https://github.com/torvalds/linux/commit/c8f460d991df93d87de01a96b783cad5a2da9616), [workaround via fork](https://community.frame.work/t/exploring-the-embedded-controller/12846/122)
- [x] system wakes if AC is connected during sleep: pending firmware update, BIOS 03.05 workaround in [Linux >=6.9](https://github.com/torvalds/linux/commit/f609e7b1b49e4d15cf107d2069673ee63860c398), BIOS 03.03 workaround in [Linux >=6.7](https://github.com/torvalds/linux/commit/a55bdad5dfd1efd4ed9ffe518897a21ca8e4e193), otherwise [workaround via udev rules](https://community.frame.work/t/tracking-framework-amd-ryzen-7040-series-lid-wakeup-behavior-feedback/39128/45)
- [ ] Bluetooth LE Audio unsupported by MT7922: see [MediaTek MT7922 controller crashes after LE Setup Isochronous Data Path](https://lore.kernel.org/linux-bluetooth/38cb99f2b63dc55763e9e2c8ae4d4cb14afc6770.camel@tlvince.com/)
- [x] [Systemd suspend-then-hibernate wakes up after 5 minutes](https://community.frame.work/t/resolved-systemd-suspend-then-hibernate-wakes-up-after-5-minutes/39392): fixed in [Linux >=6.8-rc.1](https://github.com/torvalds/linux/commit/3d762e21d56370a43478b55e604b4a83dd85aafc) via [kernel patch](https://lore.kernel.org/linux-kernel/20231106162310.85711-1-mario.limonciello@amd.com/), workaround: `rtc_cmos.use_acpi_alarm=1`
- [x] power-profiles-daemon does not set EPP: fixed in [v0.20](https://gitlab.freedesktop.org/upower/power-profiles-daemon/-/commit/0d3030b6109156a093b73c66aea2ef3118a79650#9f621eb5fd3bcb2fa5c7bd228c9b1ad42edc46c8)
- [x] HDMI and DisplayPort expansion cards do not autosuspend: fixed in [systemd v255](https://github.com/systemd/systemd/commit/9023630cb7025650aa4d01ee794b0bb68bfdf2c1) (via [systemd patch](https://github.com/systemd/systemd/pull/30131))
- [x] [ambient light sensor fails to init](https://bugzilla.kernel.org/show_bug.cgi?id=218223): fixed in [Linux >=6.7](https://github.com/torvalds/linux/commit/b9670ee2e975e1cb6751019d5dc5c193aecd8ba2)
- [x] [VP9 HW decoding issue](https://community.frame.work/t/vp9-hw-decoding-issue/41187), [high power use when decoding H264 & vp09 with vaapi (software decoding is more efficient)](https://gitlab.freedesktop.org/mesa/mesa/-/issues/10223) and [Power consumption for HW accelerated video decoding for Radeon iGPUs is simply outrageous](https://gitlab.freedesktop.org/drm/amd/-/issues/3195): fixed in [Linux >=6.10.7](https://github.com/torvalds/linux/commit/7d75ef3736a025db441be652c8cc8e84044a215f) (see [AMD VCN Dynamic Power Gating](https://www.phoronix.com/news/AMD-VCN-Dynamic-Power-Gating))
- [x] [Headset microphone not selectable as an input source](https://community.frame.work/t/resolved-headset-mic-on-amd-fw13-running-fedora-39/38847): fixed in Linux >=6.6.8 (via [kernel patch](https://github.com/tiwai/sound/commit/33038efb64f7576bac635164021f5c984d4c755f), [workaround via kernel params](https://github.com/NixOS/nixos-hardware/blob/fa194fc484fd7270ab324bb985593f71102e84d1/framework/13-inch/common/default.nix#L7-L13))
- [ ] [ucsi_acpi errors](https://community.frame.work/t/tracking-anyone-else-seeing-usbcon-pd-error-spam-in-dmesg-user-request/40072)
- [x] [USB Power Delivery issues with <60W chargers](https://community.frame.work/t/amd-framework-usb-c-charger-compatibility-issues/39323/36): fixed in [3.03b firmware update](https://community.frame.work/t/framework-13-amd-ryzen-7040-bios-3-03b-beta/46479)
- [x] [PCIe not utilising full USB 4 40Gb/s link speeds](https://community.frame.work/t/responded-framework-13-usb-4-pcie-lanes/40246/): fixed in [Linux >=6.8-rc.1](https://github.com/torvalds/linux/commit/466a7d115326ece682c2b60d1c77d1d0b9010b4f) via [kernel patch](https://gitlab.freedesktop.org/drm/amd/-/issues/2925#note_2240809)

## Enhancements

- [x] [AMD P-State Preferred Core](https://www.phoronix.com/search/AMD+P-State+Preferred+Core): added in Linux >=6.9
- [x] [Adaptive Backlight Management (ABM)](https://community.frame.work/t/adaptive-backlight-management-abm/41055): added in [Linux >=6.9](https://github.com/torvalds/linux/commit/63d0b87213a0ba241b3fcfba3fe7b0aed0cd1cc5) and [ppd >=0.20](https://gitlab.freedesktop.org/upower/power-profiles-daemon/-/releases/0.20)
- [ ] [Coreboot support](https://www.phoronix.com/news/Framework-13-AMD-Coreboot-WIP)

## BIOS

- GUI-based UEFI
- _can_ set custom charge limit
- no "legacy" S3 deep sleep option

## Software

- [getUserMedia / getDisplayMedia Test Page](https://mozilla.github.io/webrtc-landing/gum_test.html) - useful for testing webcam/screensharing
- [Hardware video acceleration](https://wiki.archlinux.org/title/Hardware_video_acceleration) VA-API support via `libva-mesa-driver`
  - Firefox requires [`media.ffmpeg.vaapi.enabled=true`](https://wiki.archlinux.org/title/firefox#Hardware_video_acceleration)

## Sleep

- S3 sleep unsupported
  - S0ix supported, reaches S0i3.0
- consumed ~4.7% battery in ~12 hours
- Debug via [drm/amd amd_s2idle.py](https://gitlab.freedesktop.org/drm/amd/-/blob/d0b86c9969b4e4d48629a574363065702bf8b14e/scripts/amd_s2idle.py)

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

| State                                                                             | C3%     | Power (W) |
| --------------------------------------------------------------------------------- | ------- | --------- |
| [idle (kernel: no TLP/ppd)](data/arch-linux-6.5.9-gnome-45-kernel-idle.txt)       | 92.268% | 3.92      |
| [idle (ppd balanced)](data/arch-linux-6.5.9-gnome-45-ppd-balanced-idle.txt)       | 85.5%   | 3.86      |
| [idle (ppd power saver)](data/arch-linux-6.5.9-gnome-45-ppd-power-saver-idle.txt) | 86.521% | 3.67      |

#### 2023-11-12

- 6.6.0 #1-NixOS
- GNOME 44.5
- MT7922

| State                                                                              | C3%     | Power (W) |
| ---------------------------------------------------------------------------------- | ------- | --------- |
| [idle (kernel: no TLP/ppd)](data/nixos-linux-6.6.0-gnome-44-kernel-idle.txt)       | 99.586% | 4.27      |
| [idle (ppd balanced)](data/nixos-linux-6.6.0-gnome-44-ppd-balanced-idle.txt)       | 98.32%  | 4.43      |
| [idle (TLP defaults)](data/nixos-linux-6.6.0-gnome-44-tlp-default-idle.txt)        | 97.899% | 3.93      |
| [idle (TLP power saver)](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-idle.txt) | 98.256% | 3.89      |

#### 2023-11-30

- 6.6.2 #1-NixOS
- GNOME 45.1
- `amdgpu.abmlevel=3`
- MT7922

| State                                                                                                                   | C3%     | Power (W) |
| ----------------------------------------------------------------------------------------------------------------------- | ------- | --------- |
| [idle (TLP power saver, balanced platform profile)](data/nixos-linux-6.6.2-gnome-45-tlp-power-saver-abmlevel3-idle.txt) | 99.451% | 3.10      |

#### 2023-12-29

- 6.6.8 #1-NixOS
- GNOME 45.2
- [patched](https://community.frame.work/t/tracking-ppd-v-tlp-for-amd-ryzen-7040/39423/137) power-profiles-daemon (multiple drivers)
- AX210

| State                                                                                                                                     | C3%     | Power (W) |
| ----------------------------------------------------------------------------------------------------------------------------------------- | ------- | --------- |
| [idle (ppd balanced)](data/nixos-linux-6.6.8-gnome-45-ppd-patched-balanced-idle.txt)                                                      | 99.338% | 4.51      |
| [idle (ppd balanced, powertop autotune)](data/nixos-linux-6.6.8-gnome-45-ppd-patched-balanced-powertop-autotune-idle.txt)                 | 99.382% | 4.35      |
| [idle (ppd power saver)](data/nixos-linux-6.6.8-gnome-45-ppd-patched-power-saver-idle.txt)                                                | 99.368% | 4.32      |
| [idle (TLP power saver, low power platform profile)](data/nixos-linux-6.6.8-gnome-45-tlp-power-saver-low-power-platform-profile-idle.txt) | 99.376% | 3.84      |
| [idle (TLP power saver, balanced platform profile)](data/nixos-linux-6.6.8-gnome-45-tlp-power-saver-balanced-platform-profile-idle.txt)   | 99.391% | 3.80      |

#### 2024-05-17

- 6.9.0 #1-NixOS
- GNOME 46.1
- AX210
- 40% brightness (~200 nits)
- ppd 0.21 (with Adaptive Backlight Management)

| State                                                                                     | C3%     | Power (W) |
| ----------------------------------------------------------------------------------------- | ------- | --------- |
| [idle (ppd power saver)](data/nixos-linux-6.9.0-gnome-46.1-ppd-0.21-power-saver-idle.txt) | 99.368% | 3.12      |
| [idle (ppd balanced)](data/nixos-linux-6.9.0-gnome-46.1-ppd-0.21-balanced-idle.txt)       | 99.313% | 3.61      |

### Video benchmarks

- One `foot` terminal running `powerstat`
- Firefox (in Wayland mode with hardware-video acceleration) playing 1080p vp9 YouTube video
- both windows evenly split (vertically)
- other baseline settings [as above](#battery)

#### 2023-11-12

- 6.6.0 #1-NixOS
- GNOME 44.5
- MT7922

| State                                                                                                                                           | C3%     | Power (W) |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------- | --------- |
| [video (kernel: no TLP/ppd)](data/nixos-linux-6.6.0-gnome-44-kernel-firefox-vp9-1080p.txt)                                                      | 85.867% | 10.25     |
| [video (ppd balanced)](data/nixos-linux-6.6.0-gnome-44-ppd-balanced-firefox-vp9-1080p.txt)                                                      | 86.230% | 10.36     |
| [video (ppd power saver)](data/nixos-linux-6.6.0-gnome-44-ppd-power-saver-firefox-vp9-1080p.txt)                                                | 85.531% | 10.50     |
| [video (TLP defaults)](data/nixos-linux-6.6.0-gnome-44-tlp-default-firefox-vp9-1080p.txt)                                                       | 86.122% | 10.20     |
| [video (TLP power saver)](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-firefox-vp9-1080p.txt)                                                | 84.004% | 8.59      |
| [video (TLP power saver, balanced platform profile)](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-no-platform-profile-firefox-vp9-1080p.txt) | 84.842% | 8.13      |

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

| State                                                                                                                                                   | C3%     | Power (W) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | --------- |
| [video (ppd balanced)](data/nixos-linux-6.6.8-gnome-45-ppd-patched-balanced-firefox-vp9-1080p.txt)                                                      | 84.705% | 9.35      |
| [video (ppd balanced, powertop autotune)](data/nixos-linux-6.6.8-gnome-45-ppd-patched-balanced-powertop-autotune-firefox-vp9-1080p.txt)                 | 84.891% | 8.83      |
| [video (ppd power saver)](data/nixos-linux-6.6.8-gnome-45-ppd-patched-power-saver-firefox-vp9-1080p.txt)                                                | 83.443% | 8.82      |
| [video (TLP power saver, low power platform profile)](data/nixos-linux-6.6.8-gnome-45-tlp-power-saver-low-power-platform-profile-firefox-vp9-1080p.txt) | 83.390% | 8.11      |
| [video (TLP power saver, balanced platform profile)](data/nixos-linux-6.6.8-gnome-45-tlp-power-saver-balanced-platform-profile-firefox-vp9-1080p.txt)   | 84.495% | 8.26      |

#### 2024-07-11

- 6.9.8 #1-NixOS
- GNOME 46.1
- ppd 0.21, balanced
- AX210
- mpv fullscreen
- Firefox (windowed) + terminal evenly split (vertically)

| State                                                                                                                                                               | C3%     | Power (W) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | --------- |
| [video](data/nixos-linux-6.9.8-gnome-46.1-ppd-0.21-amdgpu-before.txt) (mpv)                                                                                         | 88.199% | 7.43      |
| [video](data/nixos-linux-6.9.8-gnome-46.1-ppd-0.21-amdgpu-after.txt) ([patched amdgpu](https://gitlab.freedesktop.org/drm/amd/-/issues/3195#note_2485525), mpv)     | 87.410% | 6.72      |
| [video](data/nixos-linux-6.9.8-gnome-46.1-ppd-0.21-amdgpu-before.txt) (Firefox)                                                                                     | 88.199% | 7-8       |
| [video](data/nixos-linux-6.9.8-gnome-46.1-ppd-0.21-amdgpu-after.txt) ([patched amdgpu](https://gitlab.freedesktop.org/drm/amd/-/issues/3195#note_2485525), Firefox) | 87.410% | 5         |

### Browsing benchmarks

- One `foot` terminal running `powertop`
- Firefox, light websites (no videos)
- both windows evenly split (vertically)
- other baseline settings [as above](#battery)

#### 2023-11-16

- 6.6.1 #1-NixOS
- GNOME 44.5
- RZ616 WiFi adapter: 1.84-2.19W
- `brightnessctl set 0%`
- TLP power saver ([config](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-balanced-platform-profile-config.txt))

| State                   | Power (W) |
| ----------------------- | --------- |
| browsing, WiFi disabled | 4-5       |
| browsing, WiFi enabled  | 5-6       |

#### 2024-02-11

- 6.7.4 #1-NixOS
- GNOME 45.3
- AX210
- 40% brightness (~200 nits)
- [patched](https://community.frame.work/t/tracking-ppd-v-tlp-for-amd-ryzen-7040/39423/137) power-profiles-daemon (multiple drivers)

| State                                                                                              | C3%     | Power (W) |
| -------------------------------------------------------------------------------------------------- | ------- | --------- |
| [browsing (ppd power saver)](data/nixos-linux-6.7.4-gnome-45-ppd-patched-power-saver-browsing.txt) | 75.899% | 5.79      |
| [browsing (ppd balanced)](data/nixos-linux-6.7.4-gnome-45-ppd-patched-balanced-browsing.txt)       | 80.195% | 6.09      |

#### 2024-05-17

- 6.9.0 #1-NixOS
- GNOME 46.1
- AX210
- 40% brightness (~200 nits)
- ppd 0.21 (with Adaptive Backlight Management)

| State                                                                                             | C3%     | Power (W) |
| ------------------------------------------------------------------------------------------------- | ------- | --------- |
| [browsing (ppd power saver)](data/nixos-linux-6.9.0-gnome-46.1-ppd-0.21-power-saver-browsing.txt) | 89.596% | 5.49      |
| [browsing (ppd balanced)](data/nixos-linux-6.9.0-gnome-46.1-ppd-0.21-balanced-browsing.txt)       | 89.436% | 5.98      |

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
- [FrameworkComputer/EmbeddedController](https://github.com/FrameworkComputer/EmbeddedController/commits/lotus-zephyr/) (`lotus-zephyr` branch for AMD)
