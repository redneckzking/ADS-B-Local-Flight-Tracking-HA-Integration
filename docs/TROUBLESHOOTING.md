# Troubleshooting Guide

Common issues and solutions for the ADS-B Local Flight Tracking integration.

---

## Table of Contents

1. [REST Sensor Issues](#rest-sensor-issues)
2. [Template Sensor Issues](#template-sensor-issues)
3. [Automation Issues](#automation-issues)
4. [Dashboard Card Issues](#dashboard-card-issues)
5. [TTS Issues](#tts-issues)
6. [Distance Calculation Issues](#distance-calculation-issues)
7. [General Debugging](#general-debugging)

---

## REST Sensor Issues

### Sensor shows "unavailable" or "unknown"

**Symptoms:**
- `sensor.adsb_aircraft_raw` shows "unavailable" or "unknown"
- No aircraft count displayed

**Solutions:**

1. **Verify tar1090 URL is accessible**
   - Open browser and navigate to: `http://YOUR_IP/tar1090/data/aircraft.json`
   - You should see JSON data
   - If not accessible, check:
     - Is tar1090 running?
     - Is IP address correct?
     - Is Home Assistant on same network?
     - Any firewall blocking access?

2. **Check REST sensor configuration**
   ```yaml
   # In configuration/rest_sensor.yaml or configuration.yaml
   resource: http://192.168.1.77/tar1090/data/aircraft.json  # Is this correct?
   ```

3. **Check Home Assistant logs**
   - Go to **Settings → System → Logs**
   - Search for "rest" or "adsb"
   - Look for connection errors or timeouts

4. **Test with curl** (from Home Assistant host):
   ```bash
   curl http://YOUR_IP/tar1090/data/aircraft.json
   ```

5. **Increase timeout**
   ```yaml
   timeout: 15  # Try increasing from 10 to 15
   ```

### Sensor shows "0" but aircraft are present

**Symptoms:**
- tar1090 shows aircraft in web interface
- `sensor.adsb_aircraft_raw` shows 0

**Solutions:**

1. **Check JSON structure**
   - Open: `http://YOUR_IP/tar1090/data/aircraft.json`
   - Verify structure matches:
     ```json
     {
       "now": 1234567890,
       "aircraft": [
         { "hex": "...", "flight": "...", ... }
       ]
     }
     ```

2. **Check value_template**
   ```yaml
   value_template: "{{ value_json.aircraft | count }}"
   ```
   - This must match your JSON structure

3. **Test template in Developer Tools**
   - Go to **Developer Tools → Template**
   - Test:
     ```yaml
     {{ state_attr('sensor.adsb_aircraft_raw', 'aircraft') | count }}
     ```

---

## Template Sensor Issues

### Template sensors show "unavailable"

**Symptoms:**
- `sensor.adsb_local_aircraft_simplified` is unavailable
- Other template sensors show errors

**Solutions:**

1. **Check REST sensor is working first**
   - Verify `sensor.adsb_aircraft_raw` has data
   - Template sensors depend on REST sensor

2. **Check template syntax**
   - Go to **Developer Tools → Template**
   - Test your template code piece by piece
   - Look for syntax errors

3. **Reload templates**
   - **Developer Tools → YAML → Template Entities → Reload**

4. **Check logs for template errors**
   - **Settings → System → Logs**
   - Filter by "template"

### Template sensor shows empty aircraft array

**Symptoms:**
- Sensor state shows correct count
- Attributes show `aircraft: []` (empty)

**Solutions:**

1. **Verify raw sensor has aircraft attribute**
   - Check `sensor.adsb_aircraft_raw` attributes
   - Should have `aircraft: [...]` with data

2. **Check field names match**
   - Your tar1090 might use different field names
   - Common variations:
     - `flight` vs `callsign`
     - `alt_baro` vs `altitude`
     - `lat`/`lon` vs `latitude`/`longitude`

3. **Test with raw data**
   ```yaml
   {{ state_attr('sensor.adsb_aircraft_raw', 'aircraft') }}
   ```

4. **Check for None values**
   - If aircraft don't have lat/lon, they're still included but with `none`
   - This is expected behavior

---

## Automation Issues

### Automation never triggers

**Symptoms:**
- Automation enabled but never runs
- No announcements

**Solutions:**

1. **Check conditions**
   - Is `input_boolean.adsb_announcements_enabled` turned ON?
   - Is your device tracker showing "home"?
   - Are there actually aircraft within range?

2. **Check triggers**
   - Verify `sensor.adsb_local_aircraft_simplified` is updating
   - Check sensor state changes in Developer Tools → States

3. **Test manually**
   - Go to automation
   - Click **⋮ Menu → Run**
   - Check trace to see where it stops

4. **Check thresholds are reasonable**
   - Distance: Set to 500 miles temporarily (disables distance filter)
   - Altitude: Set to 50000 feet (catches all aircraft)

5. **Disable cooldown temporarily**
   - Comment out this condition:
     ```yaml
     # - condition: template
     #   value_template: "{{ callsign != last_callsign }}"
     ```

### Automation triggers but no announcement

**Symptoms:**
- Automation runs (shows in traces)
- No voice announcement heard

**Solutions:**

1. **Check TTS is configured**
   - Go to **Developer Tools → Services**
   - Test: `tts.speak` or `tts.piper`
   - Try manual announcement

2. **Check media player**
   - Is media player entity correct?
   - Is speaker on and volume up?
   - Test with: `media_player.play_media`

3. **Check TTS action syntax**
   ```yaml
   action: tts.speak  # Not tts.piper if using Piper
   target:
     entity_id: tts.piper
   ```

4. **Simplify message temporarily**
   ```yaml
   message: "Test announcement"
   ```

5. **Check logs for TTS errors**
   - Search logs for "tts" or "piper"

### Only announces same aircraft repeatedly

**Symptoms:**
- Announces one aircraft over and over
- Ignores other aircraft

**Solutions:**

1. **Check cooldown logic**
   - `input_text.adsb_last_announced_flight` should update
   - After announcement, check its value

2. **Verify callsigns are different**
   - Some aircraft might have same/similar callsigns
   - Check `sensor.adsb_local_aircraft_simplified` attributes

3. **Mode is set to "restart"**
   - Verify automation mode:
     ```yaml
     mode: restart
     ```

---

## Dashboard Card Issues

### Cards show "Custom element doesn't exist"

**Symptoms:**
- Red error box on dashboard
- "custom:button-card doesn't exist"

**Solutions:**

1. **Install button-card**
   - HACS → Frontend → Search "button card"
   - Install and restart

2. **Clear browser cache**
   - Hard refresh: Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
   - Or clear cache in browser settings

3. **Add to resources manually**
   - Settings → Dashboards → ⋮ → Resources
   - Add: `/hacsfiles/lovelace-button-card/button-card.js`

### Cards show incorrect aircraft count

**Symptoms:**
- Dashboard shows different count than sensor
- Count doesn't match expectations

**Solutions:**

1. **Cards filter by distance**
   - Cards calculate distance in real-time
   - Sensor shows ALL aircraft from tar1090
   - This is expected behavior

2. **Check distance threshold**
   - Adjust `input_number.adsb_distance_threshold`
   - Set to 500 to effectively disable filtering

3. **Verify zone.home coordinates**
   - Go to **Settings → Areas & Zones → Home**
   - Check latitude/longitude are correct

4. **Test distance calculation**
   - Use Developer Tools → Template
   - Test: `{{ distance('zone.home', LAT, LON) }}`

### Cards not updating

**Symptoms:**
- Count stays same even when aircraft change
- No real-time updates

**Solutions:**

1. **Hard refresh browser**
   - Ctrl+F5 or Cmd+Shift+R

2. **Check sensor is updating**
   - Watch `sensor.adsb_local_aircraft_simplified` in States
   - Should change as aircraft come/go

3. **Clear frontend cache**
   - Settings → System → Repairs
   - Clear frontend cache if available

---

## TTS Issues

### Altitude announcement sounds wrong

**Symptoms:**
- "1500 feet" announced as "one five zero zero feet"

**Solutions:**

This is intentional! The automation formats altitude as individual digits for clarity (aviation standard). If you prefer natural numbers:

```yaml
spoken_altitude: "{{ alt_ft | int }}"
# Instead of: {{ alt_ft | int | string | list | join(', ') }}
```

### Voice sounds robotic or unclear

**Solutions:**

1. **Try Piper TTS** (most natural)
   - Install Piper add-on
   - Configure Wyoming Protocol integration

2. **Adjust Piper voice**
   - Different voices available
   - Settings → Integrations → Piper → Configure

3. **Alternative TTS**
   - Google Translate TTS
   - Amazon Polly
   - Microsoft TTS

### Announcement cuts off mid-sentence

**Solutions:**

1. **Check media player**
   - Some media players have issues with TTS
   - Try different speaker/group

2. **Add delay before announcement**
   ```yaml
   - delay:
       seconds: 1
   - action: tts.speak
   ```

3. **Cache issues**
   - Try setting `cache: false` temporarily

---

## Distance Calculation Issues

### Distance seems incorrect

**Symptoms:**
- Card shows aircraft at wrong distance
- Distance doesn't match expectations

**Solutions:**

1. **Verify Home zone coordinates**
   - Settings → Areas & Zones → Home
   - Latitude/longitude must be accurate

2. **Check unit system**
   - Settings → System → General → Unit system
   - Should be "Imperial" for miles
   - If "Metric", update card to use kilometers

3. **Aircraft missing lat/lon**
   - Some aircraft don't report position
   - These show as 999999 miles (filtered out)

4. **Haversine formula**
   - Card uses Haversine for accuracy
   - Small differences vs. straight-line distance are normal

---

## General Debugging

### Enable Debug Logging

Add to `configuration.yaml`:

```yaml
logger:
  default: info
  logs:
    homeassistant.components.rest: debug
    homeassistant.components.template: debug
    homeassistant.components.automation: debug
```

Restart Home Assistant and check logs.

### Use Developer Tools

1. **States Tab**
   - Monitor sensor values in real-time
   - Check attributes for data

2. **Template Tab**
   - Test template code before adding to sensors
   - Debug template errors

3. **Services Tab**
   - Test TTS announcements
   - Test automation actions manually

### Check Automation Traces

1. Go to automation
2. Scroll to bottom
3. Click **Last Triggered** or **Traces**
4. See exactly what happened during last run
5. Identify which condition/action failed

### Validate YAML Syntax

Before restarting:
- **Developer Tools → YAML**
- Check configuration validity
- Fix any syntax errors shown

---

## Still Having Issues?

1. **Check GitHub Issues**
   - [Existing issues](https://github.com/redneckzking/ADS-B-Local-Flight-Tracking-HA-Integration/issues)
   - Search for similar problems

2. **Create New Issue**
   - Include:
     - Home Assistant version
     - Error messages from logs
     - Relevant YAML configuration
     - Steps to reproduce

3. **Home Assistant Community**
   - Post in Home Assistant forums
   - Include configuration and logs
   - Describe expected vs actual behavior

---

## Diagnostic Checklist

When reporting issues, provide:

- [ ] Home Assistant version
- [ ] REST sensor state and attributes
- [ ] Template sensor state and attributes
- [ ] Helper values
- [ ] Automation trace (if applicable)
- [ ] Relevant log entries
- [ ] tar1090 URL accessibility
- [ ] Browser console errors (for card issues)
