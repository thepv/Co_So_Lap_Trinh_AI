# 📊 CHƯƠNG 4: Trực quan hóa dữ liệu và tiền xử lý văn bản

* **Chuẩn đầu ra:** CLO2, CLO3
* **Thời lượng:** 3 tiết lý thuyết · 6 giờ tự học
* **Khung bài giảng:** Nhìn thấy cấu trúc và chuẩn hóa văn bản có chủ đích

> 💡 **Nguyên tắc cốt lõi:** Biểu đồ bắt đầu từ câu hỏi phân tích; tiền xử lý văn bản bắt đầu từ thông tin cần bảo toàn cho nhiệm vụ phía sau.

---

## 🎯 Mục tiêu chương
Sau khi hoàn thiện chương này, người học có thể:
* Tạo và làm chủ 4 dạng biểu đồ cơ bản: bar chart (biểu đồ cột), line plot (biểu đồ đường), scatter plot (biểu đồ phân tán) và histogram (biểu đồ tần suất).
* Đọc và phân tích phân bố dữ liệu một cách khoa học, tránh suy diễn quá mức.
* Lựa chọn biểu đồ phù hợp theo mục tiêu phân tích trong pipeline AI.
* Đọc và giải thích ma trận nhầm lẫn (confusion matrix) ở mức nhập môn.
* Làm sạch khoảng trắng và ký tự đặc biệt bằng biểu thức chính quy (Regex).
* Thực hiện tokenization (tách từ/tách tiếng) cơ bản cho văn bản tiếng Việt.

---

## 4.1. Trực quan hóa dữ liệu 📉

Trực quan hóa dữ liệu là quá trình chuyển dữ liệu dạng bảng hoặc dạng số thành hình ảnh để con người nhìn thấy cấu trúc, xu hướng và bất thường nhanh hơn so với việc đọc từng dòng dữ liệu. Trong một bài toán AI, biểu đồ thường xuất hiện ở ba thời điểm: **khám phá dữ liệu ban đầu (EDA), kiểm tra kết quả tiền xử lý, và đánh giá mô hình**. Một biểu đồ tốt không chỉ đẹp, mà phải trả lời được một câu hỏi phân tích cụ thể và không làm người đọc hiểu sai dữ liệu.

🔍 Ở giai đoạn EDA, biểu đồ giúp phát hiện phân bố lệch, nhóm có ít mẫu, điểm outlier, quan hệ giữa hai biến hoặc sự thay đổi theo thời gian. Ở giai đoạn mô hình, biểu đồ có thể dùng để theo dõi loss theo epoch, so sánh metric giữa các mô hình hoặc xem lỗi tập trung ở nhóm dữ liệu nào. 

⚠️ **Tuy nhiên, biểu đồ chỉ là bằng chứng khám phá:**
* Histogram không tự chứng minh dữ liệu tuân theo phân bố chuẩn.
* Scatter plot không tự chứng minh quan hệ nhân quả.
* Một đường loss đẹp không đủ để kết luận mô hình tổng quát hóa tốt.

🛠️ **Các công cụ phổ biến trong Python:**
* **Matplotlib:** Thư viện nền tảng, cho phép kiểm soát chi tiết đến từng figure, axes, trục, nhãn, màu sắc và bố cục.
* **Seaborn:** Xây dựng trên Matplotlib, làm việc mượt mà với Pandas DataFrame và cung cấp cú pháp ngắn gọn cho các biểu đồ thống kê phức tạp.

### 4.1.1. Bar chart, line plot, scatter plot và histogram 📐

Bốn dạng biểu đồ cốt lõi trong mục này tương ứng với bốn kiểu câu hỏi phân tích thường gặp:

* 📊 **Bar chart (Biểu đồ cột):** Phù hợp khi câu hỏi là *“Nhóm nào lớn hơn?”*. Trục giá trị nên bắt đầu từ 0 để tránh phóng đại sự khác biệt. Nếu thể hiện giá trị trung bình, cần nói rõ cách tổng hợp và hiển thị độ biến thiên.
* 📈 **Line plot (Biểu đồ đường):** Phù hợp khi câu hỏi là *“Giá trị thay đổi ra sao theo thứ tự?”*. Thường dùng cho chuỗi thời gian hoặc epoch huấn luyện. Không nên nối các điểm nếu khoảng thời gian bị thiếu hoặc thứ tự trục chưa đúng.
* 🎯 **Scatter plot (Biểu đồ phân tán):** Phù hợp khi câu hỏi là *“Hai biến có cùng biến thiên không?”*. Cần kiểm tra outlier và các nhóm ẩn; luôn nhớ tương quan không đồng nghĩa với nhân quả.
* 🪵 **Histogram (Biểu đồ tần suất):** Phù hợp khi câu hỏi là *“Một biến phân bố thế nào?”*. Kết luận có thể thay đổi rõ rệt tùy thuộc vào số lượng bin, ranh giới bin và cỡ mẫu.

