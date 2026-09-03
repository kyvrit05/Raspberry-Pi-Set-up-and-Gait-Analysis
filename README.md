# Raspberry-Pi-Configuration-Set-up & Gait Analysis

## 🛠️ Hardware Troubleshooting & Fix (DeskPi Case & Pi 4)

These were the problems I encountered during setup and the troubleshooting steps I took are listed below: 

If your Raspberry Pi 4 inside a DeskPi case fails to boot and displays a **solid red LED** and a **repeating 4-flash green LED pattern**, the internal bootloader firmware has lost synchronization with the microSD card's partition table.

### Step-by-Step Resolution
1. **Isolate Hardware:** Remove the Pi 4 from the DeskPi case. Test it bare to rule out internal ribbon cable connection issues.
2. **Flash Recovery Firmware:** Open **Raspberry Pi Imager** on your computer. Go to `Choose OS` -> `Misc Utility Images` -> `Bootloader (Pi 4 family)` -> `SD Card Boot`. Flash this to your SD card.
3. **Reset the Pi:** Insert the card into the bare Pi and power it on. Wait until the green LED blinks in a **steady, continuous heartbeat pattern**, then unplug the power.
4. **Flash the OS:** Reinsert the card into your computer and use Raspberry Pi Imager to flash a fresh copy of **Raspberry Pi OS (64-bit)**. 
5. **Reassemble Carefully:** Put the Pi back into the DeskPi case. Ensure the internal microSD dummy adapter sits perfectly flush and the mounting screws are only finger-tight to avoid flexing the board and breaking pin contact.

---

## 🏃‍♂️ Gait Analysis & Model Training

The Raspberry Pi 4 (64-bit OS) serves as the primary edge computing node for this project. It is utilized to:
* Run the Python-based data pipelines to **analyse and simulate human gait patterns**.
* Preprocess streaming sensor data and **train the Random Forest Classifier model** directly on-device for real-time pattern recognition.

The model trained and the codes are included in the repository :) 

main: loads the data into a csv. file 

stimulate heatmap: Simulates footstep pressure grid data and renders it as a heatmap, so you can test/verify the Raspberry Pi display pipeline BEFORE the real mat hardware is wired up.

<p align="center">
  <img width="500" alt="Screenshot 2026-09-04 013007" src="https://github.com/user-attachments/assets/38296c91-1f68-4cec-b21a-3e0d23458048" />
</p>

## Gait Classification (GaitNDD Dataset)

Two Random Forest baselines for normal-vs-abnormal gait classification, trained on PhysioNet's [Gait in Neurodegenerative Disease Database](https://physionet.org/content/gaitndd/1.0.0/).

- **`train.py`** — classifies individual strides using raw gait-timing features (stride interval, swing %, stance %, etc.).
- **`train_subject_level.py`** — classifies whole subjects using aggregated variability features (mean/std/CV of stride timing, left-right asymmetry), evaluated with Leave-One-Subject-Out cross-validation.

Aggregating to subject-level variability features substantially outperforms per-stride classification, since gait pathology shows up more in stride-to-stride *inconsistency* than in any single stride's raw values.

### Project Outcome

| Metric | `train.py` (per-stride) | `train_subject_level.py` (per-subject) |
|---|---|---|
| Accuracy | 67% | 86% |
| ROC-AUC | 0.628 | 0.920 |
| Abnormal — Precision / Recall / F1 | 87% / 71% / 78% | 90% / 92% / 91% |
| Normal — Precision / Recall / F1 | 27% / 51% / 35% | 73% / 69% / 71% |

Moving from per-stride to per-subject classification improved overall accuracy by **19 percentage points** and nearly doubled ROC-AUC discrimination, confirming that gait variability — not raw stride timing — is the stronger signal for detecting abnormal gait in this dataset.
