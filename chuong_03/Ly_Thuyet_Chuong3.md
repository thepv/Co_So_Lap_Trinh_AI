# 🧼 CHƯƠNG 3: Làm sạch, biến đổi và mã hóa dữ liệu

* **Chuẩn đầu ra:** CLO2, CLO3  
* **Thời lượng:** 3 tiết lý thuyết · 60 giờ tự học  
* **Khung bài giảng:** Biến dữ liệu “có thể đọc” thành dữ liệu “có thể tin”

> 💡 **Nguyên tắc cốt lõi:** Mỗi phép xử lý phải gắn với quy tắc miền dữ liệu, có số liệu trước/sau và có thể lặp lại. Outlier thống kê không đồng nghĩa với lỗi cần xóa.

---

## 🎯 Mục tiêu chương
Sau chương này, người học có thể:
* Nhận diện chính xác các loại lỗi dữ liệu phổ biến.
* Phát hiện và có chiến lược xử lý outlier (giá trị ngoại lệ) phù hợp.
* Làm sạch dữ liệu số và dữ liệu chuỗi văn bản.
* Chuẩn hóa dữ liệu số theo nhiều thang đo khác nhau.
* Mã hóa biến phân loại đúng theo ý nghĩa và bản chất dữ liệu.

---

## 3.1. Làm sạch dữ liệu 🛠️

Làm sạch dữ liệu là bước biến dữ liệu thô thành dữ liệu có thể phân tích, có thể giải thích và có thể đưa vào pipeline AI mà không làm sai ý nghĩa ban đầu. Dữ liệu thô hiếm khi chỉ “bẩn” theo một cách duy nhất: cùng một bảng có thể vừa có giá trị khuyết thiếu, sai kiểu, trùng định danh, đơn vị đo không nhất quán, chuỗi nhập khác chuẩn và outlier. 

Vì vậy, làm sạch dữ liệu không phải là một lệnh đơn lẻ mà là một quy trình gồm: **lập hồ sơ lỗi ➡️ quyết định quy tắc xử lý ➡️ áp dụng trên bản sao ➡️ kiểm tra lại kết quả ➡️ ghi lại tác động**.

⚠️ **Nguyên tắc quan trọng:** Không sửa dữ liệu chỉ để “đẹp bảng”. Mỗi thay đổi phải trả lời được ba câu hỏi:
1. Lỗi được phát hiện bằng bằng chứng nào?
2. Quy tắc sửa dựa trên hiểu biết miền hay giả định nào?
3. Số bản ghi bị tác động là bao nhiêu?

*Bản dữ liệu gốc phải được giữ bất biến; bản đã làm sạch là một sản phẩm có thể tái tạo từ dữ liệu gốc và mã xử lý.*

### 3.1.1. Nhận diện nhiễu, sai lệch và các lỗi phổ biến 🔍

Nhận diện lỗi bắt đầu bằng lập hồ sơ dữ liệu. Trước khi xóa, điền hoặc chuyển đổi bất kỳ cột nào, cần quan sát cấu trúc tổng thể: số dòng, số cột, tên cột, kiểu dữ liệu, số giá trị thiếu, số định danh trùng, miền giá trị hợp lệ và các mức danh mục đang xuất hiện. One bảng “không có NaN” vẫn có thể bẩn nếu tuổi được ghi là `250`, điểm được ghi là chuỗi `"8,5"`, thành phố được nhập thành nhiều biến thể, hoặc cùng một sinh viên xuất hiện nhiều lần.

Các lỗi phổ biến có thể nhóm theo bản chất như sau:
* 🕳️ **Dữ liệu khuyết:** `NaN`, `None`, chuỗi rỗng, hoặc mã giả như `"N/A"`, `"unknown"`, `-999`.
* 🔀 **Sai kiểu:** Cột số đang ở kiểu chuỗi, cột ngày chưa được parse, cột boolean dùng nhiều cách ghi như `"yes"`, `"Y"`, `1`.
* 📐 **Sai miền:** Giá trị ngoài khoảng hợp lệ, chẳng hạn tuổi âm, điểm lớn hơn 10, tỷ lệ ngoài đoạn $[0, 1]$.
* ⚖️ **Không nhất quán đơn vị:** Cùng một cột trộn mét với centimet, triệu đồng với đồng, hoặc ngày/tháng với tháng/ngày.
* 🏷️ **Không nhất quán danh mục:** `"TP.HCM"`, `"tp hcm"`, `"Ho Chi Minh"` cùng chỉ một địa danh nhưng được ghi khác nhau.
* 👥 **Trùng lặp (duplicate):** Trùng toàn bộ dòng hoặc trùng khóa định danh do nhập lại, gộp dữ liệu từ nhiều nguồn, hoặc thu thập lặp.
* 📉 **Sai lệch thu thập:** Dữ liệu thiếu hoặc phân bố lệch theo một nhóm người dùng, thiết bị, thời điểm hoặc kênh thu thập; loại sai lệch này không thể sửa chỉ bằng code.

Dữ liệu khuyết cần được xem như một tín hiệu, không chỉ là ô trống. Thiếu dữ liệu do người dùng không trả lời câu hỏi nhạy cảm khác với thiếu dữ liệu do lỗi hệ thống; thiếu ngẫu nhiên khác với thiếu tập trung ở một nhóm. Do đó, các ngưỡng như “dưới 5% thì xóa” hoặc “trên 50% thì bỏ cột” chỉ là gợi ý khám phá, không phải quy tắc chung. Quyết định xóa hay điền phải dựa vào cơ chế thiếu dữ liệu, tỷ lệ bị ảnh hưởng và mục tiêu bài toán.

#### Bảng 3.1. Một số lỗi dữ liệu phổ biến và cách phát hiện ban đầu 📊

