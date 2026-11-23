# Klipper for ESP32-C3 and ERCF Easy BRD

This experimental fork lets an ESP32-C3 Super Mini replace the
Seeeduino XIAO SAMD21 on an ERCF Easy BRD v1.1 by using the same header pins.
The ESP32-C3 connects to the existing Klipper host over USB Serial/JTAG
and acts as the ERCF's secondary MCU.

The fork builds on XiaoK's ESP32 port and adds two changes for the
ESP32-C3. It gives the first TMC UART timer 1 ms to start and prevents
the system timer interrupt handler from reentering itself. These changes
address the `Timer too close` errors and watchdog resets observed
during bench testing.

The test setup used an ESP32-C3 Super Mini rev. 0.4, ERCF Easy BRD v1.1,
two TMC2226 drivers, and Happy Hare. Both drivers responded over the
shared UART line. Selector motion and combined Gear and servo operation
were also verified on the bench.

- [Build and flash instructions](docs/ESP32C3_ERCF.md)
- [Download the bench-tested firmware](https://github.com/vladindov/klipper/releases/tag/esp32c3-ercf-v0.1.0)
- [XiaoK's original ESP32 branch](https://github.com/kluoyun/klipper/tree/dev/esp32)

## Upstream Klipper

Welcome to the Klipper project!

[![Klipper](docs/img/klipper-logo-small.png)](https://www.klipper3d.org/)

https://www.klipper3d.org/

The Klipper firmware controls 3d-Printers. It combines the power of a
general purpose computer with one or more micro-controllers. See the
[features document](https://www.klipper3d.org/Features.html) for more
information on why you should use the Klipper software.

Start by [installing Klipper software](https://www.klipper3d.org/Installation.html).

Klipper software is Free Software. See the [license](COPYING) or read
the [documentation](https://www.klipper3d.org/Overview.html). We
depend on the generous support from our
[sponsors](https://www.klipper3d.org/Sponsors.html).
