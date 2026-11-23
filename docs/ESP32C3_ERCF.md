# ESP32-C3 for ERCF Easy BRD

This branch builds on XiaoK's experimental Klipper ESP32 port at
`4df1ad7dea327967f97f7a6cc5a65492ba14cb8f`. It was tested on an ESP32-C3
Super Mini with silicon revision v0.4 installed on the original XIAO header
pins of an ERCF Easy BRD v1.1.

- Start the first TMC UART timer 1 ms after queuing it on ESP32-C3. The
  UART bit rate stays unchanged.
- Mask the ESP32-C3 SYSTIMER interrupt during its own handler so the
  scheduler cannot reenter it when it briefly enables interrupts.

## Build

Use a Linux build host with Git, GNU Make, a native C compiler, Python 3
and Espressif's `riscv32-esp-elf` toolchain on `PATH`. The bench build
used Espressif GCC 14.2.0 (`esp-14.2.0_20241119`).

```sh
git clone --branch esp32c3-ercf --single-branch https://github.com/vladindov/klipper.git
cd klipper
make menuconfig
```

Select **Espressif ESP32**, **ESP32-C3 (revision 3 or newer)** and
**USB Serial/JTAG (on GPIO18/GPIO19)**. Save the configuration and run
`make`. The output is `out/klipper.bin`.

## Flash and connect

With an ESP32-C3 attached by USB and the Klipper host disconnected from that
serial port, run the following with the actual device path. This requires
`esptool` installed for the Python interpreter used by Make.

```sh
make flash FLASH_DEVICE=/dev/ttyACM0
```

The ESP32-C3 image uses the chip ROM's direct-boot format and is flashed
at offset `0x0`. Use the Super Mini's GPIO numbers in the Klipper
configuration. Reserve GPIO18 and GPIO19 for native USB.
Use the Espressif device's stable `/dev/serial/by-id/` path as the MCU
serial setting in Klipper.

## Bench verification

With TMC2226 drivers, the test setup read registers from both drivers
on GPIO9 at UART addresses 0 and 1. After the SYSTIMER change, Gear
completed three consecutive `STEPPER_BUZZ` runs and still answered
UART reads after each run. Gear and servo operation together was also
confirmed on the bench. These results cover this hardware configuration.

The prerelease binary is the exact image tested on the bench. It was
built before these changes were committed, so its embedded version
identifies a modified checkout of the upstream base. A fresh build
from this branch will embed a different version string.
