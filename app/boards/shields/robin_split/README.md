# Robin's 3x6+3 Split Keyboard - ZMK Configuration

## Overview
This is a ZMK shield for a wireless split keyboard with:
- 3 rows × 6 columns per side
- 3-key thumb cluster per side  
- Total: 42 keys (21 per side)
- BLE wireless connection between halves
- ZMK Studio support for runtime keymap editing

## GPIO Pin Mapping

### nice!nano v2 Pin Assignments

**Rows (shared by both sides):**
- Row 0: P1.15 (pro_micro 18 / D18)
- Row 1: P1.13 (pro_micro 15 / D15)
- Row 2: P1.11 (pro_micro 14 / D14)

**Columns (6 per side):**
- Col 0: P0.17 (pro_micro 2 / D2)
- Col 1: P0.20 (pro_micro 3 / D3)
- Col 2: P0.22 (pro_micro 4 / D4)
- Col 3: P0.24 (pro_micro 5 / D5)
- Col 4: P1.00 (pro_micro 6 / D6)
- Col 5: P0.11 (pro_micro 7 / D7)

## File Structure

```
robin_split_keyboard/
├── robin_split.dtsi              # Main matrix and transform definition
├── robin_split_left.overlay      # Left side GPIO configuration
├── robin_split_right.overlay     # Right side GPIO configuration
├── robin_split.keymap            # Default keymap (QWERTY)
├── robin_split.conf              # Main config (ZMK Studio enabled)
├── robin_split_left.conf         # Left-specific config (empty)
├── robin_split_right.conf        # Right-specific config (empty)
├── Kconfig.shield                # Shield definition
└── Kconfig.defconfig             # Default shield configuration
```

## Setup Instructions

### 1. Create ZMK Config Repo
If you don't already have a ZMK config repo:
```bash
# Use the ZMK config template
https://github.com/zmkfirmware/unified-zmk-config-template
```

### 2. Add Shield Files
Copy all files from this directory to your config repo:
```
your-zmk-config/
└── config/
    └── boards/
        └── shields/
            └── robin_split/
                ├── robin_split.dtsi
                ├── robin_split_left.overlay
                ├── robin_split_right.overlay
                ├── robin_split.keymap
                ├── robin_split.conf
                ├── robin_split_left.conf
                ├── robin_split_right.conf
                ├── Kconfig.shield
                └── Kconfig.defconfig
```

### 3. Update build.yaml
Edit `build.yaml` in your config repo root:

```yaml
include:
  - board: nice_nano_v2
    shield: robin_split_left
  - board: nice_nano_v2
    shield: robin_split_right
```

### 4. Build Firmware
Push to GitHub - Actions will automatically build both left and right UF2 files.

### 5. Flash nice!nano Controllers

**Left side (Central/Master):**
1. Download `nice_nano_v2-robin_split_left-zmk.uf2`
2. Double-tap reset button on nice!nano
3. Drag UF2 to NICENANO drive
4. Wait for reboot

**Right side (Peripheral):**
1. Download `nice_nano_v2-robin_split_right-zmk.uf2`
2. Double-tap reset button on nice!nano
3. Drag UF2 to NICENANO drive
4. Wait for reboot

## Usage

### Power Setup
**Option 1: USB-C Power Only**
- Left (Central): USB-C to computer (data + power)
- Right (Peripheral): USB-C power adapter or battery

**Option 2: Both Battery Powered**
- Both sides run on batteries
- Left connects to computer via USB when needed

### BLE Pairing
1. Power on both halves
2. Left (central) will automatically connect to right (peripheral) via BLE
3. Connect left side to computer via Bluetooth or USB
4. Computer sees ONE keyboard

### ZMK Studio
With ZMK Studio enabled, you can edit keymaps in real-time:
1. Connect keyboard via USB
2. Open ZMK Studio in browser
3. Edit keymap visually
4. Changes take effect immediately

## Customization

### Modify Keymap
Edit `robin_split.keymap` to change key assignments.

### Add Features
Edit `robin_split.conf`:
- Enable underglow: `CONFIG_ZMK_RGB_UNDERGLOW=y`
- Enable display: `CONFIG_ZMK_DISPLAY=y`
- USB logging: `CONFIG_ZMK_USB_LOGGING=y`

### Matrix Direction
Currently set to `col2row` (diodes point from columns to rows).
If your diodes are reversed, change in `robin_split.dtsi`:
```c
diode-direction = "row2col";
```

## Default Keymap Layout

```
Layer 0 (Base):
┌─────┬─────┬─────┬─────┬─────┬─────┐       ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ TAB │  Q  │  W  │  E  │  R  │  T  │       │  Y  │  U  │  I  │  O  │  P  │BSPC │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│CTRL │  A  │  S  │  D  │  F  │  G  │       │  H  │  J  │  K  │  L  │  ;  │  '  │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│SHFT │  Z  │  X  │  C  │  V  │  B  │       │  N  │  M  │  ,  │  .  │  /  │ ESC │
└─────┴─────┴─────┼─────┼─────┼─────┤       ├─────┼─────┼─────┼─────┴─────┴─────┘
                  │ GUI │ LWR │ SPC │       │ ENT │ RSE │ ALT │
                  └─────┴─────┴─────┘       └─────┴─────┴─────┘

Layer 1 (Lower): Numbers, Navigation, Bluetooth
Layer 2 (Raise): Symbols
```

## Troubleshooting

**Sides won't pair:**
- Clear Bluetooth bonds: Hold Lower+Q+W on left side during boot
- Check both sides are powered
- Verify firmware flashed correctly

**Keys not working:**
- Check matrix wiring matches GPIO definitions
- Verify diode direction
- Test with multimeter in continuity mode

**USB not recognized:**
- Try different USB cable
- Ensure nice!nano is detected as bootloader (double-tap reset)
- Check USB port isn't power-only

## Next Steps
1. Test all keys work
2. Customize keymap to your preference
3. Add features (RGB, display, encoders) as needed
4. Use ZMK Studio for quick keymap iterations

Enjoy your wireless split keyboard! 🎹
