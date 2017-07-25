# tools

Assorted scripts for Doing Things®.

This README should document how to use each script, unless the script is large or notable enough to warrant its own documentation.

## Flashing a V4 power board

Prerequisites:

* Install `stm32flash`.
* Build the firmware from the `power-v4-fw` repository, producing the `pbv4.bin` firmware image.
* Connect the board to the computer via a USB-to-serial cable plugged into the 6-pin header in the corner. (TODO: pinout?)
* Power the board through its 12V terminals.

```bash
bin/pbv4-flash-fw /path/to/usb/serial/device /path/to/pbv4.bin SERIAL_NUM
```

For example:

```bash
bin/pbv4-flash-fw /dev/ttyUSB0 ./pbv4.bin 12-34-56-78
```

## Flashing a V4 motor board

This is a two-step process - one to configure the builtin USB interface IC and another to write the firmware to the microcontroller.

### Flashing the microcontroller firmware

Prerequisites:

* Install `stm32flash`.
* Build the firmware from the `motor-v4-fw` repository, producing the `mcv4.bin` firmware image.
* Connect the board to the computer over USB.
  * It has been observed that sometimes it becomes impossible to download firmware to the board after having flashed the USB chip EEPROM. I'm not sure why this happens, but it can be worked around by flashing the firmware over UART instead. Connect a USB-to-serial cable to the 4-pin serial header on the board (TODO: pinout?) *instead of* connecting the board over USB. Connecting a USB cable and serial cable at the same time can damage the board and/or your computer, as the two cables will try to drive the board's power supply rail to different voltages!
* Power the board through its 12V terminals.
* Press the button on the side of the board.

```bash
bin/mcv4-flash-fw /path/to/usb/serial/device /path/to/mcv4.bin
```

For example:

```bash
bin/mcv4-flash-fw /dev/ttyUSB0 ./mcv4.bin
```

### Flashing the USB chip EEPROM

Prerequisites:

* Install `libftdi1`, which provides the `ftdi_eeprom` tool.
* Connect the board to the computer over USB.

```bash
bin/mcv4-flash-usbeeprom SERIAL_NUM
```

For example:

```base
bin/mcv4-flash-usbeeprom 12-34-56-78
```
