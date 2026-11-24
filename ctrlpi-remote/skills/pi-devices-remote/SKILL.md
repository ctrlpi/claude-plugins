---
name: pi-devices-remote
description: Use when the user asks to find Raspberry Pis or SSH devices, check a device, read or edit its files, run a command on it, reboot it, or install the ctrlPi API, through pi-bay running on a Raspberry Pi on the LAN (the ctrlpi-remote plugin).
---

# Pi Bay Remote devices

These tools reach the devices through a `pi-bay` running on another machine: the scan, SSH sessions and saved passwords are that machine's.

1. Call `pi_devices_list` first and use its `device_id` in every other call. With several pi-bay hosts, each tool has a `host` argument: `pi_devices_list` without it lists every host, and every other call needs the `host` the device was listed on. Pass `force_refresh` only when a device the user expects is missing. For GPIO pins on a Pi running the ctrlPi API, use the ctrlpi-mcp tools instead of shell commands.
2. If a call says the device needs a sign-in, do not ask for the password in chat. Suggest SSH keys or a password saved on the machine running pi-bay. Use `pi_device_authenticate` only when the user gives the credentials themselves, and pass `save_password` only if they ask.
3. For questions about a device, start with `pi_device_info` before running commands.
4. Read before you write: use `pi_file_read`, then `pi_file_write` with the whole new file. File paths are relative to the home folder.
5. `pi_command_run` runs with the signed-in user's rights. Ask the user before commands that change the system (installs, `sudo`, service changes, deleting outside the project), and before `pi_path_delete` and `pi_device_reboot`. Pass a larger `timeout_ms` (default 30 s, max 300 s) for long commands such as `apt`.
6. `pi_ctrlpi_install` can take several minutes. Ask first, pass `confirmed: true`, and show the user any sudo commands the result asks them to run. On a first install, ask for the agent name and API key, or let the user keep the default `your-secret-key`: if they do, say it is public and that script writes are refused until it is changed.
7. `pi_ui` opens the pi-bay page in the browser on this computer; if `opened` is false, give the user the `url`. It fails when pi-bay runs with `--ui off`; say so instead of retrying.
8. If a call fails or the tools are missing, call `remote_servers_list` and show the user the reason. "does not answer", "no pi-bay found" or "Cannot reach": pi-bay on the Pi is not running. "MCP is switched off": it runs without HTTP MCP. "is already claimed": another computer claimed it; copy its key into `~/.ctrlpi/remote-config.json`. "rejected the key": the key saved there no longer works; update it. "turned off": `targets` in that file leaves it out. After the user starts a server, call `remote_rescan`.
