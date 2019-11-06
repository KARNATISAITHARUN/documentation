# `sl` Global Options

These options can be used with any `sl` command.

Global Option | Environment Variable | Description
--- | --- | ---
`--diagnostic` | SHIFTLEFT_DIAGNOSTIC | Allow application to send diagnostic data to the ShiftLeft servers. Default is `no-diagnostic`. 
`--help`, `-h` | | Display ShiftLeft CLI help text.
`-- no-auto-update` | SHIFTLEFT_NO_AUTO_UPDATE=true | Disable automatic updating of the ShiftLeft CLI. This means that you must reinstall the ShiftLeft CLI whenever there are new features or fixes (curl or wget are required for automatic updates).
`--no-diagnostic` | SHIFTLEFT_DIAGNOSTIC | Disable diagnostic data. Default.
`--sl-home <path>` | `SHIFTLEFT_HOME=<path>` | Path for the [configuration file `shiftleft.json`](../../using-inspect-protect/protect/json-file.md) and artifacts. If `$HOME` is set, defaults to `$HOME/.shiftleft`; otherwise to `/etc/shiftleft`.
`--updates-download-timeout timeout` | 'SHIFTLEFT_UPDATES_DOWNLOAD_TIMEOUT' | Requested total timeout (e.g. '15m') to be used when downloading updates. If the `timeout` parameter is not set (the default), no timeout is applied. 
`--updates-read-timeout timeout` | 'SHIFTLEFT_UPDATES_READ_TIMEOUT' | Requested idle timeout (e.g. '30s') to be used when downloading data updates. If the `timeout` parameter is not set, no timeout is applied. Default is 3 minutes (3m0s).
`--verbose` | `SHIFTLEFT_VERBOSE=true` | Verbose mode.
`--version`, `-v` | | Display the ShiftLeft CLI version.
`None`	| `https_proxy=<proxy>` |	Proxy configuration.
