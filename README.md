# osu! on wine

### If someone knows a cleaner way to deal with fonts please let me know
### If someone knows which (latest) version of dotnet osu actually needs please let me know

### Dependencies (arch):
Best way to make sure you have all of the graphics dependencies is to install `steam-native-runtime` \
~~`pulseaudio`~~ `pipewire` for performance \
`winetricks`\
`lib32-libxcomposite`\
`lib32-gnutls`\
`pulseaudio-alsa`\
`lib32-libpulse`\
`lib32-alsa-plugins`\

### IMPORTANT:
`samba` (fixes ntlm_auth errors)


### Preperation:
Windows 10 fonts

### Wine Setup:
Some versions of wine will have trouble with winetricks commands.\
You're on your own for those.
```
env WINEPREFIX=~/.osu-wine WINEARCH=win32 wineboot --init
env WINEPREFIX=~/.osu-wine WINEARCH=win32 winetricks sound=alsa

env WINEPREFIX=~/.osu-wine WINEARCH=win32 winetricks gdiplus
env WINEPREFIX=~/.osu-wine WINEARCH=win32 winetricks -q dotnet472 dotnet48

env WINEPREFIX=~/.osu-wine WINEARCH=win32 wine regedit font-1.reg
env WINEPREFIX=~/.osu-wine WINEARCH=win32 wine regedit font-2.reg
env WINEPREFIX=~/.osu-wine WINEARCH=win32 wine regedit dsound.reg

cp path-to-windows-fonts/* ~/.osu-wine/drive_c/windows/Fonts/*
```

### Discord Rich Presence
Grab the exe file from [here](https://github.com/0e4ef622/wine-discord-ipc-bridge).\
Run this exe file with the prefix and then osu! for working rpc.
Example working usage see `osu.sh`

### Launch Script and Desktop File
For my script `osu.sh` see `osu!.desktop` and `osu! (ripple).desktop` for usage \
Symlink to launch with launcher:
```
ln -sf ~/path-to-desktop-file/osu\!/.desktop ~/.local/share/applications/
```
