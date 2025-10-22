# RoPlate - Roblox License Plate Assistant

## Prerequisites

1. **Python 3.10 or newer** (64 bit) — https://www.python.org/downloads/windows/  
2. **Microsoft Visual C++ Redistributable** — usually present on gaming PCs; install if missing.  
3. **Tesseract OCR** — download from https://github.com/UB-Mannheim/tesseract/wiki  
   - During setup tick “Add Tesseract to the system path”, otherwise note the full path to `tesseract.exe`.  
4. **YOLOv8 / PyTorch toolchain**  
   - Pick the build that matches your hardware. CPU example:  
     ```powershell
     pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
     ```
   - Install Ultralytics (already listed in the requirements but safe to pre-install): `pip install ultralytics`.  
   - Place your YOLO weights under `models/` (defaults: `roplate-yolov8n.pt` and `roplate-car-yolov8n.pt`).
5. **RapidOCR runtime** (default OCR backend):  
   ```powershell
   pip install rapidocr-onnxruntime
   ```
   The first launch downloads ~25 MB of OCR weights; after that it runs offline. Tesseract is used as a fallback.

---


### First run

1. Choose **Select Application Window** (preferred) or **Select Capture Region** to define what to capture.  
2. Click **Start Recognition** – detections and OCR output appear in the Live Recognition view and history.  
3. Use the toolbar settings (⋯) to adjust capture interval, minimum confidence, history size or auto-copy. Changes persist immediately in `config.json`.  
4. Optional: open **Show Mini View** for an always-on-top summary window.

`config.json` is generated automatically after the first run; you never need to create it manually.

### OCR troubleshooting

- RapidOCR must download its ONNX weights on first use—open the app once while connected to the internet.  
- To force Tesseract, set `"ocr_backend": "tesseract"` in `config.json` or via the settings dialog.  
- If Tesseract is installed in a non-standard location, point `tesseract_cmd` at the full path of `tesseract.exe`.

### Recognition tuning

- Keep the capture region small and focused on license plates; speed and accuracy both improve.  
- Lower `history_size` if you only care about the latest few plates.  
- Increase `min_confidence` for brighter scenes or decrease slightly for dimly lit captures.

---


### Shipping checklist

- Bundle `RoPlate.exe` together with the `models/` directory (YOLO weights).  
- Include `config.json` only if you want to provide defaults; otherwise RoPlate creates one on first launch.  
- Tesseract is not bundled—end users still need it installed locally.

---

## Customer installation (prebuilt package)

1. Install Tesseract OCR (add to PATH, or remember the installation path).  
2. Extract the delivered archive so that `RoPlate.exe` sits next to the `models/` folder.  
3. Double-click `RoPlate.exe`. On first launch RoPlate automatically writes `config.json` and, if online, downloads RapidOCR models.  
4. Use the toolbar tabs to navigate. Adjust the capture interval/confidence via the **⋯ Settings** button—no restart needed.  
5. If Tesseract is in a custom path, edit `config.json` (or use the settings dialog) and set `tesseract_cmd` accordingly.

---

## Configuration cheat sheet

`config.json` keys (auto-generated):

- `monitor_index`: 1-based monitor index for full-screen capture.  
- `capture_region`: `(left, top, width, height)` for manual region capture. [NOT WORKING] 
- `window_title`: binds to a specific window; set when you pick **Select Application Window**.  
- `capture_interval_ms`: time between captures (defaults from settings dialog).  
- `min_confidence`: YOLO detection threshold.  
- `history_size`: maximum stored plates (UI history and mini view).  
- `detector_*`: YOLO model paths and thresholds.  
- `ocr_backend`, `ocr_languages`, `ocr_require_digit`, `ocr_fallback_to_tesseract`: OCR behaviour.  
- `plate_whitelist`: list of plates to ignore (case-insensitive).  
- `officer_name`: Officer name.

You can adjust these settings directly or via the in-app dialog.

---

## Common questions

- **Plates look jagged or cropped** – ensure the Roblox window is in focus and adjust the capture region.  
- **History is cluttered** – lower `history_size` or toggle the mini view.  
- **OCR misses plates** – try bumping `capture_interval_ms` lower or reducing `min_confidence` slightly.  
- **Nothing happens on first run** – confirm the capture window is visible (not minimized) and that Tesseract/RapidOCR are installed.

Enjoy faster traffic stops
