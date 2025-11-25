# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-11-25

### Added
- Initial release of ADS-B Local Flight Tracking for Home Assistant
- REST sensor to pull aircraft data from local tar1090 server
- Template sensors to process and simplify aircraft data
- Voice announcement automation with distance and altitude filtering
- Beautiful dashboard cards with animated radar icons
- Real-time distance calculation using Haversine formula
- Smart cooldown system to prevent repeat announcements
- Configurable altitude threshold (0-50,000 ft)
- Configurable distance threshold (0-500 miles)
- Master on/off toggle for announcements
- Support for both Imperial (miles) and Metric (kilometers) units
- Comprehensive installation guide
- Detailed troubleshooting documentation
- Helper setup instructions

### Features
- **Real-time tracking**: Updates every 5 seconds from tar1090
- **Distance filtering**: Announce only aircraft within specified radius
- **Altitude filtering**: Announce only low-flying aircraft
- **Location awareness**: Only announce when home
- **Animated dashboard**: Radar icon animates when aircraft detected
- **Dual units**: Display both miles and kilometers
- **TTS integration**: Works with Piper, Google TTS, and others
- **Customizable**: Easy to adjust thresholds and messages

### Requirements
- Home Assistant Core 2025.11.3 or newer
- Local ADS-B receiver running tar1090
- Custom Button Card (via HACS)
- TTS platform configured
- Media players for announcements

### Known Limitations
- Requires aircraft to have lat/lon data (most do)
- Cooldown prevents multiple announcements of same aircraft in sequence
- Distance calculation requires Home zone to be properly configured

---

## [Unreleased]

### Planned Features
- Per-aircraft cooldown timers instead of single-flight tracking
- Filter by specific aircraft types or callsigns
- Military aircraft highlighting
- Notification support (in addition to voice announcements)
- Historical tracking and statistics
- Integration with FlightAware/Flightradar24 for additional metadata
- Configurable announcement messages
- Time-based announcement scheduling (e.g., quiet hours)

### Potential Improvements
- Optimization of Haversine calculations
- Support for multiple distance thresholds
- Card themes and color customization options
- Additional template sensors for specific use cases

---

## Version History

### v1.0.0 (2025-11-25)
- Initial public release
- Core functionality complete and tested
- Full documentation provided

---

## Contribution Guidelines

When contributing, please:
1. Update CHANGELOG.md with your changes
2. Follow semantic versioning
3. Test thoroughly before submitting PR
4. Update documentation as needed

## Links

- [GitHub Repository](https://github.com/redneckzking/ADS-B-Local-Flight-Tracking-HA-Integration)
- [Installation Guide](docs/INSTALLATION.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)
- [Issues](https://github.com/redneckzking/ADS-B-Local-Flight-Tracking-HA-Integration/issues)
