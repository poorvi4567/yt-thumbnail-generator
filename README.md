# yt-thumbnail-generator

A collection of Python prototypes for generating YouTube-ready thumbnails and related media assets from videos/images using Streamlit, OpenCV, FFmpeg, Whisper, DeepFace, and background-removal/enhancement workflows.

## What this repository contains

This repo is **not a single packaged app**; it is a set of experimental tools/scripts covering:
- keyframe extraction from videos
- transcript/keyword-based frame selection
- emotion-based frame selection
- image branding (text + logo overlay)
- image simplification/minimalistic styling
- sticker generation via background removal
- YouTube metadata + thumbnail extraction experiments

## Tech stack

- Python 3
- Streamlit (multiple UIs)
- OpenCV, Pillow, NumPy
- FFmpeg (system dependency)
- Whisper (speech-to-text)
- NLTK (keyword extraction)
- DeepFace (emotion detection)
- MoviePy (video text template)
- rembg (background removal)
- pytube + YouTube Data API (video metadata/downloading)

## Setup

### 1) Clone and move into the project
```bash
git clone <repo-url>
cd yt-thumbnail-generator
```

### 2) Create and activate a virtual environment
```bash
python -m venv env
source env/bin/activate   # macOS/Linux
# env\\Scripts\\activate    # Windows
```

### 3) Install dependencies
```bash
pip install -r requirements.txt
```

### 4) Install FFmpeg (required by multiple scripts)
- macOS: `brew install ffmpeg`
- Ubuntu/Debian: `sudo apt-get install ffmpeg`
- Windows: install from ffmpeg.org and add to PATH

### 5) Configure environment variables (if using API-backed scripts)
Create `.env` in project root:
```env
YT_DATA_API_KEY=your_youtube_data_api_key
HUGGING_FACE_API=your_huggingface_token
```

## How to run

Most tools are Streamlit apps:
```bash
streamlit run <script_name.py>
```

Examples:
```bash
streamlit run gui.py
streamlit run frame_selector.py
streamlit run engagementanalyse.py
streamlit run src/keyframe-generator.py
```

Some files are plain scripts and run directly:
```bash
python audio.py
python transcript.py
python test.py
python hugfaceenhancement.py
```

## File-by-file guide

### Root scripts/apps

- `adik.py` — Streamlit demo that extracts keywords from title/description, generates a placeholder image from keywords, and overlays text.
- `adikog.py` — Streamlit image branding tool (text + remote logo overlay + download).
- `audio.py` — CLI helper script: extract audio via FFmpeg and transcribe with Whisper, printing segment timestamps.
- `emotion-detection.py` — Streamlit image emotion detection using DeepFace.
- `emotion-detection-video.py` — Streamlit video emotion analysis; samples frames and keeps top-scoring frame per emotion.
- `engagementanalyse.py` — Streamlit app for YouTube URL analysis (metadata via YouTube Data API) and timestamp-based thumbnail extraction.
- `frame_selector.py` — Streamlit app to capture frames at user-entered timestamps.
- `gui.py` — Streamlit keyframe extraction using FFmpeg scene-change detection.
- `hugfaceenhancement.py` — Direct script calling Hugging Face ESRGAN inference API for image upscaling.
- `keyframe.py` — Cloudinary-based thumbnail generation experiment from uploaded video.
- `navigation.py` — Streamlit navigation/UI prototype with custom styling (`style.css`).
- `poorvi.py` — Streamlit minimalistic image generator using resize + k-means color reduction + filtering.
- `simpli.py` — Streamlit background-removal app using `rembg`.
- `sticker.py` — Streamlit sticker maker (background removal + fixed-size output).
- `test.py` — Non-UI script to extract keyframes from a local video using FFmpeg.
- `teststicker.py` — Alternate background-removal Streamlit app.
- `textoverlay.py` — MoviePy-based video template generator that renders title + description text.
- `transcript.py` — Streamlit app: keyword extraction from metadata + Whisper transcript + key moment frame extraction.

### `src/`

- `src/keyframe-generator.py` — Streamlit multi-mode keyframe tool:
  - emotion-detection mode (DeepFace + face detection)
  - subtitle keyword mode (Whisper + NLTK)
  - ranking by sharpness + detected faces

### Frontend/static files

- `autobrand.html` — standalone HTML canvas proof-of-concept for text and logo overlay.
- `style.css` — custom style sheet used by `navigation.py`.

### Configuration/support files

- `requirements.txt` — pinned Python dependencies.
- `.gitignore` — ignores `env/`, `.env`, generated thumbnails/keyframes folders.

### Media/test assets

- `videos/` — sample videos used for experimentation (multiple `.mp4`/`.mov` files).
- `videos/videos.txt` — ffmpeg concat-style listing of vlog clips.
- `temp_keyframe-tes.mp4`, `temp_output.mp4`, `temp_zoo.mp4` — temporary/generated video artifacts.

## Notes and caveats

- Several scripts are experimental and may require extra packages not pinned in `requirements.txt` (for example `deepface`, `rembg`, `scenedetect`, `cloudinary`).
- Some scripts assume local files/hardcoded paths (especially `hugfaceenhancement.py`) and may need path cleanup before production use.
- API-backed scripts require valid credentials in `.env`.
- This repository currently has no unified test suite or packaging structure.

## Suggested next cleanup steps

- Consolidate overlapping Streamlit apps into one modular app.
- Move reusable logic into a common `src/` package.
- Remove hardcoded secrets/paths and centralize configuration.
- Add a single entrypoint, basic tests, and CI.
