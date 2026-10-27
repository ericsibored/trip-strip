# Trip Strip

A mobile-first web app for building seamless Instagram carousels, profile-grid splits and scrolling videos from trip photos and clips. Everything runs in the browser: photos, videos and cutouts never leave the device.

## Deploy to Vercel

This is a plain static site. There is no build step and no `package.json`.

**Option A: GitHub and the Vercel dashboard**

1. Push this folder to a new GitHub repository.
2. In Vercel, choose **Add New → Project** and import the repository.
3. Set **Framework Preset** to **Other**. Leave the build command empty and the output directory as the project root.
4. Deploy.

**Option B: the command line**

```bash
npm i -g vercel        # or use: npx vercel
cd trip-strip-app
vercel                 # creates a preview deployment
vercel --prod          # promotes it to production
```

`vercel.json` sets long cache lifetimes for `/vendor` and `/models`, sets the `.wasm` content type, and keeps `sw.js` uncached so updates reach phones.

## Run locally

Any static server works:

```bash
python3 -m http.server 8080
# or: npx serve .
```

Open http://localhost:8080. The service worker (offline support and install) only registers over HTTPS, so test those on a Vercel preview URL.

## What's in the folder

| Path | What it is |
| --- | --- |
| `index.html` | The whole app: markup, styles and script |
| `sw.js` | Service worker for offline use and install. Bump `VERSION` when you ship changes |
| `manifest.webmanifest`, `icons/` | Home-screen install metadata and icons |
| `vendor/jszip.min.js` | Builds the .zip download |
| `vendor/mp4-muxer.js` | Packages encoded video into MP4 |
| `vendor/ort/` | ONNX Runtime Web (WebAssembly) for cutouts, loaded on first use |
| `models/u2netp.onnx` | Subject-segmentation model for cutouts (4.6 MB), loaded on first use |
| `vercel.json` | Headers and caching |

## Features

- **Carousels:** 1–20 slides at 4:5, 3:4, 1:1, 1.91:1 or 9:16 on one continuous canvas, with seam guides and snapping.
- **Profile grids:** split one design across 3–18 posts. Export numbers the posts in the order you post them, and there's a profile preview.
- **Draft from photos:** pick photos and get a full layout, ordered by capture date, with day captions.
- **Video:** clips go in any frame and can be trimmed. Slides with video export as H.264 MP4. You can also export a 9:16 scrolling video of the whole carousel for Reels and Stories.
- **Cutouts:** on-device subject segmentation, with erase/restore touch-up, a sticker border, and placement right on top of the source photo for pop-out effects.
- **Design tools:**
  - 25 layouts (20 carousel, 5 grid)
  - 39 stickers, plus your own PNGs
  - 13 fonts, plus font upload
  - photo shapes, including stamp and torn paper
  - 6 looks plus manual adjustments
  - gradient and pattern backgrounds
- **Editing:** layers with lock, hide and grouping; multi-select; copy and paste between trips; multiple trips saved in the browser.
- **Sharing:**
  - On phones, the share sheet saves everything to Photos in one step, or hands it to Instagram if the sheet lists it.
  - On computers you get a .zip.
  - The app can be installed to the home screen.

## Browser support

| Feature | Needs |
| --- | --- |
| Editing and image export | Any current Chrome, Edge, Safari or Firefox |
| Video export | WebCodecs with H.264: Chrome or Edge 94+, Safari 16.4+. Browsers without H.264 fall back to VP9, which Instagram may reject |
| Cutouts | WebAssembly with SIMD. Single-threaded, so no special headers are needed |

iPhone HEVC (.mov) clips play in Safari but may not decode in Chrome on desktop. Exported videos are silent; add music when posting.

## Storage

Trips, photos, videos and uploaded fonts are kept in the browser's IndexedDB on that device. Clearing site data removes them. The app asks the browser for persistent storage after your first export.

## Third-party code

See `THIRD_PARTY_NOTICES.md`.
