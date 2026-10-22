# Sony BRAVIA XR-55X90L: setup notes

TV IP: 192.168.50.132. Run these from the repo folder in PowerShell. Each needs ADB debugging switched
back on on the TV first (Settings → System → Developer options).

## Undo everything
```powershell
powershell -ExecutionPolicy Bypass -File tools\restore.ps1 -TvHost 192.168.50.132 examples\sony-xr-55x90l\debloat.txt com.google.android.apps.tv.launcherx
```
That re-enables every package in `debloat.txt` and puts the stock Google TV home back.

## Re-enable one app
```powershell
$env:TV_HOST='192.168.50.132'; tools\venv\Scripts\python.exe -I tools\tv.py 'pm enable com.spotify.tv.android'
```
To get firmware updates again: `pm enable com.sony.dtv.systemupdate` and `pm enable com.sony.dtv.update`.

## Change the apps, order or look
Edit `themes\x90l-classic\config.json` (`dock_apps` sets the order; keys are listed in `themes\README.md`), then:
```powershell
powershell -ExecutionPolicy Bypass -File tools\theme.ps1 -TvHost 192.168.50.132 themes\x90l-classic
```
For a wallpaper, put a dark 1920×1080 `wallpaper.jpg` in that folder and set `"wallpaper": true`.
On the TV, long-press the Settings button on Home for reorder and launcher settings.

## Rebuild and reinstall the launcher
```powershell
$env:JAVA_HOME = (Get-ChildItem "$env:LOCALAPPDATA\cleanyourtv-build\jdk" -Directory)[0].FullName
powershell -ExecutionPolicy Bypass -File launcher\build-and-install.ps1 -TvHost 192.168.50.132 -HomeKeyService
```

## Kept on purpose
Living Decor (the art screensaver), Chromecast, AirPlay/HomeKit, Play Store, Live TV and inputs.
The backup taken before any change is in `backup\`.

## Customisations in this copy of the repo
- Font: Hanken Grotesk (OFL) replaces Outfit. The file is still named `res/font/outfit.ttf` so no layout
  changed; `launcher/FONT-LICENSE-OFL.txt` now carries Hanken's licence.
- New theme keys added to the launcher (documented in `themes/README.md`): `glow_color` (fixed amber
  glow), `show_greeting` (off), `focus_highlight` (`none`: no sliding outline, the tile lifts instead;
  `fade` is the in-between option).
- Remote: every button (app buttons, gear, Input, Google TV) tested by the owner and works with the
  stock launcher disabled and HomeKeyService on (gear = scancode 646 in `config.xml`).
