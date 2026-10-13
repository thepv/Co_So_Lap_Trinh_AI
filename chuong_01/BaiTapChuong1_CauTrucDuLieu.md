# 📝 Bài tập 1: Phát hiện và xử lý mã định danh trùng lặp (Data Deduplication)

## 🎯 Mục tiêu
Vận dụng thành thạo các cấu trúc dữ liệu `list`, `set` và `dict` để phân tích hồ sơ sinh viên, phát hiện và xử lý các mã định danh bị trùng lặp (dựa theo bài tập Buổi 4 trong tài liệu).

## 📄 Mô tả bài toán
Cho danh sách các bản ghi sinh viên dưới dạng một `list` chứa các `dict` như sau:

```python
students = [
    {"id": "SV01", "name": "An", "score": 8.0},
    {"id": "SV02", "name": "Binh", "score": 7.5},
    {"id": "SV01", "name": "An_Update", "score": 8.5},
    {"id": "SV03", "name": "Chi", "score": 9.0},
    {"id": "SV02", "name": "Binh", "score": 7.5},
]
```

## 🛠️ Yêu cầu
1. **Trích xuất** tất cả các mã `"id"` thành một danh sách riêng biệt.
2. **Sử dụng `set`** để tìm danh sách các `id` xuất hiện từ 2 lần trở lên (`duplicate_ids`).
3. **Lọc danh sách** `students` để chỉ giữ lại bản ghi **đầu tiên** của mỗi mã sinh viên (loại bỏ toàn bộ các bản ghi trùng lặp phía sau).

## 💡 Gợi ý thuật toán
* Sử dụng một tập hợp `seen_ids = set()` để lưu trữ các mã ID đã duyệt qua.
* Sử dụng vòng lặp để duyệt qua từng sinh viên:
  * ❌ **Nếu** `student["id"] in seen_ids`: Thêm mã đó vào danh sách `duplicate_ids`.
  * ✅ **Nếu chưa gặp**: Thêm mã đó vào `seen_ids` và đưa thông tin sinh viên vào danh sách `unique_students`.  

  # 📊 Bài tập 2: Thống kê tần suất xuất hiện của nhãn (Frequency Counter)

## 🎯 Mục tiêu
Sử dụng cấu trúc dữ liệu `dict` để tự xây dựng bộ đếm tần suất xuất hiện của các nhãn dự đoán trong bài toán phân loại nhiều lớp (Multi-class Classification).

## 📄 Mô tả bài toán
Cho danh sách các nhãn dự đoán thu được từ một mô hình phân loại như sau:

```python
raw_labels = ["cat", "dog", "cat", "bird", "dog", "cat", "dog", "fish"]
```

## 🛠️ Yêu cầu
1. **Khởi tạo** một từ điển rỗng `label_counts = {}`.
2. **Duyệt qua từng phần tử** trong danh sách `raw_labels`, sử dụng phương thức `.get()` để cập nhật số lần xuất hiện của mỗi loài động vật.
3. **Tìm nhãn** có số lần xuất hiện nhiều nhất (nhãn đa số / majority class).

## 💡 Gợi ý thuật toán
* **Cú pháp đếm bằng `.get()`:** 
  ```python
  label_counts[label] = label_counts.get(label, 0) + 1
  ```
* **Để tìm nhãn xuất hiện nhiều nhất, bạn có thể áp dụng 2 cách:**
  * ✨ **Cách 1 (Ngắn gọn):** Sử dụng hàm `max()` kết hợp với key: 
    ```python
    majority_class = max(label_counts, key=label_counts.get)
    ```
  * 🔄 **Cách 2 (Tường minh):** Sử dụng vòng lặp duyệt qua các cặp `key, value` với phương thức `label_counts.items()`.  

  # 🔍 Bài tập 3: Kiểm định rò rỉ nhãn phân loại (Label Leakage & Unseen Label Check)

## 🎯 Mục tiêu
Áp dụng các phép toán tập hợp của `set` (như phép giao `&` và phép hiệu `-`) để so sánh sự nhất quán về nhãn giữa tập huấn luyện (train set) và tập kiểm tra (test set).

## 📄 Mô tả bài toán
Trong pipeline học máy, nếu tập test xuất hiện các nhãn mới mà tập train chưa từng có, các bộ mã hóa nhãn (Encoder) sẽ bị lỗi hoặc gán sai thông tin. 

Cho hai danh sách chứa dữ liệu thành phố dưới đây:

```python
train_cities = ["Hà Nội", "TP.HCM", "Đà Nẵng", "Cần Thơ", "Hải Phòng"]
test_cities = ["TP.HCM", "Huế", "Đà Nẵng", "Nha Trang"]
```

## 🛠️ Yêu cầu
1. **Chuyển đổi** hai danh sách trên thành hai tập hợp (`set`).
2. **Tìm các thành phố** xuất hiện ở cả hai tập (`common_cities`).
3. **Tìm các thành phố mới** ở tập test mà tập train chưa từng có (`unseen_cities`).
4. **Viết điều kiện kiểm tra**: Nếu tồn tại thành phố mới (`len(unseen_cities) > 0`), in ra một cảnh báo yêu cầu người dùng thiết lập chế độ `handle_unknown='ignore'` cho bộ mã hóa dữ liệu.

## 💡 Gợi ý thuật toán
* 🤝 **Tìm phần tử chung:** Dùng toán tử giao giữa hai tập hợp: `train_set & test_set`.
* 🌵 **Tìm phần tử khác biệt:** Dùng toán tử hiệu để lấy các phần tử chỉ có ở tập test: `test_set - train_set`.
