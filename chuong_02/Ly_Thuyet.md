# 🏆 CHƯƠNG 2: Tổ chức và thao tác dữ liệu với NumPy và Pandas

**🎯 Chuẩn đầu ra:** CLO1, CLO2, CLO3 | ⏱️ **Thời lượng:** 6 tiết - 12 giờ tự học
**📘 Khung bài giảng:** Từ cấu trúc dữ liệu Python đến dữ liệu dạng mảng và bảng

Người học chuyển từ thao tác từng phần tử sang tính toán vector hóa, rồi dùng nhãn có ý nghĩa để khảo sát và truy vấn dữ liệu bảng. Mọi ví dụ đều yêu cầu dự đoán kiểu dữ liệu, kích thước mảng và kết quả trước khi chạy.

---

## 🎯 Mục tiêu chương
Sau chương này, người học có thể:
- 🔹 Tạo và cắt lát mảng nhiều chiều; 
- 🔹 Giải thích tính toán vector hóa (vectorization) và cơ chế lan truyền (broadcasting); 
- 🔹 Xây dựng Series/DataFrame; 
- 🔹 Đọc, khảo sát, lọc, sắp xếp và xử lý giá trị khuyết thiếu ở mức nhập môn.

---

## 📊 2.1. NumPy và cấu trúc mảng nhiều chiều

### 📌 2.1.1. Khởi tạo, index và slice
Một `ndarray` lưu các phần tử cùng kiểu trong một cấu trúc có số chiều xác định. Ba thuộc tính cần kiểm tra đầu tiên là `ndim`, `shape` và `dtype`. Với mảng hai chiều, trục 0 thường là hàng/mẫu và trục 1 là cột đặc trưng.

**📝 Mã nguồn 23: Khởi tạo và cắt lát mảng NumPy**
```python
import numpy as np

scores = np.array([
    [8.0, 7.5, 9.0],
    [6.5, 8.0, 7.0],
    [9.0, 8.5, 9.5],
])

print(scores.shape, scores.dtype)  # (3, 3), float64
print(scores[1, 2])                # hàng 1, cột 2
print(scores[:2, 1:])              # hai hàng đầu, từ cột 1
print(scores[scores >= 8.5])       # Boolean indexing

# Kết quả mong muốn:
# (3, 3) float64
# 7.0
# [[7.5 9. ]
#  [8.  7. ]]
# [9. 9. 8.5 9.5]
```

> ⚠️ **Lưu ý:** Slicing thường trả về một *view*; sửa view có thể làm đổi mảng gốc. Khi cần dữ liệu độc lập, gọi `copy()`. Index âm đếm từ cuối; điểm dừng của slice không được lấy, tương tự các cấu trúc dữ liệu Python.

### 🧮 2.1.2. Tính toán vector hóa và cơ chế lan truyền
Tính toán vector hóa diễn đạt phép toán trên toàn mảng thay cho vòng lặp Python. Các phép `+`, `*` và so sánh mặc định thực hiện theo từng phần tử; `@` là nhân ma trận.

**📝 Mã nguồn 24: Tính toán vector hóa, tổng hợp và cơ chế lan truyền**
```python
import numpy as np

features = np.array([
    [170.0, 65.0, 20.0],
    [160.0, 52.0, 22.0],
    [180.0, 80.0, 21.0],
])

column_means = features.mean(axis=0)
centered = features - column_means  # (3, 3) - (3,)

print(column_means)
print(centered.mean(axis=0))
print(features.max(axis=0))

# Kết quả mong muốn:
# [170.         65.66666667 21.        ]
# [ 0.00000000e+00 -4.73695157e-15  0.00000000e+00]
# [180.  80.  22.]
```

> 💡 **Cơ chế lan truyền (Broadcasting):** so khớp kích thước mảng từ trục cuối về đầu. Hai kích thước tương thích khi chúng bằng nhau hoặc một trong hai bằng 1. Không chỉ hỏi "chạy được không"; phải kiểm tra kết quả có đúng ý nghĩa miền dữ liệu hay không.

### ⚡ 2.1.3. Tối ưu hiệu năng so với vòng lặp thường
NumPy thường nhanh hơn vì dữ liệu đồng nhất, bộ nhớ gọn và vòng lặp được thực hiện trong mã biên dịch. Tuy nhiên, kích thước nhỏ có thể không đáng để tối ưu; ưu tiên tính đúng và khả năng đọc trước. Benchmark phải tạo dữ liệu ngoài vùng đo, chạy lặp nhiều lần và báo cả kích thước đầu vào.

