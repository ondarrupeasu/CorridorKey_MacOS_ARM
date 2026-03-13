# CorridorKey GUI — macOS

A native macOS graphical interface for [CorridorKey](https://github.com/nikopueringer/CorridorKey), the neural green screen keyer by Corridor Digital.

Built for **Apple Silicon** (M1/M2/M3/M4). No terminal required after installation.

![Dark themed interface with file inputs, settings sliders, and progress bar](screenshot_placeholder.png)

---

## Features

- **One-click installer** — downloads and sets up CorridorKey automatically on first launch
- **Drag & drop** file inputs for green screen video, Alpha Hint, and output folder
- **Real-time progress** — frame counter and progress bar while processing
- **Full settings control** — gamma, despill strength, auto-despeckle, refiner
- **EXR → PNG conversion** — optional automatic conversion after processing
- **Custom output folder** — choose where results are saved
- **Bilingual** — English and Spanish interface
- **Cancel anytime** — safely stops processing and releases GPU memory

---

## System Requirements

| | |
|---|---|
| **Mac chip** | Apple Silicon — M1, M2, M3, or M4 |
| **RAM** | 24 GB minimum — 36 GB recommended |
| **macOS** | 12 Monterey or later |
| **Storage** | ~5 GB free for dependencies and model |
| **Internet** | Required for first-time installation (~400 MB) |

> Intel Macs are not supported. CorridorKey uses MPS (Metal Performance Shaders) for inference and requires ~22–30 GB of unified memory.

---

## Installation

### Option A — Automatic (recommended)

1. Download `CorridorKey.app` from the [Releases](../../releases) page
2. Move it to your Desktop or Applications folder
3. Double-click to open
4. On first launch, a Terminal window opens and installs everything automatically
5. When done, double-click the app again — it opens straight to the GUI

The installer handles:
- `uv` package manager
- CorridorKey repository clone
- Python dependencies (PyTorch, OpenCV, MLX backend, etc.)
- AI model download (~382 MB from Hugging Face)

### Option B — Manual

If you already have CorridorKey installed:

```bash
cd ~/CorridorKey
cp /path/to/corridorkey_gui.py .
cp /path/to/corridorkey_installer.py .
uv run python corridorkey_installer.py
```

---

## How to use

CorridorKey requires **two inputs** for every clip:

1. **Green screen video** — your original footage
2. **Alpha Hint** — a rough mask of your subject (does not need to be clean)

### Creating an Alpha Hint

The easiest way is a quick chroma key in DaVinci Resolve:

1. Import your green screen clip
2. Apply a Qualifier (chroma key) — it does not need to be perfect
3. Export as a black-and-white video (subject white, background black)
4. Use this as the Alpha Hint

Any tool with background removal works: Final Cut Pro, CapCut, Premiere Pro, etc.

### Processing

1. Open **CorridorKey.app**
2. Select your green screen video with Browse (or drag & drop)
3. Select your Alpha Hint
4. Choose an output folder
5. Adjust settings if needed
6. Click **▶ PROCESS**
7. When done, click **📁 View Results**

---

## Settings

| Setting | Description |
|---|---|
| **Color Space** | sRGB for standard footage, Linear for RAW/BRAW |
| **Despill (0–10)** | Removes green contamination from edges. Default: 5 |
| **Auto-despeckle** | Removes tracking markers and small artifacts |
| **Refiner (0.5–2.0)** | Edge refinement strength. Default: 1.0 |
| **Export PNG** | Converts EXR output to PNG after processing (EXR files are kept) |

---

## Output

Results are saved in your chosen output folder:

| Folder | Contents |
|---|---|
| `Matte/` | Alpha channel as EXR |
| `FG/` | Foreground color reconstruction as EXR |
| `Processed/` | RGBA premultiplied composite as EXR |
| `Comp/` | Preview composite as PNG |
| `*_PNG/` | PNG versions (if Export PNG is enabled) |

---

## Files

| File | Description |
|---|---|
| `corridorkey_gui.py` | Main GUI application |
| `corridorkey_installer.py` | First-launch installer window |
| `install_mac.sh` | Shell script for automatic installation |

---

## Notes

- Processing time depends on clip length and resolution. Expect ~30 seconds per frame on M3 Pro.
- The app uses the `torch` backend with MPS acceleration. The `mlx` backend requires `.safetensors` model format which is not yet publicly available.
- Input video and Alpha Hint must have the same number of frames. The app handles minor mismatches automatically with `--max-frames`.

---

## Credits

- **CorridorKey** neural network model and CLI by [Niko Pueringer / Corridor Digital](https://github.com/nikopueringer/CorridorKey)
- GUI developed with the assistance of [Claude](https://claude.ai) (Anthropic)
