# Live Emotion Detector

A browser-based real-time facial emotion detection web app. Turn on your webcam and the app will analyze your facial expressions on the fly, displaying dominant emotion, confidence scores, and a live breakdown of all detected emotions — all running entirely client-side with no backend required.

---

## Features

- **Real-time webcam feed** — live camera window with a canvas overlay showing the detected face
- **7 emotion classes** — neutral, happy, sad, angry, fearful, disgusted, surprised
- **Dynamic accent color** — the UI theme shifts based on the currently detected emotion
- **Confidence breakdown** — live bar chart showing the probability of each emotion
- **Most prominent face** — when multiple faces are in frame, the app uses the most prominent one
- **No backend** — all inference runs in the browser using face-api.js
- **Responsive** — works on desktop and mobile browsers
- **Start / Stop / Reset** controls for full camera session management

---

## How It Works

### 1. Accessing the Webcam Feed

When the user clicks **Start Camera**, the app calls the browser's `getUserMedia` API to request access to the device webcam. A video element displays the live stream in real time. A transparent canvas overlay is placed directly on top of the video at the exact same dimensions.

### 2. Detecting the Face

Every ~140 milliseconds (throttled to approximately 7 FPS for performance), the app sends the current video frame to `face-api.js` with the **TinyFaceDetector** model. This model scans the frame for faces and returns a bounding box around each detected face.

If no face is found, the UI shows a "No face detected" status and the panel resets.

If multiple faces are found, the app selects the face with the largest bounding box (most prominent in the frame) and proceeds with that one alone.

### 3. Analyzing Facial Expressions

Once a face is located, `face-api.js` runs **facial landmark detection** (68-point tiny model) to map the geometry of the face — eyes, eyebrows, nose, mouth, and jawline.

These landmarks are then fed into the **FaceExpressionNet** model, which outputs a probability distribution across all 7 emotion classes. Each emotion receives a value between 0 and 1 representing the model's confidence that the given expression matches that emotion.

### 4. Choosing the Dominant Emotion

The app iterates over all 7 emotion probabilities and selects the one with the highest value as the **dominant emotion**. This is what is shown prominently in the UI alongside its confidence percentage.

### 5. Displaying Results

- The **canvas overlay** draws a colored bounding box around the detected face with the emotion label above it
- The **emotion panel** updates all 7 bar fills in real time with each emotion's probability
- The **dominant emotion** name and confidence are displayed at the top of the panel
- The **accent color** of the entire interface transitions smoothly to match the current emotion
- The **status bar** indicates whether a face is detected, multiple faces are present, or no face is in frame

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Vanilla HTML, CSS, JavaScript |
| Face Detection | [face-api.js](https://github.com/vladmandic/face-api) (TinyFaceDetector) |
| Landmark Detection | face-api.js (faceLandmark68TinyNet) |
| Expression Analysis | face-api.js (FaceExpressionNet) |
| Model Hosting | [@vladmandic/face-api](https://cdn.jsdelivr.net/npm/@vladmandic/face-api@1.7.12/model) via jsDelivr CDN |
| Icons | None required (pure CSS/HTML) |

---

## Installation

### Prerequisites

- A modern browser (Chrome, Edge, or Firefox)
- A device with a webcam
- Node.js (optional, for serving locally)

### Option 1 — Open Directly

1. Clone or download the repository
2. Open `emotion-detector.html` in your browser

> **Note:** Camera access requires the page to be served over `localhost` or `https`. Opening via `file://` will not work due to browser security restrictions on `getUserMedia`.

### Option 2 — Local Server (Recommended)

```bash
# Using npx (Node.js required)
cd Emotion-Tracker
npx serve -l 3000
```

Then open **http://localhost:3000** in Chrome or Edge.

### Option 3 — VS Code Live Server

1. Open the project in VS Code
2. Install the **Live Server** extension
3. Right-click `emotion-detector.html` and select **"Open with Live Server"**

### Option 4 — GitHub Pages

The project is deployed on GitHub Pages at:

**https://ArvanJ.github.io/emotion-detector**

> **Note:** Some browsers may restrict camera access on third-party hosted domains. If the camera does not work on GitHub Pages, use a local server instead.

---

## Usage

1. Open the app in your browser
2. Click **Start Camera**
3. Allow camera permissions when prompted
4. Position your face in the camera frame
5. Watch the emotion panel update in real time
6. Click **Stop Camera** to end the session
7. Click **Reset** to clear the display

---

## Project Structure

```
Emotion-Tracker/
├── emotion-detector.html   # Complete single-file web app
├── .gitignore             # Git ignore rules
├── README.md              # This file
└── .git/                  # Git repository
```

---

## Privacy Note

**All webcam processing happens locally in your browser.** No video data, images, or facial analysis results are ever sent to a server or third party. The app does not store, record, or transmit any personal data. Once you close the tab or stop the camera, all session data is gone.

---

## Limitations

Emotion detection via facial expressions is **approximate and probabilistic**, not a definitive measurement of someone's true feelings. The model identifies visual patterns in facial muscle movements that correlate with certain emotional expressions, but:

- Results vary significantly with **lighting conditions** and **camera quality**
- Expressions can be **culturally variable** — the same gesture may carry different meanings across cultures
- People can **fake or suppress** emotions deliberately
- The model was trained on a specific dataset and may not generalize equally well across all demographics
- Emotions like "disgusted" and "angry" are often confused by these models

**This app is intended for educational, portfolio, and demo purposes only. Do not use it for clinical, hiring, security, or any decision-making applications where accuracy matters.**

---

## Future Improvements

- [ ] Add emotion history tracking with a graph over time
- [ ] Save emotion snapshots to local storage for journaling
- [ ] Support for multiple faces displayed simultaneously
- [ ] Audio-based sentiment analysis as a secondary input
- [ ] Deployment-ready Docker container for one-command local hosting
- [ ] Dark/Light theme toggle
- [ ] Add language support (i18n)

---

## Screenshots

> _Add screenshots here showing the app running with different emotions detected._

| Happy Detection | Sad Detection |
|-----------------|---------------|
| _(screenshot)_  | _(screenshot)_ |

---

## Contributing

Contributions are welcome! If you'd like to improve this project:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature-name`)
3. Make your changes
4. Commit and push to your fork
5. Open a Pull Request

Please open an issue first to discuss major changes.

---

## License

No explicit license provided. All rights reserved by the author.

---

## Credits

Built with [face-api.js](https://github.com/vladmandic/face-api) by Vladimira Mandic.

Developed by **Arvan Jindam** as a browser-based ML portfolio project.
