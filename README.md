# LipStream

### Visual speech recognition from silent English video

LipStream is a team graduation project for the Artificial Intelligence program at Umm Al-Qura University. It explores transcribing short English video clips using visual information rather than audio, with a PyTorch sequence model and a Flask inference API.

[Watch the recorded demo](./LipStream-demo.mp4) · [Model architecture](./models/lipreading_model.py) · [Inference API](./inference.py) · [Original team repository](https://github.com/loa24/LipStream)

## System overview

```text
Video upload -> frame extraction -> region cropping
             -> ResNet-18 -> BiLSTM -> CTC decoding -> transcript
```

- **Visual features:** a ResNet-18 backbone encodes cropped video frames.
- **Temporal modeling:** a two-layer bidirectional LSTM models the frame sequence.
- **Decoding:** greedy CTC decoding converts token predictions into text.
- **Application:** Flask exposes prediction and health endpoints; an HTML/CSS/JavaScript interface presents the interaction.

The current inference implementation loads `yolov8n.pt`, takes the first detected box, and crops its lower portion, with a fixed-region fallback when no box is detected. This is a heuristic crop, not a verified dedicated mouth detector.

## Project-reported evaluation

The team reported the following results on GRID:

| Metric | Reported value |
| :--- | ---: |
| Word accuracy | 72.10% |
| Word Error Rate (WER) | 25.83% |

These are recorded project results, not a fresh benchmark of this checkout. The two metrics are retained as separately reported measures; their definitions and evaluation configuration should be checked before comparing them or treating them as complements. The project received an A+ grade (100/100).

## Repository guide

| Path | Purpose |
| :--- | :--- |
| `models/` | PyTorch model architecture |
| `training/` | Training and evaluation scripts |
| `datasets/` | Dataset-related code |
| `scripts/` | Utilities, including vocabulary preparation |
| `utils/` | Shared helpers and tokenizer |
| `inference.py` | Flask API and video preprocessing |
| `lipstream-integrated.html` | Frontend interface |
| `LipStream-demo.mp4` | Recorded demonstration |

## Local setup and required assets

This is a research prototype, not a one-command deployment. The API loads its assets during startup and expects these paths relative to the repository root:

- `checkpoints/lipreading_epoch_037.pth`: a compatible trained model checkpoint.
- `data/vocab.txt`: the vocabulary used with that checkpoint.
- `yolov8n.pt`: the YOLO weights referenced by the current inference code.

The checkpoint and vocabulary are not included in the root file listing. Obtain the matching project assets from the team or reproduce them using the training pipeline; do not substitute an unrelated checkpoint.

The code imports PyTorch, torchvision, OpenCV, NumPy, Pillow, Flask, Flask-CORS, and Ultralytics. FFmpeg is also required for the implemented non-MP4 conversion path. `requirements.txt` is an environment export containing machine-local `file://` references, so it is not a portable installation lockfile. A clean, validated dependency file remains necessary for reproducible setup.

Once dependencies and matching assets are available, run from the repository root:

```bash
python inference.py
```

The script starts a Flask server on port `5001`; it does not implement a `--video` command-line option. The API defines:

- `GET /health`: server and device status, available after successful model loading.
- `POST /predict`: multipart form upload with the field name `video`.

Setup instructions are based on source inspection and have not been end-to-end validated in a clean environment. The server binds to all interfaces in the current code; use an isolated local environment and do not expose it publicly without security hardening.

## Scope and limitations

This prototype targets short English clips and was evaluated in the project's GRID setting. Results should not be assumed to generalize to unrestricted videos, other languages, or different speakers and recording conditions. The returned confidence score is a mean token probability, not a calibrated accuracy guarantee; displayed word timings are evenly spaced estimates rather than forced alignment.

## Team

- Jumana Abdulrazaq Alharbi
- Lamar Omar Ageel
- Renad Muhammad Alshamrani
- Sama Hassan Masmali

**Supervisor:** Dr. Huda Alhazmi

This fork presents collaborative graduation work and preserves the connection to the [original team repository](https://github.com/loa24/LipStream).
