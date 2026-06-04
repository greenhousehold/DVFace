# FACEWARE — dorsal/ventral face overlay

A single-file web app that uses your device camera and **face-api.js** to paint your
face into anatomical **dorsal (white) / ventral (black)** zones in real time —
theatrical-makeup style — plus a few animated effect modes.

No build step. Everything is in `index.html`; the ML model weights are vendored in
`weights/` so it works offline.

## Run it

Any static file server works. The camera requires a secure context, so use
`localhost` (or HTTPS) — opening the file directly with `file://` will not grant
camera access.

```bash
# from this folder
npx serve -p 8743
# then open http://localhost:8743 and allow camera access
```

(or `python3 -m http.server 8743`)

## Modes

| Mode | Effect |
|------|--------|
| **PURE** | The plain dorsal/ventral two-tone split |
| **CLOUDS** | Puffy white marshmallow clouds over the dorsal zone, drifting beyond the head |
| **RAYS** | Warm light radiating from the face center |
| **DRIP** | Black wetness oozing down off the lower face |

The **opacity** slider blends the whole overlay; **FLIP** switches the front/back camera.

## How it works

- Detection: `TinyFaceDetector`; landmarks: the full `faceLandmark68Net` (more
  accurate than the tiny variant).
- `faceGeom()` computes the zone polygons once from the 68 landmarks and exposes
  them as paintable layers + dorsal/ventral region masks. Each mode renderer
  (`renderPure` / `renderClouds` / `renderRays` / `renderDrip`) reuses that geometry,
  compositing on offscreen buffers and blitting once through the opacity slider.

## Models

Vendored in `weights/` (the jsdelivr `@0.22.2/weights/` CDN path 404s, so the files
are committed here):

- `tiny_face_detector_model-*`
- `face_landmark_68_model-*`
