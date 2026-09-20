# Frosted Glass — ESP32-P4 ForgeUI One reference runtime

![Conceptual Frosted Glass artwork for the active ESP32-P4 target](./Splash%20frosted%20glass.png)

Frosted Glass is a single-screen LVGL 9 reference project for the Waveshare ESP32-P4-WIFI6-Touch-LCD-7B. It combines an ESP-IDF 5.5.4 firmware baseline with a generated ForgeUI Studio export for the board's 7-inch, 1024×600 touchscreen.

This repository preserves a small, buildable ForgeUI One runtime target and its Frosted Glass Studio-exported screen so developers can inspect, build, flash, and extend an ESP32-P4 touchscreen baseline with the display, touch, Wi-Fi, RTC, and SD paths already wired into the project.

The hero above is conceptual artwork for the Frosted Glass theme and active target, not a photograph or physical-validation record.

## ForgeUI Ecosystem

ForgeUI is developed by [RTechAI](https://github.com/RTechAI).

The [ForgeUI website](https://forgeui.co.nz) is the official home of the ForgeUI embedded UI/HMI development ecosystem. ForgeUI Studio is the current visual embedded UI/HMI development environment for supported ESP32 hardware. [ForgeUI Hosted Studio](https://studio.forgeui.co.nz) is the hosted, browser-based ForgeUI Studio application and is available for public registration.

RTechAI's GitHub organization hosts ForgeUI public repositories, hardware references, framework baselines, examples, and related open development work. Frosted Glass is one of those ESP32-P4 reference baselines: its firmware runs a generated Studio export on the ForgeUI One runtime lineage.

## Overview

The active application is intentionally small. `app_main()` starts the Waveshare BSP display, initializes the ForgeUI runtime, creates the generated Studio screen, and brings up enabled backend services. The checked-in export currently renders a pale “Nordic Ice” screen with live clock and Wi-Fi-status labels; it is an export proof, not a complete multi-screen product UI.

## Hardware Target

- **Board:** Waveshare ESP32-P4-WIFI6-Touch-LCD-7B
- **SoC:** ESP32-P4
- **Display:** 7-inch, 1024×600 MIPI DSI panel using EK79007 support
- **Touch:** GT911 capacitive touch support
- **Wireless path:** ESP-Hosted / Wi-Fi Remote over SDIO to the board's ESP32-C6

## Software Stack

- **ESP-IDF:** 5.5.4
- **LVGL:** 9.2.2, integrated through `esp_lvgl_port` 2.7.2
- **Waveshare BSP:** `waveshare/esp32_p4_wifi6_touch_lcd_7b` 1.0.2
- **Display and touch components:** `esp_lcd_ek79007` 1.0.4 and `esp_lcd_touch_gt911` 1.2.0~2
- **Networking components:** `esp_hosted` 2.9.7 and `esp_wifi_remote` 1.3.0
- **Runtime lineage:** ForgeUI One 1.0.0, with a generated historical ESP32-P4 UI Studio export

## What This Project Demonstrates

- BSP-managed display startup, backlight control, and LVGL lifecycle on the target board.
- A generated Studio export mounted on the active LVGL screen.
- Enabled ESP-Hosted Wi-Fi, DS3231 RTC, and SDMMC storage backends.
- The required startup order for this baseline: initialize Hosted Wi-Fi before mounting SD storage.
- Optional audio support is present in the runtime and dependencies but is disabled by `FORGEUI_ENABLE_AUDIO` in the checked-in configuration.

The runtime is deliberately separated from UI ownership: Studio-generated code owns layout, widgets, colours, themes, and assets, while firmware configuration owns board and backend switches.

## UI / Theme

“Frosted Glass” identifies this repository's visual reference theme. The root splash image is conceptual theme artwork; the repository also includes reusable theme and icon assets under `main/assets/`. The currently checked-in generated export is a minimal Nordic Ice-style status screen, rather than a full Frosted Glass interaction design.

Documentation images under `docs/setup/` are setup/configuration screenshots, and `docs/images/` contains theme and icon assets. No physical hardware photograph or captured on-device UI screenshot is included in this checkout.

## Hardware and Runtime Baseline

The checked-in configuration enables RTC, Wi-Fi, and SD storage. The RTC backend is DS3231 on the BSP I2C bus; SD is mounted through SDMMC and includes a read/write test. Audio code and its managed components remain available but are not enabled in the active build.

Repository notes and source comments document prior hardware validation, including a Studio-export flash checkpoint and the Wi-Fi-before-SD boot order. They are useful engineering provenance, but this repository does not include logs, a hardware photo, or another independently reviewable physical-validation artifact.

## Project Structure

```text
.
├── main/
│   ├── main.c                  # Board bring-up and service order
│   ├── 00_ForgeUI_Config.h     # Runtime feature switches
│   ├── 01_FG_Runtime.c         # LVGL runtime entry
│   ├── 20_RTC.c                # DS3231 RTC backend
│   ├── 30_WIFI.c               # ESP-Hosted Wi-Fi backend
│   ├── 40_SD.c                 # SDMMC storage backend
│   └── 90_Studio_Export.c      # Generated Studio UI export
├── components/bsp_extra/       # Board-specific support component
├── docs/                       # Setup references, theme, and icon assets
├── dependencies.lock           # Resolved ESP-IDF component versions
└── sdkconfig.defaults          # ESP32-P4 build defaults
```

## Build and Flash

Install ESP-IDF 5.5.4 and its tools, then initialize the ESP-IDF environment in your shell. From the repository root:

```bash
idf.py set-target esp32p4
idf.py build
idf.py flash monitor
```

The project defaults to 16 MB QIO flash, 200 MHz PSRAM, and ESP32-P4. Use a data-capable USB connection to the Waveshare board. The enabled Wi-Fi and SD services follow the initialization order implemented in `main/main.c`.

## Historical ForgeUI Context

The source and historical repository copy identify this project as built with the earlier [ESP32-P4 UI Studio](https://github.com/RTechAI/esp32p4-ui-studio) and powered by [ForgeUI One](https://github.com/RTechAI/ForgeUI-One). In this checkout, that lineage is visible in the generated export and the ForgeUI One runtime ownership notes.

These are earlier ForgeUI projects. They should not be confused with the current ForgeUI Studio or ForgeUI Hosted Studio, and this repository does not establish that the current hosted application generated this export.

## Current ForgeUI Studio

The [ForgeUI website](https://forgeui.co.nz) is the official home of the ForgeUI embedded UI/HMI development ecosystem.

ForgeUI Studio is the current visual embedded UI/HMI development environment for supported ESP32 hardware. [ForgeUI Hosted Studio](https://studio.forgeui.co.nz) is the hosted, browser-based ForgeUI Studio application and is available for public registration.

## Related ForgeUI Projects

- [ForgeUI One](https://github.com/RTechAI/ForgeUI-One) — historical runtime lineage for this project.
- [ESP32-P4 UI Studio](https://github.com/RTechAI/esp32p4-ui-studio) — historical visual-design and export lineage.
- [ForgeUI P4](https://github.com/RTechAI/ForgeUI-P4) — related ESP32-P4 ForgeUI work.
- [ESP32-P4 LVGL Boilerplate 3](https://github.com/RTechAI/ESP32-P4-LVGL-Boilerplate-3) — related ESP32-P4/LVGL baseline.

## About ForgeUI

[ForgeUI](https://forgeui.co.nz) is developed by [RTechAI](https://github.com/RTechAI).

ForgeUI Studio provides visual embedded UI/HMI development workflows for supported ESP32 hardware, while [ForgeUI Hosted Studio](https://studio.forgeui.co.nz) provides the hosted, browser-based Studio application.

RTechAI is the GitHub home for ForgeUI public repositories and reference work.

## License and Third-Party Software

ForgeUI-owned code in this repository is governed by the root [ForgeUI Source Available License](LICENSE). Upstream and vendor components retain their own licenses; see [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) and the license files distributed with those components, including `components/bsp_extra/LICENSE`.

`THIRD_PARTY_LICENSES.md` correctly distinguishes third-party components, but its final ForgeUI One entry still calls ForgeUI One “MIT License,” which conflicts with the root source-available license. That wording should be reviewed in the dedicated license-consistency phase; no legal files were changed here.

## Support

For ForgeUI information, visit [forgeui.co.nz](https://forgeui.co.nz). For public repositories and reference work, see [RTechAI on GitHub](https://github.com/RTechAI).
