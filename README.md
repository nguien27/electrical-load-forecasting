# Time Series Forecasting for Electrical Load P_224

Dự án nghiên cứu dự báo tải điện P_224 sử dụng các mô hình Deep Learning và KAN (Kolmogorov-Arnold Networks).

## Tổng quan

Dự án thực hiện dự báo tải điện 24 giờ tiếp theo dựa trên dữ liệu lịch sử 24 giờ (48 mốc 30 phút). Dataset bao gồm:
- **Thời gian**: 2020-02-09 đến 2025-12-31 (103,344 mốc, tần suất 30 phút)
- **Target**: P_224 (tải điện Tân Hưng)
- **Features**: 
  - Peer load: P_474, P_476, P_478, P_480
  - Weather: temp, rhum, prcp, wspd

## Cấu trúc thư mục

```
.
├── data/                           # Thư mục dữ liệu (không commit lên git)
│   ├── train.csv
│   ├── val.csv
│   └── test.csv
├── experiments/                    # Các thực nghiệm so sánh
│   ├── deepbase_vs_deeptopo.ipynb
│   ├── kanbase_vs_kantopo.ipynb
│   └── kantopo_vs_hybrid.ipynb
├── docs/                          # Tài liệu
│   ├── dataset_overview.md
│   ├── feature_analysis.docx
│   └── ablation_results.pdf
├── README.md
├── requirements.txt
└── .gitignore
```

## Dataset

Chi tiết về dataset xem tại [docs/dataset_overview.md](docs/dataset_overview.md)

**Thống kê chính:**
- Tổng số mẫu: 103,344
- Split ratio: Train (80%) / Val (10%) / Test (10%)
- Target mean: 23.51 ± 9.39
- Seasonality: Rõ ràng theo giờ (peak 21h, trough 11h)
- Holiday effect: Giảm 43% so với ngày thường

## Môi trường

### Yêu cầu hệ thống
- Python 3.8+
- CUDA 11.x (khuyến nghị cho GPU training)

### Cài đặt

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

## Thực nghiệm

### 1. DeepBase vs DeepTopo
So sánh kiến trúc baseline với topology-aware architecture cho mô hình Deep Learning.

```bash
jupyter notebook experiments/deepbase_vs_deeptopo.ipynb
```

### 2. KANBase vs KANTopo
So sánh KAN baseline với KAN topology-aware.

```bash
jupyter notebook experiments/kanbase_vs_kantopo.ipynb
```

### 3. KANTopo vs Hybrid
So sánh KAN topology với hybrid architecture.

```bash
jupyter notebook experiments/kantopo_vs_hybrid.ipynb
```

## Kết quả

Kết quả chi tiết ablation study xem tại [docs/ablation_results.pdf](docs/ablation_results.pdf)

## Tác giả

[Thêm thông tin tác giả]

## License

[Thêm license]

## Trích dẫn

Nếu sử dụng code hoặc dữ liệu này, vui lòng trích dẫn:

```
[Thêm citation]
```
