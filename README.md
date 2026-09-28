# LAAFNet Streamlit App

This repository contains a Streamlit UI for a lesion-aware attention fusion network (LAAFNet).

The environment where this project is run must have a compatible TensorFlow installation. On macOS, the TensorFlow C++ runtime may abort the process if the wheel is incompatible with your CPU/OS. To avoid this problem while developing, the app and helper modules were modified to import TensorFlow lazily and to degrade gracefully if TensorFlow is unavailable.

Two recommended ways to run the app:

1. Run locally (requires a working TensorFlow install)

- Create and activate a Python virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install --upgrade pip setuptools wheel
```

- Install dependencies. Match the TensorFlow wheel to your macOS/CPU first (examples):
  - Apple Silicon (M1/M2):

  ```bash
  python3 -m pip install tensorflow-macos==2.15.0
  python3 -m pip install tensorflow-metal
  python3 -m pip install -r requirements.txt
  ```

  - Intel macOS:

  ```bash
  python3 -m pip install tensorflow==2.15.0
  python3 -m pip install -r requirements.txt
  ```

- Place the model file `laafnet_epoch_28_valAcc_0.8278.keras` in the repo root (or change the path in the app sidebar).

- Run Streamlit:

```bash
streamlit run app.py
```

2. Run inside Docker (recommended for reproducible environment)

- Build the image:

```bash
docker build -t laafnet-streamlit:latest .
```

- Run the container (map port and mount the model file if needed):

```bash
# Example: mount current dir so the model file is available inside the container
docker run --rm -p 8501:8501 -v "$(pwd)":/app laafnet-streamlit:latest
```

Then open http://localhost:8501 in your browser.

Notes and troubleshooting

- If the app crashes during import with a native error like "mutex lock failed: Invalid argument", this indicates an incompatible TensorFlow native binary on your machine. Use the Docker approach or install the appropriate TensorFlow wheel for your platform.

- The repository was modified to defer TensorFlow imports until model load/prediction time. This allows the Streamlit UI to start and present a friendly error message if TensorFlow is missing.

- To enable the internal TF-dependent tests in `test_app_components.py`, set the environment variable `RUN_TF_TESTS=1` before running that script (only do this when TensorFlow is installed and working):

```bash
export RUN_TF_TESTS=1
python3 test_app_components.py
```

If you want, I can:

- Build and run the Docker image here (requires Docker in this environment).
- Provide platform-specific pip commands for your Mac if you tell me whether it's Intel or Apple Silicon and your Python version.
- Attempt to install TensorFlow in this environment and run a full app test (I may be limited by permissions).

Tell me which option you prefer and I will proceed.
