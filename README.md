# PiMera
Like chimera. Get it?

PiMera allows your Pi Zero W to do a variety of handy things, with a shiny UI.
It can act as:

* CD drive or flash drive:
  Present any image you choose. No more having to `dd` a new iso to your flash
  drive every other day. Read-only or read-write for flash drive images.

Future ideas:
* USB network card:
  SSH to the Pi, or use it as a mediocre wifi adapter!
* Serial interface:
  For easy management

## Hardware
* A Linux board that supports USB gadgets and a compatible display
    * [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w/)
      [with headers](https://www.adafruit.com/product/3708),
      ~US$11-14
    * [Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/)
      with [headers](https://www.adafruit.com/product/2822),
      ~US$6-9 but more setup effort
    * NOT supported: Raspberry Pi Model A/B _do not_ support the gadget system
* A MicroUSB cable or a [USB A stem](https://www.adafruit.com/product/3945)
* Interface
    * [Waveshare 1.3in OLED Hat](https://www.waveshare.com/wiki/1.3inch_OLED_HAT),
      about US$17
    * Similar displays may be easy to support, pull requests welcome!
* A MicroSD card of at least 8GB. 2-4GB will be used for the system. A higher
  quality card will be a better experience, both in speed and longevity.

## Setup
1. Download [Raspberry Pi OS](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
   **Lite** (unless you're doing other stuff) and flash it to the SD card normally
2. Either create a new partition starting at ~4GiB (`sudo parted
  /dev/sdX mkpart p 4GiB 100%`) or flash tools/partition-table.img
3. In the boot partition, in config.txt, change:
    * Find `#dtparam=spi=on` and remove the `#`
    * Add a line `dtoverlay=dwc2` to the end of the file
4. Optionally, for ease of access:
    * [Enable SSHd](https://www.raspberrypi.org/documentation/remote-access/ssh/):
      Create an empty file named `ssh` on the boot partition
    * [Configure WiFi](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md):
      ```sh
      wpa_passphrase HomeWiFi | \
          sudo tee --append rootfs/etc/wpa_supplicant/wpa_supplicant.conf
      ```
5. Boot the Pi and install the dependencies:
    ```sh
    sudo apt update && sudo apt install -y python3-spidev python3-rpi.gpio \
        python3-pil
    ```
6. Other things

## Use
Remember to connect the Pi using the "USB" (data) port, _not_ the "PWR"
(power only) port!

With the Waveshare OLED Hat, up and down change menu items, left goes back,
right selects.

When using a writable mode, please be sure to unmount the filesystem before
unplugging it to minimize chances of corruption.

The system partition is also configured to be mounted read-only to minimize
chances of corruption. To modify the system after installation, first remount
the system read-write:
```sh
sudo mount -o remount,rw /
sudo mount -o remount,rw /boot
```
