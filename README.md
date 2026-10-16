# Hướng Dẫn Sử Dụng & Quy Chuẩn Cấu Hình

Repository này dùng để kích hoạt workflow tự động đồng bộ repository (`Auto Build ALL REPO`) thông qua GitHub Actions.

---

## ⚠️ Quy định đối với file `SYNC_REPO.txt`

File `SYNC_REPO.txt` là file trigger để workflow xác định repository nguồn cần đồng bộ.

> **QUY CHUẨN:**
> Workflow sẽ **lấy duy nhất dòng đầu tiên** của file `SYNC_REPO.txt`.
> Dòng đầu tiên **bắt buộc phải là một số nguyên dương** (chỉ bao gồm các chữ số `0-9`).

### 1. Ý nghĩa giá trị
Số nguyên tại dòng đầu tiên sẽ tương ứng với secret cấu hình trong GitHub Actions:
- Dòng 1 ghi `1` $\rightarrow$ Workflow checkout secret `REPO_1`
- Dòng 1 ghi `2` $\rightarrow$ Workflow checkout secret `REPO_2`
- Dòng 1 ghi `3` $\rightarrow$ Workflow checkout secret `REPO_3`
- ...

---

### 2. Ví dụ định dạng

#### ✅ Hợp lệ:
```text
1
```
*(Nếu có các dòng phía sau để ghi chú, workflow vẫn sẽ chỉ đọc và xử lý số ở dòng đầu tiên)*:
```text
2
ghi chú: sync repo 2 phiên bản mới
```

#### ❌ Không hợp lệ:
- Dòng đầu tiên chứa chữ hoặc ký tự đặc biệt: `r1`, `repo1`, `1d`
- Dòng đầu tiên là số thập phân: `1.5`, `2.0`
- Dòng đầu tiên có nhiều số cách nhau bởi khoảng trắng: `1 2 3`
- Dòng đầu tiên bị trống / không có nội dung

---

## 🚀 Cách thức hoạt động
1. Chỉnh sửa số nguyên ở **dòng đầu tiên** của `SYNC_REPO.txt`.
2. Commit và push thay đổi lên branch chính:
   ```bash
   git add SYNC_REPO.txt
   git commit -m "Trigger sync for REPO_X"
   git push origin main
   ```
3. GitHub Actions sẽ tự động:
   - Đọc số nguyên từ dòng đầu tiên của `SYNC_REPO.txt`.
   - Kiểm tra định dạng số nguyên hợp lệ.
   - Kiểm tra sự tồn tại của secret `REPO_<index>`.
   - Checkout repo tương ứng và chạy các script đồng bộ.
