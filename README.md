# osu! on wine 5.17 archlinux

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
`pulseaudio`\
`wine-staging`\
`winetricks`\
`lib32-libxcomposite`\
`lib32-gnutls`\
`pulseaudio-alsa`\
`lib32-libpulse`\
`lib32-alsa-plugins`

### Preperation:
osu! installer\
Windows 10 fonts\
gdiplus: [windows6.1-KB976932-X86](http://download.windowsupdate.com/msdownload/update/software/svpk/2011/02/windows6.1-kb976932-x86_c3516bc5c9e69fee6d9ac4f981f5b95977a8a2fa.exe) (32-bit) or [windows6.1-KB976932-X64](http://download.windowsupdate.com/msdownload/update/software/svpk/2011/02/windows6.1-kb976932-x64_74865ef2562006e51d7f9333b4a8d45b7a749dab.exe) (64-bit)

### Wine Prefix:
Initialize:
My prefix is `.osu-wine`, 32 or 64 bit doesn't matter.
```
env WINEPREFIX=~/.osu-wine wineboot --init
```
gdiplus:
If sha256sum check failed, look at where the terminal output tells you to rename KB976932.exe. Copy the file downloaded previously (windows6.1-KB976932-X86 or X64) to that location and run the line again.
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks gdiplus
```
Install .NET 4.7.2:
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks dotnet472
```
This will prompt multiple installations from .NET 4.0 to 4.7.2.\
**Kill wineserver as soon as you click finish on an installation. Select Restart Later if prompted.**
```
WINEPREFIX=~/osu-wine wineserver -k
```
Install .NET 4.8:
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks dotnet48
```
**Kill wineserver as soon as you click finish on an installation. Select Restart Later if prompted.**
```
WINEPREFIX=~/osu-wine wineserver -k
```

### Configuration
**Before this step, make sure you install all dependencies and OpenGL drivers for your graphics card. Then reboot.**
**As of wine 5.17, osu! installer should work**
Installation:
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 wine ~/path-to-osu\!-installer.exe
```
winecfg:
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winecfg
```
Under `Graphics`, `Window Settings`, make sure `Allow the window manager to decorate the windows` and `Allow the window manager to control the windows` are ticked.

### Fonts:
Since winetricks options for fonts no longer works for osu!, I'm going to force install fonts.
Read about extracting Windows 10 fonts [here](https://wiki.archlinux.org/index.php/Microsoft_fonts#Extracting_fonts_from_a_Windows_ISO) if you don't have a copy.\
Create 2 registry files:
```
cat > font-1.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Fonts]
"Arial (TrueType)"="arial.ttf"
"Arial Black (TrueType)"="ariblk.ttf"
"Arial Bold (TrueType)"="arialbd.ttf"
"Arial Bold Italic (TrueType)"="arialbi.ttf"
"Arial Italic (TrueType)"="ariali.ttf"
"Bahnschrift (TrueType)"="bahnschrift.ttf"
"Calibri (TrueType)"="calibri.ttf"
"Calibri Bold (TrueType)"="calibrib.ttf"
"Calibri Bold Italic (TrueType)"="calibriz.ttf"
"Calibri Italic (TrueType)"="calibrii.ttf"
"Calibri Light (TrueType)"="calibril.ttf"
"Calibri Light Italic (TrueType)"="calibrili.ttf"
"Cambria & Cambria Math (TrueType)"="cambria.ttc"
"Cambria Bold (TrueType)"="cambriab.ttf"
"Cambria Bold Italic (TrueType)"="cambriaz.ttf"
"Cambria Italic (TrueType)"="cambriai.ttf"
"Candara (TrueType)"="candara.ttf"
"Candara Bold (TrueType)"="candarab.ttf"
"Candara Bold Italic (TrueType)"="candaraz.ttf"
"Candara Italic (TrueType)"="candarai.ttf"
"Candara Light (TrueType)"="candaral.ttf"
"Candara Light Italic (TrueType)"="candarali.ttf"
"Comic Sans MS (TrueType)"="comic.ttf"
"Comic Sans MS Bold (TrueType)"="comicbd.ttf"
"Comic Sans MS Bold Italic (TrueType)"="comicz.ttf"
"Comic Sans MS Italic (TrueType)"="comici.ttf"
"Consolas (TrueType)"="consola.ttf"
"Consolas Bold (TrueType)"="consolab.ttf"
"Consolas Bold Italic (TrueType)"="consolaz.ttf"
"Consolas Italic (TrueType)"="consolai.ttf"
"Constantia (TrueType)"="constan.ttf"
"Constantia Bold (TrueType)"="constanb.ttf"
"Constantia Bold Italic (TrueType)"="constanz.ttf"
"Constantia Italic (TrueType)"="constani.ttf"
"Corbel (TrueType)"="corbel.ttf"
"Corbel Bold (TrueType)"="corbelb.ttf"
"Corbel Bold Italic (TrueType)"="corbelz.ttf"
"Corbel Italic (TrueType)"="corbeli.ttf"
"Corbel Light (TrueType)"="corbell.ttf"
"Corbel Light Italic (TrueType)"="corbelli.ttf"
"Courier New (TrueType)"="cour.ttf"
"Courier New Bold (TrueType)"="courbd.ttf"
"Courier New Bold Italic (TrueType)"="courbi.ttf"
"Courier New Italic (TrueType)"="couri.ttf"
"Ebrima (TrueType)"="ebrima.ttf"
"Ebrima Bold (TrueType)"="ebrimabd.ttf"
"Franklin Gothic Medium (TrueType)"="framd.ttf"
"Franklin Gothic Medium Italic (TrueType)"="framdit.ttf"
"Gabriola (TrueType)"="Gabriola.ttf"
"Gadugi (TrueType)"="gadugi.ttf"
"Gadugi Bold (TrueType)"="gadugib.ttf"
"Georgia (TrueType)"="georgia.ttf"
"Georgia Bold (TrueType)"="georgiab.ttf"
"Georgia Bold Italic (TrueType)"="georgiaz.ttf"
"Georgia Italic (TrueType)"="georgiai.ttf"
"Impact (TrueType)"="impact.ttf"
"Ink Free (TrueType)"="Inkfree.ttf"
"Javanese Text (TrueType)"="javatext.ttf"
"Leelawadee UI (TrueType)"="leelawui.ttf"
"Leelawadee UI Bold (TrueType)"="leelauib.ttf"
"Leelawadee UI Semilight (TrueType)"="leeluisl.ttf"
"Lucida Console (TrueType)"="lucon.TTF"
"Lucida Sans Unicode (TrueType)"="l_10646.ttf"
"Malgun Gothic (TrueType)"="malgun.ttf"
"Malgun Gothic Bold (TrueType)"="malgunbd.ttf"
"Malgun Gothic SemiLight (TrueType)"="malgunsl.ttf"
"Microsoft Himalaya (TrueType)"="himalaya.ttf"
"Microsoft JhengHei & Microsoft JhengHei UI (TrueType)"="msjh.ttc"
"Microsoft JhengHei Bold & Microsoft JhengHei UI Bold (TrueType)"="msjhbd.ttc"
"Microsoft JhengHei Light & Microsoft JhengHei UI Light (TrueType)"="msjhl.ttc"
"Microsoft New Tai Lue (TrueType)"="ntailu.ttf"
"Microsoft New Tai Lue Bold (TrueType)"="ntailub.ttf"
"Microsoft PhagsPa (TrueType)"="phagspa.ttf"
"Microsoft PhagsPa Bold (TrueType)"="phagspab.ttf"
"Microsoft Sans Serif (TrueType)"="micross.ttf"
"Microsoft Tai Le (TrueType)"="taile.ttf"
"Microsoft Tai Le Bold (TrueType)"="taileb.ttf"
"Microsoft YaHei & Microsoft YaHei UI (TrueType)"="msyh.ttc"
"Microsoft YaHei Bold & Microsoft YaHei UI Bold (TrueType)"="msyhbd.ttc"
"Microsoft YaHei Light & Microsoft YaHei UI Light (TrueType)"="msyhl.ttc"
"Microsoft Yi Baiti (TrueType)"="msyi.ttf"
"MingLiU-ExtB & PMingLiU-ExtB & MingLiU_HKSCS-ExtB (TrueType)"="mingliub.ttc"
"Modern (All res)"="modern.fon"
"Mongolian Baiti (TrueType)"="monbaiti.ttf"
"MS Gothic & MS UI Gothic & MS PGothic (TrueType)"="msgothic.ttc"
"MV Boli (TrueType)"="mvboli.ttf"
"Myanmar Text (TrueType)"="mmrtext.ttf"
"Myanmar Text Bold (TrueType)"="mmrtextb.ttf"
"Nirmala UI (TrueType)"="Nirmala.ttf"
"Nirmala UI Bold (TrueType)"="NirmalaB.ttf"
"Nirmala UI Semilight (TrueType)"="NirmalaS.ttf"
"Palatino Linotype (TrueType)"="pala.ttf"
"Palatino Linotype Bold (TrueType)"="palab.ttf"
"Palatino Linotype Bold Italic (TrueType)"="palabi.ttf"
"Palatino Linotype Italic (TrueType)"="palai.ttf"
"Roman (All res)"="roman.fon"
"Script (All res)"="script.fon"
"Segoe MDL2 Assets (TrueType)"="segmdl2.ttf"
"Segoe Print (TrueType)"="segoepr.ttf"
"Segoe Print Bold (TrueType)"="segoeprb.ttf"
"Segoe Script (TrueType)"="segoesc.ttf"
"Segoe Script Bold (TrueType)"="segoescb.ttf"
"Segoe UI (TrueType)"="segoeui.ttf"
"Segoe UI Black (TrueType)"="seguibl.ttf"
"Segoe UI Black Italic (TrueType)"="seguibli.ttf"
"Segoe UI Bold (TrueType)"="segoeuib.ttf"
"Segoe UI Bold Italic (TrueType)"="segoeuiz.ttf"
"Segoe UI Emoji (TrueType)"="seguiemj.ttf"
"Segoe UI Historic (TrueType)"="seguihis.ttf"
"Segoe UI Italic (TrueType)"="segoeuii.ttf"
"Segoe UI Light (TrueType)"="segoeuil.ttf"
"Segoe UI Light Italic (TrueType)"="seguili.ttf"
"Segoe UI Semibold (TrueType)"="seguisb.ttf"
"Segoe UI Semibold Italic (TrueType)"="seguisbi.ttf"
"Segoe UI Semilight (TrueType)"="segoeuisl.ttf"
"Segoe UI Semilight Italic (TrueType)"="seguisli.ttf"
"Segoe UI Symbol (TrueType)"="seguisym.ttf"
"SimSun & NSimSun (TrueType)"="simsun.ttc"
"SimSun-ExtB (TrueType)"="simsunb.ttf"
"Sitka Small & Sitka Text & Sitka Subheading & Sitka Heading & Sitka Display & Sitka Banner (TrueType)"="Sitka.ttc"
"Sitka Small Bold & Sitka Text Bold & Sitka Subheading Bold & Sitka Heading Bold & Sitka Display Bold & Sitka Banner Bold (TrueType)"="SitkaB.ttc"
"Sitka Small Bold Italic & Sitka Text Bold Italic & Sitka Subheading Bold Italic & Sitka Heading Bold Italic & Sitka Display Bold Italic & Sitka Banner Bold Italic (TrueType)"="SitkaZ.ttc"
"Sitka Small Italic & Sitka Text Italic & Sitka Subheading Italic & Sitka Heading Italic & Sitka Display Italic & Sitka Banner Italic (TrueType)"="SitkaI.ttc"
"Sylfaen (TrueType)"="sylfaen.ttf"
"Symbol (TrueType)"="symbol.ttf"
"Tahoma (TrueType)"="tahoma.ttf"
"Tahoma Bold (TrueType)"="tahomabd.ttf"
"Times New Roman (TrueType)"="times.ttf"
"Times New Roman Bold (TrueType)"="timesbd.ttf"
"Times New Roman Bold Italic (TrueType)"="timesbi.ttf"
"Times New Roman Italic (TrueType)"="timesi.ttf"
"Trebuchet MS (TrueType)"="trebuc.ttf"
"Trebuchet MS Bold (TrueType)"="trebucbd.ttf"
"Trebuchet MS Bold Italic (TrueType)"="trebucbi.ttf"
"Trebuchet MS Italic (TrueType)"="trebucit.ttf"
"Verdana (TrueType)"="verdana.ttf"
"Verdana Bold (TrueType)"="verdanab.ttf"
"Verdana Bold Italic (TrueType)"="verdanaz.ttf"
"Verdana Italic (TrueType)"="verdanai.ttf"
"Webdings (TrueType)"="webdings.ttf"
"Wingdings (TrueType)"="wingding.ttf"
"Yu Gothic Bold & Yu Gothic UI Semibold & Yu Gothic UI Bold (TrueType)"="YuGothB.ttc"
"Yu Gothic Light & Yu Gothic UI Light (TrueType)"="YuGothL.ttc"
"Yu Gothic Medium & Yu Gothic UI Regular (TrueType)"="YuGothM.ttc"
"Yu Gothic Regular & Yu Gothic UI Semilight (TrueType)"="YuGothR.ttc"
"Courier 10,12,15"="COURE.FON"
"Courier 10,12,15 (120)"="COURF.FON"
"MS Serif 8,10,12,14,18,24"="SERIFE.FON"
"MS Serif 8,10,12,14,18,24 (120)"="SERIFF.FON"
"MS Sans Serif 8,10,12,14,18,24"="SSERIFE.FON"
"MS Sans Serif 8,10,12,14,18,24 (120)"="SSERIFF.FON"
"Small Fonts"="SMALLE.FON"
"Small Fonts (120)"="SMALLF.FON"
"Holo MDL2 Assets (TrueType)"="holomdl2.ttf"
"DFKai-SB (TrueType)"="kaiu.ttf"
"MingLiU & PMingLiU & MingLiU_HKSCS (TrueType)"="mingliu.ttc"
"Arial Unicode MS (TrueType)"="ARIALUNI.TTF"
"Century (TrueType)"="CENTURY.TTF"
"Meiryo & Meiryo Italic & Meiryo UI & Meiryo UI Italic (TrueType)"="MEIRYO.TTC"
"Wingdings 2 (TrueType)"="WINGDNG2.TTF"
"Wingdings 3 (TrueType)"="WINGDNG3.TTF"
"Book Antiqua (TrueType)"="BKANT.TTF"
"Century Gothic (TrueType)"="GOTHIC.TTF"
"Tempus Sans ITC (TrueType)"="TEMPSITC.TTF"
"Mistral (TrueType)"="MISTRAL.TTF"
"Lucida Handwriting Italic (TrueType)"="LHANDW.TTF"
"Kristen ITC (TrueType)"="ITCKRIST.TTF"
"Juice ITC (TrueType)"="JUICE___.TTF"
"Freestyle Script (TrueType)"="FREESCPT.TTF"
"Arial Narrow (TrueType)"="ARIALN.TTF"
"Garamond (TrueType)"="GARA.TTF"
"Monotype Corsiva (TrueType)"="MTCORSVA.TTF"
"Algerian (TrueType)"="ALGER.TTF"
"Baskerville Old Face (TrueType)"="BASKVILL.TTF"
"Bauhaus 93 (TrueType)"="BAUHS93.TTF"
"Bell MT (TrueType)"="BELL.TTF"
"Berlin Sans FB Bold (TrueType)"="BRLNSB.TTF"
"Bernard MT Condensed (TrueType)"="BERNHC.TTF"
"Bodoni MT Poster Compressed (TrueType)"="BOD_PSTC.TTF"
"Britannic Bold (TrueType)"="BRITANIC.TTF"
"Broadway (TrueType)"="BROADW.TTF"
"Brush Script MT Italic (TrueType)"="BRUSHSCI.TTF"
"Californian FB (TrueType)"="CALIFR.TTF"
"Centaur (TrueType)"="CENTAUR.TTF"
"Chiller (TrueType)"="CHILLER.TTF"
"Colonna MT (TrueType)"="COLONNA.TTF"
"Cooper Black (TrueType)"="COOPBL.TTF"
"Footlight MT Light (TrueType)"="FTLTLT.TTF"
"Harlow Solid Italic (TrueType)"="HARLOWSI.TTF"
"Harrington (TrueType)"="HARNGTON.TTF"
"High Tower Text (TrueType)"="HTOWERT.TTF"
"Jokerman (TrueType)"="JOKERMAN.TTF"
"Kunstler Script (TrueType)"="KUNSTLER.TTF"
"Lucida Bright (TrueType)"="LBRITE.TTF"
"Lucida Calligraphy Italic (TrueType)"="LCALLIG.TTF"
"Lucida Fax Regular (TrueType)"="LFAX.TTF"
"Magneto Bold (TrueType)"="MAGNETOB.TTF"
"Matura MT Script Capitals (TrueType)"="MATURASC.TTF"
"Modern No. 20 (TrueType)"="MOD20.TTF"
"Niagara Engraved (TrueType)"="NIAGENG.TTF"
"Niagara Solid (TrueType)"="NIAGSOL.TTF"
"Old English Text MT (TrueType)"="OLDENGL.TTF"
"Onyx (TrueType)"="ONYX.TTF"
"Parchment (TrueType)"="PARCHM.TTF"
"Playbill (TrueType)"="PLAYBILL.TTF"
"Poor Richard (TrueType)"="POORICH.TTF"
"Ravie (TrueType)"="RAVIE.TTF"
"Informal Roman (TrueType)"="INFROMAN.TTF"
"Showcard Gothic (TrueType)"="SHOWG.TTF"
"Snap ITC (TrueType)"="SNAP____.TTF"
"Stencil (TrueType)"="STENCIL.TTF"
"Viner Hand ITC (TrueType)"="VINERITC.TTF"
"Vivaldi Italic (TrueType)"="VIVALDII.TTF"
"Vladimir Script (TrueType)"="VLADIMIR.TTF"
"Wide Latin (TrueType)"="LATINWD.TTF"
"Bookman Old Style (TrueType)"="BOOKOS.TTF"
"Meiryo Bold & Meiryo Bold Italic & Meiryo UI Bold & Meiryo UI Bold Italic (TrueType)"="MEIRYOB.TTC"
"Book Antiqua Bold (TrueType)"="ANTQUAB.TTF"
"Book Antiqua Bold Italic (TrueType)"="ANTQUABI.TTF"
"Book Antiqua Italic (TrueType)"="ANTQUAI.TTF"
"Century Gothic Bold (TrueType)"="GOTHICB.TTF"
"Century Gothic Bold Italic (TrueType)"="GOTHICBI.TTF"
"Century Gothic Italic (TrueType)"="GOTHICI.TTF"
"Arial Narrow Bold (TrueType)"="ARIALNB.TTF"
"Arial Narrow Bold Italic (TrueType)"="ARIALNBI.TTF"
"Arial Narrow Italic (TrueType)"="ARIALNI.TTF"
"Garamond Bold (TrueType)"="GARABD.TTF"
"Garamond Italic (TrueType)"="GARAIT.TTF"
"Bell MT Bold (TrueType)"="BELLB.TTF"
"Bell MT Italic (TrueType)"="BELLI.TTF"
"Berlin Sans FB Demi Bold (TrueType)"="BRLNSDB.TTF"
"Berlin Sans FB (TrueType)"="BRLNSR.TTF"
"Californian FB Bold (TrueType)"="CALIFB.TTF"
"Californian FB Italic (TrueType)"="CALIFI.TTF"
"High Tower Text Italic (TrueType)"="HTOWERTI.TTF"
"Lucida Bright Demibold (TrueType)"="LBRITED.TTF"
"Lucida Bright Demibold Italic (TrueType)"="LBRITEDI.TTF"
"Lucida Bright Italic (TrueType)"="LBRITEI.TTF"
"Lucida Fax Demibold (TrueType)"="LFAXD.TTF"
"Lucida Fax Demibold Italic (TrueType)"="LFAXDI.TTF"
"Lucida Fax Italic (TrueType)"="LFAXI.TTF"
"Bookman Old Style Bold (TrueType)"="BOOKOSB.TTF"
"Bookman Old Style Bold Italic (TrueType)"="BOOKOSBI.TTF"
"Bookman Old Style Italic (TrueType)"="BOOKOSI.TTF"
"Agency FB Bold (TrueType)"="AGENCYB.TTF"
"Agency FB (TrueType)"="AGENCYR.TTF"
"Arial Rounded MT Bold (TrueType)"="ARLRDBD.TTF"
"Bodoni MT Bold (TrueType)"="BOD_B.TTF"
"Bodoni MT Bold Italic (TrueType)"="BOD_BI.TTF"
"Bodoni MT Black Italic (TrueType)"="BOD_BLAI.TTF"
"Bodoni MT Black (TrueType)"="BOD_BLAR.TTF"
"Bodoni MT Condensed Bold (TrueType)"="BOD_CB.TTF"
"Bodoni MT Condensed Bold Italic (TrueType)"="BOD_CBI.TTF"
"Bodoni MT Condensed Italic (TrueType)"="BOD_CI.TTF"
"Bodoni MT Condensed (TrueType)"="BOD_CR.TTF"
"Bodoni MT Italic (TrueType)"="BOD_I.TTF"
"Bodoni MT (TrueType)"="BOD_R.TTF"
"Bradley Hand ITC (TrueType)"="BRADHITC.TTF"
"Bookshelf Symbol 7 (TrueType)"="BSSYM7.TTF"
"Calisto MT (TrueType)"="CALIST.TTF"
"Calisto MT Bold (TrueType)"="CALISTB.TTF"
"Calisto MT Bold Italic (TrueType)"="CALISTBI.TTF"
"Calisto MT Italic (TrueType)"="CALISTI.TTF"
"Castellar (TrueType)"="CASTELAR.TTF"
"Century Schoolbook (TrueType)"="CENSCBK.TTF"
"Copperplate Gothic Bold (TrueType)"="COPRGTB.TTF"
"Copperplate Gothic Light (TrueType)"="COPRGTL.TTF"
"Curlz MT (TrueType)"="CURLZ___.TTF"
"Dubai Bold (TrueType)"="DUBAI-BOLD.TTF"
"Dubai Light (TrueType)"="DUBAI-LIGHT.TTF"
"Dubai Medium (TrueType)"="DUBAI-MEDIUM.TTF"
"Dubai Regular (TrueType)"="DUBAI-REGULAR.TTF"
"Elephant (TrueType)"="ELEPHNT.TTF"
"Elephant Italic (TrueType)"="ELEPHNTI.TTF"
"Engravers MT (TrueType)"="ENGR.TTF"
"Eras Bold ITC (TrueType)"="ERASBD.TTF"
"Eras Demi ITC (TrueType)"="ERASDEMI.TTF"
"Eras Light ITC (TrueType)"="ERASLGHT.TTF"
"Eras Medium ITC (TrueType)"="ERASMD.TTF"
"Felix Titling (TrueType)"="FELIXTI.TTF"
"Forte (TrueType)"="FORTE.TTF"
"Franklin Gothic Book (TrueType)"="FRABK.TTF"
"Franklin Gothic Book Italic (TrueType)"="FRABKIT.TTF"
"Franklin Gothic Demi (TrueType)"="FRADM.TTF"
"Franklin Gothic Demi Cond (TrueType)"="FRADMCN.TTF"
"Franklin Gothic Demi Italic (TrueType)"="FRADMIT.TTF"
"Franklin Gothic Heavy (TrueType)"="FRAHV.TTF"
"Franklin Gothic Heavy Italic (TrueType)"="FRAHVIT.TTF"
"Franklin Gothic Medium Cond (TrueType)"="FRAMDCN.TTF"
"French Script MT (TrueType)"="FRSCRIPT.TTF"
"Gigi (TrueType)"="GIGI.TTF"
"Gill Sans MT Bold Italic (TrueType)"="GILBI___.TTF"
"Gill Sans MT Bold (TrueType)"="GILB____.TTF"
"Gill Sans MT Condensed (TrueType)"="GILC____.TTF"
"Gill Sans MT Italic (TrueType)"="GILI____.TTF"
"Gill Sans Ultra Bold Condensed (TrueType)"="GILLUBCD.TTF"
"Gill Sans Ultra Bold (TrueType)"="GILSANUB.TTF"
"Gill Sans MT (TrueType)"="GIL_____.TTF"
"Gloucester MT Extra Condensed (TrueType)"="GLECB.TTF"
"Gill Sans MT Ext Condensed Bold (TrueType)"="GLSNECB.TTF"
"Goudy Old Style (TrueType)"="GOUDOS.TTF"
"Goudy Old Style Bold (TrueType)"="GOUDOSB.TTF"
"Goudy Old Style Italic (TrueType)"="GOUDOSI.TTF"
"Goudy Stout (TrueType)"="GOUDYSTO.TTF"
"Haettenschweiler (TrueType)"="HATTEN.TTF"
"Imprint MT Shadow (TrueType)"="IMPRISHA.TTF"
"Blackadder ITC (TrueType)"="ITCBLKAD.TTF"
"Edwardian Script ITC (TrueType)"="ITCEDSCR.TTF"
"Leelawadee (TrueType)"="LEELAWAD.TTF"
"Leelawadee Bold (TrueType)"="LEELAWDB.TTF"
"Lucida Sans Regular (TrueType)"="LSANS.TTF"
"Lucida Sans Demibold Roman (TrueType)"="LSANSD.TTF"
"Lucida Sans Demibold Italic (TrueType)"="LSANSDI.TTF"
"Lucida Sans Italic (TrueType)"="LSANSI.TTF"
"Lucida Sans Typewriter Regular (TrueType)"="LTYPE.TTF"
"Lucida Sans Typewriter Bold (TrueType)"="LTYPEB.TTF"
"Lucida Sans Typewriter Bold Oblique (TrueType)"="LTYPEBO.TTF"
"Lucida Sans Typewriter Oblique (TrueType)"="LTYPEO.TTF"
"Maiandra GD (TrueType)"="MAIAN.TTF"
"Microsoft Uighur Bold (TrueType)"="MSUIGHUB.TTF"
"Microsoft Uighur (TrueType)"="MSUIGHUR.TTF"
"OCR A Extended (TrueType)"="OCRAEXT.TTF"
"MS Outlook (TrueType)"="OUTLOOK.TTF"
"Palace Script MT (TrueType)"="PALSCRI.TTF"
"Papyrus (TrueType)"="PAPYRUS.TTF"
"Perpetua Bold Italic (TrueType)"="PERBI___.TTF"
"Perpetua Bold (TrueType)"="PERB____.TTF"
"Perpetua Italic (TrueType)"="PERI____.TTF"
"Perpetua Titling MT Bold (TrueType)"="PERTIBD.TTF"
"Perpetua Titling MT Light (TrueType)"="PERTILI.TTF"
"Perpetua (TrueType)"="PER_____.TTF"
"Pristina (TrueType)"="PRISTINA.TTF"
"Rage Italic (TrueType)"="RAGE.TTF"
"MS Reference Sans Serif (TrueType)"="REFSAN.TTF"
"MS Reference Specialty (TrueType)"="REFSPCL.TTF"
"Rockwell Condensed Bold (TrueType)"="ROCCB___.TTF"
"Rockwell Condensed (TrueType)"="ROCC____.TTF"
"Rockwell (TrueType)"="ROCK.TTF"
"Rockwell Bold (TrueType)"="ROCKB.TTF"
"Rockwell Bold Italic (TrueType)"="ROCKBI.TTF"
"Rockwell Extra Bold (TrueType)"="ROCKEB.TTF"
"Rockwell Italic (TrueType)"="ROCKI.TTF"
"Century Schoolbook Bold (TrueType)"="SCHLBKB.TTF"
"Century Schoolbook Bold Italic (TrueType)"="SCHLBKBI.TTF"
"Century Schoolbook Italic (TrueType)"="SCHLBKI.TTF"
"Script MT Bold (TrueType)"="SCRIPTBL.TTF"
"Tw Cen MT Bold Italic (TrueType)"="TCBI____.TTF"
"Tw Cen MT Bold (TrueType)"="TCB_____.TTF"
"Tw Cen MT Condensed Bold (TrueType)"="TCCB____.TTF"
"Tw Cen MT Condensed Extra Bold (TrueType)"="TCCEB.TTF"
"Tw Cen MT Condensed (TrueType)"="TCCM____.TTF"
"Tw Cen MT Italic (TrueType)"="TCMI____.TTF"
"Tw Cen MT (TrueType)"="TCM_____.TTF"
"MT Extra (TrueType)"="C:\\Program Files (x86)\\Common Files\\Microsoft Shared\\EQUATION\\MTEXTRA.TTF"
"HP Simplified Bold (TrueType)"="HPSimplified_Bd.ttf"
"HP Simplified Bold Italic (TrueType)"="HPSimplified_BdIt.ttf"
"HP Simplified Italic (TrueType)"="HPSimplified_It.ttf"
"HP Simplified Light (TrueType)"="HPSimplified_Lt.ttf"
"HP Simplified Light Italic (TrueType)"="HPSimplified_LtIt.ttf"
"HP Simplified (TrueType)"="HPSimplified_Rg.ttf"
"Source Code Pro Black (TrueType)"="SourceCodePro-Black.ttf"
"Source Code Pro Bold (TrueType)"="SourceCodePro-Bold.ttf"
"Source Code Pro ExtraLight (TrueType)"="SourceCodePro-ExtraLight.ttf"
"Source Code Pro Light (TrueType)"="SourceCodePro-Light.ttf"
"Source Code Pro (TrueType)"="SourceCodePro-Regular.ttf"
"Source Code Pro Semibold (TrueType)"="SourceCodePro-Semibold.ttf"

