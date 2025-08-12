# PsNee Technical API Reference

## Function Reference

### Core Arduino Functions

#### `setup()`
```cpp
void setup()
```
**Purpose**: Arduino framework initialization function
**Called**: Once at startup
**Implementation**: 
```cpp
void setup() {
  inject_playstation();
}
```

#### `loop()`
```cpp
void loop()
```
**Purpose**: Arduino framework main execution loop
**Called**: Continuously after setup
**Implementation**:
```cpp
void loop() {
  if(lid == 0) {
    while(lid != 1);      // Wait for lid to close
    inject_playstation(); // Start new injection cycle
  }
}
```

### PlayStation Injection Functions

#### `inject_playstation()`
```cpp
void inject_playstation()
```
**Purpose**: Main orchestration function for modchip operation
**Parameters**: None
**Returns**: void
**Side Effects**: 
- Modifies DDRB and PORTB registers
- Changes pin modes between INPUT/OUTPUT
- Applies timing delays
**Implementation**:
```cpp
void inject_playstation() {
  NTSC_fix();
  
  delay(6900);
  digitalWrite(data, 0);
  pinMode(data, OUTPUT);
  delay(100);
  
  pinMode(gate, OUTPUT);
  digitalWrite(gate, 0);
  
  for (int loop_counter = 0; loop_counter < 25; loop_counter++) {
    inject_SCEE();
  }
  
  pinMode(gate, INPUT);
  pinMode(data, INPUT);
  delay(11000);
  
  pinMode(gate, OUTPUT);
  digitalWrite(gate, 0);
  
  for (int loop_counter = 0; loop_counter < 60; loop_counter++) {
    inject_SCEE();
  }
  
  pinMode(gate, INPUT);
  pinMode(data, INPUT);
}
```

#### `inject_multiple_times(int number_of_injection_cycles)`
```cpp
void inject_multiple_times(int number_of_injection_cycles)
```
**Purpose**: Executes multiple complete injection cycles
**Parameters**: 
- `number_of_injection_cycles` (int): Number of cycles to execute
**Returns**: void
**Implementation**:
```cpp
void inject_multiple_times(int number_of_injection_cycles) {
  for(int cycle_counter = 0; cycle_counter < number_of_injection_cycles; cycle_counter++) {
    inject_SCEE();
    inject_SCEA();
    inject_SCEI();
  }
}
```

### Security Code Injection Functions

#### `inject_SCEE()`
```cpp
void inject_SCEE()
```
**Purpose**: Injects European region security code
**Parameters**: None
**Returns**: void
**Data Pattern**: 44-bit sequence for SCEE authentication
**Implementation**:
```cpp
void inject_SCEE() {
  FLASH_ARRAY (boolean, SCEEData, 1,0,0,1,1,0,1,0,1,0,0,1,0,0,1,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0);
  
  for (int bit_counter = 0; bit_counter < 44; bit_counter++) {
    if (SCEEData[bit_counter] == 0) {        
      pinMode(data, OUTPUT);
      digitalWrite(data, 0);
      delay(delay_between_bits);
    } else {
      pinMode(data, INPUT);
      delay(delay_between_bits);
    }
  }
  
  pinMode(data, OUTPUT);
  digitalWrite(data, 0);
  delay(delay_between_injections);
}
```

#### `inject_SCEA()`
```cpp
void inject_SCEA()
```
**Purpose**: Injects American region security code
**Parameters**: None
**Returns**: void
**Data Pattern**: 44-bit sequence for SCEA authentication
**Implementation**:
```cpp
void inject_SCEA() {
  FLASH_ARRAY (boolean, SCEAData, 1,0,0,1,1,0,1,0,1,0,0,1,0,0,1,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0,1,0,1,1,1,1,1,0,1,0,0);
  
  for (int bit_counter = 0; bit_counter < 44; bit_counter++) {
    if (SCEAData[bit_counter] == 0) {        
      pinMode(data, OUTPUT);
      digitalWrite(data, 0);
      delay(delay_between_bits);
    } else {
      pinMode(data, INPUT);
      delay(delay_between_bits);
    }
  }
  
  pinMode(data, OUTPUT);
  digitalWrite(data, 0);
  delay(delay_between_injections);
}
```

#### `inject_SCEI()`
```cpp
void inject_SCEI()
```
**Purpose**: Injects Japanese region security code
**Parameters**: None
**Returns**: void
**Data Pattern**: 44-bit sequence for SCEI authentication
**Implementation**:
```cpp
void inject_SCEI() {
  FLASH_ARRAY (boolean, SCEIData, 1,0,0,1,1,0,1,0,1,0,0,1,0,0,1,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0,1,0,1,1,0,1,1,0,1,0,0);
  
  for (int bit_counter = 0; bit_counter < 44; bit_counter++) {
    if (SCEIData[bit_counter] == 0) {        
      pinMode(data, OUTPUT);
      digitalWrite(data, 0);
      delay(delay_between_bits);
    } else {
      pinMode(data, INPUT);
      delay(delay_between_bits);
    }
  }
  
  pinMode(data, OUTPUT);
  digitalWrite(data, 0);
  delay(delay_between_injections);
}
```

### NTSC Compatibility Functions