**📝 Mã nguồn 25: Benchmark có lặp bằng timeit**
```python
import timeit
import numpy as np

values_list = list(range(100_000))
values_array = np.arange(100_000)

list_time = timeit.timeit(
    "sum(x * x for x in values_list)", globals=globals(), number=20
)

array_time = timeit.timeit(
    "np.sum(values_array * values_array)", globals=globals(), number=20
)

print(f"List: {list_time:.4f}s; NumPy: {array_time:.4f}s")

# Kết quả mong muốn:
# List: 0.0684s; NumPy: 0.0009s
```

---

## 📑 2.2. Pandas và xử lý dữ liệu dạng bảng

### 🗂️ 2.2.1. Series và DataFrame
`Series` là dãy một chiều có index; `DataFrame` là cấu trúc bảng hai chiều được hợp thành từ các Series có chung index. Nhãn giúp biểu đạt ý nghĩa, nhưng index trùng vẫn được phép nên cần kiểm tra tính duy nhất khi index đóng vai trò định danh.

**📝 Mã nguồn 26: Tạo Series và DataFrame**
```python
import pandas as pd

levels = pd.Series(["Tốt", "Khá", "Tốt"], name="Xếp loại")
students = pd.DataFrame({
    "student_id": ["SV01", "SV02", "SV03"],
    "name": ["An", "Bình", "Chi"],
    "ai_score": [8.5, 7.0, 9.0],
    "attendance": [0.95, None, 0.90],
})

print(levels.value_counts())
print(students.dtypes)

# Kết quả mong muốn:
# Xếp loại
# Tốt    2
# Khá    1
# Name: count, dtype: int64
# student_id     object
# name           object
# ai_score      float64
# attendance    float64
# dtype: object
```

**📝 Mã nguồn 27: Căn chỉnh hai Series theo mã sinh viên**
```python
import pandas as pd

midterm = pd.Series({"SV01": 8.0, "SV02": 7.0, "SV03": 9.0})
final = pd.Series({"SV03": 8.5, "SV01": 9.0, "SV04": 7.5})

average = (midterm + final) / 2
print(average)
print("Thiếu một đầu điểm:", average[average.isna()].index.tolist())

# Kết quả mong muốn:
# SV01    8.50
# SV02     NaN
# SV03    8.75
# SV04     NaN
# dtype: float64
# Thiếu một đầu điểm: ['SV02', 'SV04']
```

**📝 Mã nguồn 28: Biểu diễn dữ liệu trạm và thời điểm bằng MultiIndex**
```python
import pandas as pd

measurements = pd.DataFrame({
    "station": ["HN", "HN", "DN", "DN"],
    "time": ["08:00", "09:00", "08:00", "09:00"],
    "temperature": [29.5, 30.2, 28.0, 28.7],
})

indexed = measurements.set_index(["station", "time"]).sort_index()

print(indexed.loc["HN"])                 # mọi thời điểm của trạm HN
print(indexed.loc[("DN", "09:00")])      # một quan sát cụ thể
print(indexed.reset_index())

# Kết quả mong muốn:
#           temperature
# time                 
# 08:00            29.5
# 09:00            30.2
# temperature    28.7
# Name: (DN, 09:00), dtype: float64
#   station   time  temperature
# 0      DN  08:00         28.0
# 1      DN  09:00         28.7
# 2      HN  08:00         29.5
# 3      HN  09:00         30.2
```

### 🔍 2.2.2. Đọc dữ liệu từ CSV/JSON và khảo sát dữ liệu
Sau khi đọc file, chưa nên biến đổi ngay. Hãy kiểm tra `shape`, `columns`, vài dòng đầu (`head`), kiểu dữ liệu (`info`), số lượng non-null và thống kê mô tả (`describe`).

**📝 Mã nguồn 29: Đọc CSV và khảo sát ban đầu**
```python
from io import StringIO
import pandas as pd

csv_text = """student_id,name,ai_score,attendance
SV01,An,8.5,0.95
SV02,Bình,7.0,
SV03,Chi,9.0,0.90
"""

df = pd.read_csv(StringIO(csv_text))
print(df.head())
df.info()
print(df.describe(include="all"))
print(df.isna().sum())
```

### ✂️ 2.2.3. Lọc, chọn, sắp xếp dữ liệu
`loc` chọn theo nhãn và điều kiện; `iloc` chọn theo vị trí nguyên. Khi kết hợp điều kiện Series, dùng `&` hoặc `|` và đặt từng điều kiện trong ngoặc.

**📝 Mã nguồn 30: Chọn, lọc và sắp xếp DataFrame**
```python
selected = df.loc[:, ["student_id", "ai_score"]]

passed = df.loc[
    (df["ai_score"] >= 8.0) & (df["attendance"].fillna(0) >= 0.8),
    ["student_id", "ai_score", "attendance"],
]

ranked = passed.sort_values(
    ["ai_score", "student_id"], ascending=[False, True]
)

print(selected.iloc[:2])
print(ranked)

# Kết quả mong muốn:
#   student_id  ai_score
# 0       SV01       8.5
# 1       SV02       7.0
#   student_id  ai_score  attendance
# 2       SV03       9.0        0.90
# 0       SV01       8.5        0.95
```

### 🧩 2.2.4. Xử lý giá trị khuyết thiếu
Giá trị khuyết thiếu có thể mang nghĩa "không đo", "không áp dụng" hoặc "chưa biết". Trước khi xóa hay điền, cần xác định ý nghĩa và tỷ lệ khuyết thiếu. Median (trung vị) phù hợp với dữ liệu lệch hoặc có outlier hơn trung bình.

**📝 Mã nguồn 31: Thống kê và điền giá trị khuyết thiếu có kiểm soát**
```python
missing_rate = df.isna().mean().mul(100).round(1)
print(missing_rate)

clean = df.copy()
attendance_median = clean["attendance"].median()
clean["attendance"] = clean["attendance"].fillna(attendance_median)

assert clean["student_id"].notna().all()
assert clean["attendance"].between(0, 1).all()
print(clean)

# Kết quả mong muốn:
# student_id     0.0
# name           0.0
# ai_score       0.0
# attendance    33.3
# dtype: float64
#   student_id  name  ai_score  attendance
# 0       SV01    An       8.5       0.950
# 1       SV02  Bình       7.0       0.925
# 2       SV03   Chi       9.0       0.900
```

---

## ✅ Hoạt động và kiểm tra đầu ra

**Phân tích một bảng dữ liệu sinh viên:** lập từ điển dữ liệu (data dictionary), kiểm tra lược đồ, thống kê giá trị khuyết thiếu, lọc bản ghi hợp lệ và giải thích một phép lan truyền.

**Bài tập tích hợp: khảo sát chất lượng cảm biến không khí.** Sử dụng một file CSV chứa thời điểm, mã trạm, nhiệt độ và độ ẩm của ba trạm:
1. Nạp dữ liệu và ghi lại kết quả của `shape`, `head()`, `info()`, `describe()` cùng tỷ lệ thiếu theo cột;
2. Chuyển hai cột nhiệt độ, độ ẩm sang mảng NumPy và tính một chỉ số nhiệt đơn giản bằng biểu thức vector hóa; ghi rõ kích thước mảng trước và sau phép tính;
3. Đặt `station` và `time` làm MultiIndex, rồi truy vấn toàn bộ quan sát của một trạm;
4. Xác định trạm có tỷ lệ khuyết thiếu cao nhất. Phân biệt dữ liệu khuyết do cảm biến lỗi với trường hợp không áp dụng trước khi chọn chiến lược xử lý;
5. Gắn cờ nhiệt độ ngoài miền vật lý đã thống nhất. Nếu bài toán cho phép thay thế, dùng median của đúng trạm và giữ lại cột cờ để tạo nhật ký xử lý;
6. Xuất báo cáo JSON gồm số dòng, tỷ lệ thiếu, số điểm bất thường và thống kê mô tả của các chỉ số đã tính.

**Chuẩn đầu ra cần đạt:**
- [x] Dự đoán đúng kích thước mảng và kết quả index/lan truyền cơ bản.
- [x] Giải thích được ảnh hưởng của kiểu dữ liệu, bố trí bộ nhớ và tính toán vector hóa đến hiệu năng.
- [x] Nạp, khảo sát, lọc và sắp xếp được CSV/JSON.
- [x] Nhận biết căn chỉnh theo nhãn và truy vấn được một MultiIndex cơ bản.
- [x] Giải thích được lựa chọn xử lý giá trị khuyết thiếu.
- [x] Phân biệt được vai trò của cấu trúc dữ liệu Python, NumPy và Pandas.
