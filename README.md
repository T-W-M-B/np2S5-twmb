# np2S5 twmb

np2S5 twmb is a PC-98 emulator core for RetroArch on a jailbroken PS5. It plays PC-98 floppy and hard disk
images, with FM sound, and is based on Neko Project II kai.

> **Status:** early. The core loads and runs on PS5 RetroArch 1.22.2 and FM sound works. Save states,
> disk swapping, CD-ROM images, long sessions and other features have **not** been tested yet.
> See [Known limitations](#known-limitations).

The ready-to-use core and `.info` file are in the [`releases/`](releases/) folder. No BIOS, font, sound ROM
or game files are included. Use files dumped from hardware you own.

## Requirements

- A jailbroken PS5 running the PS5 port of RetroArch that loads native cores (tested with 1.22.2).
- Your own PC-98 disk images (see [Loading games](#loading-games)).
- Optional but recommended: a PC-98 font and BIOS files (see [BIOS and system files](#bios-and-system-files)).

## Installing the core

Copy the two files from [`releases/`](releases/) into your RetroArch install folder (the folder the PS5
shows as `/app0`):

| File | Destination |
| --- | --- |
| `np2s5_twmb_libretro.so` | `cores/` |
| `np2s5_twmb_libretro.info` | `info/` |

Then:

1. Delete `info/core_info.cache` so RetroArch re-reads the `.info` file.
2. Fully quit RetroArch (don't just close the content) and start it again.
3. The core should appear as **NEC - PC-98 (np2S5 twmb)**.

If you replace the core later, always fully restart RetroArch. A stale copy of the old file fails with
the same error as a broken one, so compare checksums on both sides after copying.

MD5 of the `.so` in this release: `d6c997d9389f004fa746fbb5596af8c1`

## BIOS and system files

The core reads its extra files from one folder:

```
system/np2s5_twmb/
```

Create it inside RetroArch's `system` folder. The core also uses this folder as its working
directory, so it must be writable.

| File | What it does |
| --- | --- |
| `font.bmp` | PC-98 font. If it is missing the core falls back to a built-in font. |
| `bios.rom` | PC-98 BIOS (0x18000 bytes). If it is missing the core uses its built-in BIOS emulation. |
| `itf.rom` | Initial-program-load ROM. Optional. |
| `sound.rom` (and per-board names such as `sound86.rom`, `sound118.rom`, `sound14.rom`, `sound26.rom`) | Sound board ROMs. Optional; the core has defaults. |
| `2608_bd.wav`, `2608_sd.wav`, `2608_top.wav`, `2608_hh.wav`, `2608_tom.wav`, `2608_rim.wav` | Rhythm (drum) samples for the YM2608 sound chip. Without them the drum sounds on the PC-98 86 board may be missing. |
| `scsi.rom`, `sasi.rom` | Drive controller ROMs, if a game needs them. |

File names are lowercase and are read from the source code of this build. Which files a particular
game needs depends on the game.

## Loading games

Start content normally from RetroArch (Load Content). The core needs the full file path and accepts
these extensions:

`d88 88d d98 98d fdi xdf hdm dup 2hd tfd nfd hd4 hd5 hd9 fdd h01 hdb ddb dd6 dcp dcu flp img ima bin fim thd nhd hdi vhd slh hdn cmd`

Floppy and hard disk images are supported. CD-ROM images are **not** supported in this build.

## Controls (defaults)

All of these can be changed in the RetroArch Quick Menu under Core Options.

| Action | Default |
| --- | --- |
| Open the NP2 menu (disk mounting, settings) | **L2** |
| Move the mouse cursor | **Left stick** |
| Shift the stick-click from left click to right click | **R1** |
| D-pad | Keyboard **arrow keys** |

Other D-pad modes: Mouse, Arrows 3-button, Keypad, Keypad 3-button, Manual Keyboard, Atari Joypad,
Keypad Fighting, or Off. The NP2 menu button can be moved to another button or turned off.

**DualSense touchpad:** *Use mouse or touchpanel to input mouse* is on by default. The PS5 RetroArch
frontend only passes the touchpad to a fixed list of cores, so touchpad control needs frontend
support for the core name `np2S5 twmb` (exact spelling). Until then, use the stick.

## Main core options

Options marked "restart" take effect after closing and reloading the content.

| Option | Default |
| --- | --- |
| PC Model (restart) | PC-9801VX |
| CPU Feature (restart) | Intel 80386 |
| CPU Base Clock (restart) | 2.4576 MHz |
| CPU Clock Multiplier (restart) | 4 |
| RAM Size (restart) | 3 MB |
| Sound Board (restart) | PC9801-86 |
| Sound Generator (restart) | fmgen |
| Keyboard (restart) | Ja |
| Swap Disks on Drive | FDD2 |
| Async CPU (experimental, restart) | **Off** (see below) |
| Video filter | Off |
| Real Palettes | Off |

Volume sliders are available for master, FM, SSG, ADPCM, PCM, rhythm, CD-DA, floppy seek and beep.

## Known limitations

- **Async CPU should stay off.** It is experimental in NP2kai, and this build has placeholder
  thread-locking functions (`pthread_rwlock_*` that do nothing) because the PS5 runtime doesn't provide
  them. Nothing in NP2kai itself calls them, but threaded modes have not been checked for race
  conditions.
- No CD-ROM support.
- Untested so far: audio beyond FM sound, save states, disk swapping, input beyond the basics, and sessions
  longer than a few minutes.
- With some shaders, a brief screen shake every 1-2 seconds has been reported. The cause is not known yet.

## Troubleshooting

| Problem | What to try |
| --- | --- |
| Core doesn't appear in the list | Check that the `.info` file is named exactly `np2s5_twmb_libretro.info` and is in `info/`. Delete `info/core_info.cache` and fully restart RetroArch. |
| Core refuses to load | Re-copy the `.so` and compare its MD5 with the one above. Make sure the old core file was replaced and RetroArch was fully restarted. |
| Black screen or the game doesn't boot | Try adding `bios.rom` and `font.bmp` to `system/np2s5_twmb/`, and check the PC Model option. |
| No drum sounds | Add the `2608_*.wav` rhythm samples to `system/np2s5_twmb/`. |
| Settings from another PC-98 core are missing | This core uses its own option names (`np2s5_twmb_*`), so it starts with fresh defaults. |

## Credits and license

Based on Neko Project II kai by AZO and contributors (MIT license, see `LICENSE`). Built with the PS5
Payload SDK, using the PS5 RetroArch project's native core linker script.
