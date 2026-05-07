# Pre-Commit Checklist

Checklist này giúp đảm bảo code quality trước khi commit lên Git.

## ✅ Trước khi commit

### 1. Code Quality
- [ ] Code chạy được không lỗi
- [ ] Đã test các functions chính
- [ ] Không có hardcoded paths
- [ ] Không có print debug statements thừa
- [ ] Variable names rõ ràng, có ý nghĩa

### 2. Notebooks
- [ ] Đã clear output của notebooks:
  ```bash
  jupyter nbconvert --clear-output --inplace your_notebook.ipynb
  ```
- [ ] Đã thêm markdown cells giải thích
- [ ] Không có cell chạy lỗi
- [ ] Kết quả có thể reproduce

### 3. Data & Files
- [ ] Không commit file data lớn (>10MB)
- [ ] Không commit model checkpoints
- [ ] Không commit credentials/API keys
- [ ] Đã update .gitignore nếu cần

### 4. Documentation
- [ ] Đã update README.md nếu thêm features mới
- [ ] Đã thêm docstrings cho functions mới
- [ ] Đã update requirements.txt nếu thêm dependencies

### 5. Git
- [ ] Commit message rõ ràng, theo conventional commits
- [ ] Không commit trực tiếp vào main (tạo branch riêng)
- [ ] Đã review changes trước khi commit:
  ```bash
  git diff
  ```

## 🔍 Kiểm tra nhanh

```bash
# Xem files sẽ được commit
git status

# Xem chi tiết thay đổi
git diff

# Kiểm tra kích thước files
git ls-files --stage | awk '{print $4, $2}' | sort -k2 -n -r | head -10

# Clear notebook outputs (nếu có)
find . -name "*.ipynb" -exec jupyter nbconvert --clear-output --inplace {} \;
```

## 📝 Commit message template

```
<type>: <subject>

<body (optional)>

<footer (optional)>
```

**Types:**
- `feat`: Tính năng mới
- `fix`: Sửa lỗi
- `docs`: Cập nhật tài liệu
- `refactor`: Refactor code
- `test`: Thêm/sửa tests
- `chore`: Cập nhật dependencies, config

**Examples:**
```
feat: add LSTM baseline model

Implement LSTM baseline with:
- 2 LSTM layers (128, 64 units)
- Dropout 0.2
- Dense output layer

fix: correct data preprocessing pipeline

- Fix missing value handling
- Correct feature scaling order
- Add validation checks

docs: update README with installation steps
```

## 🚫 Tránh commit

- [ ] `__pycache__/` directories
- [ ] `.ipynb_checkpoints/`
- [ ] Large data files (`.csv`, `.h5`, `.pkl`)
- [ ] Model weights (`.pth`, `.ckpt`)
- [ ] Log files (`.log`)
- [ ] IDE configs (`.vscode/`, `.idea/`)
- [ ] Credentials (`.env`, `config.json`)

## ✨ Best Practices

1. **Commit nhỏ, thường xuyên**: Mỗi commit nên tập trung vào một thay đổi cụ thể
2. **Test trước khi commit**: Đảm bảo code chạy được
3. **Review changes**: Luôn xem lại thay đổi trước khi commit
4. **Clear notebook outputs**: Giảm kích thước và tránh conflicts
5. **Update docs**: Giữ documentation đồng bộ với code
