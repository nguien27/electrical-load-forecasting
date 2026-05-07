# Quick Start Guide

Hướng dẫn nhanh để bắt đầu với dự án.

## 1. Clone và Setup (5 phút)

```bash
# Clone repository
git clone <repository-url>
cd <repository-name>

# Tạo virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# hoặc
venv\Scripts\activate     # Windows

# Cài đặt dependencies
pip install -r requirements.txt
```

## 2. Tải dữ liệu (2 phút)

### Option A: Từ Kaggle

```bash
pip install kaggle
kaggle datasets download -d traanfddinhfkhair/datasetttt
unzip datasetttt.zip -d data/
```

### Option B: Từ nguồn khác

Đặt các file sau vào thư mục `data/`:
- `train.csv`
- `val.csv`
- `test.csv`

## 3. Chạy thực nghiệm đầu tiên (10 phút)

```bash
# Khởi động Jupyter
jupyter notebook

# Mở file: experiments/deepbase_vs_deeptopo.ipynb
# Chạy tất cả cells: Cell → Run All
```

## 4. Xem kết quả

Kết quả sẽ hiển thị trong notebook bao gồm:
- Training/validation loss curves
- Metrics cho từng horizon (t+1h đến t+24h)
- Visualization của predictions vs actual

## Cấu trúc dự án

```
.
├── data/                  # Dữ liệu (không commit)
├── experiments/           # Notebooks thực nghiệm
├── docs/                  # Tài liệu
├── README.md             # Tổng quan dự án
├── requirements.txt      # Dependencies
└── .gitignore           # Git ignore rules
```

## Troubleshooting nhanh

### Lỗi: Module not found
```bash
pip install --force-reinstall -r requirements.txt
```

### Lỗi: CUDA out of memory
Giảm batch size trong notebook:
```python
BATCH_SIZE = 16  # Thay vì 32
```

### Lỗi: File not found (data)
Kiểm tra đường dẫn data trong notebook và đảm bảo files đã được đặt đúng thư mục.

## Tiếp theo

- Đọc [docs/dataset_overview.md](docs/dataset_overview.md) để hiểu về dữ liệu
- Xem [docs/SETUP.md](docs/SETUP.md) cho hướng dẫn chi tiết
- Đọc [CONTRIBUTING.md](CONTRIBUTING.md) nếu muốn đóng góp
