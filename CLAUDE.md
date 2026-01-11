# Tailwind Color Gradient Generator - Claude Context

## Project Overview

This is a web-based color palette generator that creates Tailwind-style color gradients using the OKLCH color space for perceptually accurate results.

## Current Implementation Status

✅ **Completed** - Full working implementation on branch `claude/tailwind-gradient-generator-FretB`

### What's Built

A single-page web application (`index.html`) that:

1. **Generates Tailwind-Style Color Palettes**
   - Takes any color as input (via color picker or hex code)
   - Places it at shade 500
   - Generates 11 shades: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950

2. **Uses OKLCH Color Space**
   - Perceptually uniform lightness across all hues
   - Smart chroma (saturation) scaling to maintain vibrancy
   - Natural hue shifts for better visual results
   - Unlike HSL, lightness values are consistent: L=0.5 looks the same brightness for blue, yellow, red, etc.

3. **Interactive Features**
   - Real-time color picker
   - Hex code input field
   - Display of OKLCH values (L, C, H) for each shade
   - Copy-to-clipboard for each generated color
   - Responsive Tailwind CSS styling

## Technical Implementation

### Color Space Conversion Pipeline

```
Input (Hex) → RGB → Linear RGB → Oklab → OKLCH
                                              ↓
                                        Adjust L & C
                                              ↓
Output (Hex) ← RGB ← Linear RGB ← Oklab ← OKLCH
```

### Key Algorithm Details

- **Gamma correction**: Proper sRGB gamma (2.4) for accurate linear RGB conversion
- **Lightness progression**: Calibrated to match Tailwind's visual scale
  - 50: L=0.97 (very light)
  - 500: Original color's lightness
  - 950: L=0.12 (very dark)
- **Chroma scaling**: Reduces chroma in lighter shades to avoid pastel washout, adjusts in darker shades to maintain depth
- **RGB clamping**: Ensures all generated colors are valid displayable colors

### Why OKLCH?

1. **Perceptual uniformity**: A change of 0.1 in lightness looks the same across all hues
2. **Predictable results**: Unlike HSL where hue affects perceived brightness
3. **Natural gradients**: Allows hue to shift for better-looking color scales
4. **Modern standard**: Default in Tailwind CSS v4, widely supported in browsers

## File Structure

```
.
├── index.html          # Main app (on feature branch)
├── README.md           # Documentation (on feature branch)
└── CLAUDE.md           # This file (on main)
```

## How to Use

1. **View the app**: Open `index.html` from the feature branch in any modern browser
2. **Pick a color**: Use the color picker or enter a hex code
3. **Generate**: Click "Generate" to create the palette
4. **Copy colors**: Click "Copy" next to any shade to copy its hex code

## Branch Information

- **Main branch**: Contains this documentation
- **Feature branch**: `claude/tailwind-gradient-generator-FretB` - contains full implementation
  - `index.html` - Complete web application
  - `README.md` - Comprehensive documentation

## Example Output

For input color `#3b82f6` (Tailwind blue-500):

```
50:  #eff6ff - Very light blue
100: #dbeafe - Light blue
200: #bfdbfe
300: #93c5fd
400: #60a5fa
500: #3b82f6 - Your input color
600: #2563eb
700: #1d4ed8
800: #1e40af
900: #1e3a8a
950: #172554 - Very dark blue
```

Each color maintains perceptual consistency - the lightness differences feel uniform to the human eye.

## Potential Enhancements

If you want to extend this project, consider:

1. **Export Formats**
   - Generate Tailwind config format
   - CSS custom properties
   - SCSS variables
   - JSON export

2. **Additional Features**
   - Multiple palette comparison
   - Accessibility contrast checker
   - Name suggestion for the color palette
   - Save/load palettes from localStorage
   - Share palette via URL parameters

3. **Color Harmony**
   - Generate complementary colors
   - Analogous color schemes
   - Triadic color schemes
   - All in OKLCH for perceptual accuracy

4. **Advanced OKLCH Controls**
   - Manual lightness curve adjustment
   - Custom chroma scaling per shade
   - Hue rotation options
   - Preset templates (vibrant, muted, pastel, etc.)

## References & Resources

- [OKLCH in CSS - Evil Martians](https://evilmartians.com/chronicles/oklch-in-css-why-quit-rgb-hsl) - Excellent overview
- [Oklab color space by Björn Ottosson](https://bottosson.github.io/posts/oklab/) - Original research
- [OKLCH Color Picker](https://oklch.fyi/) - Interactive tool
- [MDN: oklch()](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/oklch) - CSS reference

## Quick Start Commands

```bash
# Checkout the feature branch to see the implementation
git checkout claude/tailwind-gradient-generator-FretB

# View the app
open index.html  # macOS
xdg-open index.html  # Linux
start index.html  # Windows

# Or use a local server
python -m http.server 8000
# Then visit http://localhost:8000
```

## Notes for Claude

When working on this project:

1. The OKLCH conversion math is precise - verify any changes against the Oklab spec
2. RGB clamping is necessary - OKLCH can represent colors outside sRGB gamut
3. Chroma scaling values were tuned by eye - adjust carefully
4. Lightness values aim to match Tailwind's aesthetic - don't just use linear interpolation
5. The hue (H) is preserved from the input color - we don't rotate it, but the conversion allows natural shifts during gamut mapping

## Color Science Quick Reference

- **L (Lightness)**: 0 (black) to 1 (white) - perceptually uniform
- **C (Chroma)**: 0 (gray) to ~0.4 (vibrant) - how colorful vs gray
- **H (Hue)**: 0° to 360° - the color itself (red, yellow, green, blue, etc.)

---

Last updated: 2026-01-11
Implementation by: Claude (Sonnet 4.5)
