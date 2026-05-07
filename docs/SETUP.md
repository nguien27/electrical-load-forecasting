# Hướng dẫn cài đặt chi tiết

## 1. Chuẩn bị môi trường

### Windows

```bash
# Cài đặt Python 3.8+ từ python.org
# Kiểm tra version
python --version

# Tạo virtual environment
python -m venv venv
venv\Scripts\activate

# Upgrade pip
python -m pip install --upgrade pip
```

### Linux/Mac

```bash
# Kiểm tra Python
python3 --version

# Tạo virtual environment
python3 -m venv venv
source venv/bin/activate

# Upgrade pip
pip install --upgrade pip
```

## 2. Cài đặt dependencies

### CPU only

```bash
pip install -r requirements.txt
```

### GPU (CUDA)

```bash
# Cài PyTorch với CUDA support
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# Cài các package còn lại
pip install -r requirements.txt
```

## 3. Chuẩn bị dữ liệu

### Tải dữ liệu từ Kaggle

```bash
# Cài Kaggle CLI
pip install kaggle

# Đặt API credentials tại ~/.kaggle/kaggle.json
# Download dataset
kaggle datasets download -d traanfddinhfkhair/datasetttt

# Giải nén vào thư mục data/
unzip datasetttt.zip -d data/
```

### Cấu trúc thư mục data

```
data/
├── train.csv       # 82,675 samples
├── val.csv         # 10,334 samples
└── test.csv        # 10,335 samples
```

## 4. Kiểm tra cài đặt

```python
# test_setup.py
import torch
import numpy as np
import pandas as pd

print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
if torch.cuda.is_available():
    print(f"CUDA version: {torch.version.cuda}")
    print(f"GPU: {torch.cuda.get_device_name(0)}")

print(f"NumPy version: {np.__version__}")
print(f"Pandas version: {pd.__version__}")
```

Chạy:
```bash
python test_setup.py
```

## 5. Chạy Jupyter

```bash
# Khởi động Jupyter
jupyter notebook

# Hoặc JupyterLab
jupyter lab
```

Mở browser tại `http://localhost:8888`

## 6. Troubleshooting

### Lỗi CUDA out of memory

Giảm batch size trong config:
```python
BATCH_SIZE = 32  # Thử giảm xuống 16 hoặc 8
```

### Lỗi import module

```bash
# Cài lại dependencies
pip install --force-reinstall -r requirements.txt
```

### Lỗi kernel died trong Jupyter

```bash
# Tăng memory limit
export JUPYTER_MEMORY_LIMIT=8G  # Linux/Mac
set JUPYTER_MEMORY_LIMIT=8G     # Windows
```

## 7. Cấu hình IDE (Optional)

### VS Code

Cài extensions:
- Python
- Jupyter
- Pylance

### PyCharm

1. File → Settings → Project → Python Interpreter
2. Chọn venv đã tạo
3. Enable Jupyter support
