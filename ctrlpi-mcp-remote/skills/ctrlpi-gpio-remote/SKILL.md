---
name: ctrlpi-gpio-remote
description: Use when the user asks to read, switch or configure GPIO pins, pulse a relay, read a sensor, manage scripts, set a webhook, or restart, upgrade or read the log of ctrlPi agents reached through ctrlpi-mcp running on a Raspberry Pi (the ctrlpi-mcp-remote plugin).
---

# ctrlPi Remote - GPIO agents through a ctrlpi-mcp on a Pi

These tools reach the agents through a `ctrlpi-mcp` running on another machine. The rules are the same as with a local one:

1. Start with `mcp_agents_list` to get the agent names. Use those names in every other call. If an agent the user names is missing or offline, say so and ask before calling `mcp_agents_list` with `rescan=true`. With no agents at all, one can be installed with the pi-bay or pico-bay plugin.
2. Before writing to a pin, read it or run `mcp_gpio_scan` so you know its type, name and current level. Only output pins can be written. A pin with `max` set switches back after `max` seconds even without a `duration`.
3. For a relay or anything that should switch back, use `mcp_gpio_write` with a `duration` in seconds instead of two separate writes.
4. Read sensors with `mcp_sensor_read`. To add one, write a script with `mcp_script_write`, test it with `mcp_script_run`, then register it with `mcp_sensor_config`. On a Pi a `.py` runs with Python and anything else with `sh`, and its output is the reading; on a Pico or ESP32 the `.py` defines `read(*args)` or sets `result`.
5. To run a script when a pin changes, set `script` in `mcp_gpio_config` (prefix `on:` or `off:` for one direction only; agent 0.9.50+).
6. Check `write` in `mcp_script_list` before writing a script. Writes are refused while the agent uses the default API key: tell the user, and change it with `mcp_config_write` only if they ask. Never write `write.lock` unless the user asks: these tools cannot remove it.
7. Ask the user before changing a pin's type or removing it with `mcp_gpio_config`, and before calls that change the whole agent: `mcp_config_load`, `mcp_config_write`, `mcp_agent_restart` with `reboot=true`, and `mcp_agent_upgrade`.
8. When an agent call fails, read `mcp_agent_logs`.
9. If every call fails, show the user the message. "no ctrlpi-mcp" or "Cannot reach": the server on the Pi is not running. "MCP is switched off": it runs without HTTP MCP. "is already claimed": another computer claimed it; copy its key into `~/.ctrlpi/remote-config.json`. "rejected the key": the key saved there no longer works; update it.
