# PSX WINCTRL PFPx Bridge

A lightweight bridge between **Aerowinx PSX** and a **WINCTRL / Winwing PFP CDU**.

The bridge connects directly to the CDU via HID and to PSX via TCP.
MobiFlight is no longer required.

## Features

* Sends CDU key presses from the WINCTRL PFP CDU to PSX
* Displays the active PSX CDU screen on the hardware CDU
* Supports Left, Center and Right PSX CDU selection
* Supports CDU annunciator LEDs
* Controls screen and key backlight brightness
* Shows a temporary brightness indicator while adjusting brightness
* Saves the last active CDU and brightness on clean shutdown
* Includes the CDU font internally

## Requirements

* Aerowinx PSX running with the TCP server enabled
* WINCTRL / Winwing PFP CDU connected by USB
* Python 3.9+ if running the `.py` version
* Or a packaged executable made with PyInstaller

## Files

Typical folder layout:

```text
macOS unzip and place in Applications Aerowinx PFPx.zip
psx_winctrl_pfp.exe
psx_winctrl_pfp.py
psx_winctrl_pfp.ini
```

For macOS, unzip `Aerowinx PFPx.zip` and place the application in the Applications folder.

## Configuration

## Supported devices

Set `pid` and `did` in `psx_winctrl_pfp.ini` to match the connected WINCTRL CDU.

| Device | Position      |  `pid` |  `did` |
| ------ | ------------- | -----: | -----: |
| PFP7   | Captain       | `BB37` | `33BB` |
| PFP7   | Observer      | `BB3B` | `33BB` |
| PFP7   | First Officer | `BB3F` | `33BB` |
| PFP4   | Captain       | `BB38` | `34BB` |
| PFP4   | Observer      | `BB3C` | `34BB` |
| PFP4   | First Officer | `BB40` | `34BB` |
| PFP3N  | Captain       | `BB35` | `31BB` |
| PFP3N  | Observer      | `BB39` | `31BB` |
| PFP3N  | First Officer | `BB3D` | `31BB` |
| MCDU   | Captain       | `BB36` | `32BB` |
| MCDU   | Observer      | `BB3A` | `32BB` |
| MCDU   | First Officer | `BB3E` | `32BB` |

For example, a PFP7 Captain CDU uses:

```ini
[FMC]
pid = BB37
did = 33BB
```

Example `psx_winctrl_pfp.ini`:

```ini
[PSX]
host = 127.0.0.1
port = 10747

[FMC]
pid = BB37
did = 33BB
ATC_KEY = ALTN
ACTIVE_CDU = L
BRIGHTNESS = 16
```

### PSX

| Setting | Description  |
| ------- | ------------ |
| `host`  | PSX TCP host |
| `port`  | PSX TCP port |

### FMC

| Setting      | Description                                              |
| ------------ | -------------------------------------------------------- |
| `pid`        | USB product ID of the CDU                                |
| `did`        | WINCTRL destination ID                                   |
| `ATC_KEY`    | `ATC` for original ATC key, `ALTN` to open the ALTN page |
| `ACTIVE_CDU` | Last selected CDU: `L`, `C` or `R`                       |
| `BRIGHTNESS` | Last brightness step, `0` to `23`                        |

`ACTIVE_CDU` and `BRIGHTNESS` are saved when the bridge is stopped cleanly with `CTRL+C`.

## Running

Start PSX first, then start the bridge.

Python:

```bash
python psx_winctrl_pfp.py
```

Debug mode:

```bash
python psx_winctrl_pfp.py --debug
```

Packaged executable on Windows:

```bash
psx_winctrl_pfp.exe
```

Stop the bridge with:

```text
CTRL+C
```

This allows the bridge to save the last active CDU and brightness before shutting down.

## Scratchpad commands

Enter these commands directly in the CDU scratchpad:

| Command    | Action                         |
| ---------- | ------------------------------ |
| `CDU-L`    | Select Left CDU                |
| `CDU-C`    | Select Center CDU              |
| `CDU-R`    | Select Right CDU               |
| `CDU-ATC`  | Use original ATC key behaviour |
| `CDU-ALTN` | Use ATC key to open ALTN page  |

`CDU-ATC` and `CDU-ALTN` are saved immediately.
`CDU-L`, `CDU-C` and `CDU-R` are saved on shutdown.

## Brightness

Use the CDU `BRT+` and `BRT-` keys to change screen and key backlight brightness. A temporary brightness indicator is shown on the scratchpad line while adjusting it.

The last brightness setting is saved as an example:

```ini
BRIGHTNESS = 16
```

The value range is `0` to `23`.

## Building an executable

Basic PyInstaller command:

```bash
pyinstaller --onefile --console --name psx_winctrl_pfp psx_winctrl_pfp.py
```

With an icon:

```bash
pyinstaller --onefile --console --name psx_winctrl_pfp --icon=<icon file> psx_winctrl_pfp.py
```

## Notes

* Run from a terminal when possible.
* Use `CTRL+C` to stop the bridge cleanly.
* Closing the console window directly may prevent settings from being saved.
* On macOS and Windows, `CTRL+C` is handled as a clean shutdown request.
* If the CDU display does not initialize correctly after restarting, unplug/replug the CDU and start the bridge again.
