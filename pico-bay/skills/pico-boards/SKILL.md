---
name: pico-boards
description: Use when the user asks to work with a Raspberry Pi Pico or ESP32 board over USB: list boards, read or edit files, run MicroPython or CircuitPython code, reset a board, install firmware, or install the ctrlPi API.
---

# Pico Bay boards

1. Call `pico_boards_list` first and use its `board_id` in every other call. If several boards are connected, ask which one unless the user named it.
2. Read before you write: use `pico_files_list` and `pico_file_read` to see the current code, then `pico_file_write` with the whole new file.
3. To test code, prefer `pico_python_run` with `path` for a file on the board, or `code` for a short snippet. A program still running at `timeout_ms` is interrupted; use `pico_board_interrupt` to stop one yourself.
4. Writing `main.py` or `code.py` changes what the board runs at every start. Say so when you do it.
5. Ask the user before `pico_path_delete`, `pico_board_bootloader`, `pico_install` and `pico_ctrlpi_install`. For `pico_install`, call it with `action: "check"` first and show the result.
6. `pico_ctrlpi_install` needs the Wi-Fi name and password. Ask the user for them; do not guess or reuse values from elsewhere.
7. If a board is busy or missing, another program (Thonny, a serial monitor, the Pico Bay web app) may hold its port. Ask the user to close it.
