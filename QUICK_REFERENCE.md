# Quick Reference Card

Fast reference for common tasks and configurations.

## URLs to Customize

```yaml
# In configuration/rest_sensor.yaml
resource: http://192.168.1.77/tar1090/data/aircraft.json  # CHANGE THIS

# In configuration/automation.yaml
entity_id: device_tracker.pixel_10_pro_xl  # CHANGE THIS (line 62)
entity_id: tts.piper  # CHANGE THIS (line 120)
media_player_entity_id: media_player.group_satellite_media_players  # CHANGE THIS (line 123)
```

## Entity IDs

### Sensors
- `sensor.adsb_aircraft_raw` - Raw data from tar1090
- `sensor.adsb_local_aircraft_simplified` - Processed aircraft data
- `sensor.adsb_aircraft_below_filter_altitude` - Count below threshold

### Helpers
- `input_number.adsb_altitude_threshold` - Max altitude (ft)
- `input_number.adsb_distance_threshold` - Max distance (miles)
- `input_boolean.adsb_announcements_enabled` - On/off toggle
- `input_text.adsb_last_announced_flight` - Last announced callsign

## Default Values

```yaml
Altitude Threshold: 15,000 feet
Distance Threshold: 50 miles
Scan Interval: 5 seconds
Timeout: 10 seconds
```

## File Locations (Home Assistant)

```
/config/
├── configuration.yaml (add: rest: and template: includes)
├── rest_sensor.yaml
├── template_sensors.yaml
└── automations.yaml (or add to existing)
```

## Testing Commands

### Test tar1090 Connection
```bash
curl http://YOUR_IP/tar1090/data/aircraft.json
```

### Test Template in Developer Tools
```yaml
{{ state_attr('sensor.adsb_aircraft_raw', 'aircraft') | count }}
{{ distance('zone.home', 34.8, -80.5) }}
```

### Test TTS
```yaml
Service: tts.speak
Target: tts.piper
Data:
  media_player_entity_id: media_player.your_speaker
  message: "Test announcement"
```

## Common Threshold Values

### Altitude
- **0-1000 ft**: Landing/departing only
- **1000-5000 ft**: Very low aircraft
- **5000-15000 ft**: Low to medium (default)
- **15000-30000 ft**: Most commercial traffic
- **30000-50000 ft**: All aircraft

### Distance  
- **0-5 miles**: Immediate area
- **5-25 miles**: Local area
- **25-50 miles**: Regional (default)
- **50-100 miles**: Wide area
- **100-500 miles**: Very wide area

## Troubleshooting Quick Checks

### No Data
1. Check tar1090 URL in browser
2. Verify REST sensor state in Developer Tools
3. Check Home Assistant logs

### No Announcements
1. Is toggle ON?
2. Are you home?
3. Any aircraft within thresholds?
4. Check automation trace

### Wrong Count on Card
1. Cards filter by distance (expected)
2. Check distance threshold setting
3. Verify zone.home coordinates

## Useful Developer Tools Queries

```yaml
# Check aircraft data
{{ state_attr('sensor.adsb_local_aircraft_simplified', 'aircraft') }}

# Check distance to specific coordinates
{{ distance('zone.home', 34.8, -80.5) }}

# Check all helpers
{{ states.input_number | selectattr('entity_id', 'search', 'adsb') | map(attribute='entity_id') | list }}
{{ states.input_boolean | selectattr('entity_id', 'search', 'adsb') | map(attribute='entity_id') | list }}
{{ states.input_text | selectattr('entity_id', 'search', 'adsb') | map(attribute='entity_id') | list }}
```

## Log Filters

```
Settings → System → Logs

Search for:
- "rest" - REST sensor issues
- "template" - Template sensor issues
- "automation" - Automation issues  
- "adsb" - All ADSB-related messages
- "tts" - TTS issues
```

## Card Color Customization

Change colors in dashboard card YAML:

```javascript
// Green (active aircraft)
rgba(0,255,120,...)

// Blue (no aircraft)
rgba(0,128,255,...)

// Red (alert/below threshold)
rgba(255,100,100,...)
```

## Announcement Message Customization

In automation.yaml, change message:

```yaml
message: >
  Aircraft {{ callsign }} is flying nearby at {{ spoken_altitude }} feet.

# Examples:
# Simple: "Aircraft {{ callsign }} detected"
# Detailed: "Aircraft {{ callsign }} is {{ distance_miles }} miles away at {{ spoken_altitude }} feet"
# With altitude: "Low-flying aircraft {{ callsign }} at {{ alt_ft | int }} feet"
```

## Altitude Spoken Format

```yaml
# Individual digits (aviation style) - DEFAULT
spoken_altitude: "{{ alt_ft | int | string | list | join(', ') }}"
# Result: "1, 5, 0, 0 feet"

# Natural number
spoken_altitude: "{{ alt_ft | int }}"
# Result: "1500 feet"
```

## Useful Links

- **tar1090**: http://YOUR_IP/tar1090/
- **JSON data**: http://YOUR_IP/tar1090/data/aircraft.json
- **Home Assistant**: http://homeassistant.local:8123
- **Developer Tools**: Settings → System → Developer Tools
- **Logs**: Settings → System → Logs

## Support Resources

- **GitHub Issues**: https://github.com/redneckzking/ADS-B-Local-Flight-Tracking-HA-Integration/issues
- **Installation**: docs/INSTALLATION.md
- **Troubleshooting**: docs/TROUBLESHOOTING.md
- **Home Assistant Forums**: https://community.home-assistant.io/

---

**Pro Tip**: Keep this file handy for quick reference when troubleshooting or customizing your setup!
