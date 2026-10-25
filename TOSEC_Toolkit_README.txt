==============================================================================
 TOSEC TOOLKIT  -  README
 created by normanlove
==============================================================================

TOSEC Toolkit is a set of five programs for a large TOSEC ROM collection:

  1. TOSEC Sorter   - sorts a TOSEC set into "kept" and "filtered out" folders
                      (language, hacks, bad dumps, duplicates...), one folder
                      per system, in the layout your frontend expects.
  2. ROM Archiver   - zips or unzips every file, system by system.
  3. ROM Scraper    - downloads descriptions and pictures for your games from
                      many online services.
  4. ROM Converter  - PBP to BIN/CUE, BIN/CUE to CHD, and multi-disc games
                      with .m3u playlists.
  5. The Launcher   - one window that starts the other four.

Contents
  1. Installing
  2. Using the Launcher
  3. Before you start: golden rules
  4. TOSEC Sorter
  5. ROM Archiver
  6. ROM Scraper
  7. ROM Converter (PBP / CUE / CHD / M3U)
  8. The HTML library page
  9. Files the tools create
 10. Running without installing / command line
 11. Troubleshooting
 12. Uninstalling


------------------------------------------------------------------------------
1. INSTALLING  (Windows)
------------------------------------------------------------------------------

  1. Unzip TOSEC_Toolkit_Setup.zip. Keep all the files together in one folder.
  2. Double-click "Install TOSEC Toolkit.cmd". A setup window opens (you may
     see a console flash for a second as it starts).
  3. Choose the install folder (the default is fine), tick the shortcuts you
     want, and click Install.

The installer:
  - downloads Python 3.12.10 from python.org and checks that the download is
    signed by the Python Software Foundation,
  - installs a PRIVATE copy of Python inside the install folder (it does not
    touch any Python you already have, and does not change PATH),
  - copies the tools in and makes shortcuts, including "Uninstall TOSEC
    Toolkit" in the Start menu.

Offline install: put the file python-3.12.10-amd64.exe (or -arm64.exe) next to
the installer and it will use that instead of downloading.

