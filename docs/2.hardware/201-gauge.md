# 201 Gauge Hardware

## Identification

| Item | 201 Gauge |
| --- | --- |
| Hardware version | 52 |
| BLE module | BGM220P |
| User interface | Seven-segment display with mechanical buttons |
| Main MCU | Record from the approved 201 schematic |

The 201 shares the general MCU-to-BLE architecture described in [Hardware Architecture](hardware.md), but its display and input hardware differ from the 202. Product-specific pin assignments and sensor topology must be taken from the approved 201 schematic.

## Connections to document

| Subsystem | Expected documentation |
| --- | --- |
| Main MCU | Exact device, clocking, reset, debug/programming interface, and firmware target. |
| BGM220P | UART pins, baud rate, flow control, module power/reset controls, and supported module firmware. |
| Pressure sensing | Sensor models/count, I²C addresses, mux part/channels, and any ambient sensor bus. |
| Seven-segment display | Driver/controller, MCU interface, digit mapping, and brightness control. |
| Mechanical buttons | GPIO mapping, active level, pull configuration, and debounce behavior. |
| External interfaces | RS-232 transceiver and connector details, if populated. |
| Nonvolatile storage | Part number and interface for calibration or update data, if populated. |

## Revision-specific records

The 201 schematic was not available alongside the referenced 202 block diagram during this documentation update. Add the schematic/BOM revision, MCU part number, sensor configuration, connector pinout, and programming instructions from the controlled 201 design package before using this page as a manufacturing reference.
