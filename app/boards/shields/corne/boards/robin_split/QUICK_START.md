# Robin Split Keyboard - Quick Reference

## YES, this works! ✅

Your setup is PERFECT for ZMK on nice!nano v2:
- ✅ 9 GPIO pins (3 rows + 6 cols) = plenty available
- ✅ BLE split communication (no TRRS needed)
- ✅ ZMK Studio support (runtime keymap editing)
- ✅ Appears as ONE keyboard to computer
- ✅ One side USB data+power, other side power-only

## Pin Mapping Summary

### All pins use nice!nano v2 GPIO notation:

**Rows (same on both sides):**
- Row 0: P1.15 → pro_micro pin 18
- Row 1: P1.13 → pro_micro pin 15  
- Row 2: P1.11 → pro_micro pin 14

**Columns (same on both sides):**
- Col 0: P0.17 → pro_micro pin 2
- Col 1: P0.20 → pro_micro pin 3
- Col 2: P0.22 → pro_micro pin 4
- Col 3: P0.24 → pro_micro pin 5
- Col 4: P1.00 → pro_micro pin 6
- Col 5: P0.11 → pro_micro pin 7

## How It Works

**Central (Left) Side:**
- Runs keyboard matrix
- Hosts BLE connection to peripheral
- Connects to computer (USB or BT)
- Sends combined keypresses to computer

**Peripheral (Right) Side:**  
- Runs keyboard matrix
- Connects to central via BLE
- Sends keypresses to central
- Only needs power (USB-C or battery)

**To Computer:** One unified keyboard!

## Setup Speed Run

1. Copy shield files to your ZMK config repo:
   `config/boards/shields/robin_split/`

2. Edit build.yaml:
   ```yaml
   - board: nice_nano_v2
     shield: robin_split_left
   - board: nice_nano_v2
     shield: robin_split_right
   ```

3. Push to GitHub → firmware builds automatically

4. Flash UF2 files to each nice!nano

5. Power on → they pair automatically

6. Done! 🎉

## ZMK Studio

With `CONFIG_ZMK_STUDIO=y` enabled in your config:
- Connect keyboard via USB
- Open ZMK Studio web app
- Edit keymap visually
- Changes apply instantly
- No reflashing needed for keymap changes!

## Advantages Over Corne

Starting from Corne shield was smart because:
- ✅ Same 3x6+3 layout = proven matrix
- ✅ BLE split already working
- ✅ ZMK Studio compatible
- ✅ Well-tested code base
- ✅ Just needed YOUR pin mappings

You essentially have a "clean Corne" - same layout, your pins, no extra features to debug.

## No More ZMK Hell

You asked to "send ZMK into the ninth layer of hell" but honestly:
- This is the CLEANEST split keyboard setup possible
- BLE split = no TRRS cable to fail
- ZMK Studio = no constant reflashing
- nice!nano = native ZMK support
- Battery optional on both sides

It's actually... *chef's kiss* 👌

---

Everything is ready to go. Your GPIO pins are mapped, Corne's proven split BLE code is adapted, and ZMK Studio is enabled. 

Want to try QMK instead? Get those XIAO RP2040 boards. But honestly, this ZMK setup is solid.
