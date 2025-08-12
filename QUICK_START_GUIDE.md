# PsNee Quick Start Guide

## 🚀 Get Your PlayStation Modchip Running in 5 Minutes

This guide will get you up and running with PsNee as quickly as possible. Follow these steps in order for the best results.

## Prerequisites

- Arduino IDE installed on your computer
- Arduino Uno or ATTiny microcontroller
- USB cable for programming
- Basic soldering skills (or breadboard for testing)

## Step 1: Download and Open the Code

1. **Download** the `PsNee.ino` file
2. **Open Arduino IDE**
3. **Open** the `PsNee.ino` file

## Step 2: Select Your Platform

### For Arduino Uno:
```cpp
#define ARDUINO_UNO        // Uncomment this line
// #define ATTINY           // Comment this line out
```

### For ATTiny (Recommended):
```cpp
// #define ARDUINO_UNO      // Comment this line out
#define ATTINY             // Uncomment this line
```

## Step 3: Connect Your Hardware

### Arduino Uno Pinout:
- **Pin 8** → PlayStation data point
- **Pin 9** → PlayStation gate point  
- **Pin 10** → PlayStation lid sensor
- **Pin 11** → PlayStation BIOS A18 (SCPH-102 only)
- **Pin 12** → PlayStation BIOS D2 (SCPH-102 only)

### ATTiny Pinout:
- **Pin 0** → PlayStation data point
- **Pin 1** → PlayStation gate point
- **Pin 2** → PlayStation lid sensor
- **Pin 3** → PlayStation BIOS A18 (SCPH-102 only)
- **Pin 4** → PlayStation BIOS D2 (SCPH-102 only)

## Step 4: Upload the Code

### Arduino Uno:
1. Select **Tools → Board → Arduino Uno**
2. Click **Upload** button
3. Wait for upload to complete

### ATTiny:
1. Select **Tools → Board → ATTiny25/45/85 (internal 8MHz clock)**
2. Go to **Tools → Burn Bootloader**
3. Wait for bootloader to burn
4. Click **Upload** button
5. Wait for upload to complete

## Step 5: Test Your Modchip

1. **Power on** your PlayStation
2. **Insert any disc** (original or backup)
3. **Watch for** the disc to start loading
4. **Success!** If the game loads, your modchip is working

## 🔧 Troubleshooting Quick Fixes

### Modchip Not Working?
- ✅ Check pin connections
- ✅ Verify correct platform selection
- ✅ Ensure proper power supply
- ✅ Try different timing values

### ATTiny Issues?
- ✅ Burn bootloader first
- ✅ Use internal 8MHz clock
- ✅ Check fuse settings

### Timing Problems?
- ✅ Adjust `delay_ntsc` value
- ✅ Modify `delay_between_bits`
- ✅ Test with known working PlayStation

## 📋 Quick Reference

### Key Functions:
- `inject_playstation()` - Main injection function
- `NTSC_fix()` - NTSC compatibility for PAL PS1
- `inject_SCEE/SCEA/SCEI()` - Region security codes

### Important Variables:
- `delay_ntsc` - NTSC fix timing
- `delay_between_bits` - Bit transmission speed
- `delay_between_injections` - Cycle timing

### Supported Regions:
- **SCEE** - Europe
- **SCEA** - America  
- **SCEI** - Japan

## 🎯 What Happens When It Works

1. **Power On** → Modchip starts injection cycle
2. **25 Seconds** → Complete injection cycle
3. **Idle Mode** → Modchip goes to sleep
4. **Disc Insert** → Game loads normally
5. **Lid Open/Close** → Triggers new injection cycle

## ⚡ Advanced Users

### Custom Timing:
```cpp
int delay_ntsc = 2500;                    // Increase for stability
int delay_between_bits = 5;               // Slower transmission
int delay_between_injections = 80;        // Longer cycles
```

### Platform-Specific Optimization:
```cpp
#ifdef ARDUINO_UNO
  // Arduino Uno optimized settings
  int delay_ntsc = 2350;
  int delay_between_injections = 74;
#endif

#ifdef ATTINY
  // ATTiny optimized settings  
  int delay_ntsc = 2400;
  int delay_between_injections = 68;
#endif
```

## 🆘 Need Help?

- **Check connections** - Most issues are wiring problems
- **Verify timing** - Different PlayStation models need different delays
- **Test hardware** - Use multimeter to check pin states
- **Community support** - Visit the project forum for help

## ✅ Success Checklist

- [ ] Code uploaded successfully
- [ ] Hardware connected properly
- [ ] PlayStation powers on
- [ ] Disc loads and plays
- [ ] Multi-disc games work
- [ ] NTSC games work on PAL PS1 (if applicable)

---

**🎮 You're all set! Enjoy your modded PlayStation!**

*For detailed technical information, see the full API documentation.*