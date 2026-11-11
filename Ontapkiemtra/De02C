
# ĐỀ MẪU KIỂM TRA LẦN 02C

**Học phần:** Cơ sở lập trình cho trí tuệ nhân tạo (0101103347)  
**Thời gian:** 45 phút, tự luận viết mã Python trên giấy.  
**Phạm vi:** Chương 1–4, trọng tâm Chương 3–4

Sinh viên viết mã hợp lệ, có thụt lề; có thể dùng `pandas as pd`, `matplotlib.pyplot as plt`, `re`, `unicodedata`. Không cần đọc file hay hiển thị cửa sổ biểu đồ. Mỗi câu độc lập. Chưa yêu cầu API Pipeline/ColumnTransformer của Chương 5.

## Câu 1 — Làm sạch văn bản và vẽ biểu đồ

**Hợp đồng hàm (viết đúng chữ ký và type hint trong bài làm):**

```python
def token_report(texts: list[str]) -> dict[str, int]:
    ...
```

- `texts`: danh sách câu tiếng Việt, có thể rỗng.
- Trả về dictionary ánh xạ mỗi token chữ thường đã chuẩn hóa NFC sang số lần xuất hiện trong toàn bộ danh sách; danh sách rỗng trả `{}`.
- Tác dụng phụ duy nhất được yêu cầu: vẽ bar chart của dictionary bằng `plt.bar` với nhãn hai trục.

**Yêu cầu:** Không dùng thư viện NLP ngoài Python chuẩn.

- **1.1:** Chuẩn hóa NFC và đổi chữ thường cho từng câu.
- **1.2:** Dùng regex lấy chuỗi chữ liên tiếp; bỏ số, dấu câu, dấu gạch dưới; giữ dấu Việt.
- **1.3:** Đếm tần suất trên toàn bộ danh sách và trả dictionary; danh sách rỗng cho `{}`.
- **1.4:** Vẽ bar chart theo số lần xuất hiện và gắn nhãn hai trục.

**Ví dụ:**

```python
texts = ["Ứng dụng nhanh!", "Nhanh và rõ.", "Ứng dụng rõ"]

# Số lần xuất hiện:
# ứng: 2
# dụng: 2
# nhanh: 2
# rõ: 2
# và: 1
```

Thứ tự cột theo thứ tự từ xuất hiện đầu tiên là đủ; không cần sắp xếp.

## Câu 2 — Ghép các bước tiền xử lý train/test

**Hợp đồng hàm (viết đúng chữ ký và type hint trong bài làm):**

```python
def prepare(
    train: pd.DataFrame,
    test: pd.DataFrame
) -> tuple[pd.DataFrame, pd.DataFrame]:
    ...
```

- `train`, `test`: hai bảng có cùng schema `feedback_id: str`, `rating: str`, `source: str`, `outcome: int`; train có ít nhất một giá trị số hợp lệ.
- Trả `(x_train, x_test)`: hai bảng đặc trưng giữ thứ tự hàng, cùng thứ tự cột; chỉ gồm `rating` và các cột one-hot của `source` học từ train.
- Không sửa `train`/`test`; category chỉ có ở test không tạo cột mới và được biểu diễn bằng toàn 0 ở các cột one-hot.

**Yêu cầu:** Miền giá trị hợp lệ của `rating` là 1–5.

- **2.1:** Ép kiểu cột số; chuyển giá trị không hợp lệ hoặc ngoài miền thành thiếu.
- **2.2:** Điền thiếu cột số và tạo cột one-hot cho cột phân loại.
- **2.3:** Ghép cột số và các cột one-hot thành DataFrame đặc trưng.
- **2.4:** Tính median chỉ từ train, không dùng test để khớp tham số.
- **2.5:** Dùng cùng median của train để điền thiếu cho train và test.
- **2.6:** Căn cột test theo train; category mới ở test cho toàn 0 ở các cột one-hot.
- **2.7:** Loại ID và target khỏi đặc trưng; giữ thứ tự hàng và schema đồng nhất.
- **2.8:** Hàm trả đúng cặp bảng và không sửa hai DataFrame đầu vào.

### Dữ liệu Train

| feedback_id | rating | source | outcome |
|---|---:|---|---:|
| F1 | 1 | Web | 0 |
| F2 | | App | 1 |
| F3 | 5 | Web | 0 |

### Dữ liệu Test

| feedback_id | rating | source | outcome |
|---|---:|---|---:|
| F4 | | Phone | 1 |
| F5 | 4 | App | 0 |

### Kết quả xử lý

Với dữ liệu trên, median train = `3.0`.

Kết quả có các cột: `rating`, `source_App`, `source_Web`.

**x_train:**

| rating | source_App | source_Web |
|---:|---:|---:|
| 1.0 | 0 | 1 |
| 3.0 | 1 | 0 |
| 5.0 | 0 | 1 |

**x_test:**

| rating | source_App | source_Web |
|---:|---:|---:|
| 3.0 | 0 | 0 |
| 4.0 | 1 | 0 |

**Lưu ý:** Lời giải có thể dùng `pd.get_dummies` và `reindex`, hoặc cách tương đương giữ đúng schema train.

---

*Tài liệu mẫu phục vụ học phần · 0101103347*
