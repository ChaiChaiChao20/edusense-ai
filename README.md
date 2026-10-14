# EduSense AI — Classroom Signal Lab

An on-device, browser-based research prototype based on the EduSense review brief. It accepts a live camera or a local video file and reports anonymous, aggregate face visibility, approximate head/eye orientation support, sustained eye-closure cues, and visible raised-hand cues over time. A face landmark model and a pose landmark model provide the two signal streams.

## Run

1. Keep an internet connection available the first time you open the app. The browser loads the pinned MediaPipe runtime and WebAssembly files. The bundled face and pose model assets are in `models/`.
2. From this directory, start any static HTTP server, for example:

   ```powershell
   py -m http.server 8000
   ```

3. Open `http://localhost:8000` in a current Chrome or Edge browser.
4. Choose **Start camera**, **Analyze a video**, or **Run synthetic demo**. Camera permission is required only for the first option. Use **Stop session** to end processing; **Export summary** saves aggregate JSON.

Opening `index.html` directly may block model and camera access. A localhost server avoids that browser restriction. Video and camera inference run in the page using MediaPipe Face Landmarker. The CDN/runtime version is pinned in `app.js`; the model file is sourced from the Google AI Edge model host.

## What it measures

- Current visible-face count, with anonymous short-lived tracks used only to smooth eye-closure events.
- A rough orientation-support percentage for faces presently visible, derived from eye openness, iris position and head geometry relative to a short neutral baseline.
- Long eye-closure events, triggered after 1.2 seconds of sustained closure.
- Visible wrists above their shoulder line, shown as a raised-hand cue; this is a posture landmark rule, not a classroom intent classifier.
- A sampled timeline and an optional aggregate session JSON export.

The timeline is a prototype signal, not an engagement classifier. It does not infer emotion, cognition, learning, intent, fatigue or ability. There is no student recognition, enrollment, face database, backend, image export or recording. The synthetic mode is explicitly labeled and is not model output.

## Privacy and use boundaries

The browser analyzes frames locally; no class footage is uploaded to an EduSense server or written to disk. Session landmarks and tracks remain in tab memory and are discarded at stop/close. The MediaPipe runtime and model are downloaded from Google/JSDelivr. MediaPipe documents that input processing occurs on-device and that its APIs send performance/usage metrics; see its [privacy notice](https://ai.google.dev/edge/mediapipe/solutions/guide#privacy). Obtain informed consent and follow local rules before any classroom use. Do not use this prototype for grading, discipline, diagnosis, attendance or other high-stakes decisions.

## Scope and limitations

This is a practical functional prototype, not a validated educational measurement model. The supplied review PDF contains a proposal and objectives, not a labeled dataset, model weights, or implementation source. Consequently, this repository reports no accuracy/FPS/generalization claims. Orientation, eye-closure and raised-hand thresholds are heuristic and have not been calibrated with a representative student population. The face and pose tasks each return up to six landmarks, but crowded, occluded, distant, profile, low-light or otherwise difficult views can reduce performance. The current build does not classify classroom actions or affect and does not implement a trained temporal fusion model; the UI keeps those signals visible separately to avoid implying a cognitive score.

## Attribution

MediaPipe Tasks Vision is distributed under Apache-2.0. This prototype uses its public Face Landmarker runtime and model. The model and WebAssembly files are retrieved from public provider endpoints; the application source is the project team's implementation.

## References

- [MediaPipe Face Landmarker for Web](https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker/web_js)
- [MediaPipe Tasks Vision package and privacy notice](https://www.npmjs.com/package/@mediapipe/tasks-vision)
- EduSense AI IDP Review 1 (provided project brief; not included here because it contains personal identifiers).
