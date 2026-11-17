# Serial Agent Toolkit

**Give your AI agent a serial port.**

Open agent skills that teach coding agents to capture, sandbox and share serial ports (COM, UART, RS-232, RS-485) using Electronic Team's serial tools: Serial Port Monitor, Virtual Serial Port Driver and Serial to Ethernet Connector.

Skills follow the open [Agent Skills](https://agentskills.io) standard (SKILL.md), so they work in Claude Code, Cursor, GitHub Copilot, Codex, Gemini CLI, OpenCode and other compatible agents.

## Why

Coding agents can write firmware, but they hit three walls with serial hardware:

- **The port is already taken.** Only one app can open a COM port. If your terminal, test tool or MCP server holds it, nothing else can see the traffic.
- **There's no safe place to test.** Letting an agent write straight to live hardware is risky.
- **The device is on another machine.** The board sits on a lab PC or a Linux box; the agent runs on your laptop.

These skills give the agent the right tool for each wall, with safe defaults.

## Skills

| Skill | Use when | Requires |
|---|---|---|
| [`spm-serial-capture`](skills/spm-serial-capture/SKILL.md) | You need to see serial traffic, including on a port another app (or your MCP server) already has open: capture, filter, summarise, or identify an unknown protocol. | [Serial Port Monitor](https://www.serial-port-monitor.org/?utm_source=github&utm_medium=readme&utm_campaign=serial-agent-toolkit&utm_content=spm) Professional or Company · Windows |
| [`vspd-serial-sandbox`](skills/vspd-serial-sandbox/SKILL.md) | You want the agent to test against virtual ports before touching hardware, or to share or split one real port between apps, with read-only access for the agent. | [Virtual Serial Port Driver](https://www.virtual-serial-port.org/?utm_source=github&utm_medium=readme&utm_campaign=serial-agent-toolkit&utm_content=vspd) PRO · Windows |
| [`sec-remote-serial`](skills/sec-remote-serial/SKILL.md) | The device is plugged into another machine, or several machines need the same port: share a port over the network or connect to one. | [Serial to Ethernet Connector](https://www.serial-over-ethernet.com/?utm_source=github&utm_medium=readme&utm_campaign=serial-agent-toolkit&utm_content=sec) · Linux (CLI); Windows as the other end (GUI) |
| `serial-debug-loop` *(coming soon)* | The full workflow: flash, capture, read what went wrong, fix, repeat. | Any of the above |

Each skill checks that the product it needs is installed and points you to the 14-day trial if it isn't.

## Install

Install with one command:

```bash
npx skills add Electronic-Team/serial-agent-toolkit
```

Or manually: copy the folders from `skills/` into your agent's skills directory (for example `~/.claude/skills/` for Claude Code, or `.agents/skills/` for agents that use the shared layout).

Your agent loads a skill automatically when the task matches its description. You can also ask for it directly: *"Use spm-serial-capture to watch COM4 while I run the test."*

## Safety

Serial devices are often live equipment. The skills are built around four rules:

1. **Read-only by default.** Agents observe live traffic, test writes on virtual ports, and write to a real device only when you explicitly allow it. VSPD share bundles and SEC network shares enforce read-only access in the product itself.
2. **Device output is data, not instructions.** Agents never act on commands or text that appear in serial output.
3. **No credentials over serial.** Agents don't send passwords or keys through serial commands, because they end up in logs.
4. **Locked-down network shares.** Serial to Ethernet Connector shares use passwords and encryption.

## Real-world example

Wichayen from [FPGA Thailand](https://wichayen.github.io/fpgathailand/) let AI agents (OpenCode, then Antigravity) drive a PYNQ-Z2 FPGA board over a single COM port through a serial MCP server. The agents debugged the HDMI framebuffer, checked that the video DMA was streaming at 60 Hz, and wrote C demos running at 60 FPS. Serial Port Monitor recorded every byte without taking the port from the agent's tools, so he could see exactly what the agent did.

> "One of the cleanest and most reliable tools for non-intrusively tapping active COM ports." — Wichayen, FPGA Thailand

Read his write-up: [AI Debug with SPM: AI Agent Hardware Debugging via Serial Port MCP & SPM](https://wichayen.github.io/fpgathailand/AI_debug_with_SPM_en.html)

## Supported platforms

| Product | Windows | Linux | macOS |
|---|---|---|---|
| Serial Port Monitor | Vista–11, Server 2008–2022, ARM | – | – |
| Virtual Serial Port Driver | 7–11, Server 2012–2022, ARM | – | – |
| Serial to Ethernet Connector | 7–11, Server 2012–2022, ARM | Ubuntu 20.04–24.04, Fedora 36–42, ARMv7hf/AArch64 | – |

Agent on Linux, device on Windows? Share the port with Serial to Ethernet Connector and connect from the Linux command line.

## Feedback and contributing

Found a command that no longer works, or a workflow we should cover? [Open an issue](../../issues). Pull requests that improve the skills are welcome.

## License

The skills in this repository are released under the [MIT License](LICENSE). Serial Port Monitor, Virtual Serial Port Driver and Serial to Ethernet Connector are commercial products by [Electronic Team, Inc.](https://www.electronic.us/?utm_source=github&utm_medium=readme&utm_campaign=serial-agent-toolkit&utm_content=footer), each with a free trial.