#### Bảng 4.1. Bốn dạng biểu đồ cốt lõi và mục tiêu phân tích tương ứng 📊

| Biểu đồ | Dữ liệu phù hợp | Câu hỏi nên trả lời |
| :--- | :--- | :--- |
| **Bar chart** | Nhóm rời rạc và giá trị tổng hợp | Nhóm nào cao/thấp hơn? |
| **Line plot** | Dữ liệu có thứ tự thời gian hoặc vòng lặp | Xu hướng tăng, giảm hay dao động? |
| **Scatter plot** | Hai biến số trên cùng từng quan sát | Hai biến có quan hệ hoặc cụm nào đáng chú ý? |
| **Histogram** | Một biến số và nhiều quan sát | Dữ liệu lệch, nhiều đỉnh, có khoảng trống hay outlier? |

#### 💻 Mã nguồn 47. Bốn biểu đồ cơ bản trong một khung

```python
import matplotlib.pyplot as plt
import pandas as pd

metrics = pd.DataFrame({
    "month":,
    "samples":,
    "accuracy": [0.68, 0.72, 0.71, 0.78, 0.80, 0.84],
    "group": ["A", "A", "B", "B", "C", "C"],
})

fig, axes = plt.subplots(2, 2, figsize=(10, 7))

# 1. Bar Chart
metrics.groupby("group")["samples"].mean().plot.bar(ax=axes[0, 0])
axes[0, 0].set_title("Số mẫu trung bình theo nhóm")
axes[0, 0].set_xlabel("Nhóm")
axes[0, 0].set_ylabel("Số mẫu")

# 2. Line Plot
axes[0, 1].plot(metrics["month"], metrics["accuracy"], marker="o")
axes[0, 1].set_title("Độ chính xác theo tháng")
axes[0, 1].set_xlabel("Tháng")
axes[0, 1].set_ylabel("Accuracy")

# 3. Scatter Plot
axes[1, 0].scatter(metrics["samples"], metrics["accuracy"])
axes[1, 0].set_title("Số mẫu và độ chính xác")
axes[1, 0].set_xlabel("Số mẫu")
axes[1, 0].set_ylabel("Accuracy")

# 4. Histogram
axes[1, 1].hist(metrics["accuracy"], bins=4, edgecolor="black")
axes[1, 1].set_title("Phân bố độ chính xác")
axes[1, 1].set_xlabel("Accuracy")
axes[1, 1].set_ylabel("Tần số")

fig.tight_layout()
plt.show()
```

---

### 4.1.2. Phân tích phân bố dữ liệu 🪵

Phân tích phân bố giúp trả lời câu hỏi: dữ liệu tập trung ở đâu, trải rộng đến mức nào, có lệch trái/lệch phải không, có nhiều cụm (nhiều đỉnh) không, và có điểm cực trị nào cần kiểm tra không? Đây là bước bắt buộc trước khi chọn cách điền giá trị khuyết thiếu, xử lý outlier, chuẩn hóa thang đo hoặc chọn kiến trúc mô hình.

⚠️ Histogram là công cụ đầu tiên để đọc phân bố, nhưng không nên đọc nó một mình. Số bin quá ít có thể che mất cấu trúc; số bin quá nhiều dễ làm biểu đồ bị nhiễu. Do đó, cần kết hợp histogram với các thống kê mô tả cốt lõi (`count`, `mean`, `median`, `min`, `max`, độ lệch, độ nhọn). **Boxplot (biểu đồ hộp)** là công cụ hỗ trợ tuyệt vời vì nó tóm tắt trực quan median, tứ phân vị và định vị các điểm tiềm ẩn outlier theo từng nhóm.

#### 💻 Mã nguồn 49. Kết hợp thống kê, histogram và boxplot

```python
import matplotlib.pyplot as plt
import pandas as pd

scores = pd.Series(
    [5.0, 6.0, 6.5, 7.0, 7.0, 7.5, 8.0, 8.5, 9.0, 10.0],
    name="score"
)

print(scores.describe())
print("Median:", scores.median())
print("Skewness:", round(scores.skew(), 3))

fig, axes = plt.subplots(1, 2, figsize=(10, 4))

# Histogram
scores.plot.hist(bins=[0, 5, 6, 7, 8, 9, 10], edgecolor="black", ax=axes[0])
axes[0].set_title("Histogram điểm")
axes[0].set_xlabel("Điểm")
axes[0].set_ylabel("Số sinh viên")

# Boxplot
axes[1].boxplot(scores, orientation="vertical")
axes[1].set_title("Boxplot điểm")
axes[1].set_ylabel("Điểm")

fig.tight_layout()
plt.show()
```

