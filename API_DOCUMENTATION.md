# PsNee API Documentation

## Table of Contents
1. [Overview](#overview)
2. [Project Information](#project-information)
3. [Hardware Requirements](#hardware-requirements)
4. [Pin Configuration](#pin-configuration)
5. [Configuration Options](#configuration-options)
6. [Public Functions](#public-functions)
7. [Global Variables](#global-variables)
8. [Usage Instructions](#usage-instructions)
9. [Examples](#examples)
10. [Troubleshooting](#troubleshooting)
11. [Version History](#version-history)

## Overview

PsNee is an open-source stealth modchip for the Sony PlayStation 1, designed to work on all Arduino-compatible platforms with a preference for ATTiny microcontrollers. The modchip bypasses the PlayStation's copy protection and region locking by injecting specific security codes into the system.

## Project Information

- **Project Name**: PsNee
- **Version**: 6
- **Developer**: THE FRIENDLY FRIETMAN
- **License**: Open Source
- **Last Updated**: May 14, 2016
- **Target Platform**: Arduino-compatible boards (preferably ATTiny25/45/85)

## Hardware Requirements

### Supported Platforms
- **Arduino Uno** (with ATMega328)
- **ATTiny25/45/85** (recommended)

### Required Components
- Arduino-compatible microcontroller
- Connecting wires
- PlayStation 1 motherboard access points
- Power supply (5V from PlayStation)

## Pin Configuration

### Arduino Uno (ATMega328)
```cpp
int data = 8;         // Data output pin (ATMega pin 14)
int gate = 9;         // Gate control pin (ATMega pin 15)
int lid = 10;         // CD lid sensor pin (ATMega pin 16)
int biosA18 = 11;     // BIOS A18 pin for SCPH-102 PAL mode (ATMega pin 17)
int biosD2 = 12;      // BIOS D2 pin for SCPH-102 PAL mode (ATMega pin 18)
```

### ATTiny25/45/85
```cpp
int data = 0;         // Data output pin (ATTiny pin 5)
int gate = 1;         // Gate control pin (ATTiny pin 6)
int lid = 2;          // CD lid sensor pin (ATTiny pin 7)
int biosA18 = 3;      // BIOS A18 pin for SCPH-102 PAL mode (ATTiny pin 2)
int biosD2 = 4;       // BIOS D2 pin for SCPH-102 PAL mode (ATTiny pin 3)
```

## Configuration Options

### Platform Selection
```cpp
#define ARDUINO_UNO        // Uncomment for Arduino Uno
// #define ATTINY           // Uncomment for ATTiny (default)
```

### Timing Parameters

#### Arduino Uno
```cpp
int delay_ntsc = 2350;                    // Delay before NTSC fix (microseconds)
int delay_between_bits = 4;               // Delay between bit transmissions
int delay_between_injections = 74;        // Delay between injection cycles
```

#### ATTiny
```cpp
int delay_ntsc = 2400;                    // Delay before NTSC fix (microseconds)
int delay_between_bits = 4;               // Delay between bit transmissions
int delay_between_injections = 68;        // Delay between injection cycles
```

## Public Functions

### Core Functions

#### `void setup()`
**Description**: Main initialization function called once at startup.
**Purpose**: Initiates the first PlayStation injection cycle.
**Usage**: Automatically called by Arduino framework.
**Example**: 
```cpp
void setup() {
  inject_playstation();
}
```

#### `void loop()`
**Description**: Main execution loop called continuously after setup.
**Purpose**: Monitors CD lid status and triggers new injection cycles when needed.
**Usage**: Automatically called by Arduino framework.
**Example**:
```cpp
void loop() {
  if(lid == 0) {
    while(lid != 1);      // Wait for lid to close
    inject_playstation(); // Start new injection cycle
  }
}
```

### Injection Functions

#### `void inject_playstation()`
**Description**: Main injection function that orchestrates the entire modchip operation.
**Purpose**: Performs NTSC fix, injects security codes, and manages timing.
**Parameters**: None
**Returns**: void
**Usage**: Called automatically by setup() and when CD lid is closed.
**Example**:
```cpp
inject_playstation(); // Trigger injection cycle
```

#### `void inject_multiple_times(int number_of_injection_cycles)`
**Description**: Injects all three security codes multiple times.
**Purpose**: Provides redundancy and ensures successful bypass.
**Parameters**: 
- `number_of_injection_cycles` (int): Number of complete injection cycles
**Returns**: void
**Usage**: Called internally by inject_playstation().
**Example**:
```cpp
inject_multiple_times(25); // Inject 25 complete cycles
```

#### `void inject_SCEE()`
**Description**: Injects the SCEE (Sony Computer Entertainment Europe) security code.
**Purpose**: Provides European region authentication.
**Parameters**: None
**Returns**: void
**Usage**: Called internally by injection functions.
**Example**:
```cpp
inject_SCEE(); // Inject European security code
```

#### `void inject_SCEA()`
**Description**: Injects the SCEA (Sony Computer Entertainment America) security code.
**Purpose**: Provides American region authentication.
**Parameters**: None
**Returns**: void
**Usage**: Called internally by injection functions.
**Example**:
```cpp
inject_SCEA(); // Inject American security code
```

#### `void inject_SCEI()`
**Description**: Injects the SCEI (Sony Computer Entertainment Japan) security code.
**Purpose**: Provides Japanese region authentication.
**Parameters**: None
**Returns**: void
**Usage**: Called internally by injection functions.
**Example**:
```cpp
inject_SCEI(); // Inject Japanese security code
```

### NTSC Fix Functions

#### `void NTSC_fix()`
**Description**: Implements the NTSC BIOS patch for PAL SCPH-102 PlayStation models.
**Purpose**: Allows PAL PlayStation to play NTSC games by manipulating BIOS signals.
**Parameters**: None
**Returns**: void
**Usage**: Called automatically by inject_playstation().
**Example**:
```cpp
NTSC_fix(); // Apply NTSC compatibility fix
```

## Global Variables

### Pin Definitions
All pin variables are defined based on the selected platform (Arduino Uno or ATTiny).

### Timing Variables
- `delay_ntsc`: Delay before applying NTSC fix
- `delay_between_bits`: Delay between individual bit transmissions
- `delay_between_injections`: Delay between complete injection cycles

## Usage Instructions

### 1. Platform Selection
1. Open `PsNee.ino` in Arduino IDE
2. Uncomment the appropriate platform definition:
   ```cpp
   #define ARDUINO_UNO        // For Arduino Uno
   // #define ATTINY           // For ATTiny (default)
   ```

### 2. Hardware Connection
1. Connect the modchip to your PlayStation motherboard using the pinout specified for your platform
2. Ensure proper power supply (5V from PlayStation)
3. Connect the CD lid sensor to detect disc changes

### 3. Compilation and Upload
1. Select the correct board in Arduino IDE
2. For ATTiny: Use "Extra=>Board=>ATTiny45 (internal 8MHz clock)"
3. Burn bootloader if using ATTiny (Extra=>Burn bootloader)
4. Upload the code to your device

### 4. Operation
1. Power on the PlayStation
2. The modchip automatically performs the initial injection
3. Insert any disc (original or backup)
4. The modchip will bypass copy protection and region locking
5. For multi-disc games, open and close the CD lid to trigger new injection cycles

## Examples

### Basic Usage
```cpp
// The modchip works automatically after upload
// No additional code modifications required
```

### Custom Timing (Advanced Users)
```cpp
// Modify timing parameters for specific PlayStation models
int delay_ntsc = 2500;                    // Increase NTSC fix delay
int delay_between_bits = 5;               // Increase bit transmission delay
int delay_between_injections = 80;        // Increase injection cycle delay
```

### Platform-Specific Configuration
```cpp
#ifdef ARDUINO_UNO
  // Arduino Uno specific settings
  int data = 8;
  int gate = 9;
  int lid = 10;
  int biosA18 = 11;
  int biosD2 = 12;
  int delay_ntsc = 2350;
  int delay_between_bits = 4;
  int delay_between_injections = 74;
#endif

#ifdef ATTINY
  // ATTiny specific settings
  int data = 0;
  int gate = 1;
  int lid = 2;
  int biosA18 = 3;
  int biosD2 = 4;
  int delay_ntsc = 2400;
  int delay_between_bits = 4;
  int delay_between_injections = 68;
#endif
```

## Troubleshooting

### Common Issues

#### Modchip Not Working
- Verify correct platform selection
- Check pin connections
- Ensure proper power supply
- Verify timing parameters for your PlayStation model

#### ATTiny Issues
- Ensure correct board selection in Arduino IDE
- Burn bootloader before uploading
- Verify fuse settings
- Use internal 8MHz clock setting

#### Timing Problems
- Adjust `delay_ntsc` for different PlayStation models
- Modify `delay_between_bits` if bit transmission fails
- Adjust `delay_between_injections` for optimal performance

### Debug Tips
1. Use logic analyzer to verify signal timing
2. Check pin states with multimeter
3. Verify power supply stability
4. Test with known working PlayStation model first

## Version History

### Version 6 (Current)
- PAL SCPH-102 NTSC BIOS-patch implementation
- Function-based code structure
- Perfected timing for all disc types
- Simplified gate signal usage
- AVR PORTB-only implementation

### Version 2
- Gate pin-based PlayStation model detection
- NTSC support for PAL SCPH-102
- Improved data signal slicing
- Combined loop optimization
- Enhanced pinout configuration

### Key Improvements
- **Stealth Operation**: Modchip idles after 25 seconds to avoid detection
- **Multi-Disc Support**: Automatic re-injection when CD lid is opened/closed
- **Region Compatibility**: Supports all PlayStation regions (SCEE, SCEA, SCEI)
- **NTSC Compatibility**: Allows PAL PlayStation to play NTSC games
- **Timing Optimization**: Fine-tuned delays for optimal performance

## Security and Legal Notice

This documentation is provided for educational and research purposes only. Users are responsible for ensuring compliance with local laws and regulations regarding the modification of gaming hardware. The developers are not responsible for any misuse of this software.

## Support and Community

- **Forum**: http://assemblergames.com/l/threads/psnee-a-stealth-modchip-for-all-ps1-models.57907/
- **Developer**: THE FRIENDLY FRIETMAN
- **Project Status**: Currently maintained by the community

---

*This documentation covers all public APIs, functions, and components of the PsNee project as of Version 6. For the most up-to-date information, please refer to the project repository and community forums.*