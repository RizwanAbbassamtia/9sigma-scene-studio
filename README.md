# 9 Sigma Scene Studio

Python software for a local video-production portal and app-owned Google Chrome profiles. Users submit batches; the local worker checks account identity and provider availability, saves generation receipts, and downloads confirmed results.

[Source repository](https://github.com/RizwanAbbassamtia/9sigma-scene-studio) · [Software releases](https://github.com/RizwanAbbassamtia/9sigma-scene-studio/releases) · [Setup](SETUP.md) · [Development](DEVELOPMENT.md)

## Get the full source

This repository is distributed as a release snapshot. The GitHub Code view contains documentation and a source archive, **not the expanded editable Python tree**. Download and extract `Scene-Studio-Source-2026.10.08.13.zip` from [Releases](https://github.com/RizwanAbbassamtia/9sigma-scene-studio/releases), or extract the source archive included with the published snapshot.

GitHub's **Code → Download ZIP** contains the published documentation/archive wrapper. Extract the inner source ZIP before running source-relative commands. The extracted source folder contains both application directories, tests and build recipes listed below.

## Current release status

The worker source is **2026.10.08.13**. It includes background Chrome operation, optional proxies, saved-account identification, conservative submission tracking, existing Gemini result recovery, and **Continue queued scenes** without resetting scenes needing review.

The unified Windows hub opens local setup/login on port 8792 and asks the administrator to choose a password. The frozen hub passed **14 checks using a fresh temporary installation on the development Windows host**, including setup and login without a separate Python installation. Seven launcher tests also passed. **A separate physical device has not been tested.** Use the release artifact identified by its checksum and verification notes.

**No completed clip from the new four-prompt Trial has been verified.** Earlier demonstration media is not counted as output from that Trial. Successful unit tests do not prove that Google will accept an account or complete a video request.

## Inside the extracted source archive

- `Scene-Studio-Web-Portal/`: Django login, users, batches, account-pool administration and downloads.
- `Scene-Studio-Browser-Worker/`: Windows worker using independent app-owned Chrome profile folders.
- `tests/worker/`: 10 focused worker test modules; portal tests live with the portal application.
- `packaging/`: portable build recipes. Chrome and ffprobe are not bundled.

Chrome sign-in stays visible; generation runs in the background one profile at a time. Google restrictions, verification and account eligibility must be respected. Saved green account labels identify saved profile information and do not guarantee current service access or credits.

## Accounts and other devices

Installing this application on another PC does not transfer Google logins. Profile sessions, account inventory, queues and Windows-protected secrets remain private local data. Set up accounts on the new device; do not upload profile folders or secret files to this repository.

GitHub hosts the source and downloads. It does not run the Django application. A `127.0.0.1` login address works only on the PC running Scene Studio; there is no public hosted login service in this release.

For installation, use [SETUP.md](SETUP.md). For tests and source builds, use [DEVELOPMENT.md](DEVELOPMENT.md).

