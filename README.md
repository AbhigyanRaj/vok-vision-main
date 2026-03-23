# VokVision: Professional 3D AI Reconstruction Engine

VokVision is an elite-grade 3D reconstruction platform that transforms a handful of 2D images into high-fidelity 3D Gaussian Splats. Engineered for professional workflows, it utilizes a state-of-the-art hybrid AI pipeline to deliver industrial-standard results.

---

## Elite Hybrid Architecture

Unlike basic reconstruction scripts, VokVision uses a Dual-Stage Hybrid Mapping strategy:

1.  **VLM Image Audit**: Gemini 1.5 Flash filters out blurry or poorly lit images before processing begins.
2.  **Hybrid Mapping (SfM)**: Uses original images (with background) for perfect camera triangulation, ensuring 100% stable poses.
3.  **Surgical Point-Cloud Masking**: Automatically deletes background points using AI segmentation masks before training starts.
4.  **Gaussian Splatting (30K)**: Trained on white-background datasets with 30,000 iterations for a clean, professional, floater-free 3D object.

---

## Detailed Windows Setup Guide

This engine is optimized for Windows 10/11 with NVIDIA hardware. Follow these steps carefully to ensure a successful installation.

### 1. System Requirements
- OS: Windows 10 or 11 (64-bit)
- GPU: NVIDIA RTX 30-series or 40-series (8GB+ VRAM recommended)
- RAM: 16GB minimum
- Storage: 10GB+ free space

### 2. Manual Prerequisites
- **CUDA Toolkit**: Install CUDA 11.8 or 12.1 from the NVIDIA Developer website.
- **Python 3.10**: Install from Python.org. Ensure "Add Python to PATH" is checked during installation.
- **Node.js (LTS)**: Install from nodejs.org for the orchestrator API.
- **Git**: Install Git for Windows.
- **Redis**: BullMQ requires Redis. For Windows, install [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) and run Redis inside it, or use [Memurai](https://www.memurai.com/) as a native Windows alternative.

### 3. Backend API Setup (Node.js)
```powershell
# Navigate to the API directory
cd backend/api

# Install dependencies
npm install

# Setup environment variables
copy .env.example .env
# Edit .env and add your MongoDB URL and Firebase credentials
```

### 4. AI Processor Setup (Python)
```powershell
# Navigate to the processor directory
cd backend/processor

# Create a virtual environment
python -m venv venv

# Activate the virtual environment
.\venv\Scripts\activate

# Install PyTorch with CUDA support
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install remaining requirements
pip install -r requirements.txt

# Setup environment variables
copy .env.example .env
# Add your Gemini API Key and Backend URL
```

### 5. Mobile App Setup (Flutter)
- Install the [Flutter SDK](https://docs.flutter.dev/get-started/install/windows).
- Run `flutter doctor` to ensure all dependencies are met.
- Connect your Android device or start an emulator.
```powershell
cd apps/mobile_app
flutter pub get
flutter run
```

### 6. Execution Flow
To run the full system on Windows, open three separate terminal windows:

1.  **Terminal 1 (Redis)**: Ensure your Redis server is running (`redis-server`).
2.  **Terminal 2 (API)**: 
    ```powershell
    cd backend/api
    npm run dev
    ```
3.  **Terminal 3 (Processor)**:
    ```powershell
    cd backend/processor
    .\venv\Scripts\activate
    python main.py
    ```

---

## Directory Structure

```plaintext
VokVision/
├── apps/mobile_app/        # Flutter Client (Dart)
├── backend/api/            # Node.js Orchestrator (TypeScript)
├── backend/processor/      # Python AI Engine (MASt3R, OpenSplat, VLM)
├── pipeline/               # Core AI System Modules
└── storage/                # Local data storage (Ignored by Git)
```

## Important Note on Checkpoints

The MASt3R model checkpoint (`MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric.pth`) is required.
- It will automatically download on the first run.
- For manual placement, put it in `pipeline/mast3r/`.

---

**Pro Tip**: For best results, use 20-40 images with 360-degree coverage and consistent lighting.
