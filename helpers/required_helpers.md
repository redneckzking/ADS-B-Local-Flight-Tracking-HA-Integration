# Required Helpers Setup

This integration requires four helpers to be created in Home Assistant. These provide the configuration options for altitude threshold, distance threshold, announcements toggle, and tracking the last announced flight.

## Creating Helpers via UI

Go to **Settings → Devices & Services → Helpers → Create Helper**

### 1. Altitude Threshold (input_number)

- **Type**: Number
- **Name**: ADSB Altitude Threshold
- **Entity ID**: `input_number.adsb_altitude_threshold`
- **Icon**: `mdi:airplane-marker`
- **Minimum**: 0
- **Maximum**: 50000
- **Step**: 100
- **Unit of measurement**: ft
- **Mode**: Slider
- **Default value**: 15000

This controls the maximum altitude (in feet) for aircraft announcements. Only aircraft at or below this altitude will be announced.

### 2. Distance Threshold (input_number)

- **Type**: Number
- **Name**: ADSB Distance Threshold
- **Entity ID**: `input_number.adsb_distance_threshold`
- **Icon**: `mdi:map-marker-distance`
- **Minimum**: 0
- **Maximum**: 500
- **Step**: 1
- **Unit of measurement**: miles
- **Mode**: Slider
- **Default value**: 50

This controls the maximum distance (in miles) for aircraft announcements. Only aircraft within this radius will be announced.

### 3. Announcements Enabled Toggle (input_boolean)

- **Type**: Toggle
- **Name**: ADSB Announcements Enabled
- **Entity ID**: `input_boolean.adsb_announcements_enabled`
- **Icon**: `mdi:volume-high`
- **Default state**: On

This is your master on/off switch for aircraft announcements.

### 4. Last Announced Flight (input_text)

- **Type**: Text
- **Name**: ADSB Last Announced Flight
- **Entity ID**: `input_text.adsb_last_announced_flight`
- **Icon**: `mdi:airplane`
- **Mode**: Text
- **Maximum length**: 255
- **Default value**: (leave empty)

This stores the callsign of the last announced aircraft to prevent repeat announcements.

---

## Creating Helpers via YAML (Alternative Method)

If you prefer YAML configuration, add this to your `configuration.yaml`:

```yaml
# ADS-B Helpers
input_number:
  adsb_altitude_threshold:
    name: ADSB Altitude Threshold
    min: 0
    max: 50000
    step: 100
    unit_of_measurement: "ft"
    icon: mdi:airplane-marker
    mode: slider
    initial: 15000

  adsb_distance_threshold:
    name: ADSB Distance Threshold
    min: 0
    max: 500
    step: 1
    unit_of_measurement: "miles"
    icon: mdi:map-marker-distance
    mode: slider
    initial: 50

input_boolean:
  adsb_announcements_enabled:
    name: ADSB Announcements Enabled
    icon: mdi:volume-high
    initial: on

input_text:
  adsb_last_announced_flight:
    name: ADSB Last Announced Flight
    max: 255
    icon: mdi:airplane
```

After adding to configuration.yaml, restart Home Assistant.

---

## Verification

After creating the helpers, verify they exist:

1. Go to **Developer Tools → States**
2. Search for "adsb"
3. You should see all four helpers listed:
   - `input_number.adsb_altitude_threshold`
   - `input_number.adsb_distance_threshold`
   - `input_boolean.adsb_announcements_enabled`
   - `input_text.adsb_last_announced_flight`

## Customization

You can adjust these defaults to suit your needs:

- **Altitude Threshold**: 
  - Low values (0-5000 ft) = Only very low aircraft (landing/departing)
  - Medium values (5000-15000 ft) = General aviation and some commercial
  - High values (15000-50000 ft) = All aircraft including high-altitude commercial

- **Distance Threshold**:
  - Small radius (0-10 miles) = Immediate area only
  - Medium radius (10-50 miles) = Local region
  - Large radius (50-500 miles) = Wide area tracking

Set distance to 500 miles to effectively disable distance filtering.
