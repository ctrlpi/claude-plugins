# ctrlPi plugins for Claude

[![Platform: Claude Desktop | Claude Code](https://img.shields.io/badge/platform-Claude%20Desktop%20%7C%20Claude%20Code-006400.svg)](#install-in-claude-desktop)
[![Version 0.9.51](https://img.shields.io/badge/version-0.9.51-blue.svg)](https://github.com/ctrlpi/claude-plugins/tags)
[![Node 20+](https://img.shields.io/badge/node-20%2B-blue.svg)](https://nodejs.org/)
[![Plugins: 4](https://img.shields.io/badge/plugins-4-006400.svg)](#ctrlpi-plugins-for-claude)
[![Agents: Pi, Pico & ESP32](https://img.shields.io/badge/agents-Pi%2C%20Pico%20%26%20ESP32-006400.svg)](#ctrlpi-plugins-for-claude)
[![License: MIT](https://img.shields.io/badge/license-MIT-188038.svg)](LICENSE)
![Status: Beta](https://img.shields.io/badge/status-Beta-red.svg)

Plugins that let Claude work with your Raspberry Pi, Pico and ESP32 boards. Each one starts a ctrlPi MCP server from npm at a pinned version; this repository holds only the plugin files, and the server code lives in its own repository.

| Plugin | What it does | Server |
|---|---|---|
| [ctrlpi-mcp](ctrlpi-mcp) | Control GPIO agents on your LAN: pins, relays, sensors, scripts | [ctrlpi/ctrlpi-mcp](https://github.com/ctrlpi/ctrlpi-mcp) |
| [pico-bay](pico-bay) | Pico and ESP32 boards over USB: files, Python, firmware, ctrlPi API | [ctrlpi/pico-bay](https://github.com/ctrlpi/pico-bay) |
| [pi-bay](pi-bay) | Raspberry Pis and SSH devices on your LAN: files, commands, ctrlPi API | [ctrlpi/pi-bay](https://github.com/ctrlpi/pi-bay) |
| [ctrlpi-remote](ctrlpi-remote) | ctrlpi-mcp, pico-bay and pi-bay running on a Pi, from your desktop | [ctrlpi/ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) |

The servers run on your computer and reach your boards over USB or your local network, so the plugins work in the Claude desktop app (chat, Cowork and Code) and in Claude Code.

The `ctrlpi-remote` plugin is for servers that run on a Raspberry Pi instead: it starts `ctrlpi-remote` on your computer, which finds ctrlpi-mcp, pico-bay and pi-bay on your network and forwards to each with its own key. Use `ctrlpi-mcp` and `pi-bay` either there or locally, not both (`"targets"` in `~/.ctrlpi/remote-config.json` leaves a server out); `pico-bay` can run alongside it.

## Install in Claude Desktop

Add this repository as a plugin marketplace once, then install the plugins you want from it.

In Claude Desktop, open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`. The marketplace appears as `ctrlpi`; install the plugins you want from it.

The Code tab also accepts the commands from the next section.

## Install in Claude Code

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install pico-bay@ctrlpi
```

Replace `pico-bay` with the name of another plugin for the others. Each plugin's README lists its tools and what it runs, sends and stores.

## License

[MIT](LICENSE)
