# Development and release

Use Python **3.12** on Windows for the browser worker. Install pinned tested direct dependencies with `python -m pip install -r requirements-dev/local.txt`. Chrome and `ffprobe` are separate runtime requirements.

Download and extract `Scene-Studio-Source-2026.10.08.13.zip` before using these commands. The GitHub Code view/Code ZIP is a snapshot distribution containing docs and the archive, not the expanded editable source. Run commands in the inner extracted source folder, where both application directories and `requirements-dev/` exist.

## Tests

From the extracted source root:

```powershell
& .\.venv\Scripts\python.exe -m pytest .\tests\worker -q
$env:PORTAL_DEV = '1'
$env:PORTAL_DATA = Join-Path $env:TEMP 'SceneStudio-test-data'
& .\.venv\Scripts\python.exe .\Scene-Studio-Web-Portal\manage.py check
& .\.venv\Scripts\python.exe .\Scene-Studio-Web-Portal\manage.py test workspace.test_continue_queued workspace.test_output_recovery_views workspace.test_browser_dispatch --noinput
```

The staged worker suite passed **89 tests** across 10 modules. The focused portal suite passed **33 tests** with no system-check issues. These tests cover background profiles, provider controls, identity checks, durable receipts, recovery without a new order, worker API transport and continuing queued scenes without releasing review scenes. The portal test runner creates a separate test database. Do not use tests as evidence that a live provider job succeeded.

The bundled source includes older Qt editor modules. The supported entry point here is `desktop_app.py`; dependencies/assets for those older standalone editor interfaces are outside this release scope.

## Build the worker

```powershell
& .\.venv\Scripts\python.exe -m pip install -r .\requirements-dev\build.txt
& .\.venv\Scripts\python.exe -m PyInstaller --noconfirm --clean --distpath dist --workpath build .\packaging\worker.spec
```

Output: `dist/SceneStudioDesktop/SceneStudioDesktop.exe` and its required `_internal` folder. The portable spec resolves paths from the extracted source root; it does not reference the author's PC or copy Chrome/ffprobe. Keep dependency license/notice files in the generated Playwright runtime. Install FFmpeg/ffprobe separately; no GPL FFmpeg executable is included in the source archive or Windows package.

The portable worker spec has produced a successful build from this staged source. This verifies packaging, not a clean-device launch or live video output.

## Build the unified hub

The hub entry point and spec are under `Scene-Studio-Web-Portal/`. From the extracted source root, after installing build dependencies:

```powershell
$env:PYTHONPATH = (Resolve-Path .\Scene-Studio-Web-Portal).Path
$env:SCENE_BUILD_DATA = Join-Path (Get-Location) 'build\hub-empty-data'
& .\.venv\Scripts\python.exe -m PyInstaller --noconfirm --clean --distpath dist --workpath build .\Scene-Studio-Web-Portal\SceneStudioHub.spec
```

The payload has sibling `dist/SceneStudioHub/` and `dist/SceneStudioDesktop/` folders, each retaining its `_internal` directory. The frozen hub passed all **14 self-checks** against a fresh temporary installation on the development Windows host. Seven launcher unit tests also passed. Its final source/spec are included here. Another physical device has not been tested. Do not package an old worker archive as the new hub.

## Publish only audited source

Never stage the development workspace or complete outputs folder. Publish these source folders, reviewed tests/docs/build scripts and placeholder environment files only. Exclude databases, Chrome profiles, inventory, `.env`, session tokens, DPAPI vaults, proxy credentials, private helper configurations, logs, screenshots, generated media and old archives. Credential-handling Python modules and blank `.env.example` are legitimate source and should remain.

The repository currently has no selected reuse license; public visibility alone does not grant a particular open-source license. Keep copyright and dependency notices intact.

Before release, rerun the staged tests, inspect the exact archive contents, record SHA-256 and version, and verify installation on a clean Windows device. Test local login, profile setup and one authorized video job through the portal. The current new Trial has **zero verified completed clips**; prior demonstration clips do not count.

Public hosting is separate work. A GitHub repository/release does not deploy Django, provide a hosted login URL, or synchronize Windows Google profiles.
