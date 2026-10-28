<p align="center">
  <img src="https://summerofcode.withgoogle.com/assets/media/logo.svg" alt="Google Summer of Code Logo" width="1300"/>
</p>

<h1 align="center">Google Summer of Code 2026 – Final Report</h1>

## Personal Information

- **Name:** Rahul Das
- **GitHub:** [Rahul](https://github.com/rahul31124)
- **Project:** Enhancing and Improving the Performance of PSLab and Badge Apps
- **Organization:** [FOSSASIA](https://github.com/fossasia)

## Mentors

- [Alexander Bessman](https://github.com/bessman)
- [M. Padmal](https://github.com/CloudyPadmal)
- [Marc Nause](https://github.com/marcnause)

---

## Project Overview

The goal of this project was to improve the **performance, reliability, hardware compatibility, and overall user experience** of the PSLab, BadgeMagic, and Magic E-Paper applications within the FOSSASIA.

My work covered a broad range of engineering challenges, including improving scientific instruments, integrating additional sensors and peripherals, resolving existing bugs, modernizing application architecture, and optimizing development workflows. One of the major objectives was migrating performance-critical components from Dart to **Rust**, improving hardware communication in PSLab and image processing in Magic E-Paper while retaining integration with Flutter.

Another major focus was expanding hardware compatibility. I worked on integrating the [EspoTek Labrador board](https://github.com/espotek-org/Labrador) into the PSLab application and adding support for the upcoming PSLab Mini hardware. This involved developing hardware-specific communication functionality and establishing a more extensible architecture for supporting multiple boards through shared abstractions.

I also improved CI/CD workflows, expanded sensor support, introduced configurable data export and visualization, implemented automatic sensor detection, and enhanced several existing instruments.

In addition to these improvements, I introduced **edge computing capabilities for sound classification** into the PSLab application.

Together, these contributions strengthened the applications' existing capabilities while laying the groundwork for additional hardware integrations, intelligent features, and future development.

## Contributions Before the Midterm Evaluation

During the first half of the project, I focused on **improving existing instruments, fixing bugs, expanding sensor support, enhancing CI/CD workflows, and refining application usability** across the FOSSASIA applications.

### 1. Scientific Instruments and Application Features

I implemented and improved several features that make the PSLab application more useful for scientific experiments, data collection, and hardware interaction.

- **Sound Wave Generation:** Added support for generating sound waves, expanding the application's signal-generation capabilities. ([Sound wave generation · PR #3123](https://github.com/fossasia/pslab-app/pull/3123))

- **Sawtooth Wave Generation:** Extended waveform generation with support for sawtooth signals. ([Sawtooth waveform support · PR #3147](https://github.com/fossasia/pslab-app/pull/3147))

- **Configurable Data Export:** Added configurable export options for experimental data in **JSON, CSV, and TXT** formats. ([Configurable data export · PR #3329](https://github.com/fossasia/pslab-app/pull/3329))

- **PDF Data Export:** Added the ability to export collected experimental data as PDF documents, making it easier to save and share experiment results.

- **Compass Data Recording:** Implemented recording of compass sensor data for subsequent analysis. ([Compass data recording · PR #3218](https://github.com/fossasia/pslab-app/pull/3218))

- **PSLab V5 Pin Layout:** Added the PSLab V5 board pin layout to help users identify hardware connections. ([PSLab V5 pin layout · PR #3263](https://github.com/fossasia/pslab-app/pull/3263))

- **Guide Layout Improvements:** Refined the application's guide layout to improve readability and usability. ([Guide layout enhancement · PR #3409](https://github.com/fossasia/pslab-app/pull/3409))

- **Oscilloscope Improvements:** Improved graph scaling and channel controls to make signal visualization more practical and easier to configure.

- **Logged Data Visualization:** Added charts and statistical metrics, including pie charts and line graphs, to help users analyze recorded experimental data. ([Logged data visualization · PR #3449](https://github.com/fossasia/pslab-app/pull/3449))

### 2. CI/CD, Maintenance, and Reliability

I improved automated workflows and resolved maintenance issues across PSLab, BadgeMagic, and Magic E-Paper. These changes helped reduce unnecessary CI executions, improve build reliability, and make application development and testing more efficient.

#### PSLab Application

- **More Efficient CI Workflows:** Configured workflows to run only when relevant code changes are detected, reducing unnecessary executions. ([Relevant-change CI filtering · PR #3176](https://github.com/fossasia/pslab-app/pull/3176))

- **Dependabot Restoration:** Restored Dependabot configuration to support automated dependency updates. ([Dependabot configuration · PR #3187](https://github.com/fossasia/pslab-app/pull/3187))

- **Conditional Screenshot Generation:** Updated screenshot-generation jobs to run only when the corresponding platform build succeeds. ([Conditional screenshot jobs · PR #3223](https://github.com/fossasia/pslab-app/pull/3223))

- **iOS Build Artifacts:** Added support for generating and storing iOS `.ipa` build artifacts in the artifact branch. ([iOS artifact generation · PR #3353](https://github.com/fossasia/pslab-app/pull/3353))

- **Platform-Agnostic Screenshot Workflows:** Generalized screenshot-generation workflows by replacing device-specific model names with platform-oriented identifiers such as `iphone` and `android`. This helps avoid file-not-found errors when CI device models change. ([Generalized screenshot workflow · PR #3426](https://github.com/fossasia/pslab-app/pull/3426))

- **Java-to-Kotlin Migration:** Migrated the Android `MainActivity` implementation from Java to Kotlin. ([MainActivity Kotlin migration · PR #3481](https://github.com/fossasia/pslab-app/pull/3481))

- **Multimeter Pulse-Counting Fix:** Fixed an issue that caused the multimeter's pulse-counting function to return zero. ([Pulse-counting fix · PR #3133](https://github.com/fossasia/pslab-app/pull/3133))

- **Landscape UI Improvements:** Resolved inconsistent UI behavior when the application is used in landscape orientation. ([Landscape UI fixes · PR #3292](https://github.com/fossasia/pslab-app/pull/3292))

- **Accelerometer and Gyroscope Enhancements:** Added configurable sensor selection for the MPU6050, MPU925x, and built-in mobile sensors. Also improved graph visualization with configurable dynamic auto-scaling. ([Motion sensor improvements · PR #3411](https://github.com/fossasia/pslab-app/pull/3411))

- **Windows CI Fix:** Resolved a Windows CI failure to improve automated build reliability. ([Windows CI fix · PR #3279](https://github.com/fossasia/pslab-app/pull/3279))

- **README and Project Documentation:** Updated the README with current project information, refreshed screenshots, clarified platform compatibility, and added development guidance alongside documentation for Flutter and Rust. ([README and development documentation · PR #3470](https://github.com/fossasia/pslab-app/pull/3470))

- **Instrument Guide Images:** Replaced older guide images with updated images created using the **PSLab V6 board**, improving image quality. Also added the source SVG files to the `doc` directory for future reference and guide creation. ([Updated instrument guide images · PR #3492](https://github.com/fossasia/pslab-app/pull/3492))

#### PSLab Firmware

- **UART FIFO Overflow Fix During I²C Bulk Transfers:** Fixed a firmware issue where large UART transfers could overflow the **4-byte UART FIFO** because the host sent data faster than the I²C bus could transmit it. This could cause dropped bytes and client timeouts.

  The fix introduced a **255-byte RAM buffer** in `I2C_CommandWriteBulk`, allowing the firmware to buffer the full UART payload before starting the I²C transfer. This decouples UART reception from I²C transmission and prevents FIFO overflows and data loss during large transfers. Watchdog servicing was also added during long operations to help prevent unintended resets.

  **Contribution:** [Fix UART FIFO overflow during I²C bulk writes · PR #207](https://github.com/fossasia/pslab-firmware/pull/207)
#### Wi-Fi Firmware and WebSocket Support
- Added WebSocket support to the ESP-01 firmware, enabling web-based PSLab communication over Wi-Fi. The firmware also supports **TCP connections** for native platforms, allowing both communication approaches to interact with PSLab hardware through the same firmware. Buffered serial data handling was improved for more reliable communication.

**Contribution**  [feat: support WebSocket](https://github.com/fossasia/pslab-esp01-firmware/pull/28)

#### BadgeMagic Application

- **CI Workflow Optimization:** Optimized CI workflows to avoid unnecessary executions for changes that do not affect application code. ([BadgeMagic CI optimization · PR #1708](https://github.com/fossasia/badgemagic-app/pull/1708))

- **Badge Animation Fix:** Fixed an issue that caused badge animations to reset when the cursor position changed. ([Badge animation fix · PR #1693](https://github.com/fossasia/badgemagic-app/pull/1693))

- **Clipart Import, Export, and Sharing:** Added functionality to import and export saved clipart, share designs, name clipart before saving, and access these actions directly from the Saved Clipart screen. ([Clipart management improvements · PR #1746](https://github.com/fossasia/badgemagic-app/pull/1746))

#### Magic E-Paper Application

- **Cross-Platform Build Artifacts:** Added automated application artifact builds for Windows, Linux, macOS, and Web. ([Cross-platform build artifacts · PR #432](https://github.com/fossasia/magic-epaper-app/pull/432))

- **Codebase Cleanup:** Replaced deprecated functions and cleaned up the codebase to improve maintainability. ([Deprecated code cleanup · PR #377](https://github.com/fossasia/magic-epaper-app/pull/377))

- **Storage Permission Fix:** Resolved storage permission issues affecting release builds. ([Release storage permission fix · PR #401](https://github.com/fossasia/magic-epaper-app/pull/401))

### 3. Performance Optimization 

A major part of my work involved migrating performance-critical operations from Dart to **Rust**. These changes targeted hardware communication and image processing, where reducing latency could substantially improve the user experience.

#### PSLab Backend Communication

Migrated the low-level **USB serial and Wi-Fi communication backend from Dart to Rust**, then integrated the Rust implementation with Flutter.

This work improved the performance of hardware interactions while retaining integration with the existing application.

- **Reported result:** Approximately **90% lower latency**.
- **Contribution:** [Rust-based PSLab communication backend · PR #3424](https://github.com/fossasia/pslab-app/pull/3424)

#### Magic E-Paper Image Processing

Migrated the dithering algorithms and image-processing pipeline in Magic E-Paper from Dart to Rust. The migration significantly reduced processing time and made the image-processing workflow more responsive.

- **Reported result:** Approximately **85% lower latency**.
- **Contribution:** [Rust-based image processing and dithering · PR #411](https://github.com/fossasia/magic-epaper-app/pull/411)

### 4. Sensor Integrations

I expanded the PSLab application's sensor support by integrating the following sensors.

| Sensor | Functionality | Contribution |
|---|---|---|
| **TSL2561** | Digital light sensing | [PR #3235](https://github.com/fossasia/pslab-app/pull/3235) |
| **MQ-135** | Gas sensing | [PR #3244](https://github.com/fossasia/pslab-app/pull/3244) |
| **SHT21** | Temperature and humidity sensing | [PR #3258](https://github.com/fossasia/pslab-app/pull/3258) |
| **MPU6050** | Accelerometer and gyroscope sensing | [PR #3305](https://github.com/fossasia/pslab-app/pull/3305) |
| **MAX30102** | Heart-rate and pulse-oximetry sensing | [PR #3320](https://github.com/fossasia/pslab-app/pull/3320) |
| **HMC5883L** | Digital compass and magnetic-field sensing | [PR #3343](https://github.com/fossasia/pslab-app/pull/3343) |
| **CCS811** | Air-quality sensing | [PR #3363](https://github.com/fossasia/pslab-app/pull/3363) |
| **MLX90614** | Non-contact infrared temperature sensing | [PR #3365](https://github.com/fossasia/pslab-app/pull/3365) |
| **MPU925x** | Motion sensing using an inertial measurement unit (IMU) | [PR #3408](https://github.com/fossasia/pslab-app/pull/3408) |

### 5. Automatic Sensor Detection

Implemented automatic detection for supported sensors connected to the board. This simplifies hardware setup by reducing the need for users to manually identify and configure every supported sensor.

**Contribution:** [Automatic sensor detection · PR #3326](https://github.com/fossasia/pslab-app/pull/3326)

### 6. OLED Display Instrument and Retro Games

Developed an **OLED display instrument** for the PSLab application, extending its capabilities for interacting with compatible display hardware.

**Contribution:** [OLED display instrument · PR #3395](https://github.com/fossasia/pslab-app/pull/3395)

#### OLED Retro Games

Extended the OLED functionality with retro-style games, including a car game and a Dino game. This demonstrated how the display instrument could be used for interactive applications in addition to its primary hardware functionality.

**Contribution:** [OLED retro games · PR #3429](https://github.com/fossasia/pslab-app/pull/3429)

---

By the midterm evaluation, I had addressed many existing bugs, improved scientific instruments, expanded sensor support, and strengthened the CI/CD workflows. With these foundational improvements in place, I shifted my focus during the second half of the project toward **more complex engineering challenges**, especially multi-board hardware support, cross-platform communication, and AI integration, while continuing to improve existing features.

## Contributions After the Midterm Evaluation

The second half of the project focused on extending the application's architecture and capabilities. My main areas of work included **wireless communication, firmware updates, PSLab Mini support, EspoTek Labrador integration, documentation improvements, and edge computing**.

### 1. Wireless Communication

Implemented **WebSocket-based Wi-Fi communication** for the web version of the PSLab application, enabling PSLab Web to communicate wirelessly with compatible hardware.

Key improvements included:

- **Platform-Aware Communication:** Added automatic selection of WebSocket communication when connecting to PSLab over Wi-Fi in the web application.
- **Native Platform Compatibility:** Retained raw TCP communication for native platforms and selected the appropriate communication mechanism based on the platform.
- **Rust and WebAssembly:** Used Rust WebAssembly (WASM) to support the web communication backend.
- **Hardware Testing:** Tested the implementation with real hardware and multiple instruments to verify end-to-end functionality.

**Contribution:** [WebSocket support for PSLab Web over Wi-Fi · PR #3488](https://github.com/fossasia/pslab-app/pull/3488)

### 2. In-App Firmware Updates

Added support for updating PSLab board firmware **directly from within the application**, simplifying firmware maintenance and reducing the need for separate update procedures.

**Contribution:** [In-app firmware update functionality · PR #3497](https://github.com/fossasia/pslab-app/pull/3497)

### 3. PSLab Mini Hardware Support

Expanded the PSLab application to support the upcoming **PSLab Mini** hardware. This involved implementing command definitions, device detection, instrument support, and sensor communication needed to integrate the new board into the existing application.

- **SCPI Command Dictionary:** Added the SCPI command dictionary required for communication with PSLab Mini. ([SCPI command support · PR #3558](https://github.com/fossasia/pslab-app/pull/3558))

- **Automatic Device Detection:** Implemented automatic detection so the application can identify a connected PSLab Mini board. ([PSLab Mini detection · PR #3550](https://github.com/fossasia/pslab-app/pull/3550))

- **Oscilloscope Support:** Added initial PSLab Mini support to the oscilloscope instrument. ([Oscilloscope integration · PR #3564](https://github.com/fossasia/pslab-app/pull/3564))

- **I²C Communication:** Implemented I²C communication support for connecting and using compatible sensors through PSLab Mini. ([I²C support · PR #3585](https://github.com/fossasia/pslab-app/pull/3585))

#### Automated PSLab Mini Firmware Build Artifacts

Improved the CI workflow for PSLab Mini firmware to automatically generate downloadable **UF2 firmware build artifacts** for the Pico 2 W board. Updated the build workflow to run for changes to the `dev26` and `main` branches, with optional manual triggering.

The workflow also checks that the expected firmware artifact is produced, making the build process more reliable for contributors who need to test or distribute firmware builds.

**Contribution:** [Automated PSLab Mini firmware build artifacts · PR #212](https://github.com/fossasia/pslab-mini-firmware/pull/212)

These contributions established a foundation for integrating PSLab Mini into the existing application and extending its hardware, sensor, and firmware-development capabilities.

### 4. EspoTek Labrador Hardware Integration

One of the major architectural contributions during the project was integrating the [EspoTek Labrador board](https://github.com/espotek-org/Labrador) alongside the existing PSLab hardware.

**Contribution:** [EspoTek Labrador hardware integration · PR #3622](https://github.com/fossasia/pslab-app/pull/3622)

This work required more than adding another device option. It involved adapting the application architecture so that different hardware boards could share common functionality while retaining their own communication and hardware-specific implementations.

Key contributions included:

- **Hardware Abstraction Layer (HAL):** Refactored the architecture around shared abstractions and interfaces, separating hardware-specific operations from common application functionality. This provides a more extensible foundation for supporting multiple boards.

- **Rust-Based Hardware Communication:** Implemented low-level Labrador communication in Rust and added `labrador.rs` and `labrador_bootloader.rs` to handle hardware communication and firmware-update operations.

- **Scientific Instrument Support:** Integrated Labrador support into the **oscilloscope, multimeter, waveform generator, logic analyzer, and power source**, reusing the existing application UI wherever possible.

- **Dynamic Device Management:** Updated device management so that available functionality can adapt to the connected hardware.

- **Web Compatibility:** Extended Labrador support to the web version of the application.

- **Connection UI Improvements:** Refactored the Connect Device screen to provide clearer connection steps and dedicated options for supported boards.

- **Cross-Platform Testing:** Tested the integration with PSLab and Labrador hardware across **Android, Windows desktop, and Web**.

This integration established a common architectural foundation for supporting different hardware boards without requiring a separate application or a complete duplication of the existing instruments.

### 5. On-Device Sound Classification

Integrated **YAMNet-based sound classification** into the Sound Meter instrument, enabling the application to classify environmental sounds using an edge model.

**Contribution:** [Sound classification in the Sound Meter · PR #3631](https://github.com/fossasia/pslab-app/pull/3631)

Key features included:

- **512 Sound Classes:** Supports classification across 512 sound classes.
- **Confidence Scores:** Displays the confidence percentage associated with detected sounds.
- **Testing with Realistic Samples:** Tested with audio samples containing dog, cat, cow, duck, music, and alarm sounds.
- **Cross-Platform Compatibility:** Tested the implementation across multiple platforms.

This feature introduced edge computing capabilities into the PSLab application, enabling intelligent sound analysis.

### 6. PSLab Documentation Website Modernization

Modernized the PSLab documentation website by migrating it from **Sphinx to MkDocs Material** and improving its design, structure, navigation, and overall usability.

The update addressed multiple existing problems and made the documentation more modern, responsive, and maintainable.

Key contributions included:

- **Documentation Platform Migration:** Replaced the Sphinx-based documentation setup with MkDocs Material.
- **Redesigned Website:** Improved the website's visual presentation with a more modern and professional layout.
- **Navigation and Layout Fixes:** Resolved issues affecting the navbar, top bar, and general page layout.
- **Typography and Image Improvements:** Fixed font-rendering and image-display problems while improving image handling and responsiveness.
- **Expanded Documentation:** Added missing documentation and updated outdated content where necessary.
- **Improved Structure:** Reorganized content and navigation to make documentation easier to browse and maintain.
- **Build and Deployment Updates:** Updated the documentation build and publishing workflows to use the new MkDocs setup.

**Contribution:** [Modernize the PSLab documentation website · PR #282](https://github.com/fossasia/pslab-documentation/pull/282)

This work improved the documentation experience for users and contributors while establishing a more maintainable platform for future documentation updates.

### 7. Additional Application Improvements and Maintenance

Alongside the larger hardware, edge computing, and documentation initiatives, I continued improving existing instruments, UI components, device detection, and developer workflows.

#### Hardware-Aware Instrument Filtering

Added device-based instrument filtering so users can view instruments supported by the currently connected hardware or choose to view all available instruments.

**Contribution:** [Hardware-aware instrument filtering · PR #3505](https://github.com/fossasia/pslab-app/pull/3505)

#### Reusable Graph Components

Centralized common graph UI functionality into a reusable widget for instruments such as the **Sound Meter, Barometer, Thermometer, and Lux Meter**. This reduces duplicated UI code and makes instrument interfaces easier to maintain consistently.

**Contribution:** [Reusable instrument graph widget · PR #3511](https://github.com/fossasia/pslab-app/pull/3511)

#### Labrador Pin Layout

Added the Labrador board pin layout image and pin descriptions alongside the existing PSLab V5 and V6 layouts, improving hardware reference support within the application.

**Contribution:** [Labrador pin layout · PR #3668](https://github.com/fossasia/pslab-app/pull/3668)

#### Improved Desktop Hardware Detection

Fixed a device-detection issue that could prevent PSLab hardware from being identified correctly when multiple serial or COM devices were connected to a computer.

**Contribution:** [Multiple-device detection fix · PR #3521](https://github.com/fossasia/pslab-app/pull/3521)

#### Multimeter Continuity Beep

Added an audible continuity indication to the multimeter when the measured resistance is approximately zero, making continuity testing more convenient.

**Contribution:** [Multimeter continuity indication · PR #3536](https://github.com/fossasia/pslab-app/pull/3536)

#### Native Desktop Audio Capture Using Rust

Improved audio capture for the Sound Meter and Oscilloscope on desktop platforms by integrating the Rust-based `cpal` audio backend.

The backend interfaces with platform audio systems such as **ALSA, WASAPI, and CoreAudio**, providing a platform-appropriate approach to desktop audio capture.

**Contribution:** [Rust-based desktop audio capture with `cpal` · PR #3540](https://github.com/fossasia/pslab-app/pull/3540)

#### Multimeter Stability Fix

Fixed an issue that caused the multimeter to hang when no input was available on the logic analyzer pins, improving instrument stability in this scenario.

**Contribution:** [Multimeter hang fix · PR #3554](https://github.com/fossasia/pslab-app/pull/3554)

#### Configurable Waveform Amplitude

Added a configuration option that allows users to set waveform amplitude through the waveform generator instrument.

**Contribution:** [Configurable waveform amplitude · PR #3575](https://github.com/fossasia/pslab-app/pull/3575)

#### Easier Access to CI Build Artifacts

Enhanced the CI workflow to include direct build-artifact download links in pull request comments, making it easier for contributors and reviewers to test application builds.

The workflow provides artifact access for **Android, iOS, Windows, Linux, macOS, and Web**, subject to the corresponding platform build succeeding.

**Contribution:** [Direct CI artifact links · PR #3579](https://github.com/fossasia/pslab-app/pull/3579)

#### Compass Instrument Refactoring

Refactored the compass instrument UI and improved its performance and maintainability.

**Contribution:** [Compass UI and performance improvements · PR #3600](https://github.com/fossasia/pslab-app/pull/3600)

#### Android APK Size Optimization

Optimized the Android release APK by removing support for unnecessary CPU architectures, including x86 and 32-bit ARM, according to the project's target-device requirements. The change was reported to reduce the application size by approximately **21 MB**.

- [Android APK architecture optimization · PR #3655](https://github.com/fossasia/pslab-app/pull/3655)
- [Related Android build optimization · PR #3635](https://github.com/fossasia/pslab-app/pull/3635)

#### README Update and Labrador Documentation

Updated the PSLab application README to reflect the latest project features and improvements. Added detailed documentation for the **EspoTek Labrador board**, covering its integration with PSLab and helping users understand hardware setup and usage.

**Contribution:** [Update README and add EspoTek Labrador documentation · PR #3684](https://github.com/fossasia/pslab-app/pull/3684)

---

## Screenshots

<table>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/oscilloscope.jpg" width="100%" alt="Oscilloscope"/>
      <br/>Oscilloscope
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/oscilloscope_mic.jpg" width="100%" alt="Oscilloscope with microphone input"/>
      <br/>Oscilloscope with Microphone Input
    </td>
  </tr>
</table>

<table>
  <tr>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/Saw_tooth.jpg" width="100%" alt="Sawtooth Waveform"/>
      <br/>Sawtooth Waveform
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/connected_device_labrador.jpg" width="100%" alt="Connected Device Labrador"/>
      <br/>Connect Device - Labrador
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/OLED.jpg" width="100%" alt="OLED Display"/>
      <br/>OLED Display
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/sound_class.jpg" width="100%" alt="Sound Classification"/>
      <br/>Sound Classification
    </td>
  </tr>
  <tr>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/firmware.jpg" width="100%" alt="Firmware"/>
      <br/>Firmware
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/firmware_update_labrador.jpg" width="100%" alt="Labrador Firmware Update"/>
      <br/>Firmware Update - Labrador
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/Compass.jpg" width="100%" alt="Compass"/>
      <br/>Compass
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/acc_meter.jpg" width="100%" alt="Accelerometer"/>
      <br/>Accelerometer
    </td>
  </tr>
  <tr>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/gyroscope.jpg" width="100%" alt="Gyroscope"/>
      <br/>Gyroscope
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/logged_data_chart.jpg" width="100%" alt="Logged Data Visualization"/>
      <br/>Logged Data Visualization
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/multimeter.jpg" width="100%" alt="Multimeter"/>
      <br/>Multimeter
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/sensors_screen.jpg" width="100%" alt="Sensor Screen"/>
      <br/>Sensor Screen
    </td>
  </tr>
  <tr>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/Pin_layout_labrador.jpg" width="100%" alt="Labrador Pin Layout"/>
      <br/>Labrador Pin Layout
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/PSLab_V5_Pin.jpg" width="100%" alt="PSLab V5 Pin Layout"/>
      <br/>PSLab V5 Pin Layout
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/PSLab_V6_Pin.jpg" width="100%" alt="PSLab V6 Pin Layout"/>
      <br/>PSLab V6 Pin Layout
    </td>
    <td align="center" width="25%">
      <img src="https://raw.githubusercontent.com/rahul31124/GSoC/gsoc_2026/screenshots/OLED_Guide.jpg" width="100%" alt="OLED Guide"/>
      <br/>OLED Guide
    </td>
  </tr>
</table>

## Challenges and Learnings

Working on the PSLab, BadgeMagic, and Magic E-Paper applications provided me with valuable hands-on experience across a wide range of modern software engineering domains, including **mobile and web application development, embedded firmware, hardware and sensor integration, DevOps and CI/CD automation, systems programming with Rust, and Edge Computing**. It allowed me to work across different layers of the software stack, from low-level hardware communication and performance optimization to building user-facing features and deploying cross-platform applications. **All these experiences have helped me become a better, more versatile engineer**, strengthening not only my technical skills but also my ability to solve complex problems, learn new technologies, adapt to unfamiliar challenges, and build reliable and maintainable software.

Some of the most significant learning experiences were:

- **Rust and Flutter Interoperability:** Integrating Rust-based backend components with Flutter while maintaining clear interfaces between native code and the application layer.

- **Low-Level Communication:** Working with USB serial, Wi-Fi, TCP, WebSocket, and I²C communication while addressing the challenges of reliable interaction with physical devices.

- **Firmware Reliability and Data Buffering:** Understanding how differences in UART reception and I²C transmission speeds can cause data loss, and using RAM buffering to improve the reliability of bulk transfers.

- **Hardware Abstraction and Extensibility:** Designing shared abstractions that support different hardware boards while keeping board-specific communication and functionality separate.

- **Performance Optimization:** Identifying bottlenecks in hardware communication and image processing, then migrating performance-critical operations to Rust.

- **Cross-Platform Development:** Maintaining consistent functionality across Android, Windows, Linux, macOS, and Web while accounting for platform-specific requirements.

- **Sensor Integration:** Working with different sensor interfaces, data acquisition, automatic detection, and the integration of additional peripherals into a scientific application.

- **Edge Computing:** Implementing on-device sound classification and exploring local processing capabilities.

- **CI/CD and Release Engineering:** Improving automated workflows, reducing unnecessary CI executions, enabling downloadable firmware artifacts, and optimizing application package size.


## Future Work

I plan to continue improving the PSLab, BadgeMagic, and Magic E-Paper applications, fixing bugs as they arise, enhancing existing features, and contributing new improvements. I also look forward to helping upcoming students and open-source contributors by sharing my experience, providing guidance, and supporting them in contributing.

## Acknowledgements

I would like to express my sincere gratitude to:

- **My mentors at FOSSASIA** for their guidance, feedback, and continued support throughout the project.
- **The PSLab, BadgeMagic, and Magic E-Paper communities** for their collaboration, encouragement, and contributions.
- **Google Summer of Code** for providing this amazing opportunity.