---

### 4.1.3. Chọn biểu đồ phù hợp theo mục tiêu 🎯

Trước khi đặt bút vẽ, hãy tóm tắt câu hỏi phân tích bằng một câu ngắn gọn:
* Nếu so sánh giữa các nhóm ➡️ **Bar chart**.
* Nếu diễn tiến theo thời gian/vòng lặp huấn luyện ➡️ **Line plot**.
* Nếu tìm quan hệ hoặc sự phân cụm giữa hai biến số ➡️ **Scatter plot**.
* Nếu tìm hình dạng phân bố của một biến số ➡️ **Histogram** / **Boxplot**.

📋 Một biểu đồ chuyên nghiệp dùng trong báo cáo AI luôn cần: **tiêu đề rõ ràng, nhãn trục kèm đơn vị đo, chú thích (legend)** nếu có nhiều nhóm, và một tỷ lệ trục trung thực không gây sai lệch nhận thức. Tuyệt đối tránh sử dụng biểu đồ 3D chỉ để trang trí.

#### Bảng 4.2. Chọn biểu đồ theo mục tiêu phân tích trong pipeline AI 📊

| Mục tiêu | Biểu đồ nên dùng | Kiểm tra trước khi kết luận |
| :--- | :--- | :--- |
| **So sánh nhiều mô hình** | Bar chart | Cùng tập test, cùng một metric, trục bắt đầu từ vị trí hợp lý. |
| **Theo dõi huấn luyện** | Line plot | Tập train/tập validation tách rõ, không chỉ nhìn loss trên tập train. |
| **Tìm quan hệ đặc trưng** | Scatter plot | Outlier, nhóm ẩn, tránh suy diễn quan hệ nhân quả. |
| **Kiểm tra phân bố** | Histogram | Số bin, cỡ mẫu, thống kê mô tả đi kèm. |
| **So sánh phân bố theo nhóm** | Boxplot hoặc histogram theo nhóm | Cỡ mẫu của mỗi nhóm và ý nghĩa thực tế của outlier. |

#### 🧩 Ma trận nhầm lẫn ở mức nhập môn (Confusion Matrix)
Với bài toán phân loại, một con số `accuracy` chung chung không thể chỉ ra mô hình đang nhầm lẫn cụ thể ở lớp nào. Ma trận nhầm lẫn đếm số quan sát theo cặp **Nhãn thật (True Label) - Nhãn dự đoán (Predicted Label)**. Các ô nằm trên đường chéo chính thể hiện dự đoán đúng; các ô ngoài đường chéo là các kiểu sai lầm cần được điều tra kỹ lưỡng.

#### 💻 Mã nguồn 51. Đọc lỗi phân loại bằng heatmap ma trận nhầm lẫn

```python
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

labels = ["Mèo", "Chó", "Chim"]
confusion = np.array([,
 ,
    [1, 3, 28]
])

fig, ax = plt.subplots(figsize=(6, 5))
sns.heatmap(
    confusion,
    annot=True,
    fmt="d",
    cmap="Blues",
    xticklabels=labels,
    yticklabels=labels,
    ax=ax
)

ax.set_title("Ma trận nhầm lẫn trên tập đánh giá")
ax.set_xlabel("Nhãn dự đoán")
ax.set_ylabel("Nhãn thật")
fig.tight_layout()
plt.show()
```

> 📝 **Đọc hiểu lỗi:** Nhìn vào ma trận, lỗi *“Chó bị dự đoán nhầm thành Mèo”* xuất hiện 7 lần, nhiều hơn chiều ngược lại (*“Mèo bị dự đoán nhầm thành Chó”* - 5 lần). Khi các lớp có số lượng mẫu mất cân bằng, ta nên xem xét thêm tỷ lệ chuẩn hóa theo hàng.

---

## 4.2. Tiền xử lý văn bản 🔤

