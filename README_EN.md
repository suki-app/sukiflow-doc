<p align="center">
  <img src="docs/images/sukiflow-logo.png" alt="SukiFlow" width="120">
</p>

<h1 align="center">SukiFlow</h1>

<p align="center">
  <strong>A desktop solution for automating high-volume AI image and video creation across multiple platforms.</strong>
</p>

<table align="center">
  <tr>
    <td align="center" width="260">
      <img src="https://img.shields.io/badge/Windows-10%20%2F%2011-0078D4?style=flat-square&logo=windows11&logoColor=white" alt="Windows 10/11"><br>
      <strong>SukiFlow for Windows</strong><br>
      <sub>64-bit · EXE installer</sub><br><br>
      <a href="https://github.com/suki-app/sukiflow-doc/releases/download/v1.0.0/SukiFlow_Setup_v1.0.0.exe"><img src="https://img.shields.io/badge/DOWNLOAD_INSTALLER-0078D4?style=for-the-badge&logo=windows11&logoColor=white" alt="Download SukiFlow installer for Windows"></a>
    </td>
    <td align="center" width="260">
      <img src="https://img.shields.io/badge/macOS-Apple%20Silicon-10B981?style=flat-square&logo=apple&logoColor=white" alt="macOS Apple Silicon"><br>
      <strong>SukiFlow for Mac</strong><br>
      <sub>M1 · M2 · M3 · M4</sub><br><br>
      <a href="https://github.com/suki-app/sukiflow-doc/releases/download/v1.0.0/SukiFlow_macOS_v1.0.0_arm64.dmg"><img src="https://img.shields.io/badge/DOWNLOAD_INSTALLER-10B981?style=for-the-badge&logo=apple&logoColor=white" alt="Download SukiFlow installer for Apple Silicon"></a>
    </td>
    <td align="center" width="260">
      <img src="https://img.shields.io/badge/macOS-Intel-5B5BD6?style=flat-square&logo=intel&logoColor=white" alt="macOS Intel"><br>
      <strong>SukiFlow for Mac</strong><br>
      <sub>Intel · x86_64</sub><br><br>
      <img src="https://img.shields.io/badge/DEMO_BUILD-COMING_SOON-6B7280?style=for-the-badge&logo=intel&logoColor=white" alt="Intel build coming soon">
    </td>
  </tr>
</table>

<p align="center">
  <a href="README.md">Tiếng Việt</a>
  ·
  <a href="https://github.com/suki-app/sukiflow-doc/releases/latest">Latest release</a>
  ·
  <a href="#installation">Installation</a>
  ·
  <a href="#troubleshooting">Troubleshooting</a>
</p>

> Current version: **SukiFlow 1.0.0**. SukiFlow is a desktop application for Windows and macOS. Features that depend on third-party services may vary by account, region, quota, availability, and each platform's policies.

---

## Overview

SukiFlow brings multiple AI image and video workflows into one desktop application. It supports batch prompts, multiple linked accounts, task queues, saved sessions, reusable creative presets, and direct transfer of results between tools.

Main capabilities include:

- **Google Flow:** batch image and video generation through Flow Image and Flow Video.
- **Google Pics:** batch image generation with references and direct transfer to Flow Video or Auto Flow.
- **Google Vids:** image and video generation, queue management, and transfer to Auto Flow.
- **Muse AI:** browser-assisted video generation, subject to account and regional availability.
- **Auto Flow:** build complete videos from images, multiple video clips, stock footage, or Draw Video storyboards.
- **Linked Accounts:** manage Google Flow, Google Vids, Google Pics, and Muse AI sessions separately.
- **Creative libraries:** save and reuse Character, Visual, Narrative, and Voice presets.
- **Task management:** run immediately, run selected rows, retry failures, use FIFO queues, and restore previous sessions.

![SukiFlow Flow Image interface](docs/images/flow-image.png)

## Contents

