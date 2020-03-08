# tools

Assorted scripts for Doing Things®.

This README should document how to use each script, unless the script is large or notable enough to warrant its own documentation.

## Flashing a V4 power board

* Install `stm32flash`.
* Build the firmware from the `power-v4-fw` repository, producing the `pbv4.bin` firmware image.
* Connect the board to the computer via a USB-to-serial cable plugged into the 6-pin header in the corner, following [this pinout diagram](doc/pbv4-pinout.jpg).
* Ensure the external power switch pins are shorted.
* Power the board through its 12V terminals.
* Run `bin/pbv4-flash-fw` as described below.
* Power-cycle the board to boot the new firmware.

### `pbv4-flash-fw`

```bash
bin/pbv4-flash-fw /path/to/usb/serial/device /path/to/pbv4.bin SERIAL_NUM
```

For example:

```bash
bin/pbv4-flash-fw /dev/ttyUSB0 ./pbv4.bin SRO-AA2-3EB
```

## Flashing a V4 motor board

This is a two-step process - one to configure the builtin USB interface IC and another to write the firmware to the microcontroller.

* Install `stm32flash`.
* Install `libftdi1`, which provides the `ftdi_eeprom` tool.
* Build the firmware from the `motor-v4-fw` repository, producing the `mcv4.bin` firmware image.
* Connect the board to the computer over USB.
  * It has been observed that sometimes it becomes impossible to download firmware to the board after having flashed the USB chip EEPROM. I'm not sure why this happens, but it can be worked around by flashing the firmware over UART instead. Connect a USB-to-serial cable to the 4-pin serial header on the board (TODO: pinout?) *instead of* connecting the board over USB. Connecting a USB cable and serial cable at the same time can damage the board and/or your computer, as the two cables will try to drive the board's power supply rail to different voltages!
* Power the board through its 12V terminals.
* Press the button on the side of the board.
* Run `bin/mcv4-flash-fw` as described below.
* Run `bin/mcv4-flash-usbeeprom` as described below.
* Reconnect both the USB and 12V connections to boot the new firmware and refresh USB metadata on the computer.

### `mcv4-flash-fw`

```bash
bin/mcv4-flash-fw /path/to/usb/serial/device /path/to/mcv4.bin
```

For example:

```bash
bin/mcv4-flash-fw /dev/ttyUSB0 ./mcv4.bin
```

### `mcv4-flash-usbeeprom`


```bash
bin/mcv4-flash-usbeeprom SERIAL_NUM
```

For example:

```bash
bin/mcv4-flash-usbeeprom SRO-AA2-3EB
```

## Flashing a V4 servo board

* Install `stm32flash` and `pyserial`.
* Build the firmware from the `servo-v4-fw` repository, producing the `sbv4.bin` firmware image.
* Connect the board to the computer via a USB-to-serial cable plugged into the UART port, following [this pinout diagram](doc/sbv4-pinout.jpg).
* Power the board through the USB port.
* Run `bin/sbv4-flash-fw` as described below, pressing the button on the side of the board when instructed.
* Power-cycle the board to boot the new firmware.

### `sbv4-flash-fw`

```bash
bin/sbv4-flash-fw /path/to/usb/serial/device /path/to/sbv4.bin SERIAL_NUM
```

For example:

```bash
bin/sbv4-flash-fw /dev/ttyUSB0 ./sbv4.bin SRO-AA2-3EB
```
