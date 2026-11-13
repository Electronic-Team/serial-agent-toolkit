# SPEC: spm-serial-capture

What this skill relies on, how it was tested, and what to recheck when Serial Port Monitor changes.

## Purpose

Teach AI coding agents to capture and analyse serial traffic on Windows with Serial Port Monitor (SPM), including on ports another application already has open, without ever writing to the device.

## Tested with

| | |
|---|---|
| Product | Serial Port Monitor |
| Edition | Professional |
| OS | Windows |
| Test setup | VSPD virtual pair COM10 ↔ COM11, a terminal on each end |
| Tested by | Electronic Team (Nikolai) |
| Date | November 2026 |

## Behaviour the skill depends on

Confirmed in testing or by the SPM team:

- `SerialMonitorx64.exe /new <port> <view>[:<redirection>]` starts monitoring immediately, including on a port another app holds (Professional/Company).
- Redirection to CSV (`T:C=<file>`) writes data to the file in real time, in a stable format.
- `:A` appends to an existing file instead of overwriting it.
- Paths with spaces work when the whole redirection argument is in quotes.
- Event filter digits (e.g. `T2`) work as described in the command-line documentation.
- Dump view (`D`) writes byte codes alongside characters; Terminal view (`R`) suits plain text.
- Closing SPM with `Stop-Process` leaves the redirected file complete.

## Known limitations

- The command line has **no stop command and no exit codes**; the skill stops a capture by closing SPM.
- The **demo version shows a dialog on start** that the user must close, so a trial capture can't run fully unattended.
- **Edition and licence state can't be checked from the command line**; the agent asks the user.
- Redirection captures data **from the moment it starts**; for saved `.spm` files the agent checks that the export covers the whole session.
- Saving a session as `.spm` is only possible in the GUI.

## Recheck after each SPM release

- [ ] Default install path and executable name (`SerialMonitorx64.exe`, `SerialMonitor.exe`)
- [ ] `/new` and `/open` syntax, view letters, redirection formats (`C`, `U`, `H`) and `:A`
- [ ] CSV column layout (the skill reads the header first, but examples may need updating)
- [ ] Demo dialog behaviour
- [ ] Download, edition and purchase links in the Requirements block

## References

- Command-line options: https://wiki.eltima.com/user-guides/serial-port-monitor/command-line-options.html
- Product: https://www.serial-port-monitor.org/
