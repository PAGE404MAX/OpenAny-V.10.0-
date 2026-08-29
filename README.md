OpenAny
A desktop file viewer that opens any file type, each in its own window.

Images, video, audio, PDFs, archives, fonts, spreadsheets, source code and
unknown binaries. It renders what it can and falls back to a hex dump when it
can't. Everything is read locally. There is no network code in this program.

Windows x64. MIT licensed. Zero runtime dependencies.

WHAT IT OPENS
161 file extensions across 12 viewers, plus a hex fallback for everything else.

Images png jpg jpeg gif webp avif bmp ico apng jfif
Zoom, fit, 1:1, alpha checkerboard, pixel dimensions.

Vector svg
Rendered, with a source toggle.

Video mp4 webm mov mkv ogv m4v
Player with resolution and duration.

Audio mp3 wav flac ogg oga m4a aac opus weba
Player with duration.

Documents pdf
Embedded viewer.

Code & text 100 extensions - txt log ini cfg toml yaml xml html js ts jsx
py rb rs go java kt swift c h cpp cs php lua sql sh ps1 and more
Line numbers, find with match count, wrap toggle, copy.

Markdown md markdown mdx mkd
Tables, task lists, code fences, blockquotes. Source toggle.

Tabular csv tsv tab
Quote-aware parser, delimiter autodetect, sticky headers,
numeric alignment.

Structured json jsonc geojson ipynb map webmanifest
Collapsible tree, expand/collapse all, pretty-print.

Streaming jsonl ndjson
One tree per record.

Archives zip docx xlsx pptx jar apk epub whl vsix odt ods odp war crx
nupkg
Entry listing with sizes, compression ratios, timestamps.

Fonts ttf otf woff woff2
Specimen at six sizes plus an editable preview box.

Everything Hex dump with ASCII gutter.
else

FILES WITH NO EXTENSION
Identified in four stages:

Magic bytes 12 signatures - PNG, JPEG, GIF, PDF, ZIP, gzip, ELF,
ISO media, Ogg, MP3, fonts
Known names Dockerfile, Makefile, Rakefile, Gemfile, Procfile,
LICENSE, README, CHANGELOG, .env, any .*rc file
Heuristic Samples 4 KB. No null bytes and under 6% control
characters means it is text.
Fallback Hex dump.
FEATURES
One window per file The filename is in the title bar and page content
cannot overwrite it. Drop five files, get five
windows.

Recent files sidebar Click to switch, x to remove one, Clear to empty it,
drag the edge to resize. Persists between launches.

Navigation Back, forward and home, with history. Mouse side
buttons work too.

Live reload Edit a file in another program and the view updates
itself.

Single instance Double-clicking more files routes them to the
running app instead of starting new processes.

Greyscale interface A 10-step ramp from #000000 to #f2f2f2 and nothing
else. No accent colours, no coloured emoji.

Uninstall button Removes the app, its settings, shortcuts, registry
entries and file associations. Optionally sweeps for
other copies.

KEYBOARD
Ctrl+O Open file Esc Home
Ctrl+N New window Backspace Back
Ctrl+W Close window Alt+Left Back
Ctrl+B Toggle sidebar Alt+Right Forward
Ctrl+F Find in text Ctrl+H Force hex dump
Ctrl+R Reload from disk Ctrl+Shift+R Reveal in Explorer
Ctrl+ + - 0 Zoom interface Ctrl+Q Quit

WHAT IS IN THIS PACKAGE
23 files, 632 KB unpacked. 1,553 lines of application code.

src/main.js 15 KB Electron main process. Window
management, one window per file, file
watching, IPC handlers, application
menu, the uninstall routine.

src/viewer.js 35 KB Format detection and every viewer. The
largest file and the one to read first.
743 lines.

src/viewer.css 14 KB The complete greyscale theme. All
styling for the toolbar, sidebar,
tables, code view, markdown, JSON tree,
hex dump and font specimen.

src/index.html 2 KB Application shell. Sidebar, toolbar,
stage and the welcome screen.

src/preload.js 1 KB The contextBridge API. Nine IPC
channels. This is the only connection
between the interface and the system.

src/icon.png 43 KB Icon used on the welcome screen.

package.json 3 KB Project manifest and electron-builder
configuration, including roughly 60
Windows file associations.

package-lock.json 143 KB Dependency lockfile. Needed for
reproducible builds.

installer.nsi 5 KB NSIS installer script. Handles
shortcuts, 26 file associations, the
right-click menu entry and clean
uninstall.

stamp-icon.py 3 KB Pure-Python PE resource editor. Embeds
the icon into the executable and renames
its version strings. Allows building a
Windows installer on Linux without Wine.

build-windows.cmd 1 KB One-click installer build on Windows.

build-installer-linux.sh 1 KB Wine-free installer build on Linux.

run-dev.cmd 1 KB Runs the app from source on Windows.

assets/icon.ico 71 KB Windows icon, 16 to 256 pixels.

assets/icon.png 122 KB Source artwork, 702x702.

docs/ 119 KB Three screenshots.

.github/workflows/build.yml 2 KB Continuous integration. Builds the
Windows installer on every push and
publishes a release when a version tag
is pushed.

README.md 8 KB This file.

CONTRIBUTING.md 4 KB Project rules, how to add a viewer,
required checks before a pull request.

LICENSE 1 KB MIT.

.gitignore 1 KB Excludes node_modules and release
output.

BUILDING
Requires Node.js 18 or newer.

git clone https://github.com/PAGE404MAX/openany.git
cd openany
npm install

npm start Run the app
npm run dist Build the installer into release/

On Windows you can double-click run-dev.cmd or build-windows.cmd instead.

Building a Windows installer on Linux normally fails because electron-builder
shells out to two Windows executables: rcedit.exe to stamp the icon and
makensis.exe to compress the installer. The script build-installer-linux.sh
avoids both. It packages the app with --dir, runs stamp-icon.py to write the
icon directly into the executable's resource table, then calls the Linux
makensis binary that ships inside the electron-builder NSIS package.

HOW IT WORKS
The pipeline is short:

load(path) -> read via IPC -> detect kind -> render() -> view<Kind>()

Detection tries the file extension first, then magic bytes, then a text
heuristic. render() switches on the detected kind and calls one of the twelve
view functions, each of which writes into the stage element.

Every viewer is written by hand. The Markdown renderer, the quote-aware CSV
parser, the JSON tree, the ZIP central directory reader and the hex dumper are
all plain JavaScript. There is no marked, no papaparse, no jszip. The only npm
packages in the project are electron and electron-builder, and both are
development dependencies.

PRIVACY AND SECURITY
Content Security Policy is default-src 'none'
No telemetry, no analytics, no update checks, no remote fonts
Works fully offline
contextIsolation is on, nodeIntegration is off
In-app navigation is blocked, external links go to the system browser
The interface never touches the filesystem directly. All access goes
through the nine IPC channels declared in src/preload.js
Files larger than 512 MB are refused with a message rather than loaded
KNOWN LIMITATIONS
Archive entries are listed but not extracted. You cannot yet click an
entry inside a zip to read it.
No syntax highlighting in code view. Colour would break the greyscale
rule.
Prebuilt binaries are Windows x64 only, though the source runs anywhere
Electron does.
Not code-signed, so Windows SmartScreen warns on first run. Choose
More info, then Run anyway.
LICENSE
MIT. Do what you like, no warranty. See LICENSE.
