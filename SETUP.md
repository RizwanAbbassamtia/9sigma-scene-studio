# Windows setup

## Downloaded application

Download the complete Windows package from [Releases](https://github.com/RizwanAbbassamtia/9sigma-scene-studio/releases), check its published SHA-256, extract it and launch `SceneStudioHub/SceneStudioHub.exe`. Keep all accompanying folders together. The frozen hub passed a fresh temporary-installation setup/login check on the development Windows host; another physical PC has not been tested.

The first-run page is [local setup](http://127.0.0.1:8792/setup/), where you choose the administrator password (default username `admin`). Subsequent access uses [local login](http://127.0.0.1:8792/login/). The hub supplies Python internally.

The hub keeps portal data under `%LOCALAPPDATA%\9Sigma\SceneStudio\portal` and profile data under `Documents\Codex\SceneStudioVideoPool`. It supports separate sibling `SceneStudioHub/` and `SceneStudioDesktop/` application folders. Keep each executable with its own `_internal` folder; do not merge them.

Install standard Google Chrome and FFmpeg with `ffprobe` separately. The optional `Install dependencies.cmd` uses Windows Package Manager and leaves agreement prompts to you. It does not include or redistribute FFmpeg binaries. Existing Chrome and `ffprobe` on `PATH` can also be used.

## Run the current source

First download and extract `Scene-Studio-Source-2026.10.08.13.zip` from [Releases](https://github.com/RizwanAbbassamtia/9sigma-scene-studio/releases) or the published source snapshot. GitHub's Code ZIP contains documentation and the source archive; it does not directly provide the expanded Python tree. Extract that inner archive as well.

Requirements: Windows, Python **3.12**, Google Chrome, and `ffprobe` on `PATH` or in the supported Gyan FFmpeg WinGet package. Use your normal Windows desktop login.

In PowerShell, from the extracted source folder containing `Scene-Studio-Browser-Worker/` and `Scene-Studio-Web-Portal/`:

```powershell
py -3.12 -m venv .venv
& .\.venv\Scripts\python.exe -m pip install -r .\requirements-dev\local.txt
```

Set these variables in each terminal used below:

```powershell
$env:SCENE_PROFILE_DATA = Join-Path $env:LOCALAPPDATA 'SceneStudio\profiles'
$env:PORTAL_DATA = Join-Path $env:LOCALAPPDATA 'SceneStudio\portal'
$env:PORTAL_DEV = '1'
$env:SCENE_LOGIN_SESSION = Join-Path $env:SCENE_PROFILE_DATA 'scene-server\session.json'
$env:SCENE_WORKER_PORT = '8788'
$env:BROWSER_GENERATION_ENABLED = '1'
```

For a fresh installation, create the portal database and local administrator:

```powershell
& .\.venv\Scripts\python.exe .\Scene-Studio-Web-Portal\manage.py migrate --noinput
& .\.venv\Scripts\python.exe .\Scene-Studio-Web-Portal\manage.py createsuperuser --username admin
```

Choose your password interactively. Public source does not contain the development laptop's login or Google accounts.

Run each following command in a separate terminal with the same environment:

```powershell
# Terminal 1: profile worker
& .\.venv\Scripts\python.exe .\Scene-Studio-Browser-Worker\desktop_app.py

# Terminal 2: local portal
& .\.venv\Scripts\python.exe .\Scene-Studio-Web-Portal\manage.py runserver 127.0.0.1:8791 --noreload

# Terminal 3: exactly one queue dispatcher
& .\.venv\Scripts\python.exe .\Scene-Studio-Web-Portal\manage.py run_browser_worker
```

Open [the local source login](http://127.0.0.1:8791/login/). The source worker's fresh default is port 8791; the tested bundled hub uses 8792. Existing installations should retain their configured portal port and data paths.

These source commands are for local development. Do not expose `runserver` or worker port 8788 publicly. Existing Docker files describe a separate API-backed Linux portal; they do not connect a public server to this PC's saved Chrome sessions.

## Google profiles and first batch

1. Sign into Scene Studio, open Google Account Pool and import your own email inventory.
2. Open a profile and sign into Google directly in Chrome. Complete any provider setup or verification yourself. Proxy configuration is optional.
3. Close that profile's visible Chrome window before background generation uses it.
4. Add prompts, review the batch and start. The worker checks the visible signed-in account and provider availability.
5. If a result is uncertain, inspect the existing provider result. **Check existing result** downloads from a saved Gemini receipt without a second order. **Continue queued scenes** leaves review scenes untouched. A reviewed retry may spend again.

For portable Chrome, set `SCENE_CHROME_PATH` to the actual `chrome.exe`. Do not point it at a portable launcher.

## Updates and a second PC

Preserve the local data folders when upgrading. Do not erase queues or profiles to refresh the application. Google profiles and DPAPI-protected passwords/proxy secrets are tied to the local Windows-user environment and are not a portable login package. A second PC needs its own Google account setup; no session synchronization service is included.

