# Setup Guide (kellieja/Clipping-)

This repo is a copy of [NaufalRizqullah/opensource-clipping](https://github.com/NaufalRizqullah/opensource-clipping) (MIT license), with full upstream history. See `README.md` for the complete feature list and every CLI flag.

## What you need

| Item | Required? | Where to get it |
|---|---|---|
| Python 3.10–3.12 (3.12 recommended) | Yes | python.org |
| FFmpeg on your PATH | Yes | ffmpeg.org (`brew install ffmpeg` / `apt install ffmpeg`) |
| `GOOGLE_API_KEY` (Gemini) | Yes | https://aistudio.google.com/apikey |
| NVIDIA GPU with CUDA | Strongly recommended | Without one, transcription is very slow. Use Colab or Kaggle instead (see below) |
| `PEXELS_API_KEY` | Optional, for B-roll | https://www.pexels.com/api/ |
| `HF_TOKEN` | Optional, for split-screen and camera-switch | https://huggingface.co/settings/tokens. Also accept the [pyannote agreement](https://huggingface.co/pyannote/speaker-diarization-3.1) |

## Option A: Run locally

```bash
git clone https://github.com/kellieja/Clipping-.git
cd Clipping-

python3.12 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

cp .env.sample .env                # then open .env and paste your GOOGLE_API_KEY
```

Make your first clips:

```bash
python main.py --url "https://youtube.com/watch?v=VIDEO_ID" --clips 3 --ratio 9:16
```

Finished clips, thumbnails, and metadata go to `outputs/`.

On a CPU-only machine, add `--whisper-compute-type float32` (and expect it to be slow).
If YouTube blocks the download (403 / bot check), add `--yt-cookies chrome`.

## Option B: Google Colab or Kaggle (free GPU, no local setup)

Open `notebooks/Quick_Start.ipynb` in Colab and set Runtime to **T4 GPU**.
Wherever the notebook clones the repo, change the URL to
`https://github.com/kellieja/Clipping-.git`.
Store `GOOGLE_API_KEY` in Colab Secrets.

## Option C: Web dashboard with Docker

```bash
cp .env.sample .env    # fill in keys
docker compose up --build
```

- API: http://localhost:8000/docs
- Dashboard: http://localhost:5173

## Check your install

```bash
pip install -r requirements-test.txt
python -m pytest -q          # 164 offline tests; no GPU or API keys needed
python main.py --help
```

## Getting upstream updates

```bash
git remote add upstream https://github.com/NaufalRizqullah/opensource-clipping.git
git fetch upstream
git merge upstream/main
```
