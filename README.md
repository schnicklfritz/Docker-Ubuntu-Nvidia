# CUDA Base Image

▄████▄ █ ██ ▓█████▄ ▄▄▄ ▄▄▄▄ ▄▄▄ ██████ ▓█████
▒██▀ ▀█ ██ ▓██▒▒██▀ ██▌▒████▄ ▓█████▄ ▒████▄ ▒██ ▒ ▓█ ▀
▒▓█ ▄ ▓██ ▒██░░██ █▌▒██ ▀█▄ ▒██▒ ▄██▒██ ▀█▄ ░ ▓██▄ ▒███
▒▓▓▄ ▄██▒▓▓█ ░██░░▓█▄ ▌░██▄▄▄▄██ ▒██░█▀ ░██▄▄▄▄██ ▒ ██▒▒▓█ ▄
▒ ▓███▀ ░▒▒█████▓ ░▒████▓ ▓█ ▓██▒ ░▓█ ▀█▓ ▓█ ▓██▒▒██████▒▒░▒████▒
░ ░▒ ▒ ░░▒▓▒ ▒ ▒ ▒▒▓ ▒ ▒▒ ▓▒█░ ░▒▓███▀▒ ▒▒ ▓▒█░▒ ▒▓▒ ▒ ░░░ ▒░ ░
░ ▒ ░░▒░ ░ ░ ░ ▒ ▒ ▒ ▒▒ ░ ▒░▒ ░ ▒ ▒▒ ░░ ░▒ ░ ░ ░ ░ ░
░ ░░░ ░ ░ ░ ░ ░ ░ ▒ ░ ░ ░ ▒ ░ ░ ░ ░
░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░
░ ░ ░


**Minimal NVIDIA CUDA 13.0.1 runtime base image for building specialized GPU workloads.**  
Ubuntu 24.04 with Python, Chrome, and audio support.

---

## Purpose

This is a **base image** designed to be extended for specific projects. Common dependencies are included here; project-specific packages go in derived images.

**Philosophy**: If a package is needed by 2+ projects, it gets added to this base.

---

## What's Included

- **CUDA 13.0.1 Runtime**: GPU compute without development bloat
- **Ubuntu 24.04 LTS**: Minimal system packages
- **Python 3.12 + venv**: Ready for ML/AI packages
- **Google Chrome**: For web automation/scraping
- **PulseAudio**: Audio processing support
- **Admin user**: Non-root with sudo access

---

## Quick Start

### Deploy to Quickpod

1. Use image: `schnicklbob/cuda-desktop-base:latest`
2. SSH via Quickpod's connection string
3. Switch to admin user: `su - admin`

### Access

SSH using Quickpod's UUID (from dashboard)

ssh -p <PORT> <UUID>@<HOST>
Inside container, switch to admin

su - admin
Work in the venv (already in PATH)

pip install your-packages

---

## Validation Tests

All tests passed on **RTX 5090** with **CUDA 13.0** and **driver 580.82.09**:

After SSH into container

su - admin
✅ Test Chrome

google-chrome --version
google-chrome --headless --no-sandbox --dump-dom https://example.com
✅ Test PulseAudio

pulseaudio --check || pulseaudio --start
pulseaudio --check
✅ Test GPU

nvidia-smi
✅ Test Python venv

which pip
pip install selenium torch

---

## System Requirements

- **NVIDIA Driver**: 580.65.06+
- **Supported GPUs**: Blackwell (RTX 5090), Hopper (H100), Ada (RTX 4000), Ampere (RTX 3000)
- **Not Supported**: Maxwell, Pascal, Volta (CUDA 13 dropped support)

---

## Build Locally

git clone https://github.com/Schnicklfritz/Docker-Ubuntu-Nvidia.git
cd Docker-Ubuntu-Nvidia
docker build -t cuda-base:local .


---

## Extending This Image

FROM schnicklbob/cuda-desktop-base:latest

USER admin
WORKDIR /home/admin
Add your project-specific dependencies

