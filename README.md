# Corne ZMK configuration

This configuration supports four-pin I2C 0.91-inch 128x32 OLED modules using
either an SSD1306 or SH1106 controller. ZMK's built-in status screen is shown
on the display.

## Firmware selection

GitHub Actions creates a separate firmware artifact for each controller and
keyboard half:

| OLED controller | Left half | Right half |
| --- | --- | --- |
| SSD1306 | `corne_left_ssd1306` | `corne_right_ssd1306` |
| SH1106 | `corne_left_sh1106` | `corne_right_sh1106` |

Use the pair matching the controller printed on the display listing or PCB.
If the controller is not identified, try the SSD1306 pair first; flashing the
other pair is safe if the display remains blank or appears shifted.

## Wiring

Connect the display by signal name rather than relying on the physical pin
order, which varies between modules:

| OLED pin | Corne / nice!nano connection |
| --- | --- |
| `GND` | `GND` |
| `VCC` | `3.3V` |
| `SCL` | Corne OLED `SCL` / Pro Micro pin `3` |
| `SDA` | Corne OLED `SDA` / Pro Micro pin `2` |

Power the module from 3.3 V so its I2C pull-ups cannot drive the nice!nano
GPIO pins above their supply voltage. Most modules use I2C address `0x3C`,
which is the address configured here and by ZMK's upstream Corne shield.

The display feature is enabled in `config/corne.conf`. The normal Corne builds
use upstream ZMK's SSD1306 definition. The additional
`corne_oled_sh1106` shield changes only the controller driver and its column
offset; it keeps the same I2C pins, address, and 128x32 resolution.
