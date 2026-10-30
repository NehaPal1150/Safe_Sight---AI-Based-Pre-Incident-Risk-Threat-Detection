# SafeSight AI — Pre-Incident Risk & Threat Detection

SafeSight AI is a real-time video surveillance intelligence system that detects potential threats before incidents escalate. It combines object detection, multi-object tracking, self-supervised video representations and behavioral analysis into a single risk score, streamed live to a monitoring dashboard.

## Architecture

```
safesight-ai/
├── ai_engine/
│   ├── detection/          # YOLOv8 object detection
│   ├── tracking/           # ByteTrack multi-object tracker
│   ├── behaviour/          # Loitering, crowd, running detectors
│   ├── ssl_encoder/        # SimCLR encoder + temporal autoencoder
│   ├── threat_classifier/  # Threat classifier (train / retrain)
│   └── risk_engine/        # Risk score calculation
├── backend/                # FastAPI REST + SSE API
├── dashboard/              # Streamlit live monitoring UI
├── utils/                  # Video capture & frame helpers
├── notebooks/              # Training notebooks, trained models, YOLO val results
├── requirements.txt
└── main.py                 # Pipeline entry point
```

## Quick Start

1. Install dependencies
```bash
pip install -r requirements.txt
```

2. Run the AI pipeline (webcam)
```bash
python main.py --source 0 --display
```

3. Start the backend API
```bash
uvicorn backend.api:app --host 0.0.0.0 --port 8000 --reload
```

4. Launch the dashboard
```bash
streamlit run dashboard/streamlit_app.py
```

## Detection Capabilities

| Signal | Description |
|---|---|
| Object / Weapon Detection | YOLOv8-based detection |
| Loitering | Person stationary >15s in a zone |
| Running / Panic | High-speed movement, scatter patterns |
| Crowd Anomaly | Sudden gatherings, overcrowding |
| Night Activity | Darkness-adjusted risk multiplier |

## Threat Levels

| Level | Score Range |
|---|---|
| None | 0–15% |
| Low | 15–35% |
| Medium | 35–60% |
| High | 60–80% |
| Critical | 80–100% |

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | /api/v1/incidents | List incidents |
| GET | /api/v1/incidents/{id} | Get incident details |
| PATCH | /api/v1/incidents/{id}/resolve | Resolve incident |
| GET | /api/v1/risk/summary | Current risk summary |
| GET | /api/v1/risk/trend | Risk score history |
| GET | /api/v1/stream/alerts | SSE real-time alert stream |
| GET | /api/v1/cameras | List cameras |
| POST | /api/v1/cameras | Register camera |

## Model Training

- `notebooks/dataset_preprocessing.ipynb`: dataset preparation
- `notebooks/yolo_training.ipynb`: YOLOv8 training and validation
- `notebooks/ssl_training.ipynb`: self-supervised encoder training
- `ai_engine/threat_classifier/retrain_classifier.py`: retrain the threat classifier

Trained weights are in `notebooks/models/`.

## Configuration

```python
ObjectDetector(confidence_threshold=0.45)
LoiteringDetector(loiter_threshold_seconds=15.0)
CrowdDetector(crowd_threshold=5)
RunningDetector(run_speed_threshold=80.0)  # pixels/second
```

## Tech Stack

Python, YOLOv8, ByteTrack, SimCLR (PyTorch), FastAPI, Streamlit, scikit-learn

## Author

Neha Pal | [LinkedIn](https://linkedin.com/in/neha-pal-b250a8250) | [GitHub](https://github.com/NehaPal1150)
