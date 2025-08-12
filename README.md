# PsNee
                        PPPPPPPPPPPPPPPP                  P            P      
                       P              P                  PP           P        
                      P              P                  P P          P        
                     P              P                  P  P         P          
                    P              P                  P   P        P          
                   P              P                  P    P       P            
                  P              P                  P     P      P            
                 PPPPPPPPPPPPPPPP  PPPPPPPPPPP     P      P     P  PPPPPPPPPPP  PPPPPPPPPPP
                P                 P               P       P    P  P            P
               P                 P               P        P   P  P            P  
              P                 P               P         P  P  P            P  
             P                 P               P          P P  P            P    
            P                 PPPPPPPPPPPPPP  P           PP  PPPPPPP      PPPPPPP    
           P                              P  P            P  P            P      
          P                              P  P            P  P            P        
         P                              P  P            P  P            P        
        P                              P  P            P  P            P      
       P                              P  P            P  P            P        
      P                   PPPPPPPPPPPP  P            P  PPPPPPPPPPP  PPPPPPPPPPP

--------------------------------------------------
This version is from the forum  
http://assemblergames.com/l/threads/psnee-a-stealth-modchip-for-all-ps1-models.57907/
And is developed by THE FRIENDLY FRIETMAN
It is currently HS

--------------------------------------------------
PsNee, an open source stealth modchip for the Sony Playstation 1, usable on
all platforms supported by Arduino, preferably ATTiny. Finally something modern!

## 📚 Documentation

This project now includes comprehensive documentation:

- **[🚀 Quick Start Guide](QUICK_START_GUIDE.md)** - Get running in 5 minutes
- **[📖 Complete API Documentation](API_DOCUMENTATION.md)** - Full user guide with examples
- **[⚙️ Technical API Reference](TECHNICAL_API_REFERENCE.md)** - Developer reference with function signatures

## 🎯 Quick Start

1. **Choose your platform**: Arduino Uno or ATTiny (recommended)
2. **Upload the code**: Use Arduino IDE with correct board selection
3. **Connect hardware**: Wire up to PlayStation motherboard points
4. **Test**: Power on and insert any disc

For detailed setup instructions, see the [Quick Start Guide](QUICK_START_GUIDE.md).

## 🔧 What It Does

PsNee is a stealth modchip that:
- **Bypasses copy protection** on PlayStation 1 games
- **Removes region locking** (plays games from any region)
- **Enables NTSC games** on PAL PlayStation models
- **Works automatically** - no manual intervention needed
- **Goes stealth** after 25 seconds to avoid detection

## 🎮 How It Works

The modchip injects security codes (SCEE, SCEA, SCEI) into the PlayStation's authentication system, tricking it into accepting any disc as legitimate. It's designed to be undetectable and works with all PlayStation 1 models.

## 📋 Requirements

- Arduino-compatible microcontroller (ATTiny25/45/85 recommended)
- PlayStation 1 console
- Basic soldering skills
- Arduino IDE

## 🔗 Community & Support

- **Forum**: [AssemblerGames Thread](http://assemblergames.com/l/threads/psnee-a-stealth-modchip-for-all-ps1-models.57907/)
- **Developer**: THE FRIENDLY FRIETMAN
- **License**: Open Source

---

*For comprehensive usage instructions, troubleshooting, and technical details, see the full documentation above.*