| Dấu hiệu | Cách phát hiện | Rủi ro nếu bỏ qua |
| :--- | :--- | :--- |
| **Giá trị khuyết thiếu** | `isna()`, tỷ lệ khuyết thiếu theo cột/nhóm | Mô hình lỗi, mất mẫu hoặc lệch phân bố. |
| **Sai kiểu** | `dtypes`, `to_numeric` thử nghiệm | Tính toán sai, sort/lọc không đúng nghĩa số. |
| **Sai miền** | Kiểm tra khoảng hợp lệ bằng `between` | Kéo lệch thống kê, tạo tín hiệu giả. |
| **Danh mục lệch chuẩn** | `value_counts()` sau chuẩn hóa chữ/khoảng trắng | Tách sai nhóm, one-hot sinh nhiều cột vô nghĩa. |
| **Trùng lặp** | `duplicated()` theo toàn dòng hoặc khóa | Đếm mẫu sai, rò rỉ tập train/tập test, tăng overfitting. |

#### 💻 Mã nguồn 34. Lập hồ sơ lỗi dữ liệu ban đầu

```python
import pandas as pd

raw = pd.DataFrame({
    "student_id": ["SV01", "SV02", "SV02", "SV03", "SV04", "SV05"],
    "age": [20, "21", "unknown", 250, None, 19],
    "score": [8.5, "7,0", 7.0, 12.0, None, -1.0],
    "city": ["TP.HCM", " tp.hcm", "Hà Nội", None, "ha noi", "Đà Nẵng"],
})

profile = pd.DataFrame({
    "dtype": raw.dtypes.astype(str),
    "missing_count": raw.isna().sum(),
    "missing_rate": raw.isna().mean().round(3),
    "unique_count": raw.nunique(dropna=True),
})

print("Kích thước:", raw.shape)
print("ID trùng:", int(raw["student_id"].duplicated().sum()))
print(profile)
print(raw)
```

**💡 Kết quả mong muốn:**
```text
Kích thước: (6, 4)
ID trùng: 1
            dtype  missing_count  missing_rate  unique_count
student_id  object              0         0.000             5
age         object              1         0.167             5
score       object              1         0.167             5
city        object              1         0.167             5

  student_id      age score     city
0       SV01       20   8.5   TP.HCM
1       SV02       21   7,0  tp.hcm
2       SV02  unknown   7.0   Hà Nội
3       SV03      250  12.0     None
4       SV04     None  None   ha noi
5       SV05       19  -1.0  Đà Nẵng
```

> 📝 **Nhận xét:** Sau bước lập hồ sơ, người học cần phân biệt lỗi chắc chắn với điểm cần xác minh. Tuổi `250` gần như là lỗi miền; một khách hàng chi tiêu rất cao có thể là khách hàng quan trọng; một giá trị `None` ở cột `city` có thể là không thu thập được, không áp dụng, hoặc người dùng từ chối cung cấp. Nếu không xác định được ý nghĩa, hãy gắn cờ và ghi giả định thay vì âm thầm sửa.

---

### 3.1.2. Phát hiện và xử lý outliers 🚨

Outlier là quan sát nằm xa phần lớn dữ liệu còn lại theo một tiêu chí nào đó. Outlier có thể là lỗi nhập liệu, lỗi cảm biến, sự kiện hiếm, hoặc một trường hợp hợp lệ nhưng đặc biệt quan trọng. 

Trong AI, outlier có thể làm lệch trung bình, độ lệch chuẩn, phép chuẩn hóa min-max, mô hình tuyến tính và nhiều phép tổng hợp khác. Tuy nhiên, xóa mọi outlier một cách tự động là thực hành nguy hiểm vì có thể loại bỏ chính tín hiệu mà bài toán cần học.

✌️ Có hai lớp outlier cần phân biệt:
* 🌐 **Outlier theo miền:** Vi phạm quy tắc chắc chắn của dữ liệu, chẳng hạn tuổi âm, tỷ lệ lớn hơn 1, ngày kết thúc trước ngày bắt đầu. Loại này thường cần sửa, gắn thiếu hoặc loại bỏ.
* 📈 **Outlier thống kê:** Khác biệt so với phân bố quan sát, chẳng hạn thu nhập rất cao. Loại này cần phân tích thêm trước khi quyết định xử lý.

Phương pháp IQR là một cách gắn cờ outlier thống kê phổ biến. Với tứ phân vị thứ nhất $Q_1$, tứ phân vị thứ ba $Q_3$ và $IQR = Q_3 - Q_1$, điểm nằm dưới $Q_1 - 1.5 \times IQR$ hoặc trên $Q_3 + 1.5 \times IQR$ được xem là đáng kiểm tra. Đây là heuristic khám phá, không phải bằng chứng tuyệt đối rằng dữ liệu sai.

#### 💻 Mã nguồn 35. Gắn cờ outlier bằng IQR mà không xóa tự động

```python
import pandas as pd

income = pd.Series([8, 9, 10, 11, 12, 13, 120], name="income_million")
q1, q3 = income.quantile([0.25, 0.75])
iqr = q3 - q1
lower, upper = q1 - 1.5 * iqr, q3 + 1.5 * iqr
is_outlier = ~income.between(lower, upper)

report = pd.DataFrame({
    "value": income,
    "is_outlier": is_outlier,
    "reason": is_outlier.map({True: "outside_iqr", False: "within_iqr"}),
})

print(f"Khoảng IQR hợp lệ: {lower:.2f} đến {upper:.2f}")
print(report)
```

**💡 Kết quả mong muốn:**
```text
Khoảng IQR hợp lệ: 5.00 đến 17.00
   value  is_outlier       reason
0      8       False   within_iqr
1      9       False   within_iqr
2     10       False   within_iqr
3     11       False   within_iqr
4     12       False   within_iqr
5     13       False   within_iqr
6    120        True  outside_iqr
```

Sau khi gắn cờ, có **bốn hướng xử lý** thường gặp:
1. 🔍 **Xác minh nguồn:** Đối chiếu log, biểu mẫu nhập, đơn vị đo hoặc người phụ trách miền dữ liệu.
2. 🛠️ **Sửa theo quy tắc miền:** Đổi đơn vị, sửa dấu thập phân, chuẩn hóa mã lỗi nếu quy tắc chắc chắn.
3. 🚩 **Giữ nguyên kèm cờ:** Giữ giá trị đặc biệt nhưng thêm biến `is_outlier` để mô hình hoặc báo cáo biết đây là điểm cần chú ý.
4. ✂️ **Cắt ngưỡng hoặc loại bỏ:** Winsorize/cap về ranh giới, hoặc lọc dòng; chỉ làm khi có lý do và báo cáo số bản ghi bị tác động.