#### `NTSC_fix()`
```cpp
void NTSC_fix()
```
**Purpose**: Applies NTSC BIOS patch for PAL SCPH-102 PlayStation
**Parameters**: None
**Returns**: void
**Hardware Requirements**: Requires biosA18 and biosD2 pin connections
**Implementation**:
```cpp
void NTSC_fix() {
  // Make all pins inputs
  DDRB = 0x00;
  
  // Wait until just before the pulse on BIOS A18 arrives
  delay(delay_ntsc);
  
  // Wait for BIOS A18 signal
  while(!(PINB & B00001000)) {
    ;  // Wait
  }
  
  delayMicroseconds(12);
  PORTB = B00000000;
  DDRB = B00010000;
  delayMicroseconds(5);
  DDRB = 0x00;
}
```

## Data Structures

### Security Code Arrays
All security codes use the `FLASH_ARRAY` macro to store data in program memory:

```cpp
// SCEE (European) - 44 bits
FLASH_ARRAY (boolean, SCEEData, 1,0,0,1,1,0,1,0,1,0,0,1,0,0,1,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0);

// SCEA (American) - 44 bits  
FLASH_ARRAY (boolean, SCEAData, 1,0,0,1,1,0,1,0,1,0,0,1,0,0,1,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0,1,0,1,1,1,1,1,0,1,0,0);

// SCEI (Japanese) - 44 bits
FLASH_ARRAY (boolean, SCEIData, 1,0,0,1,1,0,1,0,1,0,0,1,0,0,1,1,1,1,0,1,0,0,1,0,1,0,1,1,1,0,1,0,0,1,0,1,1,0,1,1,0,1,0,0);
```

### Bit Structure
Each security code follows this 44-bit pattern:
- **Start Bit**: Always 1
- **Data Bits**: 40 bits representing the region code
- **Stop Bits**: 3 bits (always 0)

## Configuration Macros

### Platform Selection
```cpp
#define ARDUINO_UNO        // Arduino Uno with ATMega328
// #define ATTINY           // ATTiny25/45/85 (default)
```

### Conditional Compilation
The code uses preprocessor directives to configure platform-specific settings:

```cpp
#ifdef ARDUINO_UNO
  // Arduino Uno configuration
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
  // ATTiny configuration
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

## Global Variables

### Pin Definitions
```cpp
int data;        // Data output pin
int gate;        // Gate control pin
int lid;         // CD lid sensor pin
int biosA18;     // BIOS A18 pin for NTSC fix
int biosD2;      // BIOS D2 pin for NTSC fix
```

### Timing Parameters
```cpp
int delay_ntsc;                    // Delay before NTSC fix (microseconds)
int delay_between_bits;            // Delay between bit transmissions
int delay_between_injections;      // Delay between injection cycles
```

## Hardware Register Usage

### DDRB (Data Direction Register B)
- Used to configure pins as inputs or outputs
- Modified in `NTSC_fix()` function
- Pattern: `B00010000` for specific pin control

### PORTB (Port B Data Register)
- Used to set pin states
- Modified in `NTSC_fix()` function
- Pattern: `B00000000` for clearing pins

### PINB (Port B Input Pins)
- Used to read pin states
- Monitored in `NTSC_fix()` function
- Pattern: `B00001000` for checking biosA18

## Dependencies

### Required Libraries
```cpp
#include <Flash.h>  // For FLASH_ARRAY macro
```

### Arduino Framework
- Requires standard Arduino functions: `pinMode()`, `digitalWrite()`, `delay()`, `delayMicroseconds()`
- Uses Arduino pin numbering system
- Compatible with Arduino IDE compilation

## Memory Usage

### Program Memory
- Security code arrays stored in flash memory using `FLASH_ARRAY`
- 44 bits × 3 regions = 132 bits of security data
- Minimal RAM usage for variables

### RAM Usage
- Pin variables: 5 × 2 bytes = 10 bytes
- Timing variables: 3 × 2 bytes = 6 bytes
- Loop counters: 2 × 2 bytes = 4 bytes
- **Total**: ~20 bytes RAM

## Timing Specifications

### Critical Timing Values
```cpp
// Arduino Uno
int delay_ntsc = 2350;                    // 2.35ms
int delay_between_bits = 4;               // 4ms
int delay_between_injections = 74;        // 74ms

// ATTiny
int delay_ntsc = 2400;                    // 2.4ms
int delay_between_bits = 4;               // 4ms
int delay_between_injections = 68;        // 68ms
```

### Operation Timing
- **Initial Injection**: ~25 seconds
- **Idle Period**: Indefinite until lid change
- **Re-injection**: Triggered by CD lid open/close
- **Bit Transmission**: 4ms per bit
- **Complete Cycle**: ~25 injection cycles

## Error Handling

### No Explicit Error Handling
- Functions do not return error codes
- Hardware failures may cause silent failures
- Timing-sensitive operations may fail if interrupts occur

### Debugging Support
- No built-in debugging output
- Requires external tools (logic analyzer, oscilloscope)
- Pin state monitoring for troubleshooting

## Performance Characteristics

### Execution Speed
- **Setup Phase**: ~25 seconds for initial injection
- **Main Loop**: Minimal CPU usage during idle
- **Re-injection**: ~25 seconds when triggered

### Power Consumption
- **Active**: Higher during injection cycles
- **Idle**: Minimal power consumption
- **Stealth Mode**: Reduced power after 25 seconds

## Compatibility Notes

### Arduino Versions
- Compatible with Arduino 1.0+
- Uses standard Arduino functions
- No version-specific dependencies

### Microcontroller Variants
- **ATTiny25/45/85**: Fully supported, recommended
- **ATMega328**: Fully supported
- **Other AVR**: May work with pin modifications

### PlayStation Models
- **SCPH-102 PAL**: Full NTSC compatibility support
- **Other Models**: Basic copy protection bypass
- **Region Support**: All regions (SCEE, SCEA, SCEI)