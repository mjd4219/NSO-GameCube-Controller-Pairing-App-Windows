![Screenshot](images/screenshot.png)

# NSO GameCube Controller Pairing App - Windows Fork

This is a Windows-focused fork of
[RyanCopley/NSO-GameCube-Controller-Pairing-App](https://github.com/RyanCopley/NSO-GameCube-Controller-Pairing-App).

It is intended for Windows users who want a prebuilt EXE for the Nintendo Switch Online
GameCube Controller. The build has been tested on Windows 11 only.

## Support Status

- Tested on Windows 11.
- Not tested on macOS.
- Not tested on Linux.
- macOS/Linux users should use the upstream project unless they are comfortable testing
  and debugging this fork themselves.

This fork keeps the upstream Python source structure, but its releases and README are
aimed at Windows users.

## What This Fork Adds

- **Prebuilt Windows EXE**: available from this fork's GitHub Releases page.
- **Fixed system tray support in the compiled EXE**: adds the missing `pystray`
  dependency so "Minimize to system tray" works in packaged Windows builds.
- **Windows BLE frozen-build fixes**: includes WinRT modules needed by Bleak when
  packaged with PyInstaller.
- **Better BLE diagnostics**: forwards BLE subprocess stderr into the app log.
- **Safer BLE reconnect behavior**: avoids connecting to a different controller when
  reconnecting to a known target address.
- **Single-instance guard**: prevents accidentally launching multiple copies of the app.
- **Persistent debug log**: writes `gc_controller.log` on every run while keeping the
  console quiet by default.
- **Windows Startup folder autostart**: uses a per-user Startup folder launcher rather
  than Task Scheduler.
- **Optional ZL disconnect shortcut**: lets ZL disconnect the active controller when
  enabled in settings.
- **Home button mapping fix**: maps Home to Guide instead of Left Thumb.

## Download

Download the latest Windows build from:

https://github.com/mjd4219/NSO-GameCube-Controller-Pairing-App-Windows/releases

The release asset is:

`NSO-GameCube-Controller-Pairing-App.exe`

## Requirements

- Windows 11
- Nintendo Switch Online GameCube Controller
- ViGEmBus driver for Xbox 360 controller emulation:
  https://github.com/nefarius/ViGEmBus/releases

For Bluetooth, Windows uses Bleak/WinRT inside the packaged app. No Linux/macOS BLE
setup is covered by this fork.

## Usage

1. Install ViGEmBus and reboot if the installer asks you to.
2. Download `NSO-GameCube-Controller-Pairing-App.exe` from Releases.
3. Run the EXE.
4. Connect the controller over USB, or use **Pair Controller** for Bluetooth.
5. Choose your emulation/settings options in the app.

Settings and logs are stored in the app's user data folder. The debug log is named:

`gc_controller.log`

## Known Windows Notes

- USB rumble is limited by the standard Windows HID driver. Bluetooth rumble is the
  intended path for rumble on Windows.
- For USB rumble, advanced users may need a WinUSB driver via Zadig for the controller's
  vendor-specific interface. This is not required for normal Xbox 360 emulation.
- If Bluetooth input is laggy, try a USB Bluetooth adapter plugged into a front USB port
  or another port with a clearer signal path.

## Relationship To Upstream

This fork is not intended to replace upstream. It exists to publish a Windows 11-tested
build and collect Windows-specific fixes in one place.

Several changes have been proposed upstream as separate pull requests so maintainers can
review them independently.

## Building From Source On Windows

Install Python dependencies:

```bash
pip install -r requirements.txt
```

Run from source:

```bash
python -m gc_controller
```

Build the Windows executable:

```bat
platform\windows\build.bat
```

The compiled executable is written under `dist\windows`.

## Disclaimer

This fork has only been tested on Windows 11. I cannot claim macOS or Linux compatibility
for these fork-specific changes.
