# LEAD AE – ASSIST

LEAD AE – ASSIST is an Electron desktop application for assistant editors and post teams. It combines verified media ingest, batch transcoding, transcription and caption export, Premiere automation through a bundled CEP panel, project-setup tooling, diagnostics, and offline help in a single app.

This repository is the application source and packaging workspace for that desktop app.

---

## What the app does

The product is organized around a set of operational panels rather than a single monolithic workflow:

- **Ingest** — verified copy, dual-copy backup, duplicate skipping, extension filtering, Clone Mode, and Watch Mode.
- **Transcode** — batch transcode, watch-folder transcode, audio-only exports, LUT application, metadata preservation, caption attach into MXF, and output verification.
- **Transcribe** — transcript export, subtitle export (`.srt`, `.vtt`), broadcast caption export (`.scc`, `.mcc`), script CSV, burn-in output, translation, diarization, and Subtitle Editor handoff.
- **Adobe Automate** — copy-first workflow that can import verified files into Premiere, build bins, create proxies, and attach them back through the Premiere CEP panel.
- **Subtitle Editor** — separate editor window for `.json`, `.srt`, `.vtt`, `.scc`, and `.mcc` documents with QC, preview, correction export, burn-in, SCC/MCC export, and MCC service-aware editing.
- **Project Organizer** — creates standardized project folder trees and can copy starter assets into the generated structure.
- **Speed Test** — storage and network benchmarking before copy, transcode, or watch-folder operations.
- **NLE Utilities** — Avid reset/rebuild tools plus Adobe cache, autosave, and preview cleanup.
- **Log Viewer** — reads the app’s canonical raw log library, supports live updates, filtering, and TXT/JSON/CSV export.
- **Preferences / Account & Licensing** — app-wide settings, secure API key handling, runtime behavior, CEP install/repair, licensing, billing, and support version info.


### AI and subtitle pipeline

`ai/` contains the text/subtitle/caption formatting and parsing layer:

- transcription normalization and preparation
- Whisper-oriented helpers
- SRT/VTT parsing, validation, and writing
- SCC shaping and output writing
- subtitle parser aggregation
- plain text formatting

This code is shared between Transcribe, Subtitle Editor, and some QC/export flows.

### Offline help system

The help docs live in `help/` as markdown source. They are compiled into a static offline help bundle under `resources/help/` by `scripts/build-help.js`.

The help build is intentionally:

- offline
- deterministic
- free of runtime markdown parsing
- compatible with a strict CSP

### Adobe / Premiere integration

The Premiere integration is a CEP panel with source-of-truth files under:

- `cep/extensions/com.leadae.panel/`

Packaging and installation expect mirrored/generated artifacts under:

- `resources/cep/extensions/com.leadae.panel/`
- `resources/cep/zxp/LeadAEAssist_CEP.zxp`

The desktop app talks to the CEP panel through a tokenized loopback bridge rather than by exposing random network endpoints.

### Host platform note

The app source includes cross-platform path handling and user-data paths for macOS and Windows, but the current Electron Builder configuration is set up for **macOS arm64 distribution artifacts** (`zip` and `pkg`).

If you are packaging releases, assume macOS is the supported build host.

## Legal and third-party notices

Relevant notices in this repo include:

- `LICENSE`
- `legal/NOTICE.md`
- `legal/FFmpeg-LGPLv2.1.txt`
- `legal/Electron-MIT.txt`

The first-party LeadAEAssist codebase and assets are proprietary. Third-party notices under `legal/` do not grant rights to copy or redistribute first-party LeadAEAssist materials.

Review those files before distributing builds externally.

