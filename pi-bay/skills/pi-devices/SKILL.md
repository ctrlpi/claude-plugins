---
name: pi-devices
description: Use when the user asks to find Raspberry Pis or SSH devices on their network, check a device's details, read or edit files on it, run a command on it, reboot it, or install the ctrlPi API on a Pi.
---

# Pi Bay devices

1. Call `pi_devices_list` first and use its `device_id` in every other call. Pass `force_refresh` only when a device the user expects is missing.
2. If a call says the device needs a sign-in, do not ask for the password in chat. Suggest the user signs in once in the Pi Bay web app (`pi_ui`) with the password saved, or sets up SSH keys. Use `pi_device_authenticate` only when the user gives the credentials themselves.
3. For questions about a device, start with `pi_device_info` before running commands.
4. Read before you write: use `pi_file_read`, then `pi_file_write` with the whole new file. File paths are relative to the home folder.
5. `pi_command_run` runs with the signed-in user's rights. Ask the user before commands that change the system (installs, `sudo`, service changes, deleting outside the project), and before `pi_path_delete` and `pi_device_reboot`.
6. `pi_ctrlpi_install` can take several minutes. Ask first, pass `confirmed: true`, and show the user any sudo commands the result asks them to run.
