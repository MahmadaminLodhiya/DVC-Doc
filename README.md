# Dataset Versioning with DVC for YOLO Training

## Introduction
This guide explains how to use **Data Version Control (DVC)** to manage datasets for YOLO training. DVC allows efficient tracking of dataset versions locally without storing large files in Git.

---

## 1. Installation
To install DVC:
```bash
pip install dvc
```

---

## 2. Initialize DVC
Run the following inside your **project folder**:
```bash
git init
dvc init
git commit -m "Initialize DVC for local dataset tracking"
```

---

## 3. Add Your Dataset to DVC
Assume your dataset is stored in `datasets/my-dataset/`.

Run:
```bash
dvc add datasets/my-dataset
```
This:
- Moves the dataset to **DVC cache** (`.dvc/cache`).
- Creates a **metadata file**: `datasets/my-dataset.dvc`.

Now, commit this change to Git:
```bash
git add datasets/my-dataset.dvc .gitignore
git commit -m "Track dataset with DVC"
```

---

## 4. Track Dataset Changes Locally
Each time you update the dataset (e.g., adding new images), run:
```bash
dvc add datasets/my-dataset
git add datasets/my-dataset.dvc
git commit -m "Updated dataset version"
```
This updates the dataset version **without duplication**.

To see what changed:
```bash
dvc status  # Check untracked changes
dvc diff HEAD^  # Compare current and previous dataset versions
```

---

## 5. Check Dataset Versions
To show all the dataset versions stored in Git:
```bash
git log --oneline
```
Each commit represents a different version of the dataset. To see a detailed history with commit messages and timestamps:
```bash
git log --pretty=format:"%h - %an, %ar : %s"
```
If you want to see differences between specific versions:
```bash
dvc diff <commit-hash-1> <commit-hash-2>
```
This helps in identifying changes made between dataset versions.

To list all the available dataset versions tracked by DVC:
```bash
dvc list . --dvc-only
```

## 6. Restore Previous Versions
To switch to an older dataset version:
```bash
git checkout <commit-hash>
dvc checkout
```
This restores the dataset to the version stored in that commit.

---





