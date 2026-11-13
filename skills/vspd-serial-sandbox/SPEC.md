# SPEC: vspd-serial-sandbox

What this skill relies on, how it was tested, and what to recheck when Virtual Serial Port Driver changes.

## Purpose

Teach AI coding agents to create virtual serial ports on Windows with Virtual Serial Port Driver PRO (pairs, loopbacks) and to share or split real COM ports between applications, with read-only access for the agent's tools by default.

## Tested with

| | |
|---|---|
| Product | Virtual Serial Port Driver PRO |
| OS | Windows |
| Test setup | Virtual pairs and loopbacks with terminals on each port |
| Tested by | Electronic Team (Nikolai) |
| Date | November 2026 |

## Behaviour the skill depends on

Confirmed in testing or by the VSPD team:

- `vspdpro_service.exe` lives in `C:\Program Files\Electronic Team\Virtual Serial Port Driver <version>\` and **only runs with administrator rights**; in a normal shell Windows asks to elevate every time.
- Bundles are managed with `add-bundle`, `add-port`, `show`, `list`, `delete-bundle`, `backup` and `restore`, as in the command-line documentation.
- `set-share` takes the **full path to the application's .exe**. Rights `1` = read on, write and control off; `3` = read and write; `7` = read, write and control.
- A share bundle **can't be created while the real port is open** in another application.
- `delete-bundle` **removes the ports immediately, even if an application has them open**; that application loses the port.

## Known limitations

- **No exit codes:** the agent must check every change with `show` or `list`.
- **Admin rights for every command:** the user has to run the terminal or agent as administrator, or approve each elevation prompt.
- **The skill needs VSPD PRO** (command-line tool, share and split bundles).
- Line-break emulation isn't part of the command-line documentation, so the skill doesn't offer it.

## Recheck after each VSPD release

- [ ] Install folder name (it includes the version) and executable name `vspdpro_service.exe`
- [ ] Command names and argument order, especially `add-port` sides (`in`, `out`) and port types (`virtual`, `real`, `share`, `vspd`)
- [ ] `set-share` rights values
- [ ] Behaviour of `delete-bundle` on open ports
- [ ] Whether VSPD Standard gains command-line access (version 11 ships Standard and PRO in one build)
- [ ] Download, edition and purchase links in the Requirements block

## References

- Command-line options: https://help.electronic.us/support/solutions/articles/44002333909-command-line-options-of-virtual-serial-port-driver
- Product: https://www.virtual-serial-port.org/
