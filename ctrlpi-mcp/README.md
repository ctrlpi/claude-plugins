# ctrlPi (control Raspberry Pi, Pico & ESP32)

Control the GPIO pins of Raspberry Pi, Pico W and ESP32 boards on your local network from Claude. Ask in plain words to read a pin, switch an output, pulse a relay for a few seconds, read a sensor, or write and run a small script on a board.

The plugin starts the [ctrlpi-mcp](https://github.com/ctrlpi/ctrlpi-mcp) MCP server from npm. Each board runs a ctrlPi agent: [pi-gpio-api](https://github.com/ctrlpi/pi-gpio-api) on a Raspberry Pi, [pico-gpio-api](https://github.com/ctrlpi/pico-gpio-api) on a Pico W or Pico 2 W, or [esp32-gpio-api](https://github.com/ctrlpi/esp32-gpio-api) on an ESP32. The server talks to those agents over their REST API.

## Requirements

- Node.js 20 or newer.
- At least one ctrlPi agent reachable on your LAN. The pico-bay and pi-bay plugins can install one.
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install **ctrlPi (control Raspberry Pi, Pico & ESP32)**.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install ctrlpi-mcp@ctrlpi
```

Then ask, for example: "List my ctrlPi agents" or "Pulse the relay on garage-pi for 2 seconds".

## Tools

| Tool | What it does |
|---|---|
| `mcp_agents_list` | List the agents, with address, type, version and whether each is online |
| `mcp_gpio_read`, `mcp_gpio_scan`, `mcp_gpio_watched` | Read one pin or all pins, and list watched pins |
| `mcp_gpio_write` | Set an output on, off or toggle, optionally for a number of seconds |
| `mcp_gpio_config` | Set a pin's type, pull resistor, start value, maximum pulse and name |
| `mcp_sensor_config`, `mcp_sensor_read` | Define sensors backed by scripts, and read them |
| `mcp_script_list`, `mcp_script_read`, `mcp_script_write`, `mcp_script_run` | Manage and run files in the agent's `scripts/` folder |
| `mcp_config_read`, `mcp_config_write`, `mcp_config_load` | Read, change or replace an agent's configuration |
| `mcp_agent_restart`, `mcp_agent_upgrade`, `mcp_agent_logs` | Restart or reboot an agent, upgrade it to the latest release, read its log |

## What it runs, sends and stores

- **Runs:** `npx -y ctrlpi-mcp@0.9.40`, which downloads that exact version from the npm registry and starts it over stdio. The package has no dependencies.
- **Local network:** at start and every hour, the server probes every address of your local /24 subnet on port 8314 to find agents. All tool calls are HTTP requests to your agents, with each agent's API key.
- **On the boards:** tools can switch outputs, change pin and agent settings, write and run scripts, restart or reboot a board, and upgrade an agent. On upgrade, the agent downloads its own new release from GitHub.
- **Stored on your computer:** the agent list and keys in `~/.ctrlpi/agents-config.json`.
- **Nothing else:** the server sends no data to ctrlPi or any other service on the internet, and has no telemetry.

Agents ship with a public default key. Change it on each agent, or start the server with `--rotate-keys` (see the [ctrlpi-mcp README](https://github.com/ctrlpi/ctrlpi-mcp#readme)).

## License

[MIT](LICENSE)