- [Installation](#installation)
- [Getting started](#getting-started)
- [Google Flow](#google-flow)
- [Google Pics](#google-pics)
- [Google Vids](#google-vids)
- [Muse AI](#muse-ai)
- [Auto Flow](#auto-flow)
- [Character, Visual, Narrative, and Voice libraries](#character-visual-narrative-and-voice-libraries)
- [Data and privacy](#data-and-privacy)
- [Troubleshooting](#troubleshooting)

---

## Installation

### System requirements

| Platform | Recommended version | Installer |
|---|---|---|
| Windows | Windows 10/11 64-bit | `SukiFlow_Setup_v1.0.0.exe` |
| macOS Apple Silicon | macOS 12 or later | `SukiFlow_macOS_v1.0.0_arm64.dmg` |
| macOS Intel | Demo build in progress | Coming soon |

Official installers include the runtime components required by the application. End users do not need to install Python or FFmpeg separately.

### Windows

1. Download **SukiFlow Setup** using the button at the top of this page.
2. Close any running SukiFlow instance before installing or updating.
3. Open `SukiFlow_Setup_v1.0.0.exe` and complete the installation.
4. Launch SukiFlow from the Start Menu or the Desktop shortcut.
5. If Windows SmartScreen appears, select **More info** → **Run anyway**. Only install packages downloaded from this official Releases page.

### macOS

1. Download `SukiFlow_macOS_v1.0.0_arm64.dmg`.
2. Open the DMG and drag **SukiFlow.app** into **Applications**.
3. On first launch, right-click SukiFlow and choose **Open** → **Open**.
4. If macOS still blocks the application, open **System Settings → Privacy & Security** and choose **Open Anyway** for SukiFlow.

If macOS reports that the application is quarantined and you are certain it came from the official Releases page, run:

```bash
xattr -dr com.apple.quarantine "/Applications/SukiFlow.app"
```

> The current macOS package targets Apple Silicon (`arm64`). Check your Mac model under **Apple menu → About This Mac** before downloading.

---

## Getting started

1. Open SukiFlow and select **LOGIN GOOGLE** to sign in to your SukiFlow account.
2. Go to **Setting → Linked Accounts**.
3. Select the service you want to use and choose **Add account**:
   - Google Flow
   - Google Vids
   - Google Pics
   - Muse AI
4. Complete sign-in in the browser window opened by SukiFlow.
5. Return to the relevant feature tab, enter a prompt, and select **Run now**.

> Do not close the automated browser window while SukiFlow is creating images or videos through Google Flow, Google Vids, or Google Pics. SukiFlow uses that browser session to execute tasks and collect results.

If a required linked account is missing, SukiFlow displays a setup dialog and can take you directly to the appropriate Linked Accounts section.

---

## Google Flow

### Flow Image

Flow Image supports batch generation with the following controls:

- Aspect ratios: `16:9`, `9:16`, `1:1`, `4:3`, and `3:4`.
- Nano Banana 2, Nano Banana 2 Lite, and Nano Banana Pro models.
- One to four images per prompt.
- 1K, 2K, or 4K output sizes.
- Up to four concurrent rows with configurable delay between rows.
- Direct prompt entry or TXT, CSV, and XLSX import.
- Automatic character references using `@CharacterName`.
- Optional watermark processing when supported by the local environment.
- Run selected rows, retry failed rows, and restore previous sessions.
- Send completed images directly to Flow Video or Auto Flow.

![Flow Image](docs/images/flow-image.png)

### Flow Image queue

Select one or more prompts and add them to the queue. Each queued task preserves its own settings and runs in order. The configured concurrency limit applies across active and retried tasks.

![Flow Image queue](docs/images/flow-image-queue.png)

### Flow Video

Flow Video creates videos from prompts or Start/End frames:

- `16:9` and `9:16` aspect ratios.
- Omni 1.1 Flash and Veo 3.1 Lite, Fast, and Quality modes.
- 360p or 720p resolution.
- Four-to-ten-second duration options, depending on model and account availability.
- Up to four videos per prompt.
- Start–End frame chaining and direct image transfer from Flow Image or Google Pics.
- Configurable concurrency, delay, output folder, and watermark processing.

![Flow Video](docs/images/flow-video.png)

---

## Google Pics

Google Pics provides a separate batch image workflow:

- Common image aspect ratios with original, 2K, or 4K exports.
- Up to four images per prompt.
- TXT, CSV, and XLSX prompt import.
- Automatic character matching through `@CharacterName`.
- Queue execution, retry, previous-session restore, and selected-row controls.
- Direct transfer of completed images to Flow Video or Auto Flow.

![Google Pics](docs/images/google-pics.png)

---

## Google Vids

Google Vids supports prompt and reference-image workflows, per-row result tracking, and transfer of completed clips into Auto Flow for long-form video assembly.

- Vids Video and Vids Image workspaces.
- Automatic use of saved characters when prompts contain `@CharacterName`.
- Queue management, retry, selected-row execution, and previous sessions.
- Output to a timestamped job folder or a selected parent directory.

![Google Vids](docs/images/google-vids.png)

---

## Muse AI

Muse AI uses a dedicated browser session and provides controls for aspect ratio, duration, concurrent rows, delay, and output folder.

![Muse AI](docs/images/muse-ai.png)

> Muse AI may be unavailable in certain countries or for certain accounts. Access depends on availability provided by the Muse platform.

---

## Auto Flow

Auto Flow assembles complete videos from multiple media sources.

### Images → Video

- Assemble a folder of images or images transferred from Flow Image and Google Pics.
- Configure camera movement and visual effects for individual scenes.
- Generate AI voice-over, use existing audio files, or build a video without narration.
- Synchronize timeline, subtitles, background music, and final export.
- Render a short preview before starting the full video build.

![Auto Flow Images to Video](docs/images/autoflow-image-to-video.png)

### Videos → Video

- Combine multiple clips into a longer video.
- Reorder and edit individual scenes.
- Keep or mute original audio and add narration, music, and subtitles.

![Auto Flow Videos to Video](docs/images/autoflow-videos-to-video.png)

### Stock Video

- Generate a script from a topic or use an existing script.
- Find matching footage from Pexels and Pixabay.
- Arrange footage into scenes and add effects, narration, music, and subtitles.
- Optionally replace selected scenes with AI video from Google Flow, Google Vids, or Muse AI.

![Auto Flow Stock Video](docs/images/autoflow-stock-video.png)

Stock Video requires both a **Pexels API Key** and a **Pixabay API Key** under **Setting → General → API Key Settings**. Some script-generation, analysis, or voice steps may require a Gemini API Key or AI33 API Key. If a required key is missing, SukiFlow can open the corresponding settings section.

### Draw Video

Draw Video turns a topic or script into illustrated storyboard scenes and combines voice, timing, subtitles, and music into a complete video.

![Auto Flow Draw Video](docs/images/autoflow-draw-video.png)

### Jetflag Video

Jetflag Video is designed to transform a source video into localized versions for different languages and markets. This feature is currently under development.

![Auto Flow Jetflag Video](docs/images/autoflow-jetflag-video.png)

---

## Character, Visual, Narrative, and Voice libraries

### Character

Store one or more reference images for a character. Names are preserved exactly as entered and can be referenced in prompts with `@CharacterName`.

![Character library](docs/images/setting-character.png)

### Visual

Save reusable visual-style presets for multiple projects and prompts.

![Visual library](docs/images/setting-visual.png)

### Narrative

Save storytelling styles, content structures, and writing instructions for Auto Flow.

![Narrative library](docs/images/setting-narrative.png)

### Voice

Manage voice presets, voice IDs, and speed settings for repeated use in video projects.

![Voice library](docs/images/setting-voice.png)

---

## Data and privacy

- SukiFlow is a desktop application. Generated images, videos, and project files are saved locally in your selected folders.
- Sign-in sessions for supported platforms are stored in the application's data area so you do not need to sign in every time the application starts.
- Signing out of the SukiFlow device does not automatically delete all linked platform accounts. Manage or remove them individually under **Setting → Linked Accounts**.
- Never share API keys, cookies, browser profiles, or application data folders.
- SukiFlow uses browser sessions to interact with third-party platforms. You are responsible for complying with the terms and policies of those platforms.

### Default output folders

Modules may use these default folders when no custom folder is selected:

| Module | Typical folder |
|---|---|
| Google Flow Image/Video | `Pictures/GoogleFlow_Output` |
| Google Pics | `Pictures/GooglePics_Output` |
| Auto Flow | `Pictures/AutoFlow_Output` |

---

## Troubleshooting

### Connection status never becomes active

- SukiFlow may hide the connection badge while no task is running.
- When a worker starts, the status changes to **Connecting ...**.
- When the worker is ready, the status shows the number of active connections.
- Do not close a browser window opened by SukiFlow while a task is running.

### No linked account is available

Go to **Setting → Linked Accounts**, choose the correct service, and add an account. A Google Flow account does not automatically replace a Google Vids, Google Pics, or Muse AI account.

### Windows installer reports “Access is denied”

Close SukiFlow completely, including related browser and terminal windows, and then run the installer again. Do not select **Skip this file**, because that can leave the installation incomplete.

### Image or video generation is unusually slow

- Confirm that the automated browser is still open and the account remains signed in.
- Check platform quota, credits, and any warning shown by the provider.
- Reduce concurrency to one row for diagnosis.
- Retry with a short prompt and no reference images.
- Open **Setting → Logs** and save the logs before requesting support.

### Watermark processing is slow or never completes

Watermark processing requires a bundled model and local processing resources. Disable watermark processing temporarily to verify that the main image or video workflow is working. Performance can vary by CPU architecture and machine configuration.

### Muse AI does not work

Muse AI may be restricted by account or region. This does not necessarily indicate an installation problem in SukiFlow.

---

## Updates

SukiFlow checks version policy at startup. When a mandatory update is available, the application displays an update notice and the official download link.

Always download official builds from:

**[GitHub Releases — SukiFlow](https://github.com/suki-app/sukiflow-doc/releases/latest)**

---

## Disclaimer

SukiFlow is a workflow automation tool. It is not affiliated with, sponsored by, or endorsed by Google, Meta, Pexels, Pixabay, or any other third-party platform mentioned in this document. All trademarks belong to their respective owners.

<p align="center">
  <img src="docs/images/sukiflow-logo.png" alt="SukiFlow" width="72"><br>
  <strong>SukiFlow 1.0.0</strong><br>
  © 2026 SukiFlow
</p>
