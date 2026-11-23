# ctrlPi Remote - GPIO agents through ctrlpi-mcp on a Pi

Control your ctrlPi GPIO agents from Claude through a `ctrlpi-mcp` that runs on a Raspberry Pi (or any other always-on machine) on your local network, instead of on this computer. Ask Claude to read and switch pins, pulse a relay, read a sensor, or manage an agent's scripts.

The plugin starts [ctrlpi-remote](https://github.com/ctrlpi/ctrlpi-remote) from npm, which finds the `ctrlpi-mcp` on your network and forwards every request to it. The tools are the ones of the [ctrlpi-mcp plugin](../ctrlpi-mcp).

**Use this or `ctrlpi-mcp`, not both:** the two would control the same agents.

## Requirements

- Node.js 20 or newer on this computer.
- `ctrlpi-mcp` 0.9.50 or newer running on the other machine with `--mcp http` (port 8315). See the [ctrlpi-mcp README](https://github.com/ctrlpi/ctrlpi-mcp#readme).
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install `ctrlpi-mcp-remote`.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install ctrlpi-mcp-remote@ctrlpi
```

Then ask, for example: "Which agents are online?" or "Pulse the garage relay for 2 seconds".

## How it connects

The first time, it searches your network for ctrlpi-mcp, gives it a new random key, and saves the address and key in `~/.ctrlpi/remote-config.json`. From then on ctrlpi-mcp accepts only that key. If the machine gets a new address, it is found again.

If two `ctrlpi-mcp` servers answer on your network, only the first one is used: two would drive the same agents.

If ctrlpi-mcp was already claimed by another computer, put its key in `remote-config.json` (see the [ctrlpi-remote README](https://github.com/ctrlpi/ctrlpi-remote#readme)), or reset it on the machine running it.

## What it runs, sends and stores

- **Runs:** `npx -y ctrlpi-remote@0.9.50 ctrlpi-mcp`, which downloads that exact version from the npm registry and starts it over stdio. The package has no dependencies.
- **Local network:** at the first start (and whenever a saved address stops answering) it probes every address of your local /24 subnet on port 8315 with `GET /hello` to find ctrlpi-mcp. Every tool call is an HTTP request to that machine's `/mcp`, with the key.
- **Key:** the first time, it gives ctrlpi-mcp a new random key, so only this computer can use it afterwards. The key never goes through your conversation with Claude.
- **Stored on your computer:** the address and key in `~/.ctrlpi/remote-config.json`, readable by you only.
- **Nothing else:** it sends no data to ctrlPi or any other service on the internet, and has no telemetry.

Plain HTTP: the key crosses your local network unencrypted. Use it on trusted networks only.

Claude may show "Can run code without asking" and "Can send data to other services" when you install this plugin, because it starts a local server. The list above is exactly what it runs, sends and stores.

## License

[MIT](LICENSE)
