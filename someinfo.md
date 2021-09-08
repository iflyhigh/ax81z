<!--
OSC:OPZ
DT1=[01110000]
MUL=[00001111]
FXR=[01110000] (FIXRANGE)
FXF=[00001111] (FIXFREQ)
 OW=[01110000] (OSC WAVE)
FIN=[00001111] (FINE TUNE PITCH)
 TL=[01111111]
 KS=[11000000]
FIX=[00100000] (FIXED)
 AR=[00011111]
AME=[10000000]
D1R=[00011111] (DR)
DT2=[11000000]
EGS=[11000000] (EGSHIFT)
REV=[00000111] (REV)
D2R=[00011111] (SR)
D1L=[11110000] (SL)
 RR=[00001111]
+[DT2,REV,EGSFT,FINE,FIXRG,FIX,OSW]

OSC:OPM
DT1=[01110000]                     1
MUL=[00001111]                     1
 TL=[01111111]                     1
 KS=[11000000]                     1
 AR=[00011111]                     1
AME=[10000000]                     1
D1R=[00011111] (DR)                1
DT2=[11000000]                     1
D2R=[00011111] (SR)                1
D1L=[11110000] (SL)                1
 RR=[00001111]                     1
+[DT2]

OSC:OPN
 DT=[01110000] (DT1)            1
MUL=[00001111]                  1
 TL=[01111111]                  1
 KS=[11000000]                  1
 AR=[00011111]                  1
 DR=[00011111] (D1R)            1
 SR=[00011111] (D2R)            1
 SL=[11110000] (D1L)            1
 RR=[00001111]                  1
SSG=[00001111]
-[DT2,AME] +[SSG]

OSC:OPNA
 DT=[01110000] (DT1)
MUL=[00001111]
 KS=[11000000]
 AR=[00011111]
AME=[10000000]
 DR=[00011111] (D1R)
 SR=[00011111] (D2R)
 SL=[11110000] (D1L)
 RR=[00001111]
SSG=[00001111]
-[DT2] +[SSG]

OSC:OPN2
 DT=[01110000] (DT1)        1            
MUL=[00001111]              1
 TL=[01111111]              1
 KS=[11000000]              1
 AR=[00011111]              1
AME=[10000000]              1
 DR=[00011111] (D1R)        1
 SR=[00011111] (D2R)        1
 SL=[11110000] (D1L)        1
 RR=[00001111]              1
SSG=[00001111]
-[DT2] +[SSG]

OSC:OPL (1984)
 AM=[10000000] (Tremolo/Amp Modulation)
VIB=[01000000] (Vibrato)
EGT=[00100000] (Sustain/EG Type)
KSR=[00010000] (EG Scaling Enable/Key Scale Rate)
MUL=[00001111]
KSL=[11000000] (KS) (Key Scale Level)
 TL=[00111111]                                      1
 AR=[11110000]                                      1
 DR=[00001111] (D1R)                                1
 SL=[11110000] (D1L)                                1
 RR=[00001111]                                      1
-[D2R,DT1,DT2,AME], ?[TL,AR,DR], +[AM,VIB,EGT,KSR]

OSC:OPL2 (1985)
 AM=[10000000] (Tremolo/Amp Modulation)
VIB=[01000000] (Vibrato)
EGT=[00100000] (Sustain/EG Type)
KSR=[00010000] (EG Scaling Enable/Key Scale Rate)
MUL=[00001111]
KSL=[11000000] (KS) (Key Scale Level)
 TL=[00111111]
 AR=[11110000]
 DR=[00001111] (D1R)
 SL=[11110000] (D1L)
 RR=[00001111]
 WS=[00000011]
-[D2R,DT1,DT2,AME], ?[TL,AR,DR], +[AM,VIB,EGT,KSR]

OSC:OPLL (1986)
 AM=[10000000]
VIB=[01000000]
EGT=[00100000]
KSR=[00010000]
MUL=[00001111]
KSL=[11000000]
 TL=[00111111]
 WS=[00011000]
 FB=[00000111]
 AR=[11110000]
 DR=[00001111]
 SL=[11110000]
 RR=[00001111]