RUN pip install transformers accelerate
Copy your code

COPY --chown=admin:admin . /home/admin/project

CMD ["python", "project/main.py"]

---

## Use Cases

- **Base for LLM projects**: Add transformers, vLLM
- **Base for web scraping**: Selenium already has Chrome
- **Base for audio AI**: TTS, STT with PulseAudio
- **Base for vision tasks**: Add OpenCV, PIL
- **Base for AI music generation**: Perfect for YuE and similar models

---

## Example Project: YuE AI Music Generation

This base image is ideal for running **YuE**, an open-source AI music generation model that creates full songs with lyrics, vocals, and instrumentals.

### About YuE

**YuE (乐)** is a groundbreaking open-source foundation model for music generation (similar to Suno.ai but open). It transforms lyrics into complete songs with vocals and accompaniment.

### Available YuE Resources

**🔥 Official Repository (Most Up-to-Date)**
- **Repository**: [multimodal-art-projection/YuE](https://github.com/multimodal-art-projection/YuE)
- **Latest Update**: June 2025 - Added LoRA finetuning support
- **Features**: Latest models, inference scripts, prompt engineering guide
- **Paper**: [YuE Technical Report (arXiv)](https://arxiv.org/abs/2503.08638)
- **Demo**: [Official Demo Page](https://map-yue.github.io/)
- **License**: Apache 2.0

**🎛️ YuE Interfaces & Tools**

1. **YuE-Interface** (Docker + Gradio)
   - Repository: [alisson-anjos/YuE-Interface](https://github.com/alisson-anjos/YuE-Interface)
   - Docker-based web UI with Gradio
   - Easy model management and batch generation
   - Last updated: April 2025

2. **YuE-UI** (Advanced Interface)
   - Repository: [joeljuvel/YuE-UI](https://github.com/joeljuvel/YuE-UI)
   - Supports incremental song generation
   - Interactive timeline, session save/load
   - Optimized for 8GB VRAM with quantized models

3. **YuE-exllamav2-UI** (Faster Inference)
   - Repository: [WrongProtocol/YuE-exllamav2-UI](https://github.com/WrongProtocol/YuE-exllamav2-UI)
   - Optimized inference using exllamav2
   - Faster generation with slight quality trade-off

4. **YuE-extend** (Music Continuation)
   - Repository: [Mozer/YuE-extend](https://github.com/Mozer/YuE-extend)
   - Supports music continuation
   - Google Colab support

5. **YuEGP** (Memory-Optimized)
   - Repository: [deepbeepmeep/YuEGP](https://github.com/deepbeepmeep/YuEGP)
   - Memory management for limited GPU resources
   - Better for GPUs with <24GB VRAM

### Using YuE with This Base Image

This CUDA base image provides the perfect foundation for YuE:
- ✅ CUDA 13.0.1 runtime
- ✅ Python 3.12 with venv
- ✅ PulseAudio for audio processing
- ✅ Ubuntu 24.04 LTS

**Example Dockerfile extending this base for YuE:**

```dockerfile
FROM schnicklbob/cuda-desktop-base:latest

USER admin
WORKDIR /home/admin

# Install YuE dependencies
RUN pip install torch torchvision torchaudio \
    && pip install transformers accelerate \
    && pip install gradio \
    && pip install flash-attn --no-build-isolation

# Clone YuE repository
RUN git clone https://github.com/multimodal-art-projection/YuE.git \
    && cd YuE/inference/ \
    && git clone https://huggingface.co/m-a-p/xcodec_mini_infer

WORKDIR /home/admin/YuE

CMD ["bash"]
```

### GPU Requirements

- **Minimum**: RTX 3090 (24GB) for 2 sessions
- **Recommended**: H100/A100 (80GB) for full songs
- **Budget**: 8GB VRAM possible with quantized models (INT8/NF4)

### Key Features

- **Full song generation**: Creates complete songs with vocals and instruments
- **Multi-language support**: English, Chinese, Japanese, Korean
- **Style transfer**: ICL mode for voice cloning and music style transfer
- **Chain-of-Thought (CoT)**: Enhanced reasoning for better generation
- **In-Context Learning (ICL)**: Reference audio for style matching
- **LoRA finetuning**: Customize models for specific styles (added June 2025)

### Recommended Workflow

1. **For latest features**: Use the [official YuE repository](https://github.com/multimodal-art-projection/YuE)
2. **For easy Docker setup**: Use [YuE-Interface by alisson-anjos](https://github.com/alisson-anjos/YuE-Interface)
3. **For incremental generation**: Use [YuE-UI by joeljuvel](https://github.com/joeljuvel/YuE-UI)
4. **For limited VRAM**: Use quantized models with [YuEGP](https://github.com/deepbeepmeep/YuEGP)

### Community

- **Discord**: [Join the YuE community](https://discord.gg/ssAyWMnMzu)
- **Models**: Available on [Hugging Face](https://huggingface.co/m-a-p)

---

## Installed Packages

**System:**
- openssh-server
- python3, python3-pip, python3-venv
- pulseaudio
- google-chrome-stable
- netcat-openbsd, wget, sudo

**Python:**
- pip (upgraded to latest)
- Virtual environment at `/home/admin/venv`

**Not included:**
- No CUDA development tools (nvcc)
- No Jupyter
- No GUI/desktop
- No specific ML frameworks (add in derived images)

---

## Evolution Strategy

This base will grow based on usage:
- Used in 2+ projects → Add to base
- Single-use dependency → Keep in derived image
- Breaking changes → New major version tag

---

## Size & Performance

- **Image size**: ~5GB (runtime vs ~8GB devel)
- **Startup**: <10 seconds on Quickpod
- **Memory**: Minimal overhead (~200MB)
- **Tested on**: NVIDIA RTX 5090, 32GB VRAM

---

## Project Structure

Docker-Ubuntu-Nvidia/
├── Dockerfile # Base image definition
├── scripts/
│ └── entrypoint.sh # Startup script
└── README.md # This file

---

## Troubleshooting

### Chrome issues

Always use these flags in headless mode

--headless --no-sandbox --disable-dev-shm-usage
dbus errors are expected and harmless in containers

### PulseAudio not running

pulseaudio --start --log-target=syslog
pulseaudio --check # Should return 0



### GPU not detected

nvidia-smi # Check driver version (need 580.65.06+)
Verify CUDA runtime

python -c "import subprocess; print(subprocess.run(['nvidia-smi', '--query-gpu=name', '--format=csv,noheader'], capture_output=True, text=True).stdout)"


### Need CUDA development tools?
Switch base image to `nvidia/cuda:13.0.1-devel-ubuntu24.04` in your Dockerfile.

---

## Contributing

This is a living base image. Submit PRs for:
- Common dependencies (used in 2+ projects)
- Security improvements
- Size optimization
- Bug fixes

**Not accepted:**
- Project-specific packages
- Breaking changes without version bump
- Bloated dependencies

---

## Versioning

- `latest`: Tracks current stable
- `13.0.1`: CUDA version pinned
- `13.0.1-v1`: Base image iteration

---

## License

MIT License

---

## Related

- [Docker Hub](https://hub.docker.com/r/schnicklbob/cuda-desktop-base)
- [GitHub](https://github.com/Schnicklfritz/Docker-Ubuntu-Nvidia)
- [Quickpod Docs](https://docs.quickpod.io)

---

════════════════════════════════════════════════════════════════════════════════
Crafted with precision by AI assistance | Base image - Extend - Deploy
════════════════════════════════════════════════════════════════════════════════


**Status**: Base image - extend for projects  
**CUDA**: 13.0.1 Runtime  
**OS**: Ubuntu 24.04 LTS  
**Platform**: Quickpod optimized  
**Validated**: RTX 5090, 32GB VRAM, Driver 580.82.09

