---
name: pico-boards-remote
description: "Use when the user asks to work with a Raspberry Pi Pico or ESP32 board plugged into another machine, such as a Raspberry Pi on the LAN, through pico-bay running there (the ctrlpi-remote plugin): list boards, read or edit files, run code, reset, install firmware or the ctrlPi API."
---

# Pico Bay Remote boards

These tools reach the boards plugged into another machine, not the ones on this computer (the local `pico-bay` plugin handles those).

1. Call `pico_boards_list` first and use its `board_id` in every other call. With several Pis, each tool has a `host` argument: `pico_boards_list` without it lists every host, and every other call needs the `host` the board was listed on. If several boards are connected, ask which one unless the user named it.
2. Read before you write: use `pico_files_list` and `pico_file_read` to see the current code, then `pico_file_write` with the whole new file.
3. To test code, prefer `pico_python_run` with `path` for a file on the board, or `code` for a short snippet. A program still running at `timeout_ms` is interrupted; use `pico_board_interrupt` to stop one yourself.
4. Writing `main.py` or `code.py` changes what the board runs at every start. Say so when you do it. On a board running the ctrlPi agent it replaces the agent: drive its pins with the ctrlpi-mcp tools instead.
5. Ask the user before `pico_path_delete`, `pico_board_bootloader`, `pico_install` and `pico_ctrlpi_install`, and pass `confirmed: true` once they agree. Call `pico_install` with `action: "check"` first and show the result; for a Pico, ask whether it is a W model (`wifi`).
6. `pico_ctrlpi_install` needs the Wi-Fi name and password. Ask the user for them; do not guess or reuse values from elsewhere. Ask for an API key too, or let the user keep the default `your-secret-key`: if they do, say it is public and that script writes are refused until it is changed.
7. `pico_ui` opens the pico-bay page in the browser on this computer; if `opened` is false, give the user the `url`. It fails when the Pi runs pico-bay with `--ui off`; say so instead of retrying.
8. If a call fails or the tools are missing, call `remote_servers_list` and show the user the reason. "does not answer", "no pico-bay found" or "Cannot reach": pico-bay on the Pi is not running. "MCP is switched off": it runs without HTTP MCP. "is already claimed": another computer claimed it; copy its key into `~/.ctrlpi/remote-config.json`. "rejected the key": the key saved there no longer works; update it. "turned off": `targets` in that file leaves it out. After the user starts a server, call `remote_rescan`.
