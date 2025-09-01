## Split-Video (Gaze-Swap)

A lightweight mobile-first web app for short-form videos with a split-screen twist:
- **Top/bottom screens** show two different videos.
- When you stop looking at one half, the other half plays and the non‑attended half is swapped for a new video.
- Works with **eye tracking (WebGazer)** and has a **Pointer mode** fallback (mouse/touch).

### Quick Start (GitHub Pages)
- Commit and push `index.html` (this folder) to your GitHub repository.
- In your repo: Settings → Pages → Build and deployment:
  - **Source**: Deploy from a branch
  - **Branch**: `main` (or your default) → `/root`
  - Click Save. Your site will be available at the URL shown by GitHub Pages.

### Usage
- Open the site on mobile or desktop.
- Tap “Start Calibration” to begin.
- If prompted, allow camera access for gaze mode. If denied or unavailable, the app switches to Pointer mode automatically.
- Tap the yellow dot at each calibration point.
- After calibration:
  - Look at the top/bottom half to control which video plays.
  - The other half swaps to a new video when attention changes.
  - Tap “🔊 Enable Sound” (or “Unmute” in the sheet) after first user interaction.

### Controls (⋯ bottom sheet)
- Playback: Unmute, Swap Now
- Mode: Gaze or Pointer; Show/Hide cursor
- Calibration: Recalibrate anytime
- Upload: Add videos from device (stored locally), Reset library

### Uploading Videos (local)
- Tap “Add Videos” and select files from your device.
- Files are added to a local playlist and persisted in `localStorage`.
- Note: Local object URLs are only valid on your device/session. They won’t sync to other users.

### Future Cloud Upload Plan
This static app is GitHub Pages‑friendly. For multi-user uploads and sharing, plug in a managed storage backend:
- Option A: **Firebase Storage + Security Rules**
  - Use client SDK for uploads; store metadata in Firestore.
  - Add Cloud Functions for content moderation and transcoding (e.g., via Cloud Run/MediaConvert/ffmpeg).
- Option B: **Supabase Storage**
  - Direct client uploads with Row Level Security policies.
  - Postgres table for video metadata; edge functions for post‑processing.
- Option C: **Cloudflare R2 + Workers**
  - Workers generate presigned URLs; uploads direct to R2.
  - Durable Objects/Queues to trigger transcoding pipelines.

Recommended next steps for cloud:
- Add an `.env` and small `config.js` loader (keys via environment/Pages secrets).
- Implement an `UploadService` with pluggable provider (Firebase/Supabase/R2).
- Replace local playlist when remote uploads are enabled; keep local as offline fallback.

### Privacy & Permissions
- Gaze mode uses the device camera for eye tracking via WebGazer. The camera preview is hidden and not uploaded anywhere by this app.
- Users can switch to Pointer mode if camera access is denied or undesired.

### Tech Notes
- Eye tracking: `webgazer.js` CDN (`https://webgazer.cs.brown.edu/webgazer.js`).
- Mobile: `playsinline`, initial muted autoplay, sound unlock overlay.
- Settings persisted to `localStorage`: mode, cursor visibility, and video library.

### Development
- All logic is in `index.html` for simplicity. No build step required.
- To test locally, open `index.html` with a local server (so camera permissions and autoplay behave more like production).


