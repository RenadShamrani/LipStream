# LipStream — Visual Speech Recognition System


LipStream is an end-to-end web-based system that transcribes short, silent English video clips into text by reading lip movement only (no audio). It was built as a graduation project for the Artificial Intelligence program at Umm Al-Qura University, and received an A+ grade (100/100).


## Demo


A recorded demo is included in the repository: `LipStream-demo.mp4`.


## How It Works


The pipeline processes an uploaded silent video clip in four stages:


1. **Mouth detection** — YOLOv8 locates and crops the mouth region from each frame.
2. **Visual feature extraction** — ResNet-18 encodes each cropped frame into a feature vector.
3. **Sequence modeling** — a Bidirectional LSTM (BiLSTM) processes the frame sequence over time.
4. **Decoding** — CTC decoding maps the sequence to the final text transcript.


A Flask web interface (HTML, CSS, JavaScript) handles video upload, validation, and transcription display.


## Results


Evaluated on the GRID dataset:


| Metric | Value |
|---|---|
| Word accuracy | 72.10% |
| Word Error Rate (WER) | 25.83% |


## Repository Structure


```
datasets/    Dataset loading and preprocessing
models/      Model architectures (YOLOv8 detection, ResNet-18 + BiLSTM encoder, CTC)
training/    Training and evaluation scripts
scripts/     Utility scripts (e.g., vocabulary building)
utils/       Shared helpers
inference.py Runs transcription on a video file
lipstream-integrated.html  Integrated web interface
requirements.txt  Python dependencies
```


## Getting Started


```bash
git clone https://github.com/RenadShamrani/LipStream.git
cd LipStream
pip install -r requirements.txt
python inference.py --video <path-to-video>
```


## Team

- Jumana Abdulrazaq Alharbi
- Lamar Omar Ageel
- Renad Muhammad Alshamrani
- Sama Hassan Masmali

**Supervisor:** Dr. Huda Alhazmi

Graduation project, Artificial Intelligence program, Umm Al-Qura University.

Original team repository: [loa24/LipStream](https://github.com/loa24/LipStream).