Use code with caution.
Dữ liệu văn bản (text) là nguồn tài nguyên quan trọng trong AI (phản hồi khách hàng, hội thoại chat, mô tả sản phẩm...). Khác với dữ liệu số, văn bản thô chứa rất nhiều biến thể do cách gõ, viết hoa/thường, dấu câu, emoji, URL, hoặc lỗi mã hóa Unicode. Nếu đưa trực tiếp văn bản thô vào mô hình, nhiễu sẽ tăng cao và làm giảm nghiêm trọng hiệu năng học.
🎯 Mục tiêu tiền xử lý: Chuẩn hóa các phần không mang ý nghĩa ngữ nghĩa cho bài toán cụ thể và bảo toàn tối đa các tín hiệu có ích. Ví dụ: Trong bài toán phân tích cảm xúc, dấu chấm than ! hoặc emoji là tín hiệu cực mạnh; nhưng trong bài toán dịch máy, các ký tự đặc biệt thừa cần được dọn dẹp sạch sẽ.

4.2.1. Làm sạch ký tự đặc biệt và khoảng trắng 🧹

Các thao tác phổ biến bao gồm:
• Chuẩn hóa Unicode: Đưa văn bản tiếng Việt về dạng NFC để đảm bảo các ký tự có dấu có biểu diễn nhất quán.
• Gom khoảng trắng: Sửa các lỗi khoảng trắng thừa, tab, hoặc xuống dòng liên tiếp thành một dấu cách duy nhất.
• Chuẩn hóa chữ: Chuyển toàn bộ về chữ thường (lower()) nếu bài toán không phân biệt hoa/thường.
• Thay thế mẫu đặc biệt: Thay thế URL thành nhãn <URL>, email thành <EMAIL> để mô hình học được sự xuất hiện của chúng mà không bị nhiễu bởi nội dung chi tiết.

💻 Mã nguồn 52. Chuẩn hóa Unicode, chữ thường và khoảng trắng

```python
import re
import unicodedata
SPACE_PATTERN = re.compile(r"\s+")
def normalize_text(text: str) -> str:
text = unicodedata.normalize("NFC", text)
text = text.strip().lower()
return SPACE_PATTERN.sub(" ", text)
raw_texts = [
"  Trí   tuệ\nnhân tạo  tại TP.HCM! ",
"Python   cho\tAI",
"Dữ liệu tiếng Việt cần giữ dấu.",
]
for text in raw_texts:
print(normalize_text(text))
```
💡 Kết quả mong muốn:
text trí tuệ nhân tạo tại tp.hcm! python cho ai dữ liệu tiếng việt cần giữ dấu. 

4.2.2. Biểu thức chính quy (Regex) trong làm sạch dữ liệu văn bản 🧩

Regex là ngôn ngữ mô tả mẫu ký tự mạnh mẽ để tìm kiếm và thay thế các cấu trúc văn bản phức tạp (URL, email, số điện thoại, hashtag...).
⚠️ Cảnh báo rủi ro: Regex quá rộng hoặc quá phức tạp rất dễ gây lỗi. Một biểu thức xóa “mọi ký tự không phải chữ” có thể vô tình phá hỏng các ký hiệu công nghệ quan trọng như C++, F1-score, hoặc mã sản phẩm.
💡 Thực hành tốt nhất:
• Luôn sử dụng chuỗi raw (ví dụ: r"\s+") để tránh lỗi gạch chéo ngược (\).
• Nên biên dịch trước bằng re.compile() để tăng tốc độ và mã nguồn rõ ràng.
• Luôn viết bộ kiểm thử gồm cả ví dụ dương (phải khớp) và ví dụ âm (không được khớp) để bảo vệ tính toàn vẹn của dữ liệu.

💻 Mã nguồn 55. Pipeline regex nhỏ, rõ mục đích

```python
import re
PATTERNS = [
(re.compile(r"https?://\S+|www.\S+", re.IGNORECASE), ""),
(re.compile(r"[\w.+-]+@[\w-]+(?:.[\w-]+)+"), ""),
(re.compile(r"\b\d{9,11}\b"), ""),
]
SPACE_PATTERN = re.compile(r"\s+")
def replace_known_patterns(text: str) -> str:
for pattern, replacement in PATTERNS:
text = pattern.sub(replacement, text)
return SPACE_PATTERN.sub(" ", text).strip()
sample = "Gọi 0901234567 hoặc gửi mail test@example.com. Xem tại www.demo.vn"
print(replace_known_patterns(sample))
```
💡 Kết quả mong muốn:
text Gọi <PHONE> hoặc gửi mail <EMAIL>. Xem tại <URL> 

4.2.3. Tokenization cơ bản (Tách từ) ✂️

