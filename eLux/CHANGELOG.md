# Change Log

Distant Driver for VDI eLux change log.

- This change log follows [keepachangelog.com](http://keepachangelog.com/) recommendations.
- This project adheres to [Semantic Versioning](http://semver.org/), however, eLux packages also include a -X identifier, which represents the version of the eLux package

## 1.7.1-1 - 2024-09-11
### Fixed
- eLux module requirements updated to reflect correct dependencies. `KAJ-2171`

## 1.7.1-0 - 2023-10-13
### Added
- Emergency 911 information is sent as part of channel info. `KAJ-1881`
- E911 - Populate configuration file with ThinClient Generic Data. `KAJ-1883`
- E911 - Populate configuration file with Whereami data. `KAJ-1882`

### Fixed
- Allow same ID to be used to create a session after simulated browser crash. `KAJ-1612`
- Improved thread handling and cleanup in robustness scenarios. `KAJ-1835`
- Any Distant Session Message and Protocol Heartbeat operation requests will return a SESSION_ERROR event if that session doesn't exist or if it isn't in a ready state. `KAJ-1870`, `KAJ-1933`
- Improved handling of command line key-value pair arguments. `KAJ-1888`

## 1.7.0 - 2022-07-12

### Added
- Support for multiple distant sessions. `KAJ-1030`
- Logs to indicate if orchestrator fails to open. `KAJ-1164`

- Feature to enable Chrome developer tools (eLux only). See DebugUrlEnabled in the Configuration section in [README.md](https://github.com/RibbonCommunications/webrtc-distant-vdi/blob/master/eLux/README.md). `KAJ-1095`

### Fixed
- Issue when comming out of sleep / hibernate. `KAJ-1247`
- Commlink Crash on launching Citrix session. `KAJ-1340`
- Distant Reinitialization fails during Virus Scan. `KAJ-1299`
- Orchestrator process does not close after browser crash. `KAJ-716`
- Browser Message Handler alters session state. `KAJ-1303`
- Quickly closing a session right after opening it will cause the VDI solution to freeze. `KAJ-1009`
- Opening and closing sessions multiple times can cause the Window to disapear. `KAJ-1007`

- Resolved segfault on CWA2104. `KAJ-1205`
- Internal VDI error when removing device while application is running. `KAJ-1006`
- Citrix hangs when coming out of sleep on eLux. `KAJ-1355`

### Changed
- Renamed the Virtual Driver file from kandy_distant.so to KandyDistant.DLL. `KAJ-1210`
- Code Signing certificate was changed to use a different certificate authority. See the [README.md](https://github.com/RibbonCommunications/webrtc-distant-vdi/blob/KAJ-1201-1195/eLux/README.md)


## 1.6.1 - 2022-02-9
### Changed
- This release is a complete refactoring to align with the new architecture on Windows and Mac.
See the README [here](README.md).

## 1.5.2-1 - 2021-05-14
### Fixed
- Compatibility issue with Citrix Worspace app 2101+. `KAJ-695`

## 1.5.1-0 - 2020-08-14
### Fixed
- Keyboard focus is getting stolen from the VDI session when clicking on the overlay video. `KAJ-457`

### Changed
- Updated the code siging certificate to Ribbon Communications Canada ULC.

Note that a 32-bit (RP5) version was not released

## 1.5.0-0 - 2020-03-05
### Added
- A 64-bit (RP6) version of this package is available

Note that a 32-bit (RP5) version was not released

### Deprecated

- RP5 is now deprecated.

## 1.4.0-3 - 2020-01-17
### Fixed
- Regression with the log location.

## 1.4.0-2 - 2020-01-10

### Fixed

- Interroperability issue with other packages on eLux RP5

## 1.4.0-0 - 2019-11-06

### Changed

- Change the configuration to be loaded from /setup/kandy.ini
- Change the log level of most logs to use verbose level. `KAJ-133`

### Added

- Added Log rotation so that /var/log/kandy/rtc.log is copied to rtc-n.log on startup `KAJ-71`

## 1.3.0-0 - 2019-10-04

### Added

- Added the ability to import certificates to the certificate store
- Added fpm to disable the logs

## 1.2.2-0 - 2019-08-01

### Fixed

- Remove logs from Elux Install Script `KAA-1902`
- Interoperability with Kandy HID Driver `KAA-1875`

### Added

- Added Description for the Elux package `KAA-1931`
- Ability to Debug the Remote application via an Optional package `KAA-1893`

## 1.2.1-0 - 2019-06-27

### Fixed

- Session Crash on broken pipe `KAA-1784`
- Crash on Closing session with FillPacketVirtualWrites `KAA-1809`
- Crash on closing the driver, due to packetization destructor `KAA-1883`
- Regression, window is shown on move window `KAA-1812`

## 1.2.0-0 - 2019-06-10

### Added

- Added configurable cache folder, and configurable Chromium command line switches `KAA-1631`
- Added configurable debugger port (disabled by default) `KAA-1623`
- Added missing V8 context snapshot cache file for faster loading. `KAA-1746`

### Changed

- Updated the Virtual Channel SDK version to 13.10 from 13.8 `KAA-1631`
- Use of the correct version of the Virtual Channel SDK library, depending on the build type `KAA-1624`
