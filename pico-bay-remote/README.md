# Pico Bay Remote - Pico and ESP32 boards on a Pi

Work with the Pico and ESP32 boards plugged into a Raspberry Pi on your local network, from Claude on your desktop. Ask Claude to list the boards, read and edit their files, run Python, reset a board, install MicroPython, or install the ctrlPi API.

The plugin starts [ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) from npm, which finds the `pico-bay` on your network and forwards every request to it. The tools are the ones of the [pico-bay plugin](../pico-bay).

It can run alongside the `pico-bay` plugin: that one handles the boards on this computer, this one the boards on the Pi.

## Requirements

- Node.js 20 or newer on this computer.
- `pico-bay` 0.9.50 or newer on the Pi, started with `npx pico-bay` (port 7426). Its web interface has no key: disable it with `--ui off`. See the [pico-bay README](https://github.com/ctrlpi/pico-bay#readme).
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install `pico-bay-remote`.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install pico-bay-remote@ctrlpi
```

Then ask, for example: "What boards are on the Pi?" or "Show me main.py on the Pico and run it".

## How it connects

The first time, it searches your network for pico-bay, gives it a new random key, and saves the address and key in `~/.ctrlpi/remote-config.json`. From then on pico-bay accepts only that key. If the machine gets a new address, it is found again.

With pico-bay on several Pis, all are used, each with its own key: every tool gets a `host` argument, and listing the boards asks every Pi.

If pico-bay was already claimed by another computer, put its key in `remote-config.json` (see the [ctrlpi-remote README](https://github.com/ctrlpi/ctrlpi-remote#readme)), or reset it on the machine running it.

## What it runs, sends and stores

- **Runs:** `npx -y ctrlpi-remote@0.9.50 pico-bay`, which downloads that exact version from the npm registry and starts it over stdio. The package has no dependencies.
- **Local network:** at the first start (and whenever a saved address stops answering) it probes every address of your local /24 subnet on port 7426 with `GET /hello` to find pico-bay. Every tool call is an HTTP request to that machine's `/mcp`, with the key.
- **Key:** the first time, it gives pico-bay a new random key, so only this computer can use it afterwards. The key never goes through your conversation with Claude.
- **Stored on your computer:** the address and key in `~/.ctrlpi/remote-config.json`, readable by you only.
- **Nothing else:** it sends no data to ctrlPi or any other service on the internet, and has no telemetry.

Plain HTTP: the key crosses your local network unencrypted. Use it on trusted networks only.

Claude may show "Can run code without asking" and "Can send data to other services" when you install this plugin, because it starts a local server. The list above is exactly what it runs, sends and stores.

## License

[MIT](LICENSE)