#### 💻 Mã nguồn 36. So sánh giữ cờ, cắt ngưỡng và lọc outlier

```python
import pandas as pd

income = pd.Series([8, 9, 10, 11, 12, 13, 120], name="income_million")
q1, q3 = income.quantile([0.25, 0.75])
iqr = q3 - q1
lower, upper = q1 - 1.5 * iqr, q3 + 1.5 * iqr

clean = pd.DataFrame({"income_million": income})
clean["is_outlier"] = ~clean["income_million"].between(lower, upper)
clean["income_capped"] = clean["income_million"].clip(lower, upper)
filtered = clean.loc[~clean["is_outlier"]].copy()

print(clean)
print("Số dòng nếu lọc:", len(filtered), "/", len(clean))
```

**💡 Kết quả mong muốn:**
```text
   income_million  is_outlier  income_capped
0               8       False              8
1               9       False              9
2              10       False             10
3              11       False             11
4              12       False             12
5              13       False             13
6             120        True             17
Số dòng nếu lọc: 6 / 7
```

Use code with caution.
⚠️ Lưu ý quan trọng: Không nên dùng cùng một quy tắc outlier cho mọi cột. Cột định danh không có ý nghĩa thống kê để tính IQR; cột đếm có phân bố lệch tự nhiên; cột tiền tệ có thể cần log transform hoặc phân nhóm trước khi đánh giá. Trong pipeline học máy, mọi ngưỡng học từ dữ liệu như quantile, median hoặc ranh giới IQR phải được fit trên tập train, rồi áp dụng cho tập validation/tập test để tránh rò rỉ dữ liệu.

3.1.3. Làm sạch dữ liệu số và chuỗi 🔤

Sau khi biết vấn đề nằm ở đâu, bước tiếp theo là chuyển dữ liệu về dạng nhất quán.
• Làm sạch dữ liệu số thường gồm chuyển kiểu, xử lý ký hiệu thập phân, đưa mã lỗi về giá trị khuyết thiếu, kiểm tra miền hợp lệ và quyết định cách điền hoặc loại bỏ.
• Làm sạch dữ liệu chuỗi thường gồm chuẩn hóa Unicode, loại khoảng trắng thừa, thống nhất chữ hoa/thường, ánh xạ danh mục và loại trùng theo khóa định danh.
Chuyển kiểu phải có kiểm soát. Với Pandas, pd.to_numeric(..., errors="coerce") giúp chuyển giá trị không hợp lệ thành giá trị khuyết thiếu để thống kê được, thay vì làm chương trình dừng ngay. Tuy nhiên, sau khi ép kiểu phải báo số giá trị bị chuyển thành khuyết thiếu; nếu không, lỗi nhập liệu có thể bị che giấu.

💻 Mã nguồn 37. Làm sạch cột số có ghi nhận tác động

```python
import pandas as pd
raw_scores = pd.Series(["8.5", "7,0", "unknown", "12", "-1", None], name="score")
normalized = raw_scores.astype("string").str.strip().str.replace(",", ".", regex=False)
scores = pd.to_numeric(normalized, errors="coerce")
invalid_numeric = scores.isna() & raw_scores.notna()
out_of_range = scores.notna() & ~scores.between(0, 10)
clean_scores = scores.mask(out_of_range, pd.NA)
report = pd.DataFrame({
"raw": raw_scores,
"normalized": normalized,
"score": clean_scores,
"invalid_numeric": invalid_numeric,
"out_of_range": out_of_range,
})
print(report)
print("Số giá trị không chuyển được:", int(invalid_numeric.sum()))
print("Số giá trị ngoài miền:", int(out_of_range.sum()))
```
💡 Kết quả mong muốn:
text raw normalized score  invalid_numeric  out_of_range 0      8.5        8.5   8.5            False         False 1      7,0        7.0   7.0            False         False 2  unknown    unknown  <NA>             True         False 3       12         12  <NA>            False          True 4       -1         -1  <NA>            False          True 5     None       <NA>  <NA>            False         False Số giá trị không chuyển được: 1 Số giá trị ngoài miền: 2 
Với dữ liệu chuỗi tiếng Việt, không nên bỏ dấu nếu nhiệm vụ cần phân biệt nghĩa. Chuẩn hóa Unicode về NFC, gom khoảng trắng và ánh xạ danh mục là các bước an toàn hơn. Ánh xạ danh mục nên được viết rõ trong bảng quy tắc để người khác biết vì sao "tp hcm" và "TP.HCM" được xem là một giá trị.

💻 Mã nguồn 38. Làm sạch chuỗi, danh mục và dữ liệu trùng lặp

```python
import pandas as pd
import unicodedata
raw = pd.DataFrame({
"student_id": ["SV01", "SV02", "SV02", "SV03", "SV04"],
"city": ["TP.HCM", "  tp hcm ", "TP.HCM", "ha noi", None],
"score": [8.5, 7.0, 7.0, 9.0, None],
})
def normalize_text(value):
if pd.isna(value):
return pd.NA
value = unicodedata.normalize("NFC", str(value))
return " ".join(value.strip().lower().split())
city_map = {
"tp.hcm": "TP.HCM",
"tp hcm": "TP.HCM",
"ha noi": "Hà Nội",
"hà nội": "Hà Nội",
}
clean = raw.copy()
clean["city_normalized"] = clean["city"].map(normalize_text)
clean["city_clean"] = clean["city_normalized"].map(city_map).fillna("Không rõ")
clean["is_duplicate_id"] = clean["student_id"].duplicated(keep="first")
clean = clean.drop_duplicates(subset=["student_id"], keep="first")
print(clean)
```
💡 Kết quả mong muốn:
text student_id       city  score city_normalized city_clean  is_duplicate_id 0       SV01     TP.HCM    8.5         tp.hcm      TP.HCM            False 1       SV02    tp hcm     7.0         tp hcm      TP.HCM            False 3       SV03     ha noi    9.0         ha noi      Hà Nội            False 4       SV04       None    NaN           <NA>    Không rõ            False 
Khi xử lý trùng lặp, cần xác định rõ “trùng” theo tiêu chí nào. Trùng toàn bộ dòng thường dễ xử lý hơn trùng khóa định danh nhưng khác một vài thuộc tính. Nếu cùng một student_id có hai điểm khác nhau, không nên tự động giữ dòng đầu tiên nếu không biết dòng nào mới hơn hoặc đáng tin hơn. Khi có cột thời gian, có thể giữ bản ghi mới nhất; khi có cột nguồn dữ liệu, có thể ưu tiên nguồn có độ tin cậy cao hơn.
Một quy trình làm sạch tối thiểu cho mục 3.1 nên tạo được ba đầu ra: bảng dữ liệu đã xử lý, bảng hoặc dict thống kê tác động, và danh sách giả định cần xác nhận. Điều này giúp người học không chỉ viết code chạy được mà còn tạo được bằng chứng chất lượng dữ liệu trước khi chuyển sang biến đổi, chuẩn hóa và mã hóa ở các mục sau.

