# VCC-GND Studio YD-ESP32-S3 N16R8

ESP-Claw board definition for the VCC-GND Studio YD-ESP32-S3 core board using an ESP32-S3-WROOM-1 with 16 MB flash and 8 MB octal PSRAM.

Hardware details are based on the [official YD-ESP32-S3 repository](https://github.com/vcc-gnd/YD-ESP32-S3), especially its README and `5-public-YD-ESP32-S3-Hardware info/YD-ESP32-S3-SCH-V1.4.pdf`.

## Onboard hardware

| Function | Connection |
| --- | --- |
| WS2812 RGB LED | GPIO48 |
| BOOT/user button | GPIO0, active-low |
| Native USB D- / D+ | GPIO19 / GPIO20 |
| CH343P UART0 TX / RX | GPIO43 / GPIO44 |
| Reset | Hardware EN/reset circuit |
| Power LED | Hardware-controlled, not software-controlled |
| TX/RX indicator LEDs | Associated with GPIO43/GPIO44; not independent device outputs |

GPIO35, GPIO36, and GPIO37 are reserved by the ESP32-S3-WROOM-1 octal flash/PSRAM interface and must not be used as expansion GPIOs. The board has no fixed display, camera, audio, SD-card, I2C, SPI, or I2S device wiring; attached peripherals should be configured for the user's wiring separately.

## USB ports

The board has two USB-C connectors:

- The native ESP32-S3 USB connector is connected to GPIO19/GPIO20 and is the primary ESP-Claw USB Serial/JTAG console for this board configuration.
- The CH343P USB-to-UART connector is connected to UART0 and is useful for ROM bootloader flashing and UART communication. Its host-side port is not the native USB Serial/JTAG console.

If flashing remains at `Connecting...`, hold BOOT, press and release RESET, release BOOT, and retry.

## Build

From the application directory, export ESP-IDF in the usual way and generate the board-manager sources:

```powershell
cd application/edge_agent
idf.py set-target esp32s3
idf.py bmgr --customer-path ./boards -b vcc_gnd_yd_esp32_s3_n16r8
idf.py build
```

The shorter board-manager form used by some repository versions is:

```powershell
idf.py bmgr -c ./boards -b vcc_gnd_yd_esp32_s3_n16r8
```

Regenerate board-manager sources after changing any YAML file under this board directory. The generated sources are placed under `components/gen_bmgr_codes/`.

## Flash and monitor

Use the CH343 COM port or the native USB port as appropriate for your host setup:

```powershell
idf.py -p COMx flash monitor
```

At boot, verify that the log reports 16 MB flash and 8 MB octal PSRAM. The board uses the 16 MB partition layout selected by `sdkconfig.defaults.board`.
