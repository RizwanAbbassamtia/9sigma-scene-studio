# Scene Studio handoff — 8 October 2026

Public project: https://github.com/RizwanAbbassamtia/9sigma-scene-studio

Windows preview and full Python source: https://github.com/RizwanAbbassamtia/9sigma-scene-studio/releases/tag/v2026.10.08.13

Read SETUP.md for installation and DEVELOPMENT.md for source builds and tests. The editable Python project is inside `Scene-Studio-Source-2026.10.08.13.zip`; the GitHub repository currently distributes a snapshot archive.

## What is verified

- Windows package includes the local Django portal, first-run administrator setup, login/logout, static assets, queue scheduler and the separate browser worker.
- Hub build: `2026.10.08.1`. Worker build: `2026.10.08.13`, worker API saved-profile generation revision `6`.
- 89 focused worker tests, 33 portal tests, 7 launcher tests and 14 frozen setup/login checks passed on the development Windows computer.
- Frozen setup tests used a fresh temporary database and also passed without Python on PATH. Another physical computer has not been tested.
- Source and Windows ZIP integrity passed. GitHub's published SHA-256 digests match the local archives.

## Video generation remains incomplete

The current new four-scene trial has no verified completed clips. Earlier demonstration clips are separate and must not be imported as new trial results.

Live integration findings to address next:

1. Flow's current interface reaches its generation controls, but preflight can fail with `Flow does not offer the requested setting: Video`. Inspect the actual current controls before updating the adapter. Preserve identity, model, aspect, duration, count, balance and price checks; do not submit on guessed settings.
2. One Gemini request ended with a completed provider refusal. Version .13 now recognizes that specific completed assistant response promptly; recovery verified this without submitting another request.
3. The next Gemini request returned a completed text answer saying it had no video-rendering tool. The diagnostic showed Flash mode and no video player or download control. The worker's narrow refusal detector does not cover this response and continues waiting. This is evidence about that conversation, not proof that the account has no video entitlement. Verify the video tool before submission and classify completed non-video responses promptly.
4. Requested output duration is validated after download. Provider output that differs from the requested duration requires review; do not report it as a completed matching clip.

Relevant worker modules are under `Scene-Studio-Browser-Worker/sceneforge/production/`, particularly `flow_browser.py`, `gemini_browser.py`, and `output_recovery.py`. The local API is in `scene_server.py`; portal integration is in `Scene-Studio-Web-Portal/workspace/google_login.py` and `batch_views.py`.

Keep the saved submission receipt when diagnosing a result. `Check existing result` reopens an existing Gemini conversation without creating another generation attempt. `Continue queued scenes` preserves review scenes and runs only untouched queued scenes. A retry is a separate, potentially chargeable action.

## New device and deployment boundaries

Extract the Windows ZIP completely and run `Start Scene Studio.cmd`. Install Chrome and FFmpeg/ffprobe through the included dependency script or official distributions. Python is bundled. The setup/login pages are local to that computer on port 8792.

Create the local administrator and sign into the desired Google profiles on the new device. The public release contains no Google logins, account inventories, proxy secrets, database, prompts or generated videos. Windows-protected secrets and Chrome sessions are not a portable login package.

GitHub provides the public source/download location. No internet-hosted Django login service or cross-device session synchronization has been deployed. Do not expose the loopback worker or development server directly to the internet.
