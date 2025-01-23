# Ender 3 Pro Upgraded Configuration

## Overview

This project contains custom configuration files and settings for the **Ender 3 Pro** with multiple hardware upgrades and the latest version (as of January 23, 2025) of **Marlin 2.1.3 Beta 2**. The primary goal is to enhance reliability, precision, and print capabilities with a direct drive setup. You can find all the upgrades, that i purchased in the links below.

## Upgrades

The printer has been modified with the following components:

1. **Mainboard**: [BIGTREETECH SKR Mini E3 V3 with TMC2209 drivers.](https://www.aliexpress.com/item/1005006042517491.html)
2. **Direct Drive**: [Redesigned filament feeding system for improved reliability.](https://www.aliexpress.com/item/4000395495656.html)
3. **CR Touch**: [For automatic bed leveling.](https://www.aliexpress.com/item/1005007359991266.html)
4. **Dual Z-Axis**: [Improved stability and precision on the Z-axis.](https://www.aliexpress.com/item/1005006140643642.html)
5. **Hotend Fan**: [Noctua A4 FLX 40x40x10](https://www.amazon.com/Noctua-Cooling-Blades-Bearing-NF-A4x10/dp/B009NQLT0M/ref=sr_1_1?dib=eyJ2IjoiMSJ9.y0llkIF5Rh2xr60zWm4pcqizDatI0t2GnXKHkpV7rR-LDPxFcZe2LbS76aXA5m0JW-MlyULai4Ruero9JDhKPvnwRpmQ5IkKq3Gaxe_gl7-NkQjwaaWTD5s8Zy098bcXRvZrxbvCIhefscDoHmQsIb3kUcO22T9PY7248CbfLCQ1qYx_AswaUBSjyzhh7CzrMq-Rr4EfAgiLo_aYfg0p8ii5gLlsBcSJPEFWfRELnOg.5QjkMp3IU0sQJsPKA7Oi1Ls00qijGcnkSOr3ZBOkMlU&dib_tag=se&keywords=Noctua+A4+40x40x10&qid=1737628433&sr=8-1) with a [24V-to-12V converter](https://www.aliexpress.com/item/32742116421.html) for quiet and efficient cooling.
6. **PEI Sheet**: [Flexible metal sheet for improved bed adhesion and easy part removal.](https://www.aliexpress.com/item/1005005818524087.html)

### Stock Components

Everything else on the printer remains stock as it comes from the factory, including:
- Frame
- Hotend
- Part-cooling fan
- Stepper motors
- Belts
- Carriages
- Other mechanical components

## Software Used

- **Marlin 2.1.3 Beta 2**: Latest version.
- **Ender 3 Pro configuration files**: Latest version.

## Download compiled firmware
Download the latest firmware version from the [Releases page](https://github.com/ttsvetanov92/Ender3ProDD-marlin/releases).

## How to Compile the Firmware

To compile the firmware, you will need **Visual Studio Code (VSCode)** with the **PlatformIO** extension. Follow these steps:

1. Install **Visual Studio Code** if you haven’t already.
2. Open VSCode and install the **PlatformIO IDE** extension from the Extensions Marketplace.
3. Clone this repository or download it as a ZIP file and extract it.
4. Open the firmware folder in VSCode.
5. Use PlatformIO to build the project:
   - Click on the checkmark (✔) in the bottom toolbar, or run `PlatformIO: Build` from the Command Palette.
   - This will generate the compiled firmware file (`.bin`) in the `.pio/build/` folder.

For a detailed guide, you can refer to [this video tutorial](https://www.youtube.com/watch?v=PMG4bC9I3DA).

## Full Changelog

### Print Motion Settings
**Configuration.h**
- Line 25: `#define SHORT_BUILD_VERSION "Version 2.01"`
- Line 26: `#define STRING_DISTRIBUTION_DATE "2025-01-22"`
- Line 1311: `#define DEFAULT_MAX_FEEDRATE { 200, 200, 12, 120 }` (changed from `{ 500, 500, 5, 25 }`)
- Line 1324: `#define DEFAULT_MAX_ACCELERATION { 1000, 1000, 200, 2500 }` (default `{ 500, 500, 100, 5000 }`)
- Line 1339: `#define DEFAULT_ACCELERATION 1000` (default `500`)
- Line 1340: `#define DEFAULT_RETRACT_ACCELERATION 1500` (default `500`)
- Line 1341: `#define DEFAULT_TRAVEL_ACCELERATION 1200` (default `500`)
- Line 1898: `#define Z_MAX_POS 210` (reduced from `250` for safety)



### CR Touch Settings
**Configuration.h**
- Line 1409: Commented out `#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN` (to use CR Touch for Z endstop)
- Line 1412: `#define USE_PROBE_FOR_Z_HOMING` (replacing classic endstop switch)
- Line 1469: `#define BLTOUCH`
- Line 1645: `#define NOZZLE_TO_PROBE_OFFSET { -45, -5, 0 }` (measured values)
- Line 1926: Commented out `#define MIN_SOFTWARE_ENDSTOP_Z` (allows negative Z offset)
- Line 2111: `#define AUTO_BED_LEVELING_BILINEAR`
- Line 2113: Comment `#define MESH_BED_LEVELING` (instead of bilinear leveling)
- Line 2347: `#define Z_SAFE_HOMING`

**Configuration_adv.h**
- Line 1515: `#define PROBE_OFFSET_WIZARD`
- Line 1522: `#define PROBE_OFFSET_WIZARD_START_Z -4.0`
- Line 2293: `#define BABYSTEPPING`
- Line 2316: `#define BABYSTEP_ZPROBE_OFFSET` (combines `M851 Z` with Babystepping)

### Direct Drive Settings
**Configuration.h**
- Line 1299: `#define DEFAULT_AXIS_STEPS_PER_UNIT { 80, 80, 400, 97.33 }` (calibrated extruder, default was `93`)
- Line 1351: Uncomment `#define CLASSIC_JERK` (default values: `X=10, Y=10, Z=0.3, E=5`)

**Configuration_adv.h**
- Line 2340: `#define LIN_ADVANCE`
- Line 2345: `#define ADVANCE_K 0.0` (K-factor is specific for filament type and will be fine-tuned later in slicer)
- Line 2923: `#define FILAMENT_CHANGE_UNLOAD_LENGTH 120` (default for bowden is `400`)
- Line 2932: `#define FILAMENT_CHANGE_FAST_LOAD_LENGTH 100` (default for bowden is `350`)

### Other Settings
PETG preheat option added to the LCD menu (**Configuration.h**, lines 2472–2491):
```c
#define PREHEAT_1_LABEL       "PLA"
#define PREHEAT_1_TEMP_HOTEND 215
#define PREHEAT_1_TEMP_BED     60
#define PREHEAT_1_TEMP_CHAMBER 35
#define PREHEAT_1_FAN_SPEED   255

#define PREHEAT_2_LABEL       "PETG"
#define PREHEAT_2_TEMP_HOTEND 240
#define PREHEAT_2_TEMP_BED     80
#define PREHEAT_2_TEMP_CHAMBER 35
#define PREHEAT_2_FAN_SPEED     0

#define PREHEAT_3_LABEL       "ABS"
#define PREHEAT_3_TEMP_HOTEND 240
#define PREHEAT_3_TEMP_BED    110
#define PREHEAT_3_TEMP_CHAMBER 35
#define PREHEAT_3_FAN_SPEED   255
