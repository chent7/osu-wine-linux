# osu! on wine 5.16+

#### This guide is for arch linux users, use this as a reference for other distros.

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

~~**.NET 4.0**: [dotNetFx40_Full_x86_x64.exe](https://download.microsoft.com/download/9/5/A/95A9616B-7A37-4AF6-BC36-D6EA96C8DAAE/dotNetFx40_Full_x86_x64.exe)~~

### Setup wine prefix:
Mine will be `osu-wine`.

`env WINEPREFIX=~/osu-wine WINEARCH=win64 wineboot --init`

**say no to mono and gecko.**

~~`env WINEPREFIX=~/osu-wine WINEARCH=win64 winecfg`\
Change your prefix to Windows XP.~~

Install .NET 4.0:\
~~`env WINEPREFIX=~/osu-wine WINEARCH=win64 wine ~/Downloads/dotNetFx40_Full_x86_x64.exe /q && WINEPREFIX=~/osu-wine wineserver -k`~~\
`env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks --force dotnet40`\
I do not recommend `-q` flag especially for installing .NET 4.0, in wine 5.16 there is a problem after installation where .NET Runtime Optimization Service causes recursive processess to fill RAM and swap space.

**Kill wineserver as soon as you click finish on the installation.**\
`WINEPREFIX=~/osu-wine wineserver -k`
