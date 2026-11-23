# Pi Bay Remote - SSH devices through pi-bay on a Pi

Work with the Raspberry Pis and other SSH devices on your network through a `pi-bay` that runs on a Raspberry Pi (or any other always-on machine), instead of on this computer. Ask Claude to find the devices, show a device's details, read and edit files, run commands, reboot a device, or install the ctrlPi API.

The plugin starts [ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) from npm, which finds the `pi-bay` on your network and forwards every request to it. The tools are the ones of the [pi-bay plugin](../pi-bay); the scan, the SSH sessions and the saved passwords are those of the machine running pi-bay.

**Use this or `pi-bay`, not both:** the two would reach the same devices.

## Requirements

- Node.js 20 or newer on this computer.
- `pi-bay` 0.9.50 or newer on the other machine, started with `npx pi-bay` (port 3141; add `--mcp http --host 0.0.0.0` on a machine that is not a Raspberry Pi). Its web interface (files, terminal, commands) has no key: disable it with `--ui off`. See the [pi-bay README](https://github.com/ctrlpi/pi-bay#readme).
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install `pi-bay-remote`.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install pi-bay-remote@ctrlpi
```

Then ask, for example: "Find the Raspberry Pis on my network" or "How much free space is on garage-pi?".

## How it connects

The first time, it searches your network for pi-bay, gives it a new random key, and saves the address and key in `~/.ctrlpi/remote-config.json`. From then on pi-bay accepts only that key. If the machine gets a new address, it is found again.

With pi-bay on several machines, all are used, each with its own key: every tool gets a `host` argument, and listing the devices asks every one.

If pi-bay was already claimed by another computer, put its key in `remote-config.json` (see the [ctrlpi-remote README](https://github.com/ctrlpi/ctrlpi-remote#readme)), or reset it on the machine running it.

## What it runs, sends and stores

- **Runs:** `npx -y ctrlpi-remote@0.9.50 pi-bay`, which downloads that exact version from the npm registry and starts it over stdio. The package has no dependencies.
- **Local network:** at the first start (and whenever a saved address stops answering) it probes every address of your local /24 subnet on port 3141 with `GET /hello` to find pi-bay. Every tool call is an HTTP request to that machine's `/mcp`, with the key.
- **Key:** the first time, it gives pi-bay a new random key, so only this computer can use it afterwards. The key never goes through your conversation with Claude.
- **Stored on your computer:** the address and key in `~/.ctrlpi/remote-config.json`, readable by you only.
- **Nothing else:** it sends no data to ctrlPi or any other service on the internet, and has no telemetry.

Plain HTTP: the key crosses your local network unencrypted. Use it on trusted networks only.

Claude may show "Can run code without asking" and "Can send data to other services" when you install this plugin, because it starts a local server. The list above is exactly what it runs, sends and stores.

## License

[MIT](LICENSE)
