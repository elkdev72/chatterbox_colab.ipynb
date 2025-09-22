# Chatterbox TTS on Google Colab (T4 GPU)

Run the [Chatterbox-TTS-Server](https://github.com/devnen/Chatterbox-TTS-Server) instantly on **Google Colab** with GPU acceleration.
This setup makes it easy to demo and experiment with **text-to-speech (TTS)** directly in your browser.

---

## 🚀 Quick Start

### 1. Open in Colab

Click the button below to launch the notebook in **Google Colab** with GPU support:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-username/your-repo-name/blob/main/chatterbox_colab.ipynb)

---

### 2. Enable GPU (T4 Recommended)

* In Colab, go to:
  `Runtime` → `Change runtime type` → Set **Hardware accelerator** = **GPU**
* Confirm you’re on a **T4 GPU** under *Runtime Info*.

---

### 3. Install and Run

Copy-paste the following cells into Colab, or just use the provided `.ipynb` file.

```python
# Clone the Chatterbox-TTS-Server repository
!git clone https://github.com/devnen/Chatterbox-TTS-Server.git

# Navigate into the cloned directory
%cd Chatterbox-TTS-Server

# Install PyTorch (CUDA 12.1 build)
!pip install torch==2.5.1+cu121 torchaudio==2.5.1+cu121 torchvision==0.20.1+cu121 \
  --index-url https://download.pytorch.org/whl/cu121 -q

# Install Chatterbox
!pip install git+https://github.com/devnen/chatterbox.git -q

# Install server + audio dependencies
!pip install fastapi uvicorn pyyaml soundfile librosa safetensors -q
!pip install python-multipart requests jinja2 watchdog aiofiles unidecode inflect tqdm -q
!pip install pydub audiotsm -q
```

---

### 4. Start the Server

```python
import threading, time, uvicorn, gc
from google.colab.output import serve_kernel_port_as_window

# Clear any old servers
!fuser -k 8004/tcp 2>/dev/null || echo "Port clear"
!pkill -f "uvicorn.*server" 2>/dev/null || echo "No running servers"
gc.collect()

def run_server():
    from server import app
    uvicorn.run(app, host="0.0.0.0", port=8004, log_level="warning", access_log=False)

print("🚀 Starting TTS server...")
thread = threading.Thread(target=run_server, daemon=True)
thread.start()
time.sleep(25)  # wait for model to load

print("🎉 Server ready! Click below:")
serve_kernel_port_as_window(8004)
```

---

## 📺 YouTube Guide

This repo is designed for a **step-by-step YouTube tutorial** on running **Chatterbox-TTS-Server** inside Google Colab.
👉 Follow along with the video to learn how to:

* Set up dependencies in Colab
* Use a **T4 GPU** for fast TTS inference
* Run the Chatterbox server and access it via a browser

---

## 🔗 Repositories

* [Chatterbox-TTS-Server](https://github.com/devnen/Chatterbox-TTS-Server)
* [Chatterbox](https://github.com/devnen/chatterbox)

---

## ⚡ Notes

* First run may take \~20–30s to load the model.
* If Colab disconnects, simply **re-run all cells**.
* This setup is for **demo/educational use** — not optimized for production deployment.
