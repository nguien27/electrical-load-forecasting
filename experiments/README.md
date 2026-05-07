# Experiments

Thư mục này chứa các notebook thực nghiệm so sánh các kiến trúc mô hình.

## Danh sách thực nghiệm

### 1. DeepBase vs DeepTopo
**File**: `deepbase_vs_deeptopo.ipynb`

So sánh kiến trúc Deep Learning baseline với topology-aware architecture.

**Mục tiêu**: Đánh giá hiệu quả của việc tích hợp thông tin topology vào mô hình deep learning.

### 2. KANBase vs KANTopo
**File**: `kanbase_vs_kantopo.ipynb`

So sánh KAN (Kolmogorov-Arnold Networks) baseline với KAN topology-aware.

**Mục tiêu**: Kiểm tra khả năng của KAN trong việc học các mối quan hệ phi tuyến và tác động của topology.

### 3. KANTopo vs Hybrid
**File**: `kantopo_vs_hybrid.ipynb`

So sánh KAN topology-aware với hybrid architecture (kết hợp nhiều kiến trúc).

**Mục tiêu**: Tìm kiếm kiến trúc tối ưu bằng cách kết hợp ưu điểm của các phương pháp.

## Cách chạy

```bash
# Activate virtual environment
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Start Jupyter
jupyter notebook

# Mở notebook tương ứng và chạy từng cell
```

## Metrics đánh giá

Các thực nghiệm sử dụng các metrics sau:
- **MAE** (Mean Absolute Error)
- **RMSE** (Root Mean Square Error)
- **MAPE** (Mean Absolute Percentage Error)
- **R²** (Coefficient of Determination)

Đánh giá riêng cho từng horizon: t+1h, t+2h, ..., t+24h

## Kết quả

Xem chi tiết kết quả tại [../docs/ablation_results.pdf](../docs/ablation_results.pdf)