-[D2R,DT1,DT2,AME], ?[TL,AR,DR], +[AM,VIB,EGT,KSR]

OSC:OPL3 (1988)
 AM=[10000000] (Tremolo/Amp Modulation)
VIB=[01000000] (Vibrato)
EGT=[00100000] (Sustain/EG Type)
KSR=[00010000] (EG Scaling Enable/Key Scale Rate)
MUL=[00001111]
KSL=[11000000] (KS) (Key Scale Level)
 TL=[00111111]
 AR=[11110000]
 DR=[00001111] (D1R)
 SL=[11110000] (D1L)
 RR=[00001111]
 WS=[00000111]
-[D2R,DT1,DT2,AME], ?[TL,AR,DR], +[AM,VIB,EGT,KSR]
-->

<!-- https://sites.google.com/site/undocumentedsoundchips/yamaha -->
<!-- http://sr4.sakura.ne.jp/fmsound/index.html -->
<!-- http://nuxmicromedia.com/4ophq/cgi-bin/dx4op.cgi?mode=compare -->
<!-- http://minirevver.weebly.com/vgm-1985.html -->
<!-- https://www.reddit.com/r/chiptunes/comments/42l8c5/discussion_which_yamaha_fm_chip_do_you_think_is/ -->
<!-- https://www.vogons.org/viewtopic.php?f=5&t=19264&sid=ce4e97fc6c0481566a3ef0e915af81ab -->

<!--
sega genesis
----------
shinobi iii
revenge of shinobi
castlevania bloodlines
gunstar heroes
sonic 2, sonic and knuckles
gauntlet iv
contra (hard corp?)
langrisser 2
Midnight resistance
shinobi
ristar
alien soldier
red zone------bad
gods--------bad


----------------
SOR2: Dreamer, Slow Moon, Intro (SOR Supermix), Good Ending, Beta-Walking Bottom, 
ThunderForce4: Fighting Back, Evil Destroyer, Space Walk (is ok), The Sky Line, Sand Hell, Metal Squad, SHooting Stars, Silvery Light of the Moon, Love Dream, Stand up against myself,  Remember of Knight of Legend, Omake 3, Omake 6, Omake 8, Omake 10
ShiningForce2: Lively Town, Wandering Warriors, To Arms!, dying wishes, Shrine, Water Goddess Mitula, Ending
MegaTurrican: Stage 1-1, Stage 2-1, Stage 5-1, Ending Theme
Bloodlines: Reincarnated Soul, Part 2 (Stage 1), Calling From Heaven (Stage 6), Requiem for the Nameless Victims (Staff Roll)

I love the YM2608 chip and the YM2203 has some gems as well although it's not quite as capable. Top 40s from a list I've made based mainly on sound design:
YM2203/OPN (ARC & PC-88/98, some arcade games use two of the same chip (marked with "2x")):
Grounseed (OPN), Space Harrier (ARC), Thunder Dragon 2/Big Bang: PS (ARC), Xenon: MnS (OPN), Ys 1-3 (OPN), Princess Maker 2 (OPN), Dragon Slayer: TLoH 1-2 (OPN), Star Trader (OPN), Power Instinct 1-2 & Gogetsuji Legends, Puyo Puyo (PC-98, OPN), 

Digan no Maseki (PC-88), Marionette Mind, Rookies (PC-98), Mahou Shoujo Fancy Coco (PC-98), Night Slave (PC-98), Air Buster/Aero Blasters (ARC), Rapid Hero/Arcadia, Death Knights of Krynn (PC-98), Queen's Library (PC-98), Dracula Hakushaku (PC-98), 

Hotdog Storm, Herzog, The Scheme (OPN) & The Scheme (Music Mode), Champions of Krynn (PC-98), Death Bringer (PC-98), Princess Maker, Star Cruiser II: TOP (PC-98), Power Slave (PC-98), Emerald Dragon (OPN), Eye of the Beholder (PC-98), 

