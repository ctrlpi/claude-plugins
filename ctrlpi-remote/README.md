# ctrlPi Remote - ctrlpi-mcp, Pico Bay and Pi Bay on a Pi

Use the ctrlPi servers that run on a Raspberry Pi (or any other always-on machine) on your local network, from Claude on your desktop:

- **ctrlpi-mcp:** control your GPIO agents. Read and switch pins, pulse a relay, read a sensor, manage an agent's scripts.
- **pico-bay:** work with the Pico and ESP32 boards plugged into the Pi. List them, read and edit their files, run Python, install MicroPython or the ctrlPi API.
- **pi-bay:** work with the Raspberry Pis and SSH devices it reaches. Read and edit files, run commands, install the ctrlPi API.

The plugin starts [ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) from npm, which finds these servers on your network and forwards every request to the right one. The tools are the ones of the [ctrlpi-mcp](../ctrlpi-mcp), [pico-bay](../pico-bay) and [pi-bay](../pi-bay) plugins, for each server it finds.

**Next to the local plugins:** `pico-bay` can run alongside it (that one handles the boards on this computer). Use `ctrlpi-mcp` and `pi-bay` either here or locally, not both: the two would control the same agents and devices. To leave a server out, list the ones to use in `~/.ctrlpi/remote-config.json`, for example `"targets": ["ctrlpi-mcp", "pico-bay"]`. Without `targets`, all three are used.

## Requirements

- Node.js 20 or newer on this computer.
- On the other machine, 0.9.50 or newer of any of:
  - `ctrlpi-mcp` with `--mcp http` (port 8315). See the [ctrlpi-mcp README](https://github.com/ctrlpi/ctrlpi-mcp#readme).
  - `pico-bay`, started with `npx pico-bay` (port 7426). See the [pico-bay README](https://github.com/ctrlpi/pico-bay#readme).
  - `pi-bay`, started with `npx pi-bay` (port 3141; add `--mcp http --host 0.0.0.0` on a machine that is not a Raspberry Pi). See the [pi-bay README](https://github.com/ctrlpi/pi-bay#readme).
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install `ctrlpi-remote`.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install ctrlpi-remote@ctrlpi
```

Then ask, for example: "Which agents are online?", "What boards are on the Pi?" or "How much free space is on garage-pi?".

## How it connects

The first time, it searches your network for the three servers, gives each a new random key, and saves the addresses and keys in `~/.ctrlpi/remote-config.json`. From then on each server accepts only its key. If a machine gets a new address, it is found again.

It searches when it starts. After you start a server, ask Claude to search again, and it gets the new tools (some apps need a restart to show them). Ask Claude which servers it found, too: the `remote_servers_list` and `remote_rescan` tools do that.

With pico-bay or pi-bay on several machines, all are used, each with its own key: their tools get a `host` argument, and listing the boards or devices asks every machine. With two `ctrlpi-mcp` servers, only the first one is used: two would drive the same agents.

If a server was already claimed by another computer, put its key in `remote-config.json` (see the [ctrlpi-remote README](https://github.com/ctrlpi/ctrlpi-remote#readme)), or reset it on the machine running it.

## The web interfaces

Ask Claude to open the pico-bay or pi-bay web interface, and it opens in the browser on this computer, at the address of the machine running it. The web interfaces have no key, so anyone on your network can open them. To turn them off, start pico-bay or pi-bay with `--ui off`; Claude then cannot open them.

## What it runs, sends and stores

- **Runs:** `npx -y ctrlpi-remote@0.9.51`, which downloads that exact version from the npm registry and starts it over stdio. The package has no dependencies.
- **Local network:** at the start (and when you ask it to search again) it probes every address of your local /24 subnet on ports 8315, 7426 and 3141 with `GET /hello` to find the servers. Every tool call is an HTTP request to that machine's `/mcp`, with its key.
- **Key:** the first time, it gives each server a new random key, so only this computer can use it afterwards. The keys never go through your conversation with Claude.
- **Stored on your computer:** the addresses and keys in `~/.ctrlpi/remote-config.json`, readable by you only.
- **Browser:** it opens the pico-bay or pi-bay web interface on this computer when you ask Claude to open it, and only at the address of the server that answered.
- **Nothing else:** it sends no data to ctrlPi or any other service on the internet, and has no telemetry.

Plain HTTP: the keys cross your local network unencrypted. Use it on trusted networks only.

Claude may show "Can run code without asking" and "Can send data to other services" when you install this plugin, because it starts a local server. The list above is exactly what it runs, sends and stores.

## License

[MIT](LICENSE)
