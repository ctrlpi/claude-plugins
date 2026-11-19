# Pi Bay - manage Raspberry Pi and SSH devices over LAN

Work with the Raspberry Pis and other SSH devices on your local network, from Claude. Ask Claude to find the devices, show a device's hardware and system details, read and edit files in its home folder, run shell commands, reboot it, or install the ctrlPi API on a Pi.

The plugin starts the [pi-bay](https://github.com/ctrlpi/pi-bay) MCP server from npm.

## Requirements

- Node.js 20 or newer.
- Devices with SSH enabled, on the same local network as your computer.
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install **Pi Bay - manage Raspberry Pi and SSH devices over LAN**.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install pi-bay@ctrlpi
```

Then ask, for example: "Find the Raspberry Pis on my network" or "How much free space is on garage-pi?".

## Signing in

Only `pi_device_authenticate` takes a username and password. A password given to it passes through your conversation with Claude. To keep it out of the conversation, sign in once in the Pi Bay web app (ask Claude to open it with `pi_ui`) and save the password there, or use SSH keys. Later calls sign in by themselves with the saved password or your SSH keys.

## Tools

| Tool | What it does |
|---|---|
| `pi_devices_list` | Scan the network for Raspberry Pis and SSH devices |
| `pi_device_info` | Show a device's model, system, storage, memory, uptime and ctrlPi status |
| `pi_device_authenticate` | Sign in to a device, optionally saving the password in the system keychain |
| `pi_files_list`, `pi_file_read` | List and read files in the device's home folder |
| `pi_file_write`, `pi_directory_create`, `pi_path_rename`, `pi_path_delete` | Write, create, rename and delete files and folders in the home folder |
| `pi_command_run` | Run a shell command on the device |
| `pi_device_reboot` | Reboot the device |
| `pi_ctrlpi_install` | Install or update the ctrlPi API agent (pi-gpio-api) on a Pi |
| `pi_ui` | Open or stop the Pi Bay web app in your browser |

`pi_ctrlpi_install` takes a `confirmed` flag and does nothing until Claude has your confirmation.

## What it runs, sends and stores

- **Runs:** `npx -y pi-bay@0.9.41 --mcp-stdio`, which downloads that exact version and its dependencies (`ssh2`, `ws`, `@xterm/xterm`) from the npm registry and starts it over stdio.
- **Local network:** a scan probes every address of your local /24 subnet for SSH (port 22) and a ctrlPi agent (port 8314), and reads the computer's ARP table and Bonjour names. All device work goes over SSH and SFTP.
- **On the devices:** tools can write, rename and delete files in the home folder, run any shell command with the signed-in user's rights, and reboot the device. Installing the ctrlPi API on a Pi downloads its installer from `raw.githubusercontent.com/ctrlpi`.
- **Stored on your computer:** saved devices (address, MAC, username, model) in `~/.ctrlpi/pi-bay-devices.json`, and saved passwords in the macOS Keychain or Linux Secret Service, under the service name `pi-bay`.
- **Web app:** `pi_ui` starts a web server on `localhost` port 8383 and opens it in your browser. It stops when you ask.
- **Nothing else:** the server sends no data to ctrlPi or any other service, and has no telemetry.

## License

[MIT](LICENSE)
