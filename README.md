# osu! on wine 5.16+
# This guide is incomplete

**This guide is for arch linux users, use this as a reference for other distros.**\
**Currently this guide is only for running osu in wine on linux. For tablet drivers follow [this](https://wiki.archlinux.org/index.php/wacom_tablet#Installation).\
I recommend using KDE plasma as your desktop environment which has a [GUI for tablet settings](https://www.archlinux.org/packages/?name=kcm-wacomtablet).**

Sources:\
[Lutris install script by unknown](https://lutris.net/games/install/3548/view),
[ThePoon's Blog](https://blog.thepoon.fr/osuLinuxAudioLatency/),
[Francesco149's guide](https://gist.github.com/Francesco149/a2f796683a4e5195458f4bb171d88eb0),
[AppDB wine](https://appdb.winehq.org/objectManager.php?sClass=version&iId=28025),
[.NET 4+ on 64bit](https://www.reddit.com/r/wine_gaming/comments/8r6low/guide_how_to_install_net_45_on_64bit_prefixes/?utm_source=amp&utm_medium=&utm_content=post_body),
[PulseAudio](https://wiki.archlinux.org/index.php/PulseAudio),
[Gaming on arch linux](https://wiki.archlinux.org/index.php/gaming)
[Wine ALSA PulseAudio sound](https://wiki.archlinux.org/index.php/PulseAudio#ALSA)

### Dependencies:
Provided you have your graphics drivers, desktop environment and `PulseAudio` for sound setup, you will need:\
`wine` or `wine-staging` (5.16 at the time of writing)\
`winetricks`\
`lib32-libxcomposite` (if osu! fails to render and exits)\
`lib32-gnutls` (if osu! fails to connect to the internet)\

### Preperation:
You will need a **copy of osu! installation from a Windows machine**, installer doesn't work in wine 5.16.

**gdiplus**: [windows6.1-KB976932-X86](http://download.windowsupdate.com/msdownload/update/software/svpk/2011/02/windows6.1-kb976932-x86_c3516bc5c9e69fee6d9ac4f981f5b95977a8a2fa.exe) or [windows6.1-KB976932-X64](http://download.windowsupdate.com/msdownload/update/software/svpk/2011/02/windows6.1-kb976932-x64_74865ef2562006e51d7f9333b4a8d45b7a749dab.exe) depending on your prefix, because winetricks 🙂. Keep these files just in case.

### Setup wine prefix:
#### Initialize:
My prefix is `osu-wine`, and I'm using 64bit, just because.\
**Remember to cancel the installation for Mono and Gecko.**
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 wineboot --init
```

#### Install gdiplus:
If sha256sum check failed, look at where the terminal output tells you to rename KB976932.exe. Copy the file downloaded previously (windows6.1-KB976932-X86 or X64) to that location and run the line again.\
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks gdiplus
```

#### Install fonts:
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks cjkfonts meiryo vlgothic`

### Install .NET 4.XX
Installing .NET is pain in wine 5.16. I have no idea which version works the best or if there is any benefit.\
I believe for osu! .NET 4.5 is required at the very least.

**I do not recommend `-q` flag especially for installing .NET, in wine 5.16 there is a problem after installation where .NET Runtime Optimization Service causes recursive processess to fill RAM and swap space.**

#### Install .NET 4.0-4.7.2:
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks dotnet472`\
This will prompt multiple installations from .NET 4.0 to 4.7.2.\
**Kill wineserver as soon as you click finish on an installation. Select Restart Later if prompted.**\
`WINEPREFIX=~/osu-wine wineserver -k`

#### (Optional) Install .NET 4.8:
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks dotnet48`\
**Kill wineserver as soon as you click finish on an installation. Select Restart Later if prompted.**\
`WINEPREFIX=~/osu-wine wineserver -k`

### Configuration
Copy your osu! installation into your wine prefix.\
Run it with your prefix and check that graphics and fonts are displaying correctly.\
Don't worry if you have no sound, we just need to check if assets are showing up nicely.\
`env WINEPREFIX=~/osu-wine WINEARCH=win64 wine ~/path-to-osu.exe`

#### winecfg
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winecfg`\
Under `Graphics`, `Window Settings`, make sure `Allow the window manager to decorate the windows` and `Allow the window manager to control the windows` are ticked.

#### Font rendering
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks settings fontsmooth=rgb`

#### Sound
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks sound=alsa`.\
Switch to ALSA. Remember to install `pulseaudio-alsa` if you are using PulseAudio, or follow wiki guide [here](https://wiki.archlinux.org/index.php/PulseAudio#ALSA).

#### Registry file for low latency (Not Recommended):
This registry edit was from Francesco149's guide.
I'm not recommending it because it introduces stuttering, with some freezing the game for up to 3 seconds.
While changes after applying this registry file is noticable, there isn't a lot of latency to begin with.
Looking at higher values for `HelBuflen` and `SndQueueMax` might work.
More about these values [here](https://wiki.winehq.org/Useful_Registry_Keys) and [here](https://www.codeweavers.com/support/wiki/linux/faq/cxoffice_soundissues).
```
cat > dsound.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Wine\DirectSound]
"HelBuflen"="512"
"SndQueueMax"="3"
EOF

env WINEPREFIX=~/osu-wine WINEARCH=win64 wine regedit dsound.reg
```

### Comment:
32bit prefix should work exactly the same.\
You don't need .NET 4.8 for osu! (I believe .NET 4.5 will do). This is just me trying to see if the newer stuff works better than the older ones. Just like why I'm trying to run osu on the latest version of wine.
