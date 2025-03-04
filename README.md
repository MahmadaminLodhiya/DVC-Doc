# Dataset Versioning with DVC for YOLO Training

## Introduction
This guide explains how to use **Data Version Control (DVC)** to manage datasets for YOLO training. DVC allows efficient tracking of dataset versions locally without storing large files in Git.

---

## 1. Prerequisites
Ensure you have the following installed:

- Python (>=3.7)
- Git
- DVC
- YOLO (e.g., Ultralytics YOLOv8)

To install DVC:
```bash
pip install dvc
```

---

## 2. Install and Initialize DVC
Run the following inside your **YOLO project folder**:
```bash
git init
dvc init
git commit -m "Initialize DVC for local dataset tracking"
```

---

## 3. Add Your Dataset to DVC
Assume your dataset is stored in `datasets/my-yolo-dataset/`.

Run:
```bash
dvc add datasets/my-yolo-dataset
```
This:
- Moves the dataset to **DVC cache** (`.dvc/cache`).
- Creates a **metadata file**: `datasets/my-yolo-dataset.dvc`.

Now, commit this change to Git:
```bash
git add datasets/my-yolo-dataset.dvc .gitignore
git commit -m "Track dataset with DVC"
```

---

## 4. Track Dataset Changes Locally
Each time you update the dataset (e.g., adding new images), run:
```bash
dvc add datasets/my-yolo-dataset
git add datasets/my-yolo-dataset.dvc
git commit -m "Updated dataset version"
```
This updates the dataset version **without duplication**.

To see what changed:
```bash
dvc status  # Check untracked changes
dvc diff HEAD^  # Compare current and previous dataset versions
```

---

## 5. Restore Previous Versions
To switch to an older dataset version:
```bash
git checkout <commit-hash>
dvc checkout
```
This restores the dataset to the version stored in that commit.

---

## 6. Train YOLO with Versioned Data
Each time you train YOLO, ensure you have the correct dataset:
```bash
dvc checkout  # Ensure dataset is in place
python train.py --data datasets/my-yolo-dataset --epochs 50
```

After training, **version the trained model**:
```bash
dvc add runs/train/exp/weights/best.pt  # Track YOLO model
git add runs/train/exp/weights/best.pt.dvc
git commit -m "Versioned YOLO trained model"
```
Now you can rollback to older **trained model versions** anytime.

---

## 7. Use DVC Pipeline (Optional)
To automate dataset processing and model training:
```bash
dvc run -n train_yolo \
    -d datasets/my-yolo-dataset \
    -d train.py \
    -o runs/train/exp/weights/best.pt \
    "python train.py --data datasets/my-yolo-dataset --epochs 50"
```
Now, you can run:
```bash
dvc repro train_yolo  # Runs only if dataset changes
```

---



