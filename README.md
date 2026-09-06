# Xây dựng Hệ thống Phát hiện Dấu hiệu Buồn ngủ khi Lái xe qua Webcam (DrowsyAlert: A CNN-based Driver Drowsiness Detection System)

## DrowsyAlert

A CNN-based Driver Drowsiness Detection System.

## Requirements

- Python >= 3.12
- uv
- NVIDIA GPU + compatible driver (recommended)

## Installation

### 1. Clone repository and install uv

```powershell
git clone <repository-url>
cd drowsyalert
powershell -c "irm https://astral.sh/uv/install.ps1 | more
```

### 2. Install dependencies

```powershell
uv sync
```

### 3. run the application

```powershell
uv run python main.py
```

# Folder Structure

```DrowsyAlert/
│
├── pyproject.toml
├── uv.lock
├── README.md
├── .gitignore
│
├── src/
│   └── drowsyalert/
│       ├── __init__.py
│       │
│       ├── detection/
│       │   ├── __init__.py
│       │   ├── face_detector.py
│       │   ├── face_mesh.py
│       │   └── eye_landmarks.py
│       │
│       ├── features/
│       │   ├── __init__.py
│       │   ├── eye_aspect_ratio.py
│       │   ├── perclos.py
│       │   ├── yawn.py
│       │   └── head_pose.py
│       │
│       ├── models/
│       │   ├── __init__.py
│       │   ├── eye_cnn.py
│       │   ├── train.py
│       │   ├── evaluate.py
│       │   └── predict.py
│       │
│       ├── pipeline/
│       │   ├── __init__.py
│       │   ├── realtime.py
│       │   ├── drowsiness_detector.py
│       │   └── alert_manager.py
│       │
│       ├── experiments/
│       │   ├── __init__.py
│       │   ├── baseline_perclos.py
│       │   ├── perclos_yawn.py
│       │   ├── perclos_head_pose.py
│       │   └── compare_methods.py
│       │
│       └── utils/
│           ├── __init__.py
│           ├── config.py
│           ├── logger.py
│           ├── metrics.py
│           ├── visualization.py
│           └── video_utils.py
│
├── scripts/
│   ├── prepare_dataset.py
│   ├── train_model.py
│   ├── evaluate_model.py
│   ├── collect_data.py
│   └── run_webcam.py
│
├── data/
│   ├── raw/
│   │   └── .gitkeep
│   │
│   ├── processed/
│   │   └── .gitkeep
│   │
│   └── volunteer/
│       └── .gitkeep
│
├── models/
│   ├── checkpoints/
│   │   └── .gitkeep
│   │
│   └── exported/
│       └── .gitkeep
│
├── configs/
│   ├── config.yaml
│   └── thresholds.yaml
│
├── notebooks/
│   ├── 01_explore_dataset.ipynb
│   ├── 02_preprocess_data.ipynb
│   ├── 03_train_eye_cnn.ipynb
│   ├── 04_evaluate_model.ipynb
│   └── 05_compare_methods.ipynb
│
├── experiments/
│   ├── results/
│   │   └── .gitkeep
│   ├── figures/
│   │   └── .gitkeep
│   └── logs/
│       └── .gitkeep
│
├── demo/
│   ├── app.py
│   └── assets/
│       └── alert.wav
│
├── tests/
│   ├── __init__.py
│   ├── test_eye_aspect_ratio.py
│   ├── test_perclos.py
│   ├── test_yawn.py
│   ├── test_head_pose.py
│   └── test_drowsiness_detector.py
│
└── docs/
    ├── architecture.md
    ├── methodology.md
    ├── dataset.md
    └── experiments.md
```

### Roles based on the folder structure:

```
src/drowsyalert/
│
├── detection/       → Phát hiện mặt + landmark
├── features/        → EAR, PERCLOS, ngáp, head pose
├── models/          → CNN mắt mở/nhắm
├── pipeline/        → Ghép toàn bộ thành hệ thống realtime
├── experiments/     → 3 phương pháp nghiên cứu
└── utils/           → Các hàm dùng chung

data/                → Dataset
models/              → Model đã train
configs/             → Ngưỡng và cấu hình
notebooks/           → Khám phá/thử nghiệm
scripts/             → Các chương trình chạy từ command line
experiments/         → Kết quả thực nghiệm
demo/                → Ứng dụng webcam cuối cùng
tests/               → Unit tests
docs/                → Tài liệu dự án
```

### Workflow

```
                    ┌─────────────────┐
                    │     Webcam      │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │    detection    │
                    │ Face + Landmarks │
                    └────────┬────────┘
                             ↓
              ┌──────────────┼──────────────┐
              ↓              ↓              ↓
          Eye/CNN          PERCLOS       Head Pose
              │              │              │
              └──────────────┼──────────────┘
                             ↓
                    ┌─────────────────┐
                    │ Drowsiness      │
                    │ Detector        │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │ Alert Manager   │
                    └────────┬────────┘
                             ↓
                       🔊 Cảnh báo
```