CHD conversion needs chdman.exe, which is NOT included. It is part of MAME:
download MAME from mamedev.org, take chdman.exe out of the zip and copy it
into the install folder (the launcher's "Open tools folder" button opens it).

Linux / macOS: there is no installer. Install Python 3.8+ with Tk, then run
the .py files directly (see section 10).


------------------------------------------------------------------------------
2. USING THE LAUNCHER
------------------------------------------------------------------------------

Open "TOSEC Toolkit" from the Start menu or desktop. It shows a card for each
tool with a Launch button.

  - Launch starts the tool with NO console window. While it is open, the
    button reads "Running...".
  - If a tool closes with an error, the launcher offers to open its log.
  - "Open tools folder" opens the install folder (put chdman.exe here).
  - "Open logs" opens the folder with each tool's output and error log.
  - The footer tells you whether chdman was found.


------------------------------------------------------------------------------
3. BEFORE YOU START: GOLDEN RULES
------------------------------------------------------------------------------

  * Choose your ROM folder. The Sorter and Archiver each have a "ROM folder"
    box at the top with a Browse button. Choose the folder that holds your
    collection. It is remembered. The Archiver and Scraper automatically
    start from the folder you last used in the Sorter.
  * Always run a PREVIEW first. Preview changes nothing and (if enabled)
    writes a CSV report so you can see exactly what would happen.
  * Work on a copy first if the collection is irreplaceable.
  * The Sorter only MOVES files and can undo its last run. The Archiver
    deletes originals after verifying the new files unless you untick that
    option. The Converter never deletes anything unless you tick "delete
    sources".
  * The recommended order is:  Sorter -> Archiver (optional) -> Scraper.
    Run the Converter on disc-based games (PlayStation etc.) separately.


------------------------------------------------------------------------------
4. TOSEC SORTER
------------------------------------------------------------------------------

What it does: scans your ROM folder and all its subfolders, reads the TOSEC
tags in each file name, and MOVES files into two folders (names are
configurable):

    English ROMs / <System> / ...      ROMs that passed your filters
    Filtered Out / <System> / ...      everything that was sorted out

Nothing is deleted. It handles any language, not just English.

HOW TO USE
  1. Click Browse... and choose the folder that holds your TOSEC collection.
  2. Go through the tabs below and set your options.
  3. Click "Preview (moves nothing)" and read the log / CSV report.
  4. Click "Sort ROMs" to do it for real.
  5. Changed your mind? Click "Undo last run" - every file goes back.

Buttons
  Preview            show what would happen, move nothing
  Sort ROMs          do it (asks you to confirm)
  Undo last run      move everything from the last run back
  Build library now  make the HTML library page without sorting
  Reset options      put all options back to their defaults

TAB: Language
  - "Sort out ROMs that are not in the languages below" - turn language
    filtering on or off.
  - Languages to keep: TOSEC language codes, comma separated (for example
    "en", or "en, de", or "ja"). If you keep several, you get one copy of each
    ROM per language.
  - ROMs with no language tag: decide by country - so (US) counts as English
    and (JP) as Japanese. A ROM with no country counts as a match.
  - Accept multi-language ROMs tagged like (M3), where the languages are not
    listed.

TAB: Sort out (ticked items are moved to the filtered-out folder)
  - Hacks and modified ROMs: cracked, fixed, hacked, modified, pirated,
    translated, trained, and so on (TOSEC flags cr f h m p t tr).
  - Bad dumps: bad, over dump, under dump, virus (b o u v).
  - Alternates (a).
  - Pre-release / development builds: alpha, beta, preview, pre-release,
    proto.
  - Demos: demo versions.

TAB: Duplicates
  - "Keep only one copy of each ROM" - the other versions are sorted out as
    duplicates.
  - "If every copy of a title would be sorted out by the filters, keep the
    best one anyway" - so a game never disappears completely.
  - Which copy wins: highest version number (most up to date) or lowest
    (closest to the original release).
  - Preferred regions when otherwise equal (for example GB, US, EU).
  - Always preferred first: unmodified over hacked, undamaged dumps, and
    finished builds over pre-releases.

TAB: Layout
  - System folder names: name the system folders the way your frontend
    expects. Choices: TOSEC (original names), Batocera, MiSTer, RetroPie,
    ES-DE, Recalbox, or a custom mapping.
  - Export mapping file...: writes tosec_sorter_systems.json. Edit it to
    rename any system folder, then pick the custom option.
  - TOSEC categories: TOSEC splits one system into several folders (Games,
    Demos, Applications...). Off = merged into one system folder (best for
    frontends). On = kept separate, but all inside that system's folder.
  - Alphabetical subfolders: split each system folder into 0-9, A, B, C...
    "Letters per folder" groups them (3 gives A-C, D-F, ...). There is also an
    option to do the same inside the filtered-out folder.

TAB: Output
  - Folder names for kept and filtered-out ROMs.
  - Split the filtered-out folder by reason (Other languages, Duplicates,
    Hacks and modified...).
  - Remove empty folders left behind after moving.
  - Folder levels to skip before the system folder (for collections wrapped in
    extra folders).
  - Reports: write a CSV report after each run (one row per file: what
    happened and why); build the HTML library after each sort; optionally
    include the filtered-out games in the library.

Undo: the Sorter keeps a journal of the last run. "Undo last run" works from
that journal even if you have since chosen a different ROM folder.


------------------------------------------------------------------------------
5. ROM ARCHIVER
------------------------------------------------------------------------------

What it does: gives every file its own .zip (or takes every file out of its
.zip), system by system. Use it after the Sorter. It works on the folders the
Sorter made ("English ROMs" and "Filtered Out").

HOW TO USE
  1. ROM folder: Browse to the folder that holds "English ROMs" and
     "Filtered Out" (it defaults to the Sorter's last folder).
  2. "What do you want to do?": Zip, or Unzip.
  3. "Which folders?": pick English, Filtered, or both (and the folder names).
  4. "Which systems?": click rows to tick or untick them. Shortcut buttons:
       Zip-friendly systems              tick systems whose emulators read zips
       Systems that need unzipped files  tick systems that need loose files
       Tick all / Tick none / Refresh list
     The "Emulator support" column is based on Batocera's per-system lists of
     accepted ROM formats. Disc-based systems (CD32, Sega CD, PC Engine CD,
     Neo Geo CD...) are marked as needing unzipped files.
  5. Set the options, click "Preview (changes nothing)", then "Run".
  6. "Stop" finishes the files currently in progress, then stops.

Options
  - Delete the original once the new files are written and verified
    (recommended). Untick to keep originals.
  - Compression when zipping: maximum, normal, or store (no compression).
  - Zip name: "Game.zip" or "Game.adf.zip" (keeps the original extension).
  - Leave files that are already archives (.zip .7z .rar .lha ...) alone.
  - When unzipping, put zips that hold several files into a folder named after
    the zip.
  - Write a CSV report after each run.
  - Include the filtered-out games in the library (see section 8).
  - Files processed at once: 2 is safe for hard drives; try 4 or more on an
    SSD.

Safety: each new zip is written, verified, and only then is the original
removed. Interrupted runs leave temporary files marked ".rom_archiver_tmp_".


------------------------------------------------------------------------------
6. ROM SCRAPER
------------------------------------------------------------------------------

What it does: finds your games on online databases and saves descriptions,
release dates, developers, publishers, genres, player counts, ratings and
pictures (box art, screenshots, title screens, logos, video) in the format
your frontend reads.

HOW TO USE (it is organised in four steps/tabs)

  1. Folders
     - "Add folder..." - add one or more folders of ROMs. Every system folder
       inside them is found automatically. It opens in your last Sorter
       folder. "Remove" and "Re-scan" are also there.
     - "Systems found" - click rows to tick or untick the systems to scrape.
       Tick all / Untick all.

  2. Services
     - Tick the services to use. The TOP one is tried first; the ones below
       only fill in what it is missing. Use Move up / Move down to reorder.
     - Services that need a login ask for it before scraping starts, and test
       it ("Test login"). "Login / key..." lets you enter or change it. You
       can tick "Remember this on this computer" - this saves it as plain text
       in rom_scraper_credentials.json, so do not share that file.
     Available services:
       ScreenScraper         best coverage for retro systems; matches by file
                             hash, then name. Needs a developer ID and
                             developer password.
       TheGamesDB            community database. Free API key.
       IGDB                  large modern database. Free Twitch Client ID and
                             Secret.
       MobyGames             well-researched retro database. API key (its free
                             tier is slow).
       RAWG                  mostly modern games. Free API key.
       Libretro Thumbnails   box art, screenshots, title screens. No login.
       LaunchBox Games DB    descriptions, dates, art. No login (it downloads
                             one large metadata file the first time).

  3. What to save
     - Information: description, release date, developer, publisher, genre,
       players, rating.
     - Pictures and video: box art, screenshot, title screen, logo/marquee,
       video (large files).
     - Where it goes: choose the output format, or leave it on "Detect from my
       folders (recommended)", which recognises Batocera, RetroPie, Recalbox
       and ES-DE layouts:
         Batocera / EmulationStation / RetroPie  gamelist.xml + images and
                                                 videos folders
         Recalbox                                gamelist.xml + media/images,
                                                 media/thumbnails, media/videos
         ES-DE                                   downloaded_media + gamelists
         Just a data file                        rom_scraper_data.json and a
                                                 rom_scraper_media folder (for
                                                 example for MiSTer)
     - "Save somewhere else (optional)": blank means gamelist.xml and pictures
       go inside each system's own folder. If a format needs a separate folder
       (ES-DE, data file) and you leave this blank, it is created beside your
       ROMs.
     - Options: match strictness (0.5 loose to 1.0 exact), preferred regions,
       language, games at once, skip games already done, retry games that were
       not found last time, overwrite existing information/pictures, and read
       ROM contents to identify them (hash lookup).
     - Existing gamelists are MERGED: your own names, favourites and extra tags
       are kept, and a backup of the gamelist is made.

  4. Speed & schedule
     - A live speed meter shows the current download speed.
     - Speed limit: Full speed, or "Limit to" a number of KB/s. It changes
       straight away, even mid-run.
     - Time-of-day schedule: tick "Use the schedule", then add rules (From /
       to times, days, and a speed: full, pause, or a number of KB/s, or e.g.
       2mb). The first rule that matches the current time wins. Times are
       24-hour on your computer's clock; a rule can cross midnight. Example:
       full speed overnight, 300 KB/s by day.

Buttons: "Scrape" starts. "Dry run" looks things up and saves nothing. "Pause"
pauses (and becomes "Resume"). "Stop" ends the run. Your progress is saved, so
the next run carries on where it stopped and skips games already done.

Tips
  - Start with Libretro + LaunchBox (no logins needed), then add others.
  - Try one small system first.
  - Free API services have daily limits; the scraper backs off and can be
    resumed another day.


------------------------------------------------------------------------------
7. ROM CONVERTER  (PBP / BIN-CUE / CHD / M3U)
------------------------------------------------------------------------------

What it does:
  1. PBP -> BIN/CUE   (PlayStation 1 "PSone Classics" EBOOT.PBP files; built
                       in, no extra tool needed)
  2. BIN/CUE -> CHD   (uses chdman.exe - see section 1 for where to get it)
  3. Multi-disc games get grouped, optionally put in their own folder, and get
     an .m3u playlist.

IMPORTANT - PBP support: the PBP reader was written from the file format
description and checked against test files built to that description, not
against a large set of real games. Convert ONE PBP first, keep the BIN/CUE
("Keep BIN/CUE" ticked), and check it plays before converting the rest.
Encrypted PBPs and PSP (UMD) PBPs are rejected with a message, never silently
wrong.

HOW TO USE
  1. "Add folder..." - add the folders to scan (subfolders included).
  2. Output folder: leave empty to write next to each source, or choose one
     (the folder structure is copied).
  3. chdman: shows whether chdman was found (next to the tools, or on PATH).
     Use Browse... if it lives elsewhere.
  4. Tick the steps and options, then Start. Stop cancels (including a running
     chdman job).

Steps and options
  - Convert PBP -> BIN/CUE.
  - Convert BIN/CUE -> CHD (needs chdman).
  - Keep BIN/CUE made from PBPs after the CHD exists.
  - Verify each CHD after creating it (recommended). Files are written to a
    temporary name and renamed only after they pass.
  - Create .m3u playlists for multi-disc games.
  - Put multi-disc games in a folder of their own.
  - Also create a playlist for single-disc games.
  - Include existing .chd files when sorting multi-disc games - so a library
    you already converted can be grouped and given playlists.
  - Overwrite existing output files.
  - Delete source files after a verified conversion (off by default; a
    confirmation is shown).
  - Write a CSV report.
  - Dry run: shows what would happen and changes nothing.

How multi-disc games are found
  - From file names: "(Disc 1)", "(Disc 2 of 3)", "(Disk 1)", "[CD2]" or a
    trailing "CD1" / "Disc 1". The tag is removed to get the game title; files
    with the same title in the same folder form one game.
  - From the file itself: a PBP that contains several discs is split into
    "Game (Disc 1)", "Game (Disc 2)"...
  - The playlist is named after the game (for example "Game (USA).m3u") and
    lists each disc file, in order, with relative names. If any disc of a game
    fails, no playlist is written for it.
  - A playlist lists .chd files when CHD conversion is on, otherwise .cue.

Tips for frontends: RetroArch / Batocera / RetroPie / Recalbox load the .m3u
as the game and let you swap discs. For PBPs that contain several discs,
no other action is needed.


------------------------------------------------------------------------------
8. THE HTML LIBRARY PAGE
------------------------------------------------------------------------------

The Sorter and the Archiver can build "ROM Library.html" in your ROM folder:
a single, self-contained page (no internet or other files needed) that
catalogues your games. Open it in any browser. It supports searching,
filtering by system, and browsing, and works offline.

Turn it on with "Build the HTML library after each sort / run" on the Output
tab (Sorter) or the options list (Archiver), or press "Build library now" at
any time. You can include the filtered-out games too.


------------------------------------------------------------------------------
9. FILES THE TOOLS CREATE
------------------------------------------------------------------------------

In the install folder (settings and logs; safe to delete, they are recreated):
  tosec_sorter_settings.json     Sorter options and your last ROM folder
  tosec_sorter_journal.json      what the last sort moved (for Undo)
  tosec_sorter_systems.json      your custom system folder names (if exported)
  rom_archiver_settings.json     Archiver options
  rom_scraper_settings.json      Scraper options
  rom_scraper_credentials.json   saved service logins (plain text - keep private)
  rom_scraper_data.json          scraper progress (used for resume)
  launchbox_Metadata.zip         cached LaunchBox download
  logs\                          each tool's output when started by the launcher

In YOUR folders:
  CSV reports        tosec_sorter_report_*.csv, rom_archiver reports,
                     rom_scraper_report_*.csv, rom_converter_report_*.csv.
                     The Sorter and Archiver put them in your ROM folder, the
                     Scraper in your output folder or first ROM folder, the
                     Converter in the output folder or next to your sources.
  ROM Library.html   see section 8
  gamelist.xml and picture folders from the Scraper


------------------------------------------------------------------------------
10. RUNNING WITHOUT INSTALLING / COMMAND LINE
------------------------------------------------------------------------------

Every tool is a single .py file. With Python 3.8+ you can run it directly:

    python tosec_sorter.py        (opens the window)
    python rom_archiver.py
    python rom_scraper.py
    python rom_converter.py
    python tosec_toolkit.pyw      (the launcher)

The Archiver, Scraper and Converter also have command-line modes. Add --help
for the full list. Examples:

  Archiver:
    python rom_archiver.py --root "D:\Roms" --list-systems
    python rom_archiver.py --root "D:\Roms" --action zip --preset zip-friendly --dry-run
    python rom_archiver.py --root "D:\Roms" --library-only

  Scraper:
    python rom_scraper.py --list-services
    python rom_scraper.py --folders "D:\Roms\English ROMs" --services libretro,launchbox
    python rom_scraper.py --folders "D:\Roms\English ROMs" --limit 500 --dry-run

  Converter:
    python rom_converter.py "D:\PSX" -o "D:\PSX_out" --pbp --chd
    python rom_converter.py "D:\PSX" --dry-run
    python rom_converter.py "D:\PSX" --no-folder --m3u-all

On Linux, put chdman on your PATH (the package that provides it is usually
called "mame-tools" or "mame").


------------------------------------------------------------------------------
11. TROUBLESHOOTING
------------------------------------------------------------------------------

"Choose the ROM folder first"
    Click Browse... at the top of the Sorter / Archiver.

The Archiver shows no systems
    Its ROM folder must be the one that CONTAINS "English ROMs" and
    "Filtered Out". Check the folder names under "Which folders?" match.

"chdman not found"
    Copy chdman.exe into the install folder (or add it to PATH).

A tool does not open
    Use the launcher's "Open logs" button and read that tool's log.

Windows SmartScreen warns about the installer
    The installer is an unsigned script, so Windows shows an "unknown
    publisher" style warning. Choose "More info" -> "Run anyway" only if you
    trust where you got the files.

The scraper finds few matches
    Lower the match strictness a little (for example 0.7), add more services,
    or make sure your files are in system folders it recognises.

A PBP will not convert
    The message in the log says why (encrypted, PSP game, unsupported layout).
    Please keep the file and the message, so the reader can be improved.


------------------------------------------------------------------------------
12. UNINSTALLING
------------------------------------------------------------------------------

Use "Uninstall TOSEC Toolkit" in the Start menu, or Settings -> Apps ->
TOSEC Toolkit. It removes the private Python, the shortcuts and the whole
install folder - including chdman.exe if you put it there, and the saved
settings. Your ROMs and reports are never touched.


------------------------------------------------------------------------------
 TOSEC Toolkit - created by normanlove
------------------------------------------------------------------------------