EOF
```

```
cat > font-2.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Fonts]
"Arial (TrueType)"="arial.ttf"
"Arial Black (TrueType)"="ariblk.ttf"
"Arial Bold (TrueType)"="arialbd.ttf"
"Arial Bold Italic (TrueType)"="arialbi.ttf"
"Arial Italic (TrueType)"="ariali.ttf"
"Bahnschrift (TrueType)"="bahnschrift.ttf"
"Calibri (TrueType)"="calibri.ttf"
"Calibri Bold (TrueType)"="calibrib.ttf"
"Calibri Bold Italic (TrueType)"="calibriz.ttf"
"Calibri Italic (TrueType)"="calibrii.ttf"
"Calibri Light (TrueType)"="calibril.ttf"
"Calibri Light Italic (TrueType)"="calibrili.ttf"
"Cambria & Cambria Math (TrueType)"="cambria.ttc"
"Cambria Bold (TrueType)"="cambriab.ttf"
"Cambria Bold Italic (TrueType)"="cambriaz.ttf"
"Cambria Italic (TrueType)"="cambriai.ttf"
"Candara (TrueType)"="candara.ttf"
"Candara Bold (TrueType)"="candarab.ttf"
"Candara Bold Italic (TrueType)"="candaraz.ttf"
"Candara Italic (TrueType)"="candarai.ttf"
"Candara Light (TrueType)"="candaral.ttf"
"Candara Light Italic (TrueType)"="candarali.ttf"
"Comic Sans MS (TrueType)"="comic.ttf"
"Comic Sans MS Bold (TrueType)"="comicbd.ttf"
"Comic Sans MS Bold Italic (TrueType)"="comicz.ttf"
"Comic Sans MS Italic (TrueType)"="comici.ttf"
"Consolas (TrueType)"="consola.ttf"
"Consolas Bold (TrueType)"="consolab.ttf"
"Consolas Bold Italic (TrueType)"="consolaz.ttf"
"Consolas Italic (TrueType)"="consolai.ttf"
"Constantia (TrueType)"="constan.ttf"
"Constantia Bold (TrueType)"="constanb.ttf"
"Constantia Bold Italic (TrueType)"="constanz.ttf"
"Constantia Italic (TrueType)"="constani.ttf"
"Corbel (TrueType)"="corbel.ttf"
"Corbel Bold (TrueType)"="corbelb.ttf"
"Corbel Bold Italic (TrueType)"="corbelz.ttf"
"Corbel Italic (TrueType)"="corbeli.ttf"
"Corbel Light (TrueType)"="corbell.ttf"
"Corbel Light Italic (TrueType)"="corbelli.ttf"
"Courier New (TrueType)"="cour.ttf"
"Courier New Bold (TrueType)"="courbd.ttf"
"Courier New Bold Italic (TrueType)"="courbi.ttf"
"Courier New Italic (TrueType)"="couri.ttf"
"Ebrima (TrueType)"="ebrima.ttf"
"Ebrima Bold (TrueType)"="ebrimabd.ttf"
"Franklin Gothic Medium (TrueType)"="framd.ttf"
"Franklin Gothic Medium Italic (TrueType)"="framdit.ttf"
"Gabriola (TrueType)"="Gabriola.ttf"
"Gadugi (TrueType)"="gadugi.ttf"
"Gadugi Bold (TrueType)"="gadugib.ttf"
"Georgia (TrueType)"="georgia.ttf"
"Georgia Bold (TrueType)"="georgiab.ttf"
"Georgia Bold Italic (TrueType)"="georgiaz.ttf"
"Georgia Italic (TrueType)"="georgiai.ttf"
"Impact (TrueType)"="impact.ttf"
"Ink Free (TrueType)"="Inkfree.ttf"
"Javanese Text (TrueType)"="javatext.ttf"
"Leelawadee UI (TrueType)"="leelawui.ttf"
"Leelawadee UI Bold (TrueType)"="leelauib.ttf"
"Leelawadee UI Semilight (TrueType)"="leeluisl.ttf"
"Lucida Console (TrueType)"="lucon.TTF"
"Lucida Sans Unicode (TrueType)"="l_10646.ttf"
"Malgun Gothic (TrueType)"="malgun.ttf"
"Malgun Gothic Bold (TrueType)"="malgunbd.ttf"
"Malgun Gothic SemiLight (TrueType)"="malgunsl.ttf"
"Microsoft Himalaya (TrueType)"="himalaya.ttf"
"Microsoft JhengHei & Microsoft JhengHei UI (TrueType)"="msjh.ttc"
"Microsoft JhengHei Bold & Microsoft JhengHei UI Bold (TrueType)"="msjhbd.ttc"
"Microsoft JhengHei Light & Microsoft JhengHei UI Light (TrueType)"="msjhl.ttc"
"Microsoft New Tai Lue (TrueType)"="ntailu.ttf"
"Microsoft New Tai Lue Bold (TrueType)"="ntailub.ttf"
"Microsoft PhagsPa (TrueType)"="phagspa.ttf"
"Microsoft PhagsPa Bold (TrueType)"="phagspab.ttf"
"Microsoft Sans Serif (TrueType)"="micross.ttf"
"Microsoft Tai Le (TrueType)"="taile.ttf"
"Microsoft Tai Le Bold (TrueType)"="taileb.ttf"
"Microsoft YaHei & Microsoft YaHei UI (TrueType)"="msyh.ttc"
"Microsoft YaHei Bold & Microsoft YaHei UI Bold (TrueType)"="msyhbd.ttc"
"Microsoft YaHei Light & Microsoft YaHei UI Light (TrueType)"="msyhl.ttc"
"Microsoft Yi Baiti (TrueType)"="msyi.ttf"
"MingLiU-ExtB & PMingLiU-ExtB & MingLiU_HKSCS-ExtB (TrueType)"="mingliub.ttc"
"Modern (All res)"="modern.fon"
"Mongolian Baiti (TrueType)"="monbaiti.ttf"
"MS Gothic & MS UI Gothic & MS PGothic (TrueType)"="msgothic.ttc"
"MV Boli (TrueType)"="mvboli.ttf"
"Myanmar Text (TrueType)"="mmrtext.ttf"
"Myanmar Text Bold (TrueType)"="mmrtextb.ttf"
"Nirmala UI (TrueType)"="Nirmala.ttf"
"Nirmala UI Bold (TrueType)"="NirmalaB.ttf"
"Nirmala UI Semilight (TrueType)"="NirmalaS.ttf"
"Palatino Linotype (TrueType)"="pala.ttf"
"Palatino Linotype Bold (TrueType)"="palab.ttf"
"Palatino Linotype Bold Italic (TrueType)"="palabi.ttf"
"Palatino Linotype Italic (TrueType)"="palai.ttf"
"Roman (All res)"="roman.fon"
"Script (All res)"="script.fon"
"Segoe MDL2 Assets (TrueType)"="segmdl2.ttf"
"Segoe Print (TrueType)"="segoepr.ttf"
"Segoe Print Bold (TrueType)"="segoeprb.ttf"
"Segoe Script (TrueType)"="segoesc.ttf"
"Segoe Script Bold (TrueType)"="segoescb.ttf"
"Segoe UI (TrueType)"="segoeui.ttf"
"Segoe UI Black (TrueType)"="seguibl.ttf"
"Segoe UI Black Italic (TrueType)"="seguibli.ttf"
"Segoe UI Bold (TrueType)"="segoeuib.ttf"
"Segoe UI Bold Italic (TrueType)"="segoeuiz.ttf"
"Segoe UI Emoji (TrueType)"="seguiemj.ttf"
"Segoe UI Historic (TrueType)"="seguihis.ttf"
"Segoe UI Italic (TrueType)"="segoeuii.ttf"
"Segoe UI Light (TrueType)"="segoeuil.ttf"
"Segoe UI Light Italic (TrueType)"="seguili.ttf"
"Segoe UI Semibold (TrueType)"="seguisb.ttf"
"Segoe UI Semibold Italic (TrueType)"="seguisbi.ttf"
"Segoe UI Semilight (TrueType)"="segoeuisl.ttf"
"Segoe UI Semilight Italic (TrueType)"="seguisli.ttf"
"Segoe UI Symbol (TrueType)"="seguisym.ttf"
"SimSun & NSimSun (TrueType)"="simsun.ttc"
"SimSun-ExtB (TrueType)"="simsunb.ttf"
"Sitka Small & Sitka Text & Sitka Subheading & Sitka Heading & Sitka Display & Sitka Banner (TrueType)"="Sitka.ttc"
"Sitka Small Bold & Sitka Text Bold & Sitka Subheading Bold & Sitka Heading Bold & Sitka Display Bold & Sitka Banner Bold (TrueType)"="SitkaB.ttc"
"Sitka Small Bold Italic & Sitka Text Bold Italic & Sitka Subheading Bold Italic & Sitka Heading Bold Italic & Sitka Display Bold Italic & Sitka Banner Bold Italic (TrueType)"="SitkaZ.ttc"
"Sitka Small Italic & Sitka Text Italic & Sitka Subheading Italic & Sitka Heading Italic & Sitka Display Italic & Sitka Banner Italic (TrueType)"="SitkaI.ttc"
"Sylfaen (TrueType)"="sylfaen.ttf"
"Symbol (TrueType)"="symbol.ttf"
"Tahoma (TrueType)"="tahoma.ttf"
"Tahoma Bold (TrueType)"="tahomabd.ttf"
"Times New Roman (TrueType)"="times.ttf"
"Times New Roman Bold (TrueType)"="timesbd.ttf"
"Times New Roman Bold Italic (TrueType)"="timesbi.ttf"
"Times New Roman Italic (TrueType)"="timesi.ttf"
"Trebuchet MS (TrueType)"="trebuc.ttf"
"Trebuchet MS Bold (TrueType)"="trebucbd.ttf"
"Trebuchet MS Bold Italic (TrueType)"="trebucbi.ttf"
"Trebuchet MS Italic (TrueType)"="trebucit.ttf"
"Verdana (TrueType)"="verdana.ttf"
"Verdana Bold (TrueType)"="verdanab.ttf"
"Verdana Bold Italic (TrueType)"="verdanaz.ttf"
"Verdana Italic (TrueType)"="verdanai.ttf"
"Webdings (TrueType)"="webdings.ttf"
"Wingdings (TrueType)"="wingding.ttf"
"Yu Gothic Bold & Yu Gothic UI Semibold & Yu Gothic UI Bold (TrueType)"="YuGothB.ttc"
"Yu Gothic Light & Yu Gothic UI Light (TrueType)"="YuGothL.ttc"
"Yu Gothic Medium & Yu Gothic UI Regular (TrueType)"="YuGothM.ttc"
"Yu Gothic Regular & Yu Gothic UI Semilight (TrueType)"="YuGothR.ttc"
"Courier 10,12,15"="COURE.FON"
"Courier 10,12,15 (120)"="COURF.FON"
"MS Serif 8,10,12,14,18,24"="SERIFE.FON"
"MS Serif 8,10,12,14,18,24 (120)"="SERIFF.FON"
"MS Sans Serif 8,10,12,14,18,24"="SSERIFE.FON"
"MS Sans Serif 8,10,12,14,18,24 (120)"="SSERIFF.FON"
"Small Fonts"="SMALLE.FON"
"Small Fonts (120)"="SMALLF.FON"
"Holo MDL2 Assets (TrueType)"="holomdl2.ttf"
"DFKai-SB (TrueType)"="kaiu.ttf"
"MingLiU & PMingLiU & MingLiU_HKSCS (TrueType)"="mingliu.ttc"
"Arial Unicode MS (TrueType)"="ARIALUNI.TTF"
"Century (TrueType)"="CENTURY.TTF"
"Meiryo & Meiryo Italic & Meiryo UI & Meiryo UI Italic (TrueType)"="MEIRYO.TTC"
"Wingdings 2 (TrueType)"="WINGDNG2.TTF"
"Wingdings 3 (TrueType)"="WINGDNG3.TTF"
"Book Antiqua (TrueType)"="BKANT.TTF"
"Century Gothic (TrueType)"="GOTHIC.TTF"
"Tempus Sans ITC (TrueType)"="TEMPSITC.TTF"
"Mistral (TrueType)"="MISTRAL.TTF"
"Lucida Handwriting Italic (TrueType)"="LHANDW.TTF"
"Kristen ITC (TrueType)"="ITCKRIST.TTF"
"Juice ITC (TrueType)"="JUICE___.TTF"
"Freestyle Script (TrueType)"="FREESCPT.TTF"
"Arial Narrow (TrueType)"="ARIALN.TTF"
"Garamond (TrueType)"="GARA.TTF"
"Monotype Corsiva (TrueType)"="MTCORSVA.TTF"
"Algerian (TrueType)"="ALGER.TTF"
"Baskerville Old Face (TrueType)"="BASKVILL.TTF"
"Bauhaus 93 (TrueType)"="BAUHS93.TTF"
"Bell MT (TrueType)"="BELL.TTF"
"Berlin Sans FB Bold (TrueType)"="BRLNSB.TTF"
"Bernard MT Condensed (TrueType)"="BERNHC.TTF"
"Bodoni MT Poster Compressed (TrueType)"="BOD_PSTC.TTF"
"Britannic Bold (TrueType)"="BRITANIC.TTF"
"Broadway (TrueType)"="BROADW.TTF"
"Brush Script MT Italic (TrueType)"="BRUSHSCI.TTF"
"Californian FB (TrueType)"="CALIFR.TTF"
"Centaur (TrueType)"="CENTAUR.TTF"
"Chiller (TrueType)"="CHILLER.TTF"
"Colonna MT (TrueType)"="COLONNA.TTF"
"Cooper Black (TrueType)"="COOPBL.TTF"
"Footlight MT Light (TrueType)"="FTLTLT.TTF"
"Harlow Solid Italic (TrueType)"="HARLOWSI.TTF"
"Harrington (TrueType)"="HARNGTON.TTF"
"High Tower Text (TrueType)"="HTOWERT.TTF"
"Jokerman (TrueType)"="JOKERMAN.TTF"
"Kunstler Script (TrueType)"="KUNSTLER.TTF"
"Lucida Bright (TrueType)"="LBRITE.TTF"
"Lucida Calligraphy Italic (TrueType)"="LCALLIG.TTF"
"Lucida Fax Regular (TrueType)"="LFAX.TTF"
"Magneto Bold (TrueType)"="MAGNETOB.TTF"
"Matura MT Script Capitals (TrueType)"="MATURASC.TTF"
"Modern No. 20 (TrueType)"="MOD20.TTF"
"Niagara Engraved (TrueType)"="NIAGENG.TTF"
"Niagara Solid (TrueType)"="NIAGSOL.TTF"
"Old English Text MT (TrueType)"="OLDENGL.TTF"
"Onyx (TrueType)"="ONYX.TTF"
"Parchment (TrueType)"="PARCHM.TTF"
"Playbill (TrueType)"="PLAYBILL.TTF"
"Poor Richard (TrueType)"="POORICH.TTF"
"Ravie (TrueType)"="RAVIE.TTF"
"Informal Roman (TrueType)"="INFROMAN.TTF"
"Showcard Gothic (TrueType)"="SHOWG.TTF"
"Snap ITC (TrueType)"="SNAP____.TTF"
"Stencil (TrueType)"="STENCIL.TTF"
"Viner Hand ITC (TrueType)"="VINERITC.TTF"
"Vivaldi Italic (TrueType)"="VIVALDII.TTF"
"Vladimir Script (TrueType)"="VLADIMIR.TTF"
"Wide Latin (TrueType)"="LATINWD.TTF"
"Bookman Old Style (TrueType)"="BOOKOS.TTF"
"Meiryo Bold & Meiryo Bold Italic & Meiryo UI Bold & Meiryo UI Bold Italic (TrueType)"="MEIRYOB.TTC"
"Book Antiqua Bold (TrueType)"="ANTQUAB.TTF"
"Book Antiqua Bold Italic (TrueType)"="ANTQUABI.TTF"
"Book Antiqua Italic (TrueType)"="ANTQUAI.TTF"
"Century Gothic Bold (TrueType)"="GOTHICB.TTF"
"Century Gothic Bold Italic (TrueType)"="GOTHICBI.TTF"
"Century Gothic Italic (TrueType)"="GOTHICI.TTF"
"Arial Narrow Bold (TrueType)"="ARIALNB.TTF"
"Arial Narrow Bold Italic (TrueType)"="ARIALNBI.TTF"
"Arial Narrow Italic (TrueType)"="ARIALNI.TTF"
"Garamond Bold (TrueType)"="GARABD.TTF"
"Garamond Italic (TrueType)"="GARAIT.TTF"
"Bell MT Bold (TrueType)"="BELLB.TTF"
"Bell MT Italic (TrueType)"="BELLI.TTF"
"Berlin Sans FB Demi Bold (TrueType)"="BRLNSDB.TTF"
"Berlin Sans FB (TrueType)"="BRLNSR.TTF"
"Californian FB Bold (TrueType)"="CALIFB.TTF"
"Californian FB Italic (TrueType)"="CALIFI.TTF"
"High Tower Text Italic (TrueType)"="HTOWERTI.TTF"
"Lucida Bright Demibold (TrueType)"="LBRITED.TTF"
"Lucida Bright Demibold Italic (TrueType)"="LBRITEDI.TTF"
"Lucida Bright Italic (TrueType)"="LBRITEI.TTF"
"Lucida Fax Demibold (TrueType)"="LFAXD.TTF"
"Lucida Fax Demibold Italic (TrueType)"="LFAXDI.TTF"
"Lucida Fax Italic (TrueType)"="LFAXI.TTF"
"Bookman Old Style Bold (TrueType)"="BOOKOSB.TTF"
"Bookman Old Style Bold Italic (TrueType)"="BOOKOSBI.TTF"
"Bookman Old Style Italic (TrueType)"="BOOKOSI.TTF"
"Agency FB Bold (TrueType)"="AGENCYB.TTF"
"Agency FB (TrueType)"="AGENCYR.TTF"
"Arial Rounded MT Bold (TrueType)"="ARLRDBD.TTF"
"Bodoni MT Bold (TrueType)"="BOD_B.TTF"
"Bodoni MT Bold Italic (TrueType)"="BOD_BI.TTF"
"Bodoni MT Black Italic (TrueType)"="BOD_BLAI.TTF"
"Bodoni MT Black (TrueType)"="BOD_BLAR.TTF"
"Bodoni MT Condensed Bold (TrueType)"="BOD_CB.TTF"
"Bodoni MT Condensed Bold Italic (TrueType)"="BOD_CBI.TTF"
"Bodoni MT Condensed Italic (TrueType)"="BOD_CI.TTF"
"Bodoni MT Condensed (TrueType)"="BOD_CR.TTF"
"Bodoni MT Italic (TrueType)"="BOD_I.TTF"
"Bodoni MT (TrueType)"="BOD_R.TTF"
"Bradley Hand ITC (TrueType)"="BRADHITC.TTF"
"Bookshelf Symbol 7 (TrueType)"="BSSYM7.TTF"
"Calisto MT (TrueType)"="CALIST.TTF"
"Calisto MT Bold (TrueType)"="CALISTB.TTF"
"Calisto MT Bold Italic (TrueType)"="CALISTBI.TTF"
"Calisto MT Italic (TrueType)"="CALISTI.TTF"
"Castellar (TrueType)"="CASTELAR.TTF"
"Century Schoolbook (TrueType)"="CENSCBK.TTF"
"Copperplate Gothic Bold (TrueType)"="COPRGTB.TTF"
"Copperplate Gothic Light (TrueType)"="COPRGTL.TTF"
"Curlz MT (TrueType)"="CURLZ___.TTF"
"Dubai Bold (TrueType)"="DUBAI-BOLD.TTF"
"Dubai Light (TrueType)"="DUBAI-LIGHT.TTF"
"Dubai Medium (TrueType)"="DUBAI-MEDIUM.TTF"
"Dubai Regular (TrueType)"="DUBAI-REGULAR.TTF"
"Elephant (TrueType)"="ELEPHNT.TTF"
"Elephant Italic (TrueType)"="ELEPHNTI.TTF"
"Engravers MT (TrueType)"="ENGR.TTF"
"Eras Bold ITC (TrueType)"="ERASBD.TTF"
"Eras Demi ITC (TrueType)"="ERASDEMI.TTF"
"Eras Light ITC (TrueType)"="ERASLGHT.TTF"
"Eras Medium ITC (TrueType)"="ERASMD.TTF"
"Felix Titling (TrueType)"="FELIXTI.TTF"
"Forte (TrueType)"="FORTE.TTF"
"Franklin Gothic Book (TrueType)"="FRABK.TTF"
"Franklin Gothic Book Italic (TrueType)"="FRABKIT.TTF"
"Franklin Gothic Demi (TrueType)"="FRADM.TTF"
"Franklin Gothic Demi Cond (TrueType)"="FRADMCN.TTF"
"Franklin Gothic Demi Italic (TrueType)"="FRADMIT.TTF"
"Franklin Gothic Heavy (TrueType)"="FRAHV.TTF"
"Franklin Gothic Heavy Italic (TrueType)"="FRAHVIT.TTF"
"Franklin Gothic Medium Cond (TrueType)"="FRAMDCN.TTF"
"French Script MT (TrueType)"="FRSCRIPT.TTF"
"Gigi (TrueType)"="GIGI.TTF"
"Gill Sans MT Bold Italic (TrueType)"="GILBI___.TTF"
"Gill Sans MT Bold (TrueType)"="GILB____.TTF"
"Gill Sans MT Condensed (TrueType)"="GILC____.TTF"
"Gill Sans MT Italic (TrueType)"="GILI____.TTF"
"Gill Sans Ultra Bold Condensed (TrueType)"="GILLUBCD.TTF"
"Gill Sans Ultra Bold (TrueType)"="GILSANUB.TTF"
"Gill Sans MT (TrueType)"="GIL_____.TTF"
"Gloucester MT Extra Condensed (TrueType)"="GLECB.TTF"
"Gill Sans MT Ext Condensed Bold (TrueType)"="GLSNECB.TTF"
"Goudy Old Style (TrueType)"="GOUDOS.TTF"
"Goudy Old Style Bold (TrueType)"="GOUDOSB.TTF"
"Goudy Old Style Italic (TrueType)"="GOUDOSI.TTF"
"Goudy Stout (TrueType)"="GOUDYSTO.TTF"
"Haettenschweiler (TrueType)"="HATTEN.TTF"
"Imprint MT Shadow (TrueType)"="IMPRISHA.TTF"
"Blackadder ITC (TrueType)"="ITCBLKAD.TTF"
"Edwardian Script ITC (TrueType)"="ITCEDSCR.TTF"
"Leelawadee (TrueType)"="LEELAWAD.TTF"
"Leelawadee Bold (TrueType)"="LEELAWDB.TTF"
"Lucida Sans Regular (TrueType)"="LSANS.TTF"
"Lucida Sans Demibold Roman (TrueType)"="LSANSD.TTF"
"Lucida Sans Demibold Italic (TrueType)"="LSANSDI.TTF"
"Lucida Sans Italic (TrueType)"="LSANSI.TTF"
"Lucida Sans Typewriter Regular (TrueType)"="LTYPE.TTF"
"Lucida Sans Typewriter Bold (TrueType)"="LTYPEB.TTF"
"Lucida Sans Typewriter Bold Oblique (TrueType)"="LTYPEBO.TTF"
"Lucida Sans Typewriter Oblique (TrueType)"="LTYPEO.TTF"
"Maiandra GD (TrueType)"="MAIAN.TTF"
"Microsoft Uighur Bold (TrueType)"="MSUIGHUB.TTF"
"Microsoft Uighur (TrueType)"="MSUIGHUR.TTF"
"OCR A Extended (TrueType)"="OCRAEXT.TTF"
"MS Outlook (TrueType)"="OUTLOOK.TTF"
"Palace Script MT (TrueType)"="PALSCRI.TTF"
"Papyrus (TrueType)"="PAPYRUS.TTF"
"Perpetua Bold Italic (TrueType)"="PERBI___.TTF"
"Perpetua Bold (TrueType)"="PERB____.TTF"
"Perpetua Italic (TrueType)"="PERI____.TTF"
"Perpetua Titling MT Bold (TrueType)"="PERTIBD.TTF"
"Perpetua Titling MT Light (TrueType)"="PERTILI.TTF"
"Perpetua (TrueType)"="PER_____.TTF"
"Pristina (TrueType)"="PRISTINA.TTF"
"Rage Italic (TrueType)"="RAGE.TTF"
"MS Reference Sans Serif (TrueType)"="REFSAN.TTF"
"MS Reference Specialty (TrueType)"="REFSPCL.TTF"
"Rockwell Condensed Bold (TrueType)"="ROCCB___.TTF"
"Rockwell Condensed (TrueType)"="ROCC____.TTF"
"Rockwell (TrueType)"="ROCK.TTF"
"Rockwell Bold (TrueType)"="ROCKB.TTF"
"Rockwell Bold Italic (TrueType)"="ROCKBI.TTF"
"Rockwell Extra Bold (TrueType)"="ROCKEB.TTF"
"Rockwell Italic (TrueType)"="ROCKI.TTF"
"Century Schoolbook Bold (TrueType)"="SCHLBKB.TTF"
"Century Schoolbook Bold Italic (TrueType)"="SCHLBKBI.TTF"
"Century Schoolbook Italic (TrueType)"="SCHLBKI.TTF"
"Script MT Bold (TrueType)"="SCRIPTBL.TTF"
"Tw Cen MT Bold Italic (TrueType)"="TCBI____.TTF"
"Tw Cen MT Bold (TrueType)"="TCB_____.TTF"
"Tw Cen MT Condensed Bold (TrueType)"="TCCB____.TTF"
"Tw Cen MT Condensed Extra Bold (TrueType)"="TCCEB.TTF"
"Tw Cen MT Condensed (TrueType)"="TCCM____.TTF"
"Tw Cen MT Italic (TrueType)"="TCMI____.TTF"
"Tw Cen MT (TrueType)"="TCM_____.TTF"
"MT Extra (TrueType)"="C:\\Program Files (x86)\\Common Files\\Microsoft Shared\\EQUATION\\MTEXTRA.TTF"
"HP Simplified Bold (TrueType)"="HPSimplified_Bd.ttf"
"HP Simplified Bold Italic (TrueType)"="HPSimplified_BdIt.ttf"
"HP Simplified Italic (TrueType)"="HPSimplified_It.ttf"
"HP Simplified Light (TrueType)"="HPSimplified_Lt.ttf"
"HP Simplified Light Italic (TrueType)"="HPSimplified_LtIt.ttf"
"HP Simplified (TrueType)"="HPSimplified_Rg.ttf"
"Source Code Pro Black (TrueType)"="SourceCodePro-Black.ttf"
"Source Code Pro Bold (TrueType)"="SourceCodePro-Bold.ttf"
"Source Code Pro ExtraLight (TrueType)"="SourceCodePro-ExtraLight.ttf"
"Source Code Pro Light (TrueType)"="SourceCodePro-Light.ttf"
"Source Code Pro (TrueType)"="SourceCodePro-Regular.ttf"
"Source Code Pro Semibold (TrueType)"="SourceCodePro-Semibold.ttf"

