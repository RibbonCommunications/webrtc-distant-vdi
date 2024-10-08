# Distant Driver for VDI eLux

This driver adds support for [Citrix Workspace App for Linux](https://docs.citrix.com/en-us/citrix-workspace-app-for-linux.html) on [eLux OS](https://www.unicon-software.com/products/elux/) for use on thin clients.

## 1. eLux Package Signature

Validating package signatures requires the following certificates from [Sectigo's main page](https://support.sectigo.com/articles/Knowledge/Sectigo-Intermediate-Certificates):

Root Certificate:<br>
[AAA Certificate Services](https://comodoca.my.salesforce.com/sfc/p/1N000002Ljih/a/3l000000sYVG/4l82xrBbMv8Ndh.SBoUvQs0BjYk_pJlb4Sa92KfrsxY)

Intermediate Certificates:<br>
[Sectigo Public Code Signing CA R36](https://comodoca.my.salesforce.com/sfc/p/#1N000002Ljih/a/3l000000oAhy/QCCby12C7cYo50nNyic6AuG1KFcwe1rDn1EknfTaUzY)<br>
[SectigoPublicCodeSigningRootR46_AAA [ Cross Signed ]](https://comodoca.my.salesforce.com/sfc/p/1N000002Ljih/a/3l000000sYVB/t5kHfAZUjSL8NyXDwAQ3OhmfoTNSOnWgpnTmksjVyJc)

## 2. Browser Container Certificates

eLux 6.9 provides certificate management directly in the OS for our browser container. If there is a need to use custom certificates for reaching https websites in the browser container please ignore the Certificate Configuration below and put the certificate in the /setup/cacerts/browser folder as mentioned in the eLux [Documentation](https://www.unicon-software.com/udocs/en/#admin_guides/scout_enterprise/app_definition/browser/browser_config.htm?Highlight=cacert).

## 3. Configuration
### 3.1 Citrix Configuration
*The Citrix Workspace App must be configured to load the Distant plugin*
The configuration file for the Citrix Workspace App can be found here:
`/opt/Citrix/ICAClient/config/module.ini`

Edit to add the necessary settings:
1. Locate the `[ICA 3.0]` section
2. Append the Distant plugin to the `VirtualDriver` key. It should look similar to the following:
`VirtualDriver=Thinwire3.0, TWI, SmartCard, SSPI, TUI, Distant`
3. Under the same section (`[ICA 3.0]`), add a key for our plugin and assign it the value of "On":
`Distant=On`
4. Add a section for our plugin and provide the name of the driver:
```
[Distant]
DriverName=Distant.DLL
```
Note that the legacy sections, `KandyDistant` and `RibbonRTC`, will still work but `Distant` will take priority if both sections contain the same flag.
5. Set your desired logging level:
```
LogLevel=debug
```

### 3.2 Distant Configuration
Distant-specific configuration can be set and modified in your `distant.ini` file which is expected to be found in `/setup` directory.

The `Distant` section allows configuration flags that affect the browser container to be set.
Note that the legacy sections, `KandyDistant` and `RibbonRTC`, will still work but `Distant` will take priority if both sections contain the same flag.

- CachePath: location where to store the application cache, defaults to: `/tmp/distant/cache`.
- CommandSwitch: Optional Command Switch arguments to be used with the browser container. Multiple command switches can be separated by a comma.
- DebugPort: Debug port to be used for development. If no port is provided the debug port is disabled.
- ExecutablePath: The absolute path to your execution path and configuration file
- SessionOverwrite: When enabled, the Distant Driver handles Session Start requests by creating a new session which overwrites any existing session. Accepted values are: `true`, `false` (default)
- CefLogLevel: The log level that CEF will use. Defaults to `info`. Other available choices are `trace`, `debug`, `info`, `warn`, `error`, `critical`, or `off`. `debug` option will display verbose level 1 CEF logs.
- VerboseLevel: Number flag indicating how verbose the CEF logs will be. Only supports `1`.
- VerboseModules: Number flag indicating how verbose CEF logs will be on a per module basis. Where the modules are chromium modules and can be found here https://source.chromium.org/chromium/chromium/src. Number used can range from `1` to `3` and `-3` for filtering out modules. For this to work, VerboseLevel must be set to `1`.
- DebugUrlEnabled: When enabled, a new session can be started with the debug url `http://locahost:DebugPort` or a `chrome://` url. Whith thse special urls the session will be opened in a new window outside of the Citrix window. ex: Specifying a new session with the following url `chrome://version` will open the chromium version information. Accepted values are: `true`, `false` (default)

### 3.3 Sample (distant.ini)

```
[Distant]
CachePath=/tmp/distant/cache
CommandSwitch=ignore-certificate-errors,disable-extensions,disable-gpu
DebugPort=9222
SessionOverwrite=false
CefLogLevel=debug
VerboseLevel=1
VerboseModules=*webrtc*=1,*=-3
DebugUrlEnabled=true
```
In this example, VerboseModules will show verbose level 1 webrtc logs and will filter out all other modules.

## 4. Logs
By default, the logs can be found at `/var/log/distant/`.

### 4.1 Log Rotation
Each time the VDI driver is run, log files with the following format will be created:
- `distant-<pid>.log` - The vdi driver logs.
- `browser_console-<pid>.log` - The browser process CEF logs.

A maximum of 5 log files of each type are kept. When a new log file is created, the oldest log file is deleted.

## 5. Sleep & Disconnect
CWA (Citrix Workspace App) handles computer sleep and network disconnects somewhat differently on each OS when a VDI session is connected. This has some impact on Distant and your Distant sessions. It is important that your application can handle these scenarios. Please refer to the subsections for more information.

### 5.1 Sleep
When waking from a short sleep (less than approximately 3 minutes) the CWA will resume and your original Distant session will be available.

When waking from a long sleep (more than approximately 3 minutes) :
 - The CWA may exit in which case your Distant session will be closed.
 - The CWA may exit and restart in which case your Distant session may be reloaded, or closed.

### 5.2 Disconnect
When the network disconnects, the CWA will prompt asking to reconnect.

When reconnecting, the CWA will resume but the original Distant session will not be available so the user needs to restart their app.

When not reconnecting, then the CWA and Distant session will be closed. The user will need to open a new Citrix connection and create a new session once they have an internet connection.

## 6. Known Issues / Limitations