Tokenization là bước chia văn bản dài thành các đơn vị xử lý nhỏ hơn, gọi là token (có thể là từ, tiếng, số, hoặc dấu câu). Đây là cầu nối bắt buộc trước khi đưa văn bản vào các thuật toán biểu diễn số như Bag-of-Words, TF-IDF hay Word Embedding.
⚠️ Phương thức .split() thông thường chỉ tách theo khoảng trắng, điều này khiến dấu câu bị dính vào từ (ví dụ: "ai!" khác "ai"). Sử dụng Regex giúp ta tách dấu câu và từ thành các token độc lập một cách chính xác.
📝 Lưu ý với tiếng Việt: Tiếng Việt có các từ ghép gồm nhiều âm tiết cách nhau bởi khoảng trắng (ví dụ: "trí tuệ nhân tạo"). Tokenizer bằng Regex dưới đây hoạt động ở mức âm tiết/tiếng, không phải là một bộ tách từ ghép tiếng Việt hoàn chỉnh.

💻 Mã nguồn 57. Đếm tần suất token sau khi làm sạch đơn giản

```python
import collections
import re
import unicodedata
TOKEN_PATTERN = re.compile(r"[^\W_]+(?:[-'][^\W_]+)*|[^\w\s]", re.UNICODE)
SPACE_PATTERN = re.compile(r"\s+")
def normalize_text(text: str) -> str:
text = unicodedata.normalize("NFC", text)
return SPACE_PATTERN.sub(" ", text.strip().lower())
def tokenize_basic(text: str) -> list[str]:
return TOKEN_PATTERN.findall(normalize_text(text))
texts = [
"AI hỗ trợ học tập.",
"Học AI cần dữ liệu sạch!",
"Dữ liệu sạch giúp mô hình AI ổn định hơn.",
]
counter = collections.Counter()
for text in texts:
counter.update(tokenize_basic(text))
print(counter.most_common(5))
```
💡 Kết quả mong muốn:
text [('ai', 3), ('học', 2), ('.', 2), ('dữ', 2), ('liệu', 2)] 

⏱️ Checklist kiểm tra tiền xử lý văn bản

• Đã xác định rõ bài toán có cần giữ dấu tiếng Việt, dấu câu, emoji hay URL không?
• Toàn bộ văn bản đã được đưa về chuẩn Unicode NFC và dọn sạch khoảng trắng thừa chưa?
• Các biểu thức Regex đã được kiểm thử với cả ca khớp và ca không khớp chưa?
• Bộ Tokenizer đang tách dữ liệu ở mức độ nào: âm tiết, từ ghép, hay ký tự đặc biệt?
• Văn bản sau khi tiền xử lý có bị mất mát ngữ nghĩa cốt lõi cần học không?

📝 Bài tập thực hành

1. Viết hàm chuẩn hóa danh sách bình luận tiếng Việt: đưa về Unicode NFC, chuyển chữ thường, gom khoảng trắng nhưng giữ nguyên dấu tiếng Việt.
2. Thiết kế một biểu thức Regex để nhận diện và thay thế toàn bộ các hashtag (ví dụ: #Python, #AI) thành nhãn <HASHTAG>. Viết kèm 2 ví dụ đúng và 2 ví dụ sai để kiểm thử.
3. So sánh kết quả đầu ra của phương thức .split() và hàm tokenize_basic ở mã nguồn 57 trên câu văn: "Học AI, học máy!".

📦 Sản phẩm học tập của chương


🏆 Bài tập tích hợp: Báo cáo chất lượng dữ liệu phản hồi (Audit Report)

Cho một bảng dữ liệu thô gồm các cột: nội dung phản hồi (feedback), thời điểm (created_at), nhãn thật (y_true), và nhãn dự đoán của mô hình (y_pred). Hãy xây dựng một chương trình Python thực hiện các nhiệm vụ sau:
1. Chuẩn hóa toàn bộ cột phản hồi về dạng Unicode NFC, loại khoảng trắng thừa và thay thế URL/Email thành các token tương ứng.
2. Tạo thêm hai thuộc tính mới: độ dài văn bản trước khi làm sạch và độ dài sau khi làm sạch. Sử dụng Boxplot để phát hiện và gắn cờ các phản hồi có độ dài bất thường.
3. Thực hiện tách token (tokenization) mức cơ bản, vẽ Bar chart hiển thị top 10 token xuất hiện nhiều nhất.
4. Sử dụng cột y_true và y_pred để lập Ma trận nhầm lẫn, vẽ heatmap trực quan hóa và chỉ ra hai kiểu sai lầm phổ biến nhất của mô hình.
5. Xuất ra một file báo cáo tóm tắt (dạng JSON hoặc Dictionary) chứa thông tin: tổng số dòng xử lý, số lượng URL/Email đã được thay thế, danh sách các dòng có độ dài bất thường và ma trận nhầm lẫn thu được.