EOF
```
Add them to your wine prefix
```
env WINEPREFIX=~/osu-wine wine regedit font-1.reg
env WINEPREFIX=~/osu-wine wine regedit font-2.reg
```
Double check to make sure they are there.\
Then copy Windows fonts into wine prefix.
```
cp path-to-windows-fonts/* ~/.osu-wine/drive_c/windows/Fonts/*
```

**Font rendering (Optional)**
This step may not be needed at all. 
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks settings fontsmooth=rgb
```

### Sound
```
env WINEPREFIX=~/osu-wine WINEARCH=win64 winetricks sound=alsa
```
Switch to ALSA. Remember to install `pulseaudio-alsa` if you are using PulseAudio, or follow wiki guide [here](https://wiki.archlinux.org/index.php/PulseAudio#ALSA).

**Registry file for low latency:**
This registry edit was from Francesco149's guide.
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

**Tuning PulseAudio**
More information [here](https://wiki.archlinux.org/index.php/gaming#Tuning_PulseAudio).
copy PulseAudio config file to edit locally.
```
cp /etc/pulse/daemon.conf ~/.config/pulse/daemon.conf
vim ~/.config/pulse/daemon.conf
```
uncomment these lines:
```
high-priority = yes
nice-level = -11

realtime-scheduling = yes
realtime-priority = 5
```
uncomment and edit this line:
```
resample-method = speex-float-10
```

**Then, for matching hardware buffers follow [this](https://forums.linuxmint.com/viewtopic.php?f=42&t=44862):**\
**Log out of your DE and use TTY instead when following.**\
Remember local config for PulseAudio is at  `~/.config/pulse/`.

### Discord Rich Presence
Grab the exe file from [here](https://github.com/0e4ef622/wine-discord-ipc-bridge).
Run this exe file with the prefix and then osu! for working rpc.

### Launch Script
My launch script:\
Ignore lines with picom if you don't use it.\
```
#!/bin/sh

export vblank_mode=0
export WINEPREFIX=$HOME/.osu-wine/
export WINEARCH=win64

killall picom
wine $HOME/.osu-wine/winediscordipcbridge.exe &
sleep 1
wine $HOME/.osu-wine/osu!/osu!.exe

wait
wineserver -k
picom &
```
Make sure to set executable:
```
chmod +x launch-osu.sh
```

### Desktop File
Desktop file should look something like this.
```
[Desktop Entry]
Encoding=UTF-8
Name=osu!
Type=Application
Exec="/home/your-username/.osu-wine/launch-osu.sh"
Icon=/home/your-username/.osu-wine/icon.png
StartupWMClass=osu!.exe
Categories=Games
```
**Symlink for easy access**
Symlink globally:
```
ln -sf ~/path-to-desktop-file/osu\!/.desktop /usr/share/applications/osu\!/.desktop
```
Symlink locally:
```
ln -sf ~/path-to-desktop-file/osu\!/.desktop ~/.local/share/applications/osu\!/.desktop
```
