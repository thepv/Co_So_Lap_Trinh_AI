
# ĐỀ MẪU KIỂM TRA 01B

**Học phần:** Cơ sở lập trình cho trí tuệ nhân tạo  
**Thời gian:** 45 phút, tự luận viết mã Python trên giấy.  
**Phạm vi:** Chương 1–2

Sinh viên viết mã hợp lệ, có thụt lề; có thể dùng `numpy as np`, `pandas as pd`. Không cần viết lệnh nhập file hoặc giao diện. Mỗi câu độc lập. Kết quả mẫu dùng để làm rõ yêu cầu, không thay cho code.

## Câu 1 — Lọc và biến đổi mảng NumPy

**Hợp đồng hàm (viết đúng chữ ký và type hint trong bài làm):**

```python
def process_values(raw: list[int | float]) -> tuple[np.ndarray, np.ndarray]:
    ...
```

- `raw`: danh sách số biểu diễn độ ẩm cảm biến; có thể rỗng, không chứa chuỗi.
- Trả về `valid` và `transformed`: hai `np.ndarray` kiểu float, cùng chiều dài, giữ thứ tự đầu vào; `transformed` là kết quả `valid / 100` trên `valid`.
- Không sửa danh sách đầu vào; khi `raw` rỗng, trả hai mảng rỗng.

**Yêu cầu:** Miền hợp lệ là [0, 100].

- **1.1:** Chuyển danh sách đầu vào sang mảng NumPy kiểu float và trả đúng cặp mảng.
- **1.2:** Tạo mặt nạ chọn giá trị trong miền hợp lệ, bao gồm hai đầu biên.
- **1.3:** Giữ nguyên thứ tự và chỉ giữ phần tử hợp lệ; danh sách rỗng cho hai mảng rỗng.
- **1.4:** Biến đổi toàn bộ mảng hợp lệ theo công thức bằng phép toán vector hóa, không dùng vòng lặp.

**Ví dụ:**

```python
raw = [40, -1, 75, 120, 0]

valid.tolist() == [40.0, 75.0, 0.0]
transformed.tolist() == [0.4, 0.75, 0.0]
```

## Câu 2 — Chương trình xử lý bảng có cấu trúc

**Hợp đồng hàm (viết đúng chữ ký và type hint trong bài làm):**

```python
def clean_records(df: pd.DataFrame) -> pd.DataFrame:
    ...

def build_report(cleaned: pd.DataFrame) -> pd.DataFrame:
    ...

def run_pipeline(df: pd.DataFrame) -> pd.DataFrame:
    ...
```

- `df`: bảng gốc có `sample_id: str`, `station: str`, `humidity: str`; `clean_records` trả bản sao đã ép/lọc cột số, không sửa `df`.
- `cleaned`: bảng do `clean_records` trả về; `build_report` trả bảng có đúng `station`, `count`, `mean_value`, đã sắp nhóm tăng dần và đánh lại chỉ số.
- `run_pipeline(df)`: nối hai hàm trên và trả bảng báo cáo. Nếu không có hàng hợp lệ, trả bảng rỗng với đúng ba cột.

**Yêu cầu:** Chỉ giữ hàng có độ ẩm nằm trong đoạn [0, 100].

- **2.1:** Ép cột số bằng `pd.to_numeric(errors='coerce')`.
- **2.2:** Loại giá trị thiếu và hàng ngoài miền hợp lệ.
- **2.3:** Nhóm theo cột phân loại, đếm hàng và tính trung bình đúng.
- **2.4:** Sao chép DataFrame đầu vào; không sửa dữ liệu gốc.
- **2.5:** Phân rã `clean_records` và `build_report`; mỗi hàm trả kết quả đúng hợp đồng.
- **2.6:** `run_pipeline` nối đúng hai bước và trả báo cáo.
- **2.7:** Báo cáo có đúng ba cột, sắp nhóm tăng dần và đánh lại chỉ số.
- **2.8:** Không dùng ID để tính; đầu vào không có hàng hợp lệ cho bảng rỗng đúng schema.

### Ví dụ đầu vào

| sample_id | station | humidity |
|---|---|---:|
| M1 | X | 40 |
| M2 | X | hỏng |
| M3 | Y | 75 |
| M4 | Y | 25 |
| M5 | X | 120 |

### Kết quả `run_pipeline(df)`

| station | count | mean_value |
|---|---:|---:|
| X | 1 | 40.0 |
| Y | 2 | 50.0 |

