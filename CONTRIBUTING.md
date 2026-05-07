# Hướng dẫn đóng góp

## Quy tắc commit

Sử dụng conventional commits:

- `feat:` - Thêm tính năng mới
- `fix:` - Sửa lỗi
- `docs:` - Cập nhật tài liệu
- `refactor:` - Refactor code
- `test:` - Thêm/sửa tests
- `chore:` - Cập nhật dependencies, config

Ví dụ:
```
feat: add LSTM baseline model
fix: correct data preprocessing pipeline
docs: update README with installation steps
```

## Quy trình làm việc

1. Tạo branch mới từ `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Commit thay đổi:
   ```bash
   git add .
   git commit -m "feat: your feature description"
   ```

3. Push và tạo Pull Request:
   ```bash
   git push origin feature/your-feature-name
   ```

## Code style

- Python: Tuân thủ PEP 8
- Docstrings: Google style
- Đặt tên biến: snake_case
- Đặt tên class: PascalCase

## Testing

Trước khi commit, đảm bảo:
- Code chạy được không lỗi
- Notebook đã clear output (để giảm kích thước)
- Không commit file data lớn

## Notebook guidelines

- Clear output trước khi commit:
  ```bash
  jupyter nbconvert --clear-output --inplace your_notebook.ipynb
  ```
- Thêm markdown cells giải thích logic
- Đặt tên cell rõ ràng
- Tránh hardcode paths
