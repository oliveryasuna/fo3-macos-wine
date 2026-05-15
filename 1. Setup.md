# Fallout 3 + macOS + Wine

## 1. Install Wine and Winetricks

```bash
brew install --cask wine-stable
brew install --cask winetricks
```

## 2. Set up the Wine environment

1. Run:

```bash
WINEPREFIX=~/.wine-fallout3 winecfg
```

2. Under "Applications", set "Windows Version" to "Windows 10".
3. Under "Graphics", activate "Automatically capture the mouse in full-screen windows".
4. Quit the Wine config dialog.
5. Install fonts:

```bash
WINEPREFIX=~/.wine-fallout3 winetricks corefonts
WINEPREFIX=~/.wine-fallout3 winetricks ttf-dejavu
WINEPREFIX=~/.wine-fallout3 winetricks ttf-bitstream-vera
```

## 3. Install Steam

> [!IMPORTANT]
> Do not launch Steam from the installer.

```bash
WINEPREFIX=~/.wine-fallout3 winetricks steam
```

## 4. Install SteamCMD

> [!NOTE]
> The Steam desktop client uses an embedded Chromium browser (CEF) to render its UI.
> Under Wine, CEF fails to render, producing a black window that makes login and navigation impossible.
> SteamCMD is Valve's command-line Steam client — it bypasses CEF entirely, allowing you to authenticate, download, and launch games without a working GUI.

1. Download [SteamCMD](https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip).
2. Unzip the file and place `steamcmd.exe` in `~/.wine-fallout3/drive_c/Program Files (x86)/Steam/`.

## 5. Install Fallout 3

1. Run:

```bash
WINEPREFIX=~/.wine-fallout3 wine '/Users/<username>/.wine-fallout3/drive_c/Program Files (x86)/Steam/steamcmd.exe'
```

2. When prompted with `Steam>`, enter:

```bash
login <username> <password>
```

3. Install Fallout 3:

```bash
app_update 22370
```

This may take a while.

4. Exit SteamCMD:

```bash
exit
```

## 6. Patch Windows registry

> [!NOTE]
> The Fallout 3 launcher checks the Windows registry to determine whether the game is installed.
> SteamCMD does not write these entries, so the launcher will report the game as not installed.
> We need to add them manually.

```bash
WINEPREFIX=~/.wine-fallout3 wine reg add "HKLM\Software\Bethesda Softworks\Fallout3" /v "Installed Path" /t REG_SZ /d "C:\Program Files (x86)\Steam\steamapps\common\Fallout 3 goty\\" /f
WINEPREFIX=~/.wine-fallout3 wine reg add "HKLM\Software\Wow6432Node\Bethesda Softworks\Fallout3" /v "Installed Path" /t REG_SZ /d "C:\Program Files (x86)\Steam\steamapps\common\Fallout 3 goty\\" /f
```

## 7. Patch xlive

1. Download [this](https://www.nexusmods.com/fallout3/mods/22591) mod.
2. Extract the ZIP and place `xlive.dll` in `~/.wine-fallout3/drive_c/Program Files (x86)/Steam/steamapps/common/Fallout 3 goty/`.

## 8. Run Fallout 3

```bash
WINEPREFIX=~/.wine-fallout3 wine '/Users/<username>/.wine-fallout3/drive_c/Program Files (x86)/Steam/steamapps/common/Fallout 3 goty/Fallout3Launcher.exe'
```
