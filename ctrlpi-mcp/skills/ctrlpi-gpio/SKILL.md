---
name: ctrlpi-gpio
description: Use when the user asks to read or switch GPIO pins, pulse a relay, read a sensor, or manage scripts on a Raspberry Pi, Pico W or ESP32 running a ctrlPi agent.
---

# ctrlPi (control Raspberry Pi, Pico & ESP32)

1. Start with `mcp_agents_list` to get the agent names. Use those names in every other call. If an agent the user names is missing or offline, say so and ask before calling `mcp_agents_list` with `rescan=true`.
2. Before writing to a pin, read it or run `mcp_gpio_scan` so you know its type, name and current level. Only output pins can be written.
3. For a relay or anything that should switch back, use `mcp_gpio_write` with a `duration` in seconds instead of two separate writes.
4. Read sensors with `mcp_sensor_read`. To add one, write a script with `mcp_script_write`, test it with `mcp_script_run`, then register it with `mcp_sensor_config`.
5. Script writes are refused while an agent still uses the default API key. Tell the user and point them to changing the key; do not change keys yourself unless asked.
6. Ask the user before calls that change more than one pin: `mcp_config_load`, `mcp_config_write`, `mcp_agent_restart` with `reboot=true`, and `mcp_agent_upgrade`.