3.2. Biến đổi dữ liệu 📈

Sau khi phát hiện và xử lý các vấn đề cơ bản ở mục 3.1, dữ liệu vẫn thường chưa sẵn sàng để đưa vào mô hình. Nhiều cột có đơn vị đo khác nhau, khoảng giá trị chênh lệch mạnh, một số biến liên tục cần được gom thành nhóm dễ diễn giải, và các bước biến đổi nếu làm sai thời điểm có thể gây rò rỉ thông tin từ tập test sang tập train. Vì vậy, biến đổi dữ liệu không chỉ là thao tác làm đẹp bảng dữ liệu, mà là bước thiết kế lại biểu diễn của dữ liệu để mô hình học đúng tín hiệu cần học.
Trong phạm vi mục này, trọng tâm là các phép biến đổi trên dữ liệu số: chuẩn hóa thang đo, chuẩn hóa theo phân phối và rời rạc hóa.

3.2.1. Chuẩn hóa theo độ lệch chuẩn (standardization) và chuẩn hóa về miền giá trị (normalization) ⚖️

Các thuộc tính định lượng trong cùng một tập dữ liệu thường không cùng thang đo. Ví dụ, tuổi có thể nằm trong khoảng 18 đến 70, số năm kinh nghiệm chỉ khoảng 0 đến 40, còn thu nhập có thể lên tới hàng chục hoặc hàng trăm triệu đồng. Nếu đưa trực tiếp các cột này vào thuật toán dựa trên khoảng cách hoặc tối ưu hóa số học, cột có giá trị tuyệt đối lớn dễ chi phối phép tính dù nó không nhất thiết quan trọng hơn về mặt nghiệp vụ.
Chuẩn hóa thang đo đặc biệt quan trọng với KNN, SVM, hồi quy tuyến tính có regularization, mạng nơ-ron huấn luyện bằng gradient descent, PCA và các thuật toán phân cụm như K-Means. Với cây quyết định, random forest hoặc gradient boosting dựa trên cây, việc chuẩn hóa thang đo thường ít ảnh hưởng hơn vì mô hình chủ yếu so sánh ngưỡng trên từng biến riêng lẻ. Dù vậy, trong một pipeline hỗn hợp, việc chuẩn hóa thang đo vẫn cần được quyết định nhất quán và ghi lại rõ ràng.

1. Min-max normalization

Chuẩn hóa min-max đưa giá trị về một khoảng cố định, phổ biến nhất là $[0, 1]$:
$$x' = \frac{x - x_{min}}{x_{max} - x_{min}}$$
Cách này giữ nguyên thứ tự và hình dạng tương đối của phân phối ban đầu, nên phù hợp khi cần giới hạn miền giá trị đầu vào, ví dụ dữ liệu ảnh, dữ liệu cảm biến hoặc các mô hình yêu cầu đặc trưng nằm trong một khoảng xác định. Nhược điểm là phương pháp này rất nhạy với outlier: chỉ một giá trị cực lớn cũng có thể kéo $x_{max}$ lên cao và nén phần lớn các quan sát còn lại vào một vùng rất hẹp.

2. Z-score standardization

Standardization chuyển mỗi giá trị thành số độ lệch chuẩn so với trung bình:
$$z = \frac{x - \mu}{\sigma}$$
Sau biến đổi, cột dữ liệu có trung bình xấp xỉ 0 và độ lệch chuẩn xấp xỉ 1. Cách này thường được dùng khi thuật toán giả định dữ liệu được căn quanh trung tâm hoặc khi các hệ số mô hình cần so sánh trên cùng đơn vị chuẩn hóa. Cần lưu ý rằng z-score không làm dữ liệu tự động trở thành phân phối chuẩn. Ngoài ra, cả trung bình $\mu$ và độ lệch chuẩn $\sigma$ đều nhạy với outlier, nên standardization cũng có thể bị méo nếu dữ liệu có giá trị bất thường mạnh. Trong trường hợp đó, cần xử lý outlier trước hoặc cân nhắc chuẩn hóa bền vững dựa trên median và khoảng tứ phân vị.

3. Chuẩn hóa bền vững (robust scaling)

Khi dữ liệu có các giá trị cực trị hợp lệ và không nên loại bỏ, có thể lấy median làm tâm và IQR làm thang đo:
$$x' = \frac{x - \text{median}(x)}{Q_3 - Q_1}$$
Median và IQR ít bị một vài giá trị rất lớn hoặc rất nhỏ kéo lệch hơn mean và độ lệch chuẩn. Tuy nhiên, chuẩn hóa bền vững không làm outlier biến mất và không thay thế bước xác minh nghiệp vụ. Nếu $IQR = 0$, cột không có đủ độ biến thiên theo thước đo này; pipeline phải đặt chính sách rõ ràng thay vì chia cho 0.

Bảng 3.2. So sánh một số kỹ thuật chuẩn hóa dữ liệu số 📊


