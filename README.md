# pic18-auto-irrigation-system
"An automated plant irrigation controller programmed in PIC18F4520 assembly. Features custom 16-bit math, ADC sensor reading, and hysteresis-based pump control."
# GardenVille: Automatic Household Irrigation System 🪴💧

GardenVille is an automated, microcontroller-based plant irrigation system written entirely in **PIC18F4520 Assembly Language**. It reads analog data from a soil moisture sensor, calculates the exact moisture percentage using custom 16-bit mathematical routines, and controls a water pump and indicator interface based on the plant's hydration needs.

## 🚀 Features

* **Custom 16-bit Math:** Implements division and multiplication (via bit-shifting) directly in assembly to convert raw 10-bit ADC values (0-1023) into a 0-100% scale.
* **Smart Pump Control (Hysteresis):** The pump activates when moisture drops below 20% and only stops once it reaches 80%, preventing hardware wear from rapid on/off cycling.
* **False-Trigger Prevention:** Includes a 3-second software delay to filter out analog sensor noise before triggering the water pump.
* **Hardware Diagnostic:** Features a startup sequence that tests all LEDs, the buzzer, and the pump output upon boot.
* **Multi-Tiered Warning System:** Uses LEDs and an active-low buzzer with distinct patterns to alert users of critical moisture levels.

## 🛠️ Hardware Requirements & Pin Mapping

* **Microcontroller:** Microchip PIC18F4520 (8MHz Internal Oscillator)
* **Input:** Analog Soil Moisture Sensor
* **Outputs:** 6x LEDs, 1x Active-Low Buzzer, 1x Water Pump (DC Motor)

| Component | PIC18F4520 Pin | Description |
| :--- | :--- | :--- |
| **Soil Sensor (A0)** | `RA0 / AN0` | Analog input for moisture reading |
| **LED 1 (Red)** | `RC1` | 0-20% Moisture (CRITICAL / DRY) |
| **LED 2 (Yellow)** | `RC2` | 21-40% Moisture (LOW) |
| **LED 3 (Green)** | `RC3` | 41-60% Moisture (MED) |
| **LED 4 (Green)** | `RC4` | 61-80% Moisture (HIGH) |
| **LED 5 (Blue)** | `RC5` | 81-100% Moisture (WET) |
| **Warning LED** | `RB0` | Blinks when moisture is < 40% |
| **Buzzer** | `RB1` | Active-Low audio alert |
| **Water Pump** | `RB2` | Triggers external motor relay/driver |

## 📊 System Logic

The system translates the real-time moisture percentage into the following automated responses:

* **0% - 20% (Critical):** LED 1 is ON. Buzzer sounds continuously. Water pump starts (after 3-second verification delay).
* **21% - 39% (Warning):** LED 2 is ON. Warning LED blinks. Buzzer beeps in a 0.5s ON / 1.0s OFF pattern. 
* **40% - 79% (Adequate):** Respective LEDs (3 or 4) are ON. Buzzer and Warning LED are OFF.
* **80% - 100% (Well Watered):** LED 5 is ON. Water pump automatically shuts off.

## ⚙️ Configuration Notes

* **Calibration:** The `DRY_VALUE` is set to `850` (0x0352) and the `WET_VALUE` is set to `150` (0x0096) based on the specific sensor calibration. 
* **Oscillator:** The system utilizes the internal oscillator block configured to 8MHz (`OSCCON = 0x77`). All delay subroutines (`delay_10us`, `delay_500ms`, etc.) are timed specifically for this clock speed.
