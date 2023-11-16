# Framework Laptop 13 AMD Ryzen 5 7640U

Device notes and configuration under Linux for the [Framework Laptop 13 AMD Ryzen 7040 Series](https://frame.work/gb/en/products/laptop-diy-13-gen-amd?tab=overview) Ryzen 5 7640U variant, DIY edition.

Everything works of the box as of Linux v6.5 with firmware version 03.03.

## Specs

- [AMD Ryzen 7640U](https://www.amd.com/en/product/13196) (Phoenix, Zen 4)
- [SK hynix Platinum P41](https://ssd.skhynix.com/platinum_p41/) 1TB SSD
- [Crucial CT2K16G56C46S5](https://uk.crucial.com/memory/DDR5/CT2K16G56C46S5) 32GB DDR5-5600 SODIMM
- [Matte display](https://frame.work/gb/en/products/display-kit?v=FRANGX0001) 13.5", 3:2, 2256x1504, 204 ppi, 400 nits
- 55Wh battery
- RZ616 WiFi adapter
- 4x [USB-C Expansion Cards](https://frame.work/gb/en/products/usb-c-expansion-card)

## Opinion

- [Notebookcheck review](https://www.notebookcheck.net/Framework-Laptop-13-5-Ryzen-7-7840U-review-So-much-better-than-the-Intel-version.756613.0.html)
- 204 ppi display - [not ideal for HiDPI](https://github.com/cassidyjames/dippi/blob/1f5c8a7c80b0a81fc9e9313496dd72530a599dda/dpi.md#dpi-calculationsranges), needs 1.5x fractional scaling

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

| State                      | C3%     | Power (W) |
| ----------------------     | ------- | --------- |
| idle (kernel: no TLP/ppd)  | 92.268% | 3.92      |
| idle (ppd balanced)        | 85.5%   | 3.86      |
| idle (ppd power saver)     | 86.521% | 3.67      |

#### 2023-11-12

- 6.6.0 #1-NixOS
- GNOME 44.5

| State                      | C3%     | Power (W) |
| ----------------------     | ------- | --------- |
| idle (kernel: no TLP/ppd)  | 99.586% | 4.27      |
| idle (ppd balanced)        | 98.32%  | 4.43      |
| idle (TLP defaults)        | 97.899% | 3.93      |
| idle (TLP power saver)     | 98.256% | 3.89      |

### Video benchmarks

- One `foot` terminal running `powerstat`
- Firefox (in Wayland mode with hardware-video acceleration) playing 1080p vp9 YouTube video
- both windows evenly split (vertically)
- other baseline settings [as above](#battery)

#### 2023-11-12

- 6.6.0 #1-NixOS
- GNOME 44.5

| State                                                  | C3%     | Power (W) |
| ----------------------                                 | ------- | --------- |
| video (kernel: no TLP/ppd)                             | 85.867% | 10.25     |
| video (ppd balanced)                                   | 86.230% | 10.36     |
| video (ppd power saver)                                | 85.531% | 10.50     |
| video (TLP defaults)                                   | 86.122% | 10.20     |
| video (TLP power saver)                                | 84.004% | 8.59      |
| video (TLP power saver, balanced platform profile)     | 84.842% | 8.13      |

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
| ---------               | --------- |
| browsing, WiFi disabled | 4-5       |
| browsing, WiFi enabled  | 5-6       |

### Device benchmarks

- One `foot` terminal running `powertop`
- TLP power saver ([config](data/nixos-linux-6.6.0-gnome-44-tlp-power-saver-balanced-platform-profile-config.txt))
- other baseline settings [as above](#battery)

#### 2023-11-16

- 6.6.1 #1-NixOS

| Device  | State                     | Power (W) |
| ------- | ---------                 | --------- |
| Display | 0% brightness             | 3.14      |
| Display | 40% brightness            | 3.52      |
| RZ616   | Idle (0.0 pkts/s)         | 1.67      |
| RZ616   | Speed test (16557 pkts/s) | 9.16      |

## Kernel defaults

- CPU: `/sys/devices/system/cpu/cpu*/cpufreq/*`
- Platform: `/sys/firmware/acpi/platform_profile`

[ppd](https://gitlab.freedesktop.org/hadess/power-profiles-daemon/-/blob/1b18784f12e863b9c2aae5c0ff6aa08379cb896e/README.md#operations-on-amd-based-machines) (unmaintained) only sets `platform_power` and does not change the EPP, see [Sacaling governor and EPP is not set if amd_pstate=active is activated](https://gitlab.freedesktop.org/hadess/power-profiles-daemon/-/issues/124).

Addtionally, for Energy Performance Preference (EPP) to work the `powersave` governer must be used:

> If the AMD P-State scaling driver is used in `active` mode, the P-State scaling governor will be changed to `powersave` as it is the only P-State scaling governor that allows for the "Energy vs Performance Hints" to be taken into consideration.

### v6.6.0

| Param                           | Value
| -----                           | -----
| `scaling_governer`              | `performance`
| `scaling_driver`                | `amd-pstate-epp` (active)
| `energy_performance_preference` | `performance`
| `platform_profile`              | `balanced`

## Links

- [tlvince/nixos-config](https://github.com/tlvince/nixos-config)
- [lhl/linuxlaptops - Framework Laptop](https://github.com/lhl/linuxlaptops/wiki/2022-Framework-Laptop-DIY-Edition-12th-Gen-Intel-Batch-1)
