# 👁️ Eye-Controlled Mouse

A real-time, webcam-based eye-tracking application that lets you control your mouse cursor and perform clicks using only your eyes — no hands required.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Demo](#demo)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Known Limitations](#known-limitations)
- [License](#license)

---

## Overview

Eye-Controlled Mouse uses **MediaPipe Face Mesh** to detect facial landmarks in real time via your webcam. It maps the position of your right eye iris to move the mouse cursor across the screen, and detects a blink of your left eye to trigger a mouse click — all through an intuitive **PyQt5 GUI**.

This project is ideal as an accessibility tool or a demonstration of real-time computer vision with Python.

---

## ✨ Features

- 🎯 **Real-time eye tracking** using MediaPipe Face Mesh (468+ landmarks)
- 🖱️ **Mouse cursor control** mapped from iris position to screen coordinates
- 👁️ **Blink-to-click** detection using left eye landmark distance
- 🎚️ **Adjustable sensitivity** via an interactive slider
- ⏸️ **Pause / Resume** tracking at any time
- ✅ **Enable / Disable** mouse control toggle (view-only mode)
- 🖥️ **Live webcam feed** rendered inside the GUI with landmark overlays
- 🎨 **Dark-themed GUI** built with PyQt5

---

## Requirements

- Python 3.8+
- Webcam

### Python Dependencies

```
opencv-python
mediapipe
pyautogui
numpy
PyQt5
```

---

## 🔧 Installation

1. **Clone the repository**

```bash
git clone https://github.com/yourusername/eye-controlled-mouse.git
cd eye-controlled-mouse
```

2. **Create a virtual environment (recommended)**

```bash
python -m venv venv
source venv/bin/activate        # Linux/macOS
venv\Scripts\activate           # Windows
```

3. **Install dependencies**

```bash
pip install opencv-python mediapipe pyautogui numpy PyQt5
```

---

## 🚀 Usage

```bash
python eye.py
```

The application window will open. Allow webcam access when prompted.

| Action | How |
|---|---|
| Move cursor | Look in any direction |
| Click | Blink your **left** eye |
| Pause tracking | Click **⏸ Pause Eye-Mouse** button |
| Adjust sensitivity | Drag the **Sensitivity** slider |
| Disable mouse control | Uncheck **Enable mouse control** |
| Exit | Close the window |

---

## ⚙️ How It Works

### Eye Tracking (Cursor Movement)

MediaPipe's Face Mesh detects **iris landmarks 474–477** for the right eye. The center of the iris (landmark index 1 within this range) is mapped to screen coordinates using:

```
screen_x = screen_width  × landmark.x × sensitivity
screen_y = screen_height × landmark.y × sensitivity
```

`pyautogui.moveTo()` is then called to reposition the cursor.

### Blink Detection (Click)

Two specific landmarks on the **left eye** are tracked:
- Landmark **145** — lower eyelid
- Landmark **159** — upper eyelid

When the vertical distance between them falls below a threshold (`0.004` in normalized coordinates), a blink is detected and `pyautogui.click()` is triggered, followed by a 0.5-second cooldown to prevent double-clicks.

### GUI (PyQt5)

The interface is built with `QWidget`, uses a `QTimer` at 1 ms intervals to poll the webcam, and emits frames via a `pyqtSignal` for thread-safe rendering in the `QLabel` video panel.

---

## 🎛️ Configuration

| Parameter | Location | Default | Description |
|---|---|---|---|
| Sensitivity | Slider (20–200) | 100 | Scales cursor movement range |
| Blink threshold | `eye.py` line ~95 | `0.004` | Adjust for blink sensitivity |
| Timer interval | `eye.py` line ~76 | `1 ms` | Controls frame rate (~30–60 FPS) |
| Click cooldown | `eye.py` line ~97 | `0.5 s` | Prevents accidental double-clicks |

---

## 📁 Project Structure

```
eye-controlled-mouse/
│
├── eye.py          # Main application file
└── README.md       # This file
```

---

## ⚠️ Known Limitations

- **Lighting sensitivity**: Works best in well-lit environments. Poor lighting can cause landmark detection failures.
- **Single face**: Only the first detected face is used. Multiple faces may cause undefined behavior.
- **Blink threshold**: The fixed threshold (`0.004`) may need adjustment for different users and distances from the camera.
- **Screen mapping**: Sensitivity scaling is linear and does not account for screen edges — the cursor may not reach corners at default sensitivity.
- **Platform support**: Tested primarily on Windows. `pyautogui` behavior may differ slightly on macOS/Linux.

---

## 📄 License

This project is licensed under the MIT License. See `LICENSE` for details.

---

> Built with ❤️ using Python, MediaPipe, OpenCV, and PyQt5.
