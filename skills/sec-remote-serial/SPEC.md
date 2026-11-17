# SPEC: sec-remote-serial

What this skill relies on, how it was tested, and what to recheck when Serial to Ethernet Connector changes.

## Purpose

Teach AI coding agents to share a serial port over the network, or connect to one on another machine, with Serial to Ethernet Connector for Linux (`evesecli`), using read-only, password-protected, encrypted shares by default.

## Tested with

| | |
|---|---|
| Product | Serial to Ethernet Connector for Linux 9.0.852 |
| OS | Ubuntu (VMware virtual machine) |
| Test setup | `socat` pseudo-terminal pair as the device; server and client on the same machine (127.0.0.1:5000) |
| Tested by | Electronic Team |
| Date | November 2026 |

## Behaviour the skill depends on

Confirmed in testing:

- `evesecli` runs without `sudo` when the user is in the **`evesec` group**. The daemon service is **`evesecd`**.
- `evesecli license -v` prints `Yes` for a valid licence.
- `add --real <port> server --local <tcp-port> --incoming none --outgoing all --password <pw> --encrypt` is accepted in this order; `--encrypt` takes no value.
- `add --virt <path> client --remote <host:port> --password <pw>` creates a working virtual port.
- `--incoming none` **blocks writes** from the client to the device; data from the device still reaches the client.
- `set <port> server --incoming first` and `set <port> serial --baud-rate … --char-size … --parity … --stop-bits … --flow-control …` work on a **running** connection.
- `evesecli` returns `0` on success and `1` on failure (e.g. `not found`).
- `stop` and `delete` remove connections cleanly.
- Pseudo-terminals (`/dev/pts/*`) work as the real port, so the skill can be tested without hardware.

## Known limitations

- SEC for Windows is GUI-only on both sides; only the Linux side is scriptable.
- Not tested in WSL2.

## Recheck after each SEC release

- [ ] `evesecli` command names and options, especially `--char-size`, `--incoming`, `--encrypt`
- [ ] Group name `evesec` and service name `evesecd`
- [ ] Output of `license -v` and `list -s`
- [ ] Exit codes on failure
- [ ] Download and purchase links in the Requirements block

## References

- Command-line options: https://help.electronic.us/support/solutions/articles/44002208980-command-line-options
- Linux ↔ Windows guide: https://www.serial-over-ethernet.com/com-port-redirector/com-port-redirector-linux-windows/