X-Girl (PC-98), Rude Breaker, Raiga: Strato Fighter (ARC, 2x), Rance 4.1/4.2, Doukyusei 2 (PC-98), Angelus (OPN), Miho (PC-98), Sorcerian?, Eiyuu Densetsu/Legend of Heroes III: WW (OPN) & Legend of Heroes IV (OPN), Dragons of Flame (PC-98), 


YM2608/OPNA (PC-88 & PC-98, ARC; includes support for the YM2149/SSG chip): 
Grounseed (OPNA), King Breeder (PC-98), Flame Zapper Kotsujin (PC-98), Kono Yo no Hate de Koi wo Utau Shoujo YU-NO (PC-98), Miwaku no Chousho (PC-98), Rusty (PC-88/98, w/ YM2203?), Kara no Naka no Kotori (PC-98), The Scheme (OPNA, some new tracks), Etrian Odyssey (NDS), Hole Chaser (PC-88), 

Rhyme Star (PC-98), Brandish 3 (PC-98), Rouge: Manatsu no Kuchibeni (PC-88), Escape (PC-98), Kirishima Shinryoushitsu no Gogo (PC-98), Black Bird (PC-98), Zai Metajo/Zwei Metajo (PC-98), Snatcher (PC-88, OPNA), Love Escalator (PC-98), Desire (PC-98), 

Only You (PC-98), Harlem Blade (PC-98), Otome Senki (PC-98, OPNA), Popful Mail (PC-98), Revival Xanadu (PC-98), Starfire (PC-98), Rance 4.1: OKwS!!/Rance 4.2: AG (PC-98, OPNA), Misty Blue (PC-88, OPNA), Tsunyan Jaayao, Star Cruiser (PC-88), 

Brandish VT (PC-98), Dragon Slayer: TLoH II (PC-98, OPNA), Sorcerian (PC-88, OPNA), Legend of Heroes IV (PC-98, OPNA), Eiyuu Densetsu III: SM/Legend of Heroes III: WW, Metajo: Furitsu Metatoporoji Daigaku Fuzoku Joshi-kou (PC-98)?, Metajo (PC-98), Touhou Kaikidan: Mystic Square?, Yuugiri: Ningyoushi no Isan (PC-98), Steam-Heart's (PC-98, OPNA)


## Table of contents

* DX7 pre



-->



## DX7 predecessors: GS1 (1981), CE20 (1982)
- O.G. Ensemble FM synths with preprogrammed sound banks
- YM20110 (aka OP) (used in CE2) was 2-op

## DX7: YM2128(0) (OPS, FM Operator Type-S) and YM2129(0) (EGS)
- mid 1983, the _original FM synthesizer_, direct predecessor to YM2151/YM2612 etc.
- 16 voices (6-op) (32 in DX1/DX5)
- Used in DX7, DX9 (firmware reduces it to 4-op, 8 algo), TX7, DX1/DX5 (Dual)
- DX9 defines algorithms 1-8 as 1,14,8,7,5,22,31,32 (with removed OP 1 and 2). These algorithms are shared in OPN/OPM/OPZ series.
- Requires two chips

## YM2604 (OPS2) and YM3609 (EGM)
- 1986, Used in DX7 mark II, TX802
- 16 voices (6-op)
- Requires two chips

## YM2164 (OPP, FM Operator Type P)
- 1985, Used in DX21, DX27, DX100, FB-01, SFG-05 + Korg DS-8, 707
- 8 voices (4-op)
- The supposedly improved successor to OPM. It is VERY similar. Same pinout and is backwards compatible. In fact, any differences may not affect sound.
- Most differences are probably firmware-bound. For example DX21 has a "Pitch EG" which DX27/100/FB-01 do not. FB-01 has weird stuff like "AR Velocity Sensitivity". This is all probably specific to the firmware and not the chip. Velocity Sensitivity for example is fully controlled in firmware when processing MIDI.

