# Hardware Specifications - Desktronic HomeOne

## RJ50 (10P10C) Pinout Mapping

This table represents the data gathered during the reverse engineering process using an RJ50 extension cable.

| Pin (Approx) | Wire Color | Idle Voltage | UP Pressed | DOWN Pressed | Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Yellow | 5V | 5V | 5V | VCC / Logic Power |
| 2 | Grey | 5V | 5V | **0V** | **DOWN** (Active Low) |
| 3 | Blue | 5V | 5V | 5V | VCC / Logic Power |
| 4 | Purple | 5V | **0V** | 5V | **UP** (Active Low) |
| 5 | Green | 5V | 5V | 5V | VCC / Logic Power |
| 6 | Black | 1V | 1V | 1V | Control / Data |
| 7 | Orange | 0V | 0V | 0V | **Ground (GND)** |
| 8 | Red | 5V | 5V | 5V | VCC / Logic Power |
| 9 | Brown | 5V | 5V | 5V | VCC / Logic Power |
| 10 | White | 0V | 0V | 0V | GND / Shield |

## PCB Components

- **Vias**: Located near the RJ50 socket on the top side of the PCB. Two of these were confirmed to be connected to the Orange GND wire.
- **Orange Capacitor/Filter**: Located near the RJ50 connector, used for signal filtering.
- **Markings**: Only "TX" and "RX" (TX marked on the PCB) were explicitly identified on the board.

## Logic of Operation

The system uses **Active Low** logic. To trigger a movement, the corresponding signal line (UP or DOWN) must be momentarily shorted to GROUND (GND).

- **Movement UP**: Short Purple to Orange.
- **Movement DOWN**: Short Grey to Orange.
