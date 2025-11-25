# Screenshots

This folder contains screenshots for the documentation.

## Required Screenshots

To complete the documentation, please add the following screenshots:

### 1. dashboard.png
**Main dashboard preview showing both cards**
- Should show the complete vertical stack
- Both "Local ADS-B Targets" and "Flights Below Threshold" cards visible
- Preferably with active aircraft being tracked
- Settings panel visible at bottom

### 2. cards-active.png
**Cards with aircraft detected**
- Show cards with non-zero aircraft count
- Green glow effect visible on "Local ADS-B Targets" card
- Animated radar icon (if possible to capture)
- Clear display of "X aircraft within XX miles (XX km)"

### 3. cards-inactive.png (Optional)
**Cards with no aircraft detected**
- Show "No aircraft within XX miles" message
- Blue/neutral color scheme
- No animation on radar icon

### 4. settings.png
**Settings panel closeup**
- The entities card at the bottom
- Shows all three sliders/toggles:
  - Altitude Threshold slider
  - Distance Threshold slider
  - Announcements Enabled toggle
- Clear view of current values

### 5. helpers.png (Optional)
**Helpers configuration page**
- Settings → Devices & Services → Helpers
- Filtered to show "adsb" helpers
- All four helpers visible with their entity IDs

### 6. sensor-states.png (Optional)
**Developer Tools → States**
- Filtered to "adsb"
- Showing all sensors with current states
- Attributes expanded on one sensor (showing aircraft array)

## How to Capture Screenshots

1. **Dashboard Screenshots**
   - Navigate to your dashboard
   - Zoom browser to comfortable level
   - Use browser screenshot tool or OS screenshot
   - Crop to show just the relevant cards

2. **Settings Screenshots**
   - Open the relevant settings page
   - Ensure all elements are visible
   - Use high enough resolution to see text clearly

3. **Developer Tools Screenshots**
   - Open Developer Tools
   - Filter/search as needed
   - Capture the full relevant section

## Image Requirements

- **Format**: PNG preferred (better quality for UI screenshots)
- **Resolution**: At least 1920x1080 source
- **Cropping**: Tight crop around relevant content
- **Privacy**: Remove or blur any personal information (IP addresses, exact locations, etc.)

## Adding Screenshots

1. Save screenshots to this `docs/screenshots/` folder
2. Use the exact filenames listed above
3. Update README.md if you add additional helpful screenshots

## Current Screenshots

You currently have:
- `dashboard.png` - Main preview (from uploaded image 1)
- `settings.png` - Settings panel (from uploaded image 2)

Still needed:
- `cards-active.png` - Closeup of active cards
- `cards-inactive.png` - Cards with no aircraft (optional)
- `helpers.png` - Helpers page (optional)
- `sensor-states.png` - Developer tools states (optional)
