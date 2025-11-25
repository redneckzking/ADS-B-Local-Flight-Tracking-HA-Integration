# Installation Guide

Step-by-step instructions to set up ADS-B aircraft tracking and announcements in Home Assistant.

## Prerequisites

Before starting, ensure you have:

### Required
- ✅ **Home Assistant** (tested on Core 2025.11.3, OS 16.3)
- ✅ **Local ADS-B receiver** running tar1090 (ADS-B Exchange, readsb, dump1090-fa, etc.)
- ✅ **Network access** to your tar1090 server from Home Assistant

### Optional but Recommended
- ✅ **HACS** (Home Assistant Community Store) - for easy custom card installation
- ✅ **Piper TTS** - for natural-sounding voice announcements
- ✅ **Media players** - speakers for announcements
- ✅ **Mobile device with Home Assistant app** - for presence detection

---

## Part 1: Install Custom Components

### Install Button Card (Required for Dashboard)

#### Method 1: Via HACS (Recommended)

1. Open HACS in Home Assistant
2. Click **Frontend**
3. Click the **+** button
4. Search for "button card"
5. Click **Button Card** by custom-cards
6. Click **Download**
7. Restart Home Assistant

#### Method 2: Manual Installation

1. Download [button-card.js](https://github.com/custom-cards/button-card/releases) from GitHub
2. Copy to `/config/www/button-card.js`
3. Add to your Lovelace resources:
   - Go to Settings → Dashboards → ⋮ Menu → Resources
   - Add resource: `/local/button-card.js` as "JavaScript Module"

---

## Part 2: Verify tar1090 Access

Before configuring Home Assistant, verify your tar1090 server is accessible:

1. **Find your tar1090 IP address**
   - If running on same network, check your router or use `ifconfig`
   - Example: `192.168.1.77`

2. **Test in a browser**
   - Open: `http://YOUR_ADSB_IP/tar1090/data/aircraft.json`
   - You should see JSON data with aircraft information
   - Example: `http://192.168.1.77/tar1090/data/aircraft.json`

3. **Verify data structure**
   - Look for `"aircraft":` array with objects
   - Each object should have fields like `hex`, `flight`, `alt_baro`, `lat`, `lon`

If you can't access this URL, troubleshoot your tar1090 installation first.

---

## Part 3: Configure Home Assistant

### Step 1: Add REST Sensor

The REST sensor pulls aircraft data from tar1090.

**Option A: Using separate file (Recommended)**

1. Create file: `/config/rest_sensor.yaml`
2. Copy contents from [`configuration/rest_sensor.yaml`](../configuration/rest_sensor.yaml)
3. **IMPORTANT**: Change the IP address to your tar1090 server
4. Add to your `configuration.yaml`:
   ```yaml
   rest: !include rest_sensor.yaml
   ```

**Option B: Direct in configuration.yaml**

Add this to your `configuration.yaml`:

```yaml
rest:
  - platform: rest
    name: "ADSB Aircraft Raw"
    resource: http://YOUR_ADSB_IP/tar1090/data/aircraft.json  # CHANGE THIS
    scan_interval: 5
    timeout: 10
    value_template: "{{ value_json.aircraft | count }}"
    json_attributes:
      - aircraft
```

### Step 2: Add Template Sensors

Template sensors process the raw aircraft data.

**Option A: Using separate file (Recommended)**

1. Create file: `/config/template_sensors.yaml`
2. Copy contents from [`configuration/template_sensors.yaml`](../configuration/template_sensors.yaml)
3. Add to your `configuration.yaml`:
   ```yaml
   template: !include template_sensors.yaml
   ```

**Option B: Direct in configuration.yaml**

Add the template sensors from [`configuration/template_sensors.yaml`](../configuration/template_sensors.yaml) to your `configuration.yaml` under the `template:` section.

### Step 3: Create Helpers

Follow the instructions in [`helpers/required_helpers.md`](../helpers/required_helpers.md) to create:

- `input_number.adsb_altitude_threshold`
- `input_number.adsb_distance_threshold`
- `input_boolean.adsb_announcements_enabled`
- `input_text.adsb_last_announced_flight`

### Step 4: Restart Home Assistant

**Settings → System → Restart Home Assistant**

Wait for restart to complete.

### Step 5: Verify Sensors

After restart, check that sensors are working:

1. Go to **Developer Tools → States**
2. Search for "adsb"
3. Verify these sensors exist and have data:
   - `sensor.adsb_aircraft_raw` - should show aircraft count
   - `sensor.adsb_local_aircraft_simplified` - should show aircraft count
   - `sensor.adsb_aircraft_below_filter_altitude` - should show a number

4. Click on `sensor.adsb_aircraft_raw` and expand **Attributes**
   - Should see `aircraft: [...]` with data

If sensors show "unavailable" or "unknown":
- Check tar1090 URL is correct
- Verify tar1090 is running and accessible
- Check Home Assistant logs for errors

---

## Part 4: Add Automation

### Step 1: Customize Automation

Before adding, customize these values in the automation file:

1. **Device Tracker** (Line 62):
   ```yaml
   entity_id: device_tracker.pixel_10_pro_xl  # CHANGE TO YOUR DEVICE
   ```
   Replace with your device tracker entity ID

2. **TTS Platform** (Line 120):
   ```yaml
   entity_id: tts.piper  # CHANGE IF USING DIFFERENT TTS
   ```

3. **Media Players** (Line 123):
   ```yaml
   media_player_entity_id: media_player.group_satellite_media_players  # CHANGE TO YOUR SPEAKERS
   ```

### Step 2: Add Automation

**Via UI:**
1. Go to **Settings → Automations & Scenes**
2. Click **+ Create Automation**
3. Click **⋮ Menu → Edit in YAML**
4. Delete existing content
5. Paste contents from [`configuration/automation.yaml`](../configuration/automation.yaml)
6. Click **Save**

**Via File:**
1. Add to `/config/automations.yaml` or your automations file
2. Reload automations: **Developer Tools → YAML → Automations**

### Step 3: Test Automation

1. Ensure toggle is ON: `input_boolean.adsb_announcements_enabled`
2. Set reasonable thresholds (e.g., 15000 ft, 50 miles)
3. Make sure you're marked as "home" in your device tracker
4. Wait for an aircraft to fly within range
5. Automation should trigger and announce the aircraft

**Manual Test:**
- Go to your automation
- Click **⋮ Menu → Run**
- Check traces for any errors

---

## Part 5: Add Dashboard Cards

### Step 1: Add Cards to Dashboard

1. Go to your dashboard
2. Click **Edit Dashboard** (top right)
3. Click **+ Add Card**
4. Scroll down and click **Manual**
5. Paste contents from [`lovelace/adsb_dashboard_card.yaml`](../lovelace/adsb_dashboard_card.yaml)
6. Click **Save**

The cards should now appear showing:
- Aircraft count within distance threshold
- Aircraft below altitude threshold
- Settings panel with sliders

### Step 2: Customize Card Colors (Optional)

Edit the card YAML to change colors:

- **Green theme (default)**: `rgba(0,255,120,...)`
- **Blue theme**: Change to `rgba(0,128,255,...)`
- **Red alert card**: `rgba(255,100,100,...)`

---

## Part 6: Configure TTS (If Not Already Set Up)

If you don't have TTS configured:

### Option 1: Piper (Recommended)

1. Go to **Settings → Add-ons**
2. Click **Add-on Store**
3. Search for "Piper"
4. Install **Piper** add-on
5. Start the add-on
6. Go to **Settings → Devices & Services → Integrations**
7. Add **Wyoming Protocol** integration
8. Configure Piper TTS

### Option 2: Google Translate TTS

Change the automation TTS service to:
```yaml
action: tts.google_translate_say
target:
  entity_id: media_player.your_speaker
data:
  message: "Aircraft {{ callsign }} is flying nearby at {{ spoken_altitude }} feet."
```

### Option 3: Other TTS Platforms

Any Home Assistant TTS platform will work. Adjust the `tts.speak` action in the automation accordingly.

---

## Part 7: Final Verification

### Checklist

- ✅ REST sensor showing aircraft count
- ✅ Template sensors populated with data
- ✅ Helpers created and visible
- ✅ Automation added and enabled
- ✅ Dashboard cards showing data
- ✅ TTS configured and working

### Test Announcement

1. Set distance threshold to 500 miles
2. Set altitude threshold to 50000 feet
3. Enable announcements toggle
4. Ensure you're marked "home"
5. Manually run the automation
6. Should hear announcement if any aircraft are being tracked

---

## Troubleshooting

If something isn't working, see [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common issues and solutions.

---

## Next Steps

- Adjust altitude and distance thresholds to your preference
- Customize announcement message in automation
- Add additional conditions (e.g., time of day, specific callsigns)
- Create additional dashboard views
- Set up notifications for specific aircraft types

---

## Support

Having issues? Check:
1. [TROUBLESHOOTING.md](TROUBLESHOOTING.md) - Common problems and solutions
2. [GitHub Issues](https://github.com/redneckzking/ADS-B-Local-Flight-Tracking-HA-Integration/issues) - Report bugs or ask questions
3. Home Assistant Community Forum - Get help from the community
