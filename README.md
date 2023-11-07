# Framework Laptop 13 AMD Ryzen 5 7640U

Device notes and configuration under Linux for the [Framework Laptop 13 AMD Ryzen 7040 Series](https://frame.work/gb/en/products/laptop-diy-13-gen-amd?tab=overview) Ryzen 5 7640U variant, DIY edition.

Everything works of the box as of Linux v6.5 with firmware version 03.03.

## Specs

- [AMD Ryzen 7640U](https://www.amd.com/en/product/13196) (Phoenix, Zen 4)
- [SK hynix Platinum P41](https://ssd.skhynix.com/platinum_p41/) 1TB SSD
- [Crucial CT2K16G56C46S5 32GB DDR5-5600 SODIMM](https://uk.crucial.com/memory/DDR5/CT2K16G56C46S5)
- [Matte display](https://frame.work/gb/en/products/display-kit?v=FRANGX0001) 13.5", 3:2, 2256x1504, 204 ppi, 400 nits
- 55Wh battery
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
  - S0ix supported, reaches S0i2.0
- consumed ~4.7% battery in ~12 hours

## Near-idle benchmarks

Testing near-idle performance via two open foot terminals, one running `powerstat` with the following flags:

```sh
powerstat -d 0 -c -H 1 480
```

... the other running a bash one-liner:

```sh
while [ 1 ]; echo 'foo'; do sleep 1; done
```

Test environment:

- Wayland only (XWayland disabled)
- 40% brightness (~200 nits)
- WiFi connected
- Bluetooth disabled
- webcam and microphone disabled (via hardware switches)
- keyboard backlight disabled
- power button LED lowest brightness

### GNOME

- Linux 6.5.9-arch2-1
- GNOME 45.1
- `gnome-shell --no-x11`
- `gdm3`
- `gnome-settings-daemon`

| State                  | C3%     | Power (W) |
| ---------------------- | ------- | --------- |
| idle (no TLP/ppd)      | 92.268% | 3.92      |
| idle (ppd balanced)    | 85.5%   | 3.86      |
| idle (ppd power saver) | 86.521% | 3.67      |

## Links

- [lhl/linuxlaptops - Framework Laptop](https://github.com/lhl/linuxlaptops/wiki/2022-Framework-Laptop-DIY-Edition-12th-Gen-Intel-Batch-1)