Kỹ thuật	Khi nên dùng	Lưu ý chính
Chuẩn hóa min-max	Cần đưa dữ liệu về khoảng cố định như $[0, 1]$	Nhạy với giá trị nhỏ nhất và lớn nhất; cần xử lý cột hằng
Chuẩn hóa z-score	Thuật toán dùng khoảng cách, gradient hoặc hệ số tuyến tính	Không tạo phân phối chuẩn; vẫn nhạy với outlier
Chuẩn hóa bền vững	Dữ liệu có outlier nhưng vẫn cần chuẩn hóa thang đo	Dựa trên median và IQR nên ổn định hơn, nhưng không thay thế bước hiểu nghiệp vụ


💻 Mã nguồn 39. So sánh chuẩn hóa min-max, z-score và chuẩn hóa bền vững

```python
import pandas as pd
sales = pd.DataFrame({
"age":,
"income_million":,
})
features = ["age", "income_million"]
min_value = sales[features].min()
max_value = sales[features].max()
range_value = max_value - min_value
sales_minmax = (sales[features] - min_value) / range_value.replace(0, 1)
sales_minmax = sales_minmax.add_suffix("_minmax")
mean_value = sales[features].mean()
std_value = sales[features].std()
sales_zscore = (sales[features] - mean_value) / std_value.replace(0, 1)
sales_zscore = sales_zscore.add_suffix("_zscore")
median_value = sales[features].median()
q1 = sales[features].quantile(0.25)
q3 = sales[features].quantile(0.75)
iqr_value = q3 - q1
sales_robust = (sales[features] - median_value) / iqr_value.replace(0, 1)
sales_robust = sales_robust.add_suffix("_robust")
result = pd.concat([sales, sales_minmax, sales_zscore, sales_robust], axis=1)
print(result.round(3))
```
💡 Kết quả mong muốn:
text age  income_million  age_minmax  income_million_minmax  age_zscore  income_million_zscore  age_robust  income_million_robust 0   22               8       0.000                  0.000      -0.983                 -0.718      -0.571                 -0.588 1   25              12       0.115                  0.036      -0.694                 -0.632      -0.286                 -0.353 2   29              18       0.269                  0.089      -0.308                 -0.503       0.000                  0.000 3   36              35       0.538                  0.241       0.366                 -0.137       1.000                  1.000 4   48             120       1.000                  1.000       1.619                  1.990       2.714                  6.000 
⚙️ Mẹo lập trình: Đoạn mã trên cố tình dùng .replace(0, 1) cho mẫu số để tránh lỗi khi một cột là cột hằng. Trong bài toán thật, không nên chỉ sửa mẫu số rồi bỏ qua ý nghĩa của cột hằng: nếu một biến không thay đổi trên toàn bộ tập dữ liệu huấn luyện, biến đó thường không đóng góp thông tin phân biệt cho mô hình.

3.2.2. Các lưu ý khi chuyển đổi dữ liệu ⚠️

Một lỗi phổ biến khi biến đổi dữ liệu là tính tham số trên toàn bộ bảng trước khi chia tập train/tập test.
Khi chuẩn hóa thang đo dữ liệu số, các tham số như giá trị nhỏ nhất, giá trị lớn nhất, trung bình, độ lệch chuẩn, trung vị hoặc IQR phải được học (fit) từ tập train. Tập test chỉ được biến đổi (transform) bằng đúng các tham số đã học đó. Nếu tính tham số trên cả tập test, mô hình đã gián tiếp nhìn thấy phân phối của dữ liệu đánh giá, làm kết quả kiểm thử lạc quan hơn thực tế (gây ra tình trạng Rò rỉ dữ liệu - Data Leakage).

