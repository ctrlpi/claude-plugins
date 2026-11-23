# Pico Bay - manage Pico and ESP32 boards over USB

Work with Raspberry Pi Pico and ESP32 boards plugged into your computer over USB, from Claude. Ask Claude to list the boards, read and edit their files, run Python and show the output, reset a board, install MicroPython firmware, or turn a Pico W or ESP32 into a ctrlPi network agent.

The plugin starts the [pico-bay](https://github.com/ctrlpi/pico-bay) MCP server from npm. Boards can run MicroPython or CircuitPython: Pico, Pico W, Pico 2, Pico 2 W, ESP32, ESP32-C3, ESP32-C6 and ESP32-S3.

## Requirements

- Node.js 20 or newer.
- A board connected over USB.
- macOS or Linux with Perl or Python 3, or Windows with the .NET Framework 4 that ships with it.
- The Claude desktop app (chat, Cowork or Code) or Claude Code.

## Install

In Claude Desktop: open **Plugins** in the sidebar, choose **Add > Add marketplace** and enter `ctrlpi/claude-plugins`, then install `pico-bay`.

In Claude Code:

```
/plugin marketplace add ctrlpi/claude-plugins
/plugin install pico-bay@ctrlpi
```

Then ask, for example: "What boards are connected?" or "Show me main.py on the Pico and run it".

## Tools

| Tool | What it does |
|---|---|
| `pico_boards_list` | List connected boards and what each can do |
| `pico_files_list`, `pico_file_read` | List the board's files and read one |
| `pico_file_write`, `pico_directory_create`, `pico_path_rename`, `pico_path_delete` | Write, create, rename and delete files and folders on the board |
| `pico_python_run`, `pico_board_interrupt` | Run a Python file or code on the board, and stop it |
| `pico_board_reset`, `pico_board_bootloader` | Reset a board, or restart it into firmware install mode |
| `pico_install` | Install MicroPython on a board in install mode |
| `pico_ctrlpi_install` | Install or update the ctrlPi API agent on a Pico W, Pico 2 W or ESP32 |
| `pico_ui` | Open or stop the Pico Bay web app in your browser |

`pico_install` and `pico_ctrlpi_install` take a `confirmed` flag and do nothing until Claude has your confirmation.

## What it runs, sends and stores

- **Runs:** `npx -y pico-bay@0.9.50 --mcp stdio`, which downloads that exact version from the npm registry and starts it over stdio. The package has no dependencies.
- **Serial helper:** to open USB serial ports, the server runs a small helper with the system's Perl or Python 3 on macOS and Linux. On Windows it compiles a small helper with the .NET Framework compiler that ships with Windows, and keeps it in the temp folder.
- **Downloads:** MicroPython firmware from `micropython.org` when you install firmware, and the ctrlPi agent files from `raw.githubusercontent.com/ctrlpi` when you install the ctrlPi API.
- **On the boards:** tools can write, rename and delete files, run any Python code, reset the board and replace its firmware. Installing the ctrlPi API writes your Wi-Fi name and password, and an optional API key, to the board.
- **Web app:** `pico_ui` starts a web server on `localhost` port 7426 and opens it in your browser. It stops when you ask.
- **Nothing else:** the server sends no data to ctrlPi or any other service, and has no telemetry. Files, code and Wi-Fi details go only to your board.

Claude may show "Can run code without asking" and "Can send data to other services" when you install this plugin, because it starts a local server. The list above is exactly what it runs, sends and stores.

## License

[MIT](LICENSE)