## YM2414 (OPZ, FM Operator Type-Z)
- 1987, used in synths TX81Z, DX11, V50, YS100, YS200
- 8 channels (4-op), 8 waveforms, two LFOs
- combines the 8 algorithms of YM2151/OPM with 8 waveforms, allowing for sophisticated sounds. Interestingly, borrows the waveform concept from OPL series but uses custom list of waveforms.

## YMF292 (SCSP)
- 1994, used in Sega Saturn, Sega Model 2/3
- hybrid FM/PCM, uses 32 channels (4-op, but configurable). Mostly PCM was used.

## YMF271 (OPX)
- 1994?, used in Seibu SPI arcade board
- **FM**: 9 channels? - 2 operators (4 algorithms), 3 operators (8 algorithms), 4 operators (16 algorithms)
- **PCM**: 3 channels?

-------

# OPM

## YM2151 (OPM, FM Operator Type-M)
- **Year of release**: 1983
- **FM**: 8 channels (4-op)
- **Used in**: Yamaha CX5M SFG-01 (Yamaha PC, 1983), Arcade, Sharp X1 Turbo (1984), Sharp X68000 (1987)
- **Related to**: Yamaha YM2164 (aka OPP/FM Operator Type P, derivative used in DX21/27)

### Notes
[Datasheet](http://map.grauw.nl/resources/sound/yamaha_ym2151_datasheet.pdf). 4 operators per channel, using same algorithms in DX21.
The chip is possibly stereo. Channel 3 mode is absent. 

### Example music
1. [BGM - Enduro Racer (1985)](http://vgmrips.net/packs/pack/enduro-racer#enduro-racer-02-bgm) (Arcade, YM2151, SegaPCM)
2. [Passing Breeze - Out Run (1986)](http://vgmrips.net/packs/pack/out-run-arcade#02-passing-breeze) (Arcade, YM2151, SegaPCM)
3. [The Heat Waves - Super Monaco GP (1989)](http://vgmrips.net/packs/pack/super-monaco-gp#super-monaco-gp-07-the-heat-waves) (Arcade, YM2151, SegaPCM)
4. [Ending - "Last Drive" - Knight Arms: The Hyblid Framer](https://vgmrips.net/packs/pack/knight-arms-the-hyblid-framer-sharp-x68000#16-ending-last-drive) (X68000, YM2151, OKIM6258)
5. [Time Attack - GP Rider (1990)](http://vgmrips.net/packs/pack/gp-rider-sega-x#04-time-attack-automatic) (Arcade, YM2151, SegaPCM)
6. [Red-Hot Desert - R-Type Leo (1992)](http://vgmrips.net/packs/pack/r-type-leo-irem-m92#04-red-hot-desert-area-2) (Arcade, YM2151, GA20)
7. [Photonic - Room Service](https://archive.org/details/Osc63VOPM/2photonic-RoomService.mp3#) (VOPM VST)
8. [pedalsteeldrummer - Strawberries and Cream](https://archive.org/details/Osc63VOPM/8pedalsteeldrummer-StrawberriesAndCream.mp3#)  (VOPM VST)
9. [Keishi Yonao - Eusion](https://www.youtube.com/watch?v=wK60QFPhHOA)  (iYM2151 Demo song, composer of Yu-No)

-------
# OPN

## YM2203 (OPN, FM Operator Type-N)
- **Year of release**: 1984
- **FM**: 3 channels (4-op)
- **SSG**: 3 channels (YM2149 PSG(?), register-compatible with AY-3-8910)
- **Used in**: Arcade, Certain models of NEC PC-6001 (1984)/PC-6601 (1984)/PC-8001 (1985)/PC-8801 (1985)/PC-9801 (1986) 
- **Related to**: YM2608/OPNA (enhanced version of OPN), YM2612/OPN2 (also based on OPN, but no SSG)

### Notes
[Datasheet](http://pdf.datasheet.live/99e92884/datasheet.directory/YM2203.pdf).
4 operators per channel, using same algorithms in DX21 and OPM.
The chip is possibly mono. Channel 3 has two special modes:
1. Sound effect mode: Allows for individual freq control of each operator, and can mute operators for additional polyphony.
2. CSM (Composite Sine Mode): for speech synthesis (?)

### Example music
1. [Main Theme - Space Harrier (1985)](http://vgmrips.net/packs/pack/space-harrier-hang-on#02-theme) (Arcade, YM2203 + SegaPCM)
2. [Opening - Silpheed (1986)](http://vgmrips.net/packs/pack/silpheed-nec-pc-8801#01-the-legend-of-silpheed-opening) (PC-8801, YM2203)
2. [First Step Towards Wars - Ys I: Ancient Ys Vanished (1987)](http://vgmrips.net/packs/pack/ys-ancient-ys-vanished-omen-nec-pc-8801#05-first-step-towards-wars) (PC-8801, YM2203)
3. [Opening - Dragon Slayer: The Legend of Heroes (1990)](http://vgmrips.net/packs/pack/dragon-slayer-the-legend-of-heroes-nec-pc-9801) (PC-8801, YM2203)
4. [Title Theme - Rusty (1993)](http://vgmrips.net/packs/pack/rusty-nec-pc-9801-opn#03-rusty-title) (PC-9801, YM2203 using special Ch3 mode)

## YM2608 (OPNA, FM Operator Type N-A)
- **Year of release**: 1985
- **FM**: 6 channels (4-op)
- **SSG**: 3 channels (YM2149 PSG(?), register-compatible with AY-3-8910)
- **ADPCM**: 1 channel (8-bit ADPCM format at a sampling rate between 2–16 kHz)
- **RHY**: 6 channel (enabling playback of six percussion ADPCM samples/"rhythm tones" from a built-in ROM)
- **Used in**: Certain models of PC-8801 (1985)/PC-9801 (1986) 
- **Related to**: YMF288/OPN3 (stripped down version of OPNA), YM2203/OPN (predecessor), YM2612/OPN2 (very similar, no SSG etc.)

### Notes
[Datasheet](http://nemesis.hacking-cult.org/MegaDrive/Documentation/YM2608J%20Translated.PDF).
4 operators per channel, using same algorithms in DX21 and OPM.
The chip is possibly stereo.

Channel 3 has two special modes:
1. Sound effect mode: Allows for individual freq control of each operator, and can mute operators for additional polyphony.
2. CSM (Composite Sine Mode): for speech synthesis (?)

### Example music
1. [Kono yo no hate de koi o utau Shōjo YU-NO (1996)](http://vgmrips.net/packs/pack/kono-yo-no-hate-de-koi-wo-utau-shoujo-yu-no-nec-pc-9801#01-prologue) (PC-9801, YM2608)
2. [Only You - Seikimatsu no Juliet to tachi (1995)](https://www.youtube.com/watch?v=m1cRWwajCJM) (PC-9801, YM2608)
3. [Level 7 - Revival Xanadu II: Remix (1995)](https://www.youtube.com/watch?v=DJUI8i2Aavs) (PC-9801, YM2608)
4. [Shout Down - The Scheme (1988)](http://vgmrips.net/packs/pack/the-scheme-nec-pc-8801-opna#07-shout-down) (PC-8801, YM2608)

## YM2612 (OPN2, FM Operator type N-2)
- **Year of release**: 1988
- **FM**: 6 channels (4-op)
- **Used in**: Arcade, Sega Mega Drive/Genesis (1988), Fujitsu FM Towns (1989)
- **Related to**: YM2608 (enhanced version), YM2203/OPN (predecessor)

### Notes
[SMSPower documentation](http://www.smspower.org/maxim/Documents/YM2612).
Stripped down/low-cost version of YM2608.
4 operators per channel, using same algorithms in DX21 and OPM.
Paired with a 4-channel SN76489 on the Mega Drive/Genesis. 
One FM channel can be converted to 8-bit ADPCM channel. The chip is possibly stereo. 

Channel 3 has two special modes:
1. Sound effect mode: Allows for individual freq control of each operator, and can mute operators for additional polyphony.
2. CSM (Composite Sine Mode): for speech synthesis (?)

### Example music
1. [BGM - Vapor Trail (1991)](http://project2612.org/details.php?id=215#04-vapor-trail) (Mega Drive, YM2612, No PSG usage)
2. [Because You're the Number One - Thunder Force IV (1992)](http://project2612.org/details.php?id=44#36-because-you-re-the-number-one-name-entry-ace-ranking) (Mega Drive, YM2612 + PSG)
3. [Dreamer - Streets of Rage 2 (1992)](http://project2612.org/details.php?id=56#07-dreamer) (Mega Drive, YM2612 + PSG)
4. [Sortie - Gauntlet IV (1993)](http://project2612.org/details.php?id=284#09-sortie) (Mega Drive, YM2612 + PSG)
5. [Reincarnated Soul, Part 2 - Castlevania Bloodlines (1994)](http://project2612.org/details.php?id=11#07-reincarnated-soul-part-2-stage-1) (Mega Drive, YM2612, No PSG usage)

<!--
## YM2610 (OPNB, FM Operator Type N-B)
- **Year of release**: 1990
- **FM**: 4 channels (4-op)
- **SSG**: 3/4 channels (YM2149 PSG(?), register-compatible with AY-3-8910)
- **ADPCM**: 7 PCM channels.
- **Used in**: Neo Geo systems (1990)
- **Related to**: YM2610B (used in Taito Arcade games)

### Notes
[Datasheet](http://www.dtech.lv/files_ym/ym2610.pdf). This chip uses PCM audio heavily. 

### Example music
1. TBD
-->

-------
# OPL

## YM3526 (OPL, FM Operator Type-L)
- **Year of release**: 1984
- **FM**: 9 channels (2-op, 1 waveform)
- **Used in**: C64 Sound Expander and Arcade games (Bubble Bobble)
- **Related to**: Y8950 (additional ADPCM channels, used in MSX expansion cart)

### Notes
<s>Datasheet</s>.

### Example music
1. [Theme of Terracresta - Terra Cresta (1985)](http://vgmrips.net/packs/pack/terra-cresta#terra-cresta-02-theme-of-terracresta) (Arcade, YM3526)
2. [Wonder Flight - Wonder Planet (1987)](http://vgmrips.net/packs/pack/wonder-planet-karnov#02-wonder-flight) (Arcade, YM3526)

## YM3812 (OPL2, FM Operator Type-L-2)
- **Year of release**: 1985
- **FM**: 9 channels (2-op, 4 waveforms)
- **Used in**: Arcade, DOS sound cards (Adlib, Sound Blaster etc.)
- **Related to**: N/A

### Notes
<s>Datasheet</s>. OPL-series of chips are 2-op and use different algorithms.

### Example music
1. [Staff Roll - Street Smart (1989)](http://vgmrips.net/packs/pack/street-smart-snk-68000#13-staff-roll) (Arcade, YM3812)
2.  [Title - Harald Hårdtand i 'Kampen om de rene tænder' (1992)](http://vgmrips.net/packs/pack/harald-hardtand-i-kampen-om-de-rene-taender-ibm-pc-at#01-title-music) (DOS, YM3812)
3.  [Title - Fury of the Furries (1993)](http://vgmrips.net/packs/pack/fury-of-the-furries#02-title-screen) (DOS, YM3812)
4. [Field 1 - Knights of Xentar (1994)](http://vgmrips.net/packs/pack/knights-of-xentar-ibm-pc-at#14-field-1) (DOS, YM3812)
5. [Battle - Princess Maker 2 (1996)](http://vgmrips.net/packs/pack/princess-maker-2-pc#17-battle) (DOS, YM3812)
6. [Title Tune - Lollypop (1994)](http://chordian.net/files/music/adlib/lollypop/Lollypop_Title_Tune.mp3) (DOS, YM3812, Edlib)
7. [Vibrants - Fis3 (Edlib)](https://www.youtube.com/watch?v=AUP12tOJHmw)
8. [DRAX - Street Wise (Edlib)](http://chordian.net/files/music/adlib/drax/Street_Wise.mp3)
9. [DRAX - Flash (Edlib)](http://chordian.net/files/music/adlib/drax/Flash_2.mp3)
10. [DRAX - Human Nature 1 (Edlib)](http://chordian.net/files/music/adlib/drax/Human_Nature_1.mp3)
11. [DRAX - Beyond Minds (Edlib)](http://chordian.net/files/music/adlib/drax/work/Beyond_Minds.mp3)
12. [METAL - Soul Shock (Edlib)](http://chordian.net/files/music/adlib/metal/Soul_Shock.mp3)
13. [METAL - Plastic Session (Edlib)](http://chordian.net/files/music/adlib/metal/Plastic_Session.mp3)
14. [METAL - Introism (Edlib)](http://chordian.net/files/music/adlib/metal/Introism.mp3)
15. [METAL - Inside the Organ (Edlib)](http://chordian.net/files/music/adlib/metal/Inside_The_Organ.mp3)
16. [METAL&DRAX - Breaking Wind (Edlib)](http://chordian.net/files/music/adlib/metal/Breaking_Wind.mp3)
17. [JO - Drums Are Hard To Do (Edlib)](http://chordian.net/files/music/adlib/jo/Drums_Are_Hard_To_Do.mp3)

## YM2413 (OPLL, FM Operator Type-L-L)
- **Year of release**: 1986
- **FM**: 9 channels or 6 channels/5 drums (2-op, 2 waveforms)
- **Used in**: Arcade, DOS sound cards (Adlib, Sound Blaster etc.)
- **Related to**: JP Master System, MSX/MSX2, VRC7 (6 channel variant used in _one_ NES game: Lagrange Point) (1986-1988)

### Notes
<s>Datasheet</s>. Only one channel can be fully programmed. Other must be chose from 15 hard-coded instruments. There are chip variants with different instrument sets, such as **YMF281** and **YM2423**. In general, these are inferior, stripped-down versions of OPL2. TODO: Is there anything programmable? Vibrato and volume? stuff like that.

### Example music
1. [Opening Theme - GD: Greatest Driver (1988)](http://vgmrips.net/packs/pack/gd-greatest-driver-msx2-opll#01-opening-theme) (MSX2, YM2413)
2. [Ending - Fire Hawk (1989)](http://vgmrips.net/packs/pack/fire-hawk-thexder-the-second-contact-msx2-opll#24-ending) (MSX2, YM2413 + AY-3-8910)
3. [Out of Rap - F-1 Spirit 3D Special (1990)](http://vgmrips.net/packs/pack/f-1-spirit-3d-special-msx2-msx-music#05-out-of-rap) (MSX2+, MSX-Music/YM2413)
4. [Theme of Isis - Lagrange Point (1991)](http://vgmrips.net/packs/pack/lagrange-point-family-computer#01-theme-of-isis) (Famicom, VRC7/NES APU)



## YMF262 (OPL3, FM Operator Type-L-3)
- **Year of release**: 1988
- **FM**: 18 channels or 15 channels/5 drums (2-op, 8 waveforms)
- **Used in**: Arcade, NEC PC-9801, DOS sound cards (Sound Blaster 16 etc.)
- **Related to**: Yamaha YMF7xx series

### Notes
[Documentation](http://www.fit.vutbr.cz/~arnost/opl/opl3.html). Has additional capabilities over OPL2, such as 4 more waveforms, double the channels, and ability to use 4-op instruments. Up to six 4-op instruments can be created, and each take up 2 channels. So that gives you 6 4-op + 6 2-op = 12 at its most extreme. Also, there's a separate mode where you can add drums (similar to OPL2). 6 4-op + 3 2-op + 5 1-op = 14 channels. Many musicians program their own drums in trackers using the full FM mode. TODO: Find out how the drum mode sounds like.

### Example music (TODO: get an a2m player/AdPlug? for AdlibTracker and find good shit)
1. [Painful Sigh - Miwaku no Chousho (1995)](http://vgmrips.net/packs/pack/miwaku-no-chousho-pc-9801#01-painful-sigh) (PC-9801, YMF262)
2. [Sky of the City - Doukyusei 2 (1995)](http://vgmrips.net/packs/pack/doukyusei-2-ibm-pc#03-sky-of-the-city) (DOS, YMF262)
3. [Madbrain - Oskari goes to Soundblasterland](https://soundcloud.com/madbr/oskari-goes-to)

# Others

## YMU757 (MA-1)
- **Year of release**: 1999
- **FM**: 4 channels (2-op, 2 waveforms)
- **Used in**: Tons of mobile devices (cell phone, PDA)

[Datasheet](http://pdf.datasheetcatalog.com/datasheet_pdf/yamaha/YMU757.pdf). Seems quite limited. 2-op only, but has OPLL's half-sine waveform. Contains a built-in sequencer.

```
MUL=7, TL=63, AR=15, DR=15, SL=15, RR=15, VIB=1, SUS=1, EGT=1, WAV=1, FB=7
```

## YMU759 (MA-2)
- **Year of release**: 2000
- **FM**: 16 channels (2-op, 8 waveforms) or 8 channels (4-op, 8 waveforms)
- **ADPCM**: 1 channel (4-bit, 4 kHz/8 kHz)
- **Used in**: Tons of mobile devices (cell phone, PDA)

[Datasheet](#). Boasts much improved FM over MA-1. Appears to have full OPL3 feature set. Contains one very low quality ADPCM channel. Also contains a sequencer. For drum sounds, a single key note can be specified.

```
MUL=15, TL=63, AR=15, DR=15, SL=15, RR=15, VIB=1, SUS=1, EGT=1, WAV=7, FB=7
+ LFO=3, KSR=1, AM=1, ALG=6, KSL=3, DVB=3, DAM=3
```

## YMU762 (MA-3)
- **Year of release**: 2001
- **FM**: 32 channels (2-op, 29 waveforms) or 16 channels (4-op, 29 waveforms)
- **PCM/ADPCM**: 8 channels (8-bit PCM, 4-bit ADPCM, 48kHz)
- **Used in**: Tons of mobile devices (cell phone, PDA)


[Datasheet](#). Improved over MA-2. Much improved sample playback. More channels. More operator waveforms. Two extra 4-op algorithms. plus some tweaks to old ones.

```
MUL=15, TL=63, AR=15, DR=15, SL=15, RR=15, VIB=1, SUS=1, EGT=1, WAV=7, FB=7
+ LFO=3, KSR=1, AM=1, ALG=6, KSL=3, DVB=3, DAM=3
+ XOF=1, EAM=1, EVB=1, PANPOT=31, PE=1
```
A bunch of new parameters with no clue what they do, but VIB is gone?

## YMU765 (MA-5)
- **Year of release**: 2003
- **FM**: 32 channels (2-op, 29 waveforms) or 16 channels (4-op, 29 waveforms)
- **PCM/ADPCM**: 32 channels (8-bit PCM, 4-bit ADPCM, 48kHz)
- **Used in**: Tons of mobile devices (cell phone, PDA)

This uses the same FM synthesis engine of MA-3, but adds a filter called Analog Lite (AL) and a speech synthesis (HV/Humanoid Voice) in Japanese or Korean. Also it bumps up the PCM channel count to 32.

## YMU786 / YMU790 / YMU791 (MA-7/ MA-7D / MA-7I)
- **Year of release**: 2005
- **FM**: ?
- **PCM/ADPCM**: ?
- **Used in**: Tons of mobile devices (cell phone, PDA)

Based on the MA-5's FM synthesis engine. Supposedly has 128 polyphony combined FM and PCM. Has 3D positional sound (AudioEngine), as well as DSP effects (reverb, delay, overdrive etc). 16KB ram instead of 8KB. Can't find much info on it. 

## YMF825 (SD-1)
- **Year of release**: 2011
- **FM**: 16 channels (4-op, 29 waveforms)
- **Used in**: Home appliances (Chinese market)

This appears to be a version of MA-3. It has no PCM or Analog Lite capabilities. Using 2-op instruments does not give extra channels, thus it's probably best to use 4-op instruments.
