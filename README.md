# TOSEC Toolkit

A set of desktop tools for looking after a large [TOSEC](https://www.tosecdev.org/) ROM collection:
sort it, zip it, scrape artwork for it, and convert PlayStation discs, all from simple
point-and-click windows. Made with Python and Tkinter, no extra libraries needed.

*Created by normanlove.*

> **This project contains no ROMs, BIOS files or games, and does not link to any.**
> It is for organising collections that you already own.

## The tools

| Tool | What it does |
|---|---|
| **TOSEC Sorter** | Sorts a TOSEC set into *English ROMs* / *Filtered Out* folders, one folder per system. Filters by language, hacks, bad dumps, pre-release builds, demos and duplicates. Names system folders for Batocera, MiSTer, RetroPie, ES-DE or Recalbox. Moves files only (never deletes) and can undo its last run. |
| **ROM Archiver** | Zips every file on its own, or unzips them, system by system. Marks which systems' emulators accept zipped games and which need loose files. Verifies each result before removing the original. |
| **ROM Scraper** | Downloads descriptions, dates, developers, genres, ratings, box art, screenshots, title screens, logos and video from several services. Writes the format your frontend expects (detected from your folder layout). Pause/resume, download speed limit and a time-of-day schedule. |
| **ROM Converter** | PBP → BIN/CUE (built-in reader), BIN/CUE → CHD (via `chdman`), and multi-disc games grouped with `.m3u` playlists. |
| **Launcher** | One window that starts the tools with no console window. |

The Sorter and Archiver can also build **`ROM Library.html`**: a single self-contained page
(no internet, no other files) that catalogues your games and is searchable in any browser.

### Highlights

- **Safe by default.** Preview mode changes nothing and can write a CSV report. The Sorter only moves
  files and keeps a journal for *Undo*. The Archiver verifies new zips before deleting originals.
  The Converter never deletes unless you ask.
- **Pick your folders.** Every tool lets you choose which folders to work on; the choice is remembered.
- **Works with your frontend.** Layouts for Batocera, MiSTer, RetroPie, ES-DE and Recalbox,
  optional alphabetical subfolders, and editable system-name mappings.
- **Scraper services:** ScreenScraper, TheGamesDB, IGDB, MobyGames, RAWG, Libretro Thumbnails and
  LaunchBox Games Database. Services that need a key or login prompt for it first and test it.
- **Command line too.** The Archiver, Scraper and Converter have CLI modes (`--help`).

## Install (Windows)

1. Download the latest zip from the [Releases](../../releases) page and unzip it.
2. Double-click **`Install TOSEC Toolkit.cmd`**, choose a folder and shortcuts, and click **Install**.

The installer downloads Python 3.12 from python.org, checks it is signed by the Python Software
Foundation, and installs a **private copy** inside the install folder. It doesn't touch any Python you
already have and doesn't change `PATH`. It also adds Start menu entries, including an uninstaller.
For an offline install, put `python-3.12.10-amd64.exe` next to the installer.

> The installer is an unsigned script, so Windows SmartScreen may warn about an unknown publisher.
> The script is plain text, so you can read it before running it.

### CHD conversion needs `chdman`

`chdman` is part of [MAME](https://www.mamedev.org/) and is **not included**. Download MAME, copy
`chdman.exe` into the install folder (the launcher's *Open tools folder* button opens it), and the
Converter finds it automatically.

## Run without installing (Windows, Linux, macOS)

You need Python 3.8+ with Tk. Then run any tool directly:

```
python tosec_sorter.py
python rom_archiver.py
python rom_scraper.py
python rom_converter.py
python tosec_toolkit.pyw     # the launcher
```

CLI examples:

```
python rom_archiver.py --root "D:\Roms" --action zip --preset zip-friendly --dry-run
python rom_scraper.py --folders "D:\Roms\English ROMs" --services libretro,launchbox
python rom_converter.py "D:\PSX" -o "D:\PSX_out" --pbp --chd
```

On Linux, install `chdman` from your distribution (often the `mame-tools` package) so it is on your `PATH`.

## Suggested workflow

1. **Sorter**: choose your collection folder, run a **Preview**, then **Sort ROMs**.
2. **Archiver** *(optional)*: zip the systems whose emulators read zips.
3. **Scraper**: add your *English ROMs* folder, pick services, and scrape.
4. **Converter**: convert PlayStation `PBP` / `BIN/CUE` games to CHD with playlists for multi-disc games.

Full instructions for every option are in [`TOSEC_Toolkit_README.txt`](TOSEC_Toolkit_README.txt).

## Status and known limitations

- **PBP conversion** uses a built-in reader written from the file-format description and tested against
  generated test files, not a large set of real games. Convert one game first, keep the BIN/CUE, and
  check it plays. Encrypted PBPs and PSP (UMD) PBPs are rejected with a clear message.
- The **Windows installer** and the scraper's online services were developed without access to a Windows
  machine or the live services, so please report anything that behaves unexpectedly.
- Scraping services have their own terms and rate limits. Use your own API keys and follow each
  service's rules.

## Contributing

Bug reports and pull requests are welcome. When reporting a problem, please include the tool's log
(the launcher's *Open logs* button) and, for the Converter, the exact message it shows.

## Credits

Created by **normanlove**. CHD conversion uses `chdman` from the [MAME](https://www.mamedev.org/)
project, which is separate software under its own licence and is not distributed here.

TOSEC is a trademark of its respective owners; this project is not affiliated with or endorsed by TOSEC,
MAME, or any of the scraping services.
