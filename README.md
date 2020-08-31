# osu! on wine 5.16+

**This guide is for arch linux users, use this as a reference for other distros.**\
**Currently this guide is only for running osu in wine on linux. For tablet drivers follow [this](https://wiki.archlinux.org/index.php/wacom_tablet#Installation).\
I recommend using KDE plasma as your desktop environment which has a [GUI for tablet settings](https://www.archlinux.org/packages/?name=kcm-wacomtablet).**

First up, some ancient guides for running osu! on wine:\
[Lutris install script by unknown](https://lutris.net/games/install/3548/view),
[ThePoon's Blog](https://blog.thepoon.fr/osuLinuxAudioLatency/),
[Francesco149's guide](https://gist.github.com/Francesco149/a2f796683a4e5195458f4bb171d88eb0),
[AppDB wine](https://appdb.winehq.org/objectManager.php?sClass=version&iId=28025),
[.NET 4+ on 64bit](https://www.reddit.com/r/wine_gaming/comments/8r6low/guide_how_to_install_net_45_on_64bit_prefixes/?utm_source=amp&utm_medium=&utm_content=post_body)

### Dependencies:
Provided you have your graphics drivers, desktop environment and `PulseAudio` for sound setup, you will need:\
`wine` or `wine-staging` (5.16 at the time of writing)\
`winetricks`\
`lib32-libxcomposite` (if osu! fails to render and exits)\
`lib32-gnutls` (if osu! fails to connect to the internet)\
~~`samba` (installation of .NET 4.0)~~\
~~`lib32-krb5` (installation of .NET 4.0)~~

### Preperation:
You will need a **copy of osu! installation from a Windows machine**, installer doesn't work in wine 5.16.

**gdiplus**: [windows6.1-KB976932-X86](http://download.windowsupdate.com/msdownload/update/software/svpk/2011/02/windows6.1-kb976932-x86_c3516bc5c9e69fee6d9ac4f981f5b95977a8a2fa.exe) or [windows6.1-KB976932-X64](http://download.windowsupdate.com/msdownload/update/software/svpk/2011/02/windows6.1-kb976932-x64_74865ef2562006e51d7f9333b4a8d45b7a749dab.exe) depending on your prefix, because winetricks 🙂. Keep these files just in case.

### Setup wine prefix:
#### Initialize:
`env WINEPREFIX=~/osu-wine WINEARCH=win64 wineboot --init`\
My prefix is `osu-wine`, and I'm using 64bit, just because.\
**Remember to cancel the installation for Mono and Gecko.**

#### Install gdiplus:
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks gdiplus`\
If sha256sum check failed, where the terminal output tells out to rename the file at a certain location, copy the file downloaded previously (windows6.1-KB976932-X86 or X64) to that location and run the line again.

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
`WINEPREFIX=~/osu-wine wineserver -k`\

#### (Optional) Install .NET 4.8:
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks dotnet48`\
**Kill wineserver as soon as you click finish on an installation. Select Restart Later if prompted.**\
`WINEPREFIX=~/osu-wine wineserver -k`\

### Configuration
Copy your osu! installation into your wine prefix.\
Run it with your prefix and check that graphics and fonts are displaying correctly.

### Comment:
32bit prefix should work exactly the same.\
You don't need .NET 4.8 for osu! (I believe .NET 4.5 will do). This is just me trying to see if the newer stuff works better than the older ones. Just like why I'm trying to run osu on the latest version of wine.
