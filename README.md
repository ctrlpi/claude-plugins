# ctrlPi plugins for Claude

[![Platform: Claude Desktop | Claude Code](https://img.shields.io/badge/platform-Claude%20Desktop%20%7C%20Claude%20Code-006400.svg)](#install-in-claude-desktop)
[![Version 0.9.50](https://img.shields.io/badge/version-0.9.50-blue.svg)](https://github.com/ctrlpi/claude-plugins/tags)
[![Node 20+](https://img.shields.io/badge/node-20%2B-blue.svg)](https://nodejs.org/)
[![Plugins: 6](https://img.shields.io/badge/plugins-6-006400.svg)](#ctrlpi-plugins-for-claude)
[![Agents: Pi, Pico & ESP32](https://img.shields.io/badge/agents-Pi%2C%20Pico%20%26%20ESP32-006400.svg)](#ctrlpi-plugins-for-claude)
[![License: MIT](https://img.shields.io/badge/license-MIT-188038.svg)](LICENSE)
![Status: Beta](https://img.shields.io/badge/status-Beta-red.svg)

Plugins that let Claude work with your Raspberry Pi, Pico and ESP32 boards. Each one starts a ctrlPi MCP server from npm at a pinned version; this repository holds only the plugin files, and the server code lives in its own repository.

| Plugin | What it does | Server |
|---|---|---|
| [ctrlpi-mcp](ctrlpi-mcp) | Control GPIO agents on your LAN: pins, relays, sensors, scripts | [ctrlpi/ctrlpi-mcp](https://github.com/ctrlpi/ctrlpi-mcp) |
| [pico-bay](pico-bay) | Pico and ESP32 boards over USB: files, Python, firmware, ctrlPi API | [ctrlpi/pico-bay](https://github.com/ctrlpi/pico-bay) |
| [pi-bay](pi-bay) | Raspberry Pis and SSH devices on your LAN: files, commands, ctrlPi API | [ctrlpi/pi-bay](https://github.com/ctrlpi/pi-bay) |
| [ctrlpi-mcp-remote](ctrlpi-mcp-remote) | ctrlpi-mcp running on a Pi, from your desktop | [ctrlpi/ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) |
| [pico-bay-remote](pico-bay-remote) | The boards plugged into a Pi, through pico-bay there | [ctrlpi/ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) |
| [pi-bay-remote](pi-bay-remote) | pi-bay running on a Pi, from your desktop | [ctrlpi/ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) |

The servers run on your computer and reach your boards over USB or your local network, so the plugins work in the Claude desktop app (chat, Cowork and Code) and in Claude Code.

The three `-remote` plugins are for a server that runs on a Raspberry Pi instead: they start `ctrlpi-remote` on your computer, which finds that server on your network and forwards to it with its own key. Use `ctrlpi-mcp-remote` instead of `ctrlpi-mcp`, and `pi-bay-remote` instead of `pi-bay`, not both; `pico-bay-remote` can run alongside `pico-bay`.

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