💻 Mã nguồn 40. Khớp bộ chuẩn hóa trên tập train để tránh rò rỉ dữ liệu

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler, RobustScaler, StandardScaler
customer = pd.DataFrame({
"age":,
"income_million":,
"bought":,
})
X = customer[["age", "income_million"]]
y = customer["bought"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)
minmax_scaler = MinMaxScaler()
X_train_minmax = minmax_scaler.fit_transform(X_train)
X_test_minmax = minmax_scaler.transform(X_test)
standard_scaler = StandardScaler()
X_train_zscore = standard_scaler.fit_transform(X_train)
X_test_zscore = standard_scaler.transform(X_test)
robust_scaler = RobustScaler()
X_train_robust = robust_scaler.fit_transform(X_train)
X_test_robust = robust_scaler.transform(X_test)
print(pd.DataFrame(X_train_minmax, columns=X.columns).round(3))
print(pd.DataFrame(X_test_zscore, columns=X.columns).round(3))
print(pd.DataFrame(X_test_robust, columns=X.columns).round(3))
```
💡 Kết quả mong muốn:
```text
age  income_million
0  0.000           0.000
1  1.000           0.777
2  0.156           0.089
3  0.578           1.000
4  0.311           0.241
5  0.844           0.643
age  income_million
0 -1.153          -1.142
1  0.515           0.016
age  income_million
0 -0.648          -0.659
1  0.381           0.036
```
💡 Quy tắc fit trên tập train rồi transform trên tập test cũng áp dụng cho các phép biến đổi khác như mã hóa phân loại, điền giá trị khuyết thiếu theo thống kê, chọn đặc trưng, PCA hoặc rời rạc hóa theo phân vị. Với dữ liệu mới trong vận hành, hệ thống phải dùng lại cùng bộ chuẩn hóa, cùng ánh xạ và cùng ranh giới đã lưu từ giai đoạn huấn luyện.
🛑 Không chuẩn hóa thang đo mọi cột một cách máy móc. Không nên chuẩn hóa thang đo mã định danh như mã khách hàng, mã hóa đơn, số điện thoại hoặc mã bưu chính, vì các con số này không mang ý nghĩa độ lớn. Với biến nhị phân $0/1$, cần xem mô hình và pipeline cụ thể trước khi chuẩn hóa; nhiều trường hợp có thể giữ nguyên. Với biến đã là tỉ lệ hoặc phần trăm, cần kiểm tra miền giá trị đang là 0 đến 1 hay 0 đến 100 để tránh biến đổi hai lần.

Rời rạc hóa và gom nhóm dữ liệu 📦

Rời rạc hóa (discretization) hoặc gom nhóm dữ liệu (binning) biến một thuộc tính số liên tục thành các khoảng hoặc nhóm rời rạc. Cách này hữu ích khi muốn giảm nhiễu từ dao động nhỏ, tạo nhóm dễ diễn giải cho báo cáo, hoặc biểu diễn mối quan hệ phi tuyến theo từng vùng. Ví dụ, tuổi có thể được gom thành nhóm “dưới 18”, “18-39”, “40 trở lên”; giá nhà có thể được chia thành bốn phân hạng theo phân vị để mỗi nhóm có số lượng mẫu gần tương đương.
Pandas cung cấp hai hàm thường dùng:
• pd.cut(): Chia dữ liệu theo các ranh giới cố định hoặc các khoảng có độ rộng bằng nhau. Phù hợp khi ranh giới có ý nghĩa nghiệp vụ rõ ràng.
• pd.qcut(): Chia dữ liệu theo phân vị để các nhóm có tần suất (số lượng mẫu) gần bằng nhau. Phù hợp khi mục tiêu là cân bằng số lượng quan sát giữa các nhóm.

💻 Mã nguồn 41. Rời rạc hóa bằng pd.cut và pd.qcut

```python
import pandas as pd
customers = pd.DataFrame({
"age":,
"house_price":,
})
age_bins = [0, 18, 40, 100]
age_labels = ["under_18", "18_to_39", "40_plus"]
customers["age_group"] = pd.cut(customers["age"], bins=age_bins, labels=age_labels, right=False)
customers["price_rank"] = pd.qcut(customers["house_price"], q=4, labels=["low", "medium", "high", "premium"])
print(customers)
```
💡 Kết quả mong muốn:
text age  house_price age_group price_rank 0   15          900  under_18        low 1   22         1200  18_to_39        low 2   28         1500  18_to_39     medium 3   35         2600  18_to_39     medium 4   42         3100   40_plus       high 5   58         4500   40_plus    premium 6   65         8000   40_plus    premium 
Khi dùng binning cho mô hình học máy, ranh giới nhóm cũng là tham số tiền xử lý và cần được xác định từ tập train. Không nên chọn ranh giới chỉ để làm đẹp kết quả trên tập test. Ngoài ra, binning làm mất một phần thông tin chi tiết của biến liên tục, nên cần cân nhắc giữa khả năng diễn giải và độ chính xác dự báo.
🔗 Liên hệ với mã hóa dữ liệu phân loại: Sau khi phân khoảng (binning), cột mới thường là cột phân loại và cần được mã hóa trước khi đưa vào nhiều mô hình số học. Nội dung mã hóa nhãn (label encoding) và mã hóa one-hot được trình bày riêng ở mục 3.3 để tránh trộn giữa biến đổi biến số và mã hóa biến phân loại. Nguyên tắc cần nhớ là: mã hóa nhãn chỉ phù hợp khi các nhãn có thứ bậc thật sự; mã hóa one-hot phù hợp hơn với nhãn không có thứ bậc như miền, màu sắc hoặc loại sản phẩm.

⏱️ Kiểm tra nhanh trước khi biến đổi dữ liệu

• Biến này là dữ liệu đo lường, mã định danh, nhãn phân loại hay giá trị đã được mã hóa?
• Phép biến đổi có học tham số nào từ dữ liệu không, ví dụ min, max, mean, median, IQR hoặc ranh giới phân vị?
• Các tham số đó đã được khớp trên tập train và chỉ dùng để biến đổi tập test chưa?
• Outlier đã được hiểu về mặt nghiệp vụ trước khi chuẩn hóa thang đo hoặc phân khoảng chưa?
• Sau biến đổi, ý nghĩa của cột mới có còn giải thích được cho người dùng nghiệp vụ không?

📝 Bài tập thực hành

1. Cho một bảng có cột income_million. Hãy tạo hai phiên bản biến đổi bằng min-max và z-score, sau đó so sánh ảnh hưởng của một outlier rất lớn.
2. Dùng pd.qcut() chia cột house_price thành bốn nhóm có số lượng mẫu gần bằng nhau và đặt tên cột mới là price_rank.
3. Giải thích vì sao không nên khớp bộ chuẩn hóa trên toàn bộ dữ liệu trước khi chia tập train/tập test.

3.3. Mã hóa dữ liệu phân loại 🏷️

Nhiều cột trong dữ liệu thực tế không phải số đo liên tục mà là nhãn phân loại: thành phố, miền, loại sản phẩm, hạng khách hàng, mức độ hài lòng hoặc trạng thái giao dịch. Con người đọc các nhãn này dễ dàng, nhưng phần lớn mô hình AI và học máy nhận đầu vào là ma trận số. Vì vậy, mã hóa dữ liệu phân loại là bước chuyển nhãn thành biểu diễn số mà vẫn giữ đúng ý nghĩa của biến ban đầu.
Điểm mấu chốt là không phải mọi nhãn đều giống nhau. Một số nhãn có thứ bậc tự nhiên, ví dụ “Thấp”, “Trung bình”, “Cao”; một số nhãn chỉ là tên nhóm, ví dụ màu sắc, thành phố hoặc ngành hàng. Nếu dùng cùng một cách mã hóa cho cả hai loại, mô hình có thể học quan hệ giả. Gán Hà Nội bằng 0, TP.HCM bằng 1 và Đà Nẵng bằng 2 có thể khiến một số mô hình hiểu nhầm rằng Đà Nẵng “lớn hơn” TP.HCM theo nghĩa số học, dù đây chỉ là các nhãn danh nghĩa.

Bảng 3.3. Chọn kỹ thuật mã hóa theo ý nghĩa của biến phân loại 🗂️


Loại biến phân loại	Ví dụ	Cách mã hóa thường dùng
Có thứ bậc	Mức hài lòng, hạng thẻ, trình độ học vấn	Mã hóa thứ bậc/nhãn với thứ tự được khai báo rõ
Không có thứ bậc	Thành phố, màu sắc, loại sản phẩm	Mã hóa one-hot hoặc biến thể phù hợp với số mức
Định danh	Mã khách hàng, số điện thoại, mã hóa đơn	Thường không mã hóa trực tiếp làm đặc trưng dự báo
Số mức rất lớn	Mã bưu chính, mã sản phẩm, user agent	Gom nhóm, lọc mức hiếm hoặc thiết kế đặc trưng khác


3.3.1. Mã hóa nhãn (label encoding) 🔢

Mã hóa nhãn chuyển mỗi nhãn thành một số nguyên. Trong bài giảng nhập môn, khái niệm này thường được dùng cho hai tình huống cần phân biệt rõ: mã hóa nhãn mục tiêu và mã hóa đặc trưng có thứ bậc. Với nhãn mục tiêu của bài toán phân loại, ví dụ “spam” và “not spam”, việc chuyển thành $0/1$ chỉ giúp thuật toán tính toán. Với đặc trưng đầu vào, chỉ nên dùng mã hóa nhãn hoặc mã hóa thứ bậc khi các mức có thứ tự thật sự.
Ví dụ, mức hài lòng có thể được ánh xạ “Thấp” $ \rightarrow 0$, “Trung bình” $\rightarrow 1$, “Cao” $\rightarrow 2$ vì thứ tự này có ý nghĩa nghiệp vụ. Ngược lại, nếu ánh xạ màu sắc “Đỏ” $\rightarrow 0$, “Xanh” $\rightarrow 1$, “Vàng” $\rightarrow 2$, ta đã tạo ra một trật tự giả. Các mô hình tuyến tính, KNN, SVM hoặc mạng nơ-ron có thể coi khoảng cách giữa các mã số là thông tin có ý nghĩa, làm sai tín hiệu học được.

💻 Mã nguồn 42. Mã hóa đặc trưng thứ bậc có kiểm tra

```python
import pandas as pd
survey = pd.DataFrame({
"student_id": ["SV01", "SV02", "SV03", "SV04"],
"satisfaction": ["Thấp", "Cao", "Trung bình", "Cao"],
})
satisfaction_order = {
"Thấp": 0,
"Trung bình": 1,
"Cao": 2,
}
survey["satisfaction_code"] = survey["satisfaction"].map(satisfaction_order)
unknown_level = survey.loc[survey["satisfaction_code"].isna(), "satisfaction"]
if not unknown_level.empty:
raise ValueError(f"Mức chưa khai báo: {unknown_level.unique().tolist()}")
print(survey)
```
💡 Kết quả mong muốn:
text student_id satisfaction  satisfaction_code 0       SV01         Thấp                  0 1       SV02          Cao                  2 2       SV03   Trung bình                  1 3       SV04          Cao                  2 
🛡️ Thói quen tốt: Đoạn mã trên không chỉ dùng map để chuyển nhãn thành số mà còn kiểm tra mức chưa khai báo. Đây là thói quen quan trọng vì dữ liệu vận hành có thể xuất hiện nhãn mới do nhập liệu sai, thay đổi biểu mẫu hoặc phát sinh nhóm người dùng mới. Nếu không kiểm tra, các giá trị chưa ánh xạ sẽ thành khuyết thiếu và có thể bị điền hoặc loại bỏ âm thầm.
Khi dùng thư viện học máy, cùng một quy tắc vẫn áp dụng: encoder phải được fit trên tập train, sau đó transform tập validation/tập test bằng đúng mapping đã học. Nếu tự sắp xếp nhãn theo alphabet, thứ tự có thể không phản ánh nghiệp vụ. Vì vậy, với biến thứ bậc, nên khai báo thứ tự bằng tài liệu miền dữ liệu hoặc data dictionary thay vì để công cụ tự đoán.

💻 Mã nguồn 43. OrdinalEncoder với thứ tự khai báo rõ

```python
import pandas as pd
from sklearn.preprocessing import OrdinalEncoder
train = pd.DataFrame({"education": ["Phổ thông", "Cử nhân", "Thạc sĩ"]})
test = pd.DataFrame({"education": ["Cử nhân", "Tiến sĩ"]})
education_order = [["Phổ thông", "Cử nhân", "Thạc sĩ", "Tiến sĩ"]]
encoder = OrdinalEncoder(categories=education_order)
train_code = encoder.fit_transform(train[["education"]])
test_code = encoder.transform(test[["education"]])
print(train_code.ravel().tolist())
print(test_code.ravel().tolist())
```
💡 Kết quả mong muốn:
text [0.0, 1.0, 2.0] [1.0, 3.0] 

3.3.2. Mã hóa one-hot 🔲

Mã hóa one-hot tạo một cột nhị phân cho mỗi mức của biến phân loại. Nếu cột region có ba mức “Bắc”, “Trung”, “Nam”, kết quả mã hóa có thể gồm ba cột region_Bắc, region_Trung và region_Nam; mỗi dòng nhận giá trị 1 ở cột tương ứng với nhóm của nó và 0 ở các cột còn lại. Cách biểu diễn này phù hợp với biến danh nghĩa vì không áp đặt quan hệ lớn nhỏ giữa các nhãn.
Trong Pandas, pd.get_dummies rất tiện cho phân tích khám phá dữ liệu (EDA), bài tập thực hành và các bảng nhỏ. Khi dùng trong pipeline học máy, cần bảo đảm tập train và tập test có cùng cấu trúc cột sau mã hóa. Nếu tập train không có một thành phố xuất hiện trong tập test, hoặc tập test thiếu một mức có trong tập train, hai ma trận đặc trưng có thể lệch số cột nếu xử lý thủ công.

💻 Mã nguồn 44. Mã hóa one-hot bằng Pandas

```python
import pandas as pd
customer = pd.DataFrame({
"customer_id":,
"region": ["Bắc", "Nam", "Trung", "Nam"],
"tier": ["Bạc", "Vàng", "Bạc", "Bạch kim"],
})
encoded = pd.get_dummies(customer, columns=["region"], dtype=int)
print(encoded)
```
💡 Kết quả mong muốn:
text customer_id      tier  region_Bắc  region_Nam  region_Trung 0            1       Bạc           1           0             0 1            2      Vàng           0           1             0 2            3       Bạc           0           0             1 3            4  Bạch kim           0           1             0 
Với scikit-learn, OneHotEncoder phù hợp hơn cho pipeline vì bộ mã hóa lưu lại tập danh mục đã học ở bước khớp. Tham số handle_unknown="ignore" giúp dữ liệu kiểm tra hoặc dữ liệu mới không làm chương trình lỗi khi xuất hiện mức chưa từng thấy; mức lạ sẽ được biểu diễn bằng toàn bộ số 0 ở nhóm cột tương ứng. Đây không phải là cách hiểu hoàn hảo về nhãn mới, nhưng là lựa chọn an toàn ở mức kỹ thuật để pipeline không dừng đột ngột.

💻 Mã nguồn 45. OneHotEncoder giữ cấu trúc tập train/tập test nhất quán

```python
import pandas as pd
from sklearn.preprocessing import OneHotEncoder
train = pd.DataFrame({"region": ["Bắc", "Nam", "Trung", "Nam"]})
test = pd.DataFrame({"region": ["Nam", "Tây"]})
encoder = OneHotEncoder(handle_unknown="ignore", sparse_output=False)
train_encoded = encoder.fit_transform(train[["region"]])
test_encoded = encoder.transform(test[["region"]])
columns = encoder.get_feature_names_out(["region"])
print(pd.DataFrame(train_encoded, columns=columns))
print(pd.DataFrame(test_encoded, columns=columns))
```
💡 Kết quả mong muốn:
```text
region_Bắc  region_Nam  region_Trung
0         1.0         0.0           0.0
1         0.0         1.0           0.0
2         0.0         0.0         1.0
3         0.0         1.0           0.0
region_Bắc  region_Nam  region_Trung
0         0.0         1.0           0.0
1         0.0         0.0           0.0
```

💡 Các lưu ý khi một-hot

One-hot làm tăng số cột theo số mức của biến. Nếu một biến có hàng trăm hoặc hàng nghìn mức, ma trận đặc trưng có thể rất rộng, thưa và khó học. Ví dụ, one-hot mã bưu chính, mã sản phẩm hoặc mã khách hàng thường không phải lựa chọn tốt nếu không có chiến lược gom nhóm. Có thể xử lý bằng cách gộp các mức hiếm thành Other, chỉ giữ các mức xuất hiện đủ nhiều, hoặc thiết kế đặc trưng mới có ý nghĩa hơn như vùng địa lý, nhóm sản phẩm, tần suất mua hàng.
Tham số drop_first=True trong Pandas hoặc drop="first" trong scikit-learn đôi khi được dùng để tránh đa cộng tuyến trong hồi quy tuyến tính không regularization. Tuy nhiên, không nên xem đây là mặc định cho mọi mô hình. Với cây quyết định, rừng ngẫu nhiên, boosting hoặc nhiều pipeline hiện đại, giữ đủ các cột one-hot thường dễ đọc và ít gây nhầm hơn. Quyết định bỏ một cột tham chiếu cần gắn với mô hình cụ thể.

⏱️ Checklist mã hóa dữ liệu phân loại

• Biến đang mã hóa là thứ bậc, danh nghĩa hay mã định danh?
• Mapping của biến thứ bậc có được xác nhận bằng data dictionary hoặc hiểu biết nghiệp vụ không?
• Bộ mã hóa đã được khớp trên tập train và dùng để biến đổi tập test chưa?
• Pipeline xử lý thế nào nếu dữ liệu mới có nhãn chưa từng thấy?
• Số cột sau one-hot có quá lớn so với số dòng và mục tiêu mô hình không?

📝 Bài tập thực hành

1. Cho cột Trinh_Do gồm “Phổ thông”, “Cử nhân”, “Thạc sĩ”, “Tiến sĩ”. Hãy tạo mapping thứ bậc và kiểm tra mức chưa khai báo.
2. Cho cột Mien gồm “Bắc”, “Trung”, “Nam”. Hãy dùng pd.get_dummies để mã hóa one-hot và giải thích vì sao không dùng mã hóa nhãn.
3. Tạo tập train/tập test, trong đó tập test có một mức danh mục chưa xuất hiện ở tập train. Dùng OneHotEncoder với tham số handle_unknown="ignore" và giải thích kết quả dòng có nhãn mới.

📦 Sản phẩm học tập

Lập báo cáo làm sạch dữ liệu gồm vấn đề, bằng chứng, quy tắc xử lý, số bản ghi bị ảnh hưởng, kết quả trước/sau và giả định cần chuyên gia miền xác nhận.

🏆 Bài tập tích hợp: Báo cáo chất lượng dữ liệu bàn giao

Với một file khách hàng thô có mã định danh trùng, tuổi ngoài miền, thu nhập khuyết thiếu và nhãn khu vực mới ở tập test:
1. Chia tập train/tập test trước khi học median, IQR, tham số chuẩn hóa thang đo hoặc tập danh mục;
2. Tạo từ điển audit_report ghi số dòng đầu vào, mã định danh trùng, giá trị sai miền, giá trị bị chặn ngưỡng hoặc loại, median dùng để điền và các giả định chưa được chuyên gia miền xác nhận;
3. So sánh StandardScaler với RobustScaler trên cột thu nhập và giải thích ảnh hưởng của outlier thay vì chỉ chọn theo kết quả nhìn thuận mắt;
4. Khớp OneHotEncoder(handle_unknown="ignore") trên tập train và biến đổi tập test; ghi lại các danh mục đã học và số nhãn mới gặp ở tập test;
5. Dùng assert để kiểm tra tập train/tập test có cùng số cột đặc trưng, không còn NaN ở các cột bắt buộc, mã định danh sau làm sạch là duy nhất và số dòng trước/sau khớp với nhật ký xử lý.

⏱️ Tóm tắt các mốc đánh giá chất lượng (Quality Gates)

• Phân biệt outlier thống kê với dữ liệu sai.
• Chọn min-max, z-score hoặc chuẩn hóa bền vững dựa trên phân bố và mục tiêu.
• Chọn cách mã hóa dựa trên kiểu/ý nghĩa dữ liệu và xử lý được nhãn mới.
• Không học tham số biến đổi từ tập validation/tập test.
• Duy trì nhật ký xử lý và áp dụng các cổng kiểm soát chất lượng (quality gates) cho mọi thay đổi.
