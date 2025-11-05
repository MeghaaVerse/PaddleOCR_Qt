# PaddleOCR Qt Console Application

A Qt-based GUI application for PaddleOCR C++ inference, supporting text detection, classification, and recognition with visual results.

## Features

- Load and display images via file dialog  
- Text detection using PaddleOCR models  
- Automatic text angle correction  
- Text recognition with confidence scores  
- Visual bounding boxes overlaid on images  
- Single-file implementation for easy maintenance  

---

## Prerequisites

### System Requirements
- Linux (Ubuntu 18.04+ recommended)
- Qt 5.12+ or Qt 6.x
- CMake 3.10+ (for building Paddle Inference)
- GCC 7.3+ or Clang 5.0+

### Dependencies

**Required Libraries:**
- PaddlePaddle Inference Library (CPU or GPU version)
- OpenCV 3.4+ (with imgcodecs, imgproc modules)
- glog
- gflags
- protobuf
- pthread

**Install on Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install -y \
    qtbase5-dev \
    qt5-qmake \
    libopencv-dev \
    libgoogle-glog-dev \
    libgflags-dev \
    libprotobuf-dev \
    build-essential
```
## Installation

# 1. Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/PaddleOCR_Qt.git
cd PaddleOCR_Qt
```
# 2. Copy PaddleOCR Source Files
``` bash
# Copy from your PaddleOCR C++ inference directory
cp -r /path/to/PaddleOCR/deploy/cpp_infer/src .
cp -r /path/to/PaddleOCR/deploy/cpp_infer/include
```
# 3. Download Models
```bash
# Download the pre-trained models:
bashmkdir -p models


# English Detection Model
wget https://paddleocr.bj.bcebos.com/PP-OCRv3/english/en_PP-OCRv3_det_infer.tar
tar -xf en_PP-OCRv3_det_infer.tar -C models/

# English Recognition Model
wget https://paddleocr.bj.bcebos.com/PP-OCRv3/english/en_PP-OCRv3_rec_infer.tar
tar -xf en_PP-OCRv3_rec_infer.tar -C models/

# Classification Model
wget https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_mobile_v2.0_cls_infer.tar
tar -xf ch_ppocr_mobile_v2.0_cls_infer.tar -C models/

# English Dictionary
wget https://raw.githubusercontent.com/PaddlePaddle/PaddleOCR/release/2.6/ppocr/utils/en_dict.txt -O models/en_dict.txt
```
### 4. Install Paddle Inference
Download and extract Paddle Inference library:
```bash
# For CPU version
wget https://paddle-inference-lib.bj.bcebos.com/2.5.0/cxx_c/Linux/CPU/gcc8.2_avx_mkl/paddle_inference.tgz
tar -xzf paddle_inference.tgz
```

# Update the path in PaddleOCR_Qt.pro
# PADDLE_LIB = /absolute/path/to/paddle_inference

# 5. Configure Project
Edit PaddleOCR_Qt.pro and update these paths:

PADDLE_LIB: Your Paddle Inference installation path
OpenCV include paths (if not in standard locations)

Edit main.cpp and update model paths in the MainWindow constructor:
cppQString modelBasePath = "/absolute/path/to/models";

# 6. Build the Project
Using Qt Creator:

Open Qt Creator
File → Open File or Project → Select PaddleOCR_Qt.pro
Configure your Qt kit
Build → Build Project (Ctrl+B)
Run (Ctrl+R)

# Using Command Line:
```bash
qmake PaddleOCR_Qt.pro
make -j$(nproc)
./PaddleOCR_Qt
```
# Usage

Launch the application
Click "Open Image" to select an image file
Click "Run OCR" to perform text detection and recognition
View results in the right panel
Bounding boxes and numbered labels will appear on the image

## Project Structure
PaddleOCR_Qt/
├── PaddleOCR_Qt.pro       # Qt project file
├── main.cpp               # Main application (single file)
├── src/                   # PaddleOCR source files
├── include/               # PaddleOCR headers
├── models/                # Pre-trained models
├── .gitignore
└── README.md

## Configuration
You can adjust OCR parameters in main.cpp in the PaddleOCRWrapper constructor:
cppconfig_.max_side_len = 960;           // Maximum image side length
config_.det_db_thresh = 0.3;          // Detection threshold
config_.det_db_box_thresh = 0.6;      // Box threshold
config_.cls_thresh = 0.9;             // Classification threshold
config_.cpu_math_library_num_threads = 10;  // CPU threads

## Troubleshooting
# Library Loading Errors
bash# Add to ~/.bashrc or run before launching
export LD_LIBRARY_PATH=/path/to/paddle_inference/paddle/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/path/to/paddle_inference/third_party/install/protobuf/lib:$LD_LIBRARY_PATH
# Model Loading Failed

Ensure model directories contain inference.pdmodel and inference.pdiparams
Check file permissions: chmod -R 755 models/
Verify paths are absolute in main.cpp

# OpenCV Not Found
bash# Find OpenCV installation
pkg-config --cflags --libs opencv4

# Update INCLUDEPATH in .pro file
Performance Tips

*CPU Optimization*: Adjust cpu_math_library_num_threads based on your CPU cores
*GPU Acceleration*: Set config_.use_gpu = true and use CUDA-enabled Paddle Inference
*Batch Processing*: Increase rec_batch_num for multiple images

# License
This project uses PaddleOCR which is licensed under Apache License 2.0.
See the [License](LICENSE) file for details.

## Acknowledgments

PaddleOCR - OCR toolkit by PaddlePaddle
Qt Framework - Cross-platform application framework

## Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

## Support
For issues and questions:

Open an issue on GitHub
Check [PaddleOCR Documentation](https://www.paddleocr.ai/main/en/version2.x/legacy/cpp_infer.html)

## Author
[A Meghamala](https://github.com/MeghaaVerse)
