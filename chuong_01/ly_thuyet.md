[⬅️ Quay lại Mục lục chính](../README.md) | [Đi tới Bài tập ➡️](./bai_tap.md)

---

# CHƯƠNG 1: Nền tảng Python cho lập trình và trí tuệ nhân tạo

**Chuẩn đầu ra:** CLO1, CLO2, CLO3[cite: 1]

**Thời lượng:** 15 tiết - 30 giờ tự học[cite: 1]

**Khung bài giảng:** Từ yêu cầu bài toán đến chương trình xử lý dữ liệu[cite: 1].

Chương này giúp người học đọc yêu cầu, biểu diễn dữ liệu, điều khiển luồng xử lý, phân rã chương trình thành hàm và lưu trữ kết quả[cite: 1]. 

Các ví dụ sử dụng dữ liệu nhỏ để người học có thể dự đoán kết quả trước khi chạy mã[cite: 1].

---

## Mục tiêu chương

Sau khi hoàn thành chương, người học có thể[cite: 1]:

- Giải thích vai trò của Python trong luồng dữ liệu của một hệ thống AI;[cite: 1]

- Thiết lập môi trường, chạy chương trình và đọc traceback cơ bản;[cite: 1]

- Sử dụng biến, kiểu dữ liệu, toán tử, rẽ nhánh và vòng lặp;[cite: 1]

- Thiết kế hàm có đầu vào, đầu ra và phạm vi biến rõ ràng;[cite: 1]

- Lựa chọn list, tuple, dictionary hoặc set theo yêu cầu bài toán;[cite: 1]

- Xử lý chuỗi và đọc/ghi dữ liệu văn bản, CSV, JSON an toàn.[cite: 1]

---

## 1.1. Giới thiệu Python và môi trường lập trình

### 1.1.1. Vai trò của Python trong AI và xử lý dữ liệu

Python được dùng rộng rãi trong AI vì cú pháp dễ đọc, hệ sinh thái thư viện phong phú và khả năng kết nối với các thành phần hiệu năng cao viết bằng C, C++ hoặc chạy trên phần cứng chuyên dụng[cite: 1]. 

Người học có thể tập trung vào logic xử lý dữ liệu mà vẫn sử dụng được NumPy, Pandas và các công cụ AI ở những chương sau[cite: 1].

**Thông dịch, biên dịch và vai trò gắn kết:**

- Python thường được xếp vào nhóm ngôn ngữ thông dịch: người dùng chạy mã qua một trình thông dịch và nhiều lỗi kiểu dữ liệu hoặc lỗi logic chỉ xuất hiện khi luồng thực thi đi đến câu lệnh liên quan[cite: 1]. 

- Ngược lại, với quy trình điển hình của C hoặc Pascal, toàn bộ chương trình được biên dịch thành mã máy trước khi chạy, nên trình biên dịch có thể phát hiện một nhóm lỗi sớm hơn[cite: 1]. 

- Phân biệt này không tuyệt đối: CPython vẫn biên dịch mã nguồn thành bytecode, còn chương trình biên dịch vẫn có thể gặp lỗi lúc chạy[cite: 1].

**Ngôn ngữ gắn kết (Glue language):**

- Python đồng thời là một ngôn ngữ gắn kết (glue language)[cite: 1]. 

- Mã Python cung cấp giao diện dễ đọc để kết nối thư viện viết bằng C/C++, mã chạy song song trên GPU, công cụ lưu trữ và dịch vụ triển khai[cite: 1]. 

- Tốc độ của một pipeline AI không chỉ đến từ trình thông dịch Python; các phép tính nặng thường được chuyển xuống thư viện đã tối ưu, trong khi Python điều phối luồng dữ liệu và thí nghiệm[cite: 1].

**Một pipeline AI nhập môn có thể nhìn như chuỗi bốn bước**[cite: 1]:

1. Thu nhận dữ liệu: văn bản, CSV, JSON, ảnh hoặc cảm biến;[cite: 1]

2. Kiểm tra và tiền xử lý: phát hiện lỗi, làm sạch, biến đổi;[cite: 1]

3. Mô hình/thuật toán: học quy luật hoặc áp dụng quy tắc;[cite: 1]

4. Đầu ra: dự đoán, phân loại, điểm số hoặc báo cáo.[cite: 1]

Python là lớp kết nối các bước này[cite: 1]. Tuy nhiên, một chương trình không trở thành AI chỉ vì được viết bằng Python: dữ liệu, mục tiêu và thuật toán mới quyết định vai trò của nó[cite: 1].

### 1.1.2. Cài đặt môi trường và chạy chương trình đầu tiên

Trong triển khai CPython phổ biến, mã nguồn được phân tích và biên dịch thành bytecode; máy ảo Python sau đó thực thi bytecode[cite: 1]. 

Mô hình này giải thích vì sao Python có trải nghiệm tương tác linh hoạt nhưng vẫn phát hiện một số lỗi cú pháp trước khi chương trình bắt đầu chạy[cite: 1].

Hai môi trường phù hợp cho học phần là[cite: 1]:

- **Visual Studio Code:** Quản lý thư mục dự án, file py, terminal, breakpoint và debugger[cite: 1]. Đây là phần mềm được nêu trong đề cương[cite: 1].

- **Jupyter Notebook/Google Colab:** Tổ chức mã thành cell và hiển thị kết quả ngay bên dưới, phù hợp cho khám phá dữ liệu[cite: 1]. Người học phải chú ý thứ tự chạy cell để tránh trạng thái ẩn[cite: 1].

Đọc thông báo lỗi traceback nên được đọc từ dòng cuối để xác định loại lỗi, sau đó đi ngược lên để tìm file và dòng mã của người học[cite: 1]. Ba nhóm lỗi nhập môn[cite: 1]:

- SyntaxError: mã không tuân theo ngữ pháp;[cite: 1]

- Lỗi khi chạy như NameError, TypeError, ValueError;[cite: 1]

- Lỗi logic: chương trình chạy nhưng kết quả sai.[cite: 1]

---

## 1.2. Thành phần cơ bản của ngôn ngữ

### 1.2.1. Biến, kiểu dữ liệu, toán tử

Biến là tên tham chiếu đến một đối tượng trong bộ nhớ[cite: 1]. Khi viết `sample_count = 50`, Python tạo hoặc lấy đối tượng số nguyên 50 rồi gắn tên `sample_count` với đối tượng đó[cite: 1]. 

Dấu `=` trong Python là toán tử gán, không phải dấu bằng toán học[cite: 1]. Câu lệnh gán được đọc từ phải sang trái: tính giá trị ở vế phải trước, sau đó gắn kết quả cho tên ở vế trái[cite: 1]. 

Python dùng kiểu động: kiểu dữ liệu gắn với giá trị tại thời điểm chạy, không gắn cố định với tên biến[cite: 1]. Quy ước phổ biến là dùng `snake_case` cho tên biến và hàm[cite: 1].

**Một số kiểu dữ liệu nền tảng**[cite: 1]:

- `int`: Số mẫu, số vòng lặp, số dòng dữ liệu, chỉ số nguyên.[cite: 1]

- `float`: Tỷ lệ, điểm số, giá trị đo, kết quả trung bình.[cite: 1]

- `str`: Nhãn, tên cột, đường dẫn file, dữ liệu văn bản.[cite: 1]

- `bool`: Kết quả điều kiện, cờ hợp lệ không hợp lệ, trạng thái bật/tắt.[cite: 1]

- `NoneType`: Chưa có giá trị, dữ liệu thiếu, kết quả không xác định.[cite: 1]

**Các nhóm toán tử cơ bản trong Python**[cite: 1]:

- Số học (`+ - * /`): Cộng, trừ, nhân, chia. Toán tử `/` luôn trả về kết quả dạng số thực.[cite: 1]

- Số học (`// % **`): Chia lấy phần nguyên, chia lấy phần dư, lũy thừa.[cite: 1]

- So sánh (`== != > < >= <=`): Trả về True hoặc False; dùng trong rẽ nhánh, lọc dữ liệu và kiểm tra điều kiện.[cite: 1]

- Logic (`and or not`): Kết hợp nhiều điều kiện; `and` yêu cầu cả hai đúng, `or` chỉ cần ít nhất một đúng, `not` đảo kết quả.[cite: 1]

### 1.2.2. Cấu trúc rẽ nhánh và vòng lặp

Rẽ nhánh chọn khối lệnh dựa trên điều kiện[cite: 1]. Python dùng ba từ khóa chính: `if` mở điều kiện đầu tiên, `elif` kiểm tra điều kiện thay thế khi các điều kiện trước sai, và `else` bắt mọi trường hợp còn lại[cite: 1]. Mỗi dòng điều kiện kết thúc bằng dấu hai chấm `:`[cite: 1]. 

Vòng lặp thực hiện lặp lại một khối lệnh[cite: 1]. Vòng lặp `for` phù hợp khi cần duyệt qua từng phần tử của một đối tượng có thể lặp, chẳng hạn danh sách, chuỗi, dòng dữ liệu hoặc dãy số tạo bởi `range()`[cite: 1]. 

Vòng lặp `while` phù hợp khi chưa biết trước số lần lặp, nhưng biết điều kiện để tiếp tục[cite: 1]. 

### 1.2.3. Điều khiển dòng thực thi: break, continue

`continue` bỏ phần còn lại của lượt hiện tại; `break` kết thúc vòng lặp gần nhất[cite: 1]. Dùng chúng khi điều kiện bỏ qua dừng rõ ràng, tránh tạo luồng điều khiển khó theo dõi[cite: 1].

---

## 1.3. Hàm và thiết kế chương trình

### 1.3.1. Định nghĩa hàm, tham số, giá trị trả về

Hàm gom một tác vụ có tên để tái sử dụng và kiểm thử[cite: 1]. Tham số mô tả dữ liệu đầu vào; `return` kết thúc hàm và gửi kết quả về nơi gọi[cite: 1]. Nếu không có `return`, hàm trả về `None`[cite: 1].

### 1.3.2. Phạm vi biến: local, global

Tên được tra cứu theo quy tắc LEGB: local, enclosing, global, built-in[cite: 1]. Trong phạm vi đề cương, trọng tâm là local và global[cite: 1]:

- Tên tạo trong hàm là local nếu không khai báo khác;[cite: 1]

- Tên ở cấp module là global và có thể đọc từ trong hàm;[cite: 1]

- Muốn gán lại tên global trong hàm phải khai báo `global`.[cite: 1]

### 1.3.3. Tổ chức chương trình theo cấu trúc hàm

Phân rã một pipeline nhỏ thành các bước đơn nhiệm: kiểm tra, biến đổi, tổng hợp và trình bày[cite: 1]. Mỗi hàm có thể được kiểm tra độc lập[cite: 1].

---

## 1.4. Cấu trúc dữ liệu cơ bản

### 1.4.1. List, tuple và kỹ thuật xử lý

`List` và `tuple` đều là sequence có thứ tự, hỗ trợ index (chỉ mục) và slice (lát cắt)[cite: 1]. 

`List` mutable, phù hợp collection thay đổi; `tuple` có cấu trúc ngoài immutable, phù hợp nhóm giá trị có ý nghĩa vị trí ổn định[cite: 1]. 

Comprehension phù hợp cho biến đổi ngắn, rõ ràng[cite: 1]. Nếu cần nhiều nhánh, nhiều side effect hoặc logic khó diễn đạt, dùng vòng lặp thường để tăng khả năng đọc[cite: 1].

### 1.4.2. Dictionary, set và thao tác tập hợp

Dictionary ánh xạ key duy nhất đến value[cite: 1]. Key phải hashable; truy cập bằng `data[key]` phát sinh KeyError nếu thiếu, còn `data.get(key)` cho phép cung cấp giá trị mặc định[cite: 1].

Set lưu phần tử duy nhất và hỗ trợ hợp, giao, hiệu[cite: 1]. Set không bảo toàn số lần xuất hiện và không cam kết thứ tự trình bày, nên chỉ dùng để “loại trùng” khi hai đặc tính đó không quan trọng[cite: 1].

---

## 1.5. Xử lý chuỗi và đọc/ghi file

### 1.5.1. Hàm xử lý chuỗi và lỗi thường gặp

Chuỗi Python là dãy Unicode immutable[cite: 1]. Các thao tác thường dùng gồm[cite: 1]:

- `strip()` loại whitespace ở hai đầu chuỗi;[cite: 1]

- `lower()` và `upper()` đổi kiểu chữ;[cite: 1]

- `split()` tách chuỗi theo whitespace;[cite: 1]

- `separator.join(parts)` ghép các phần tử bằng một chuỗi phân tách;

- `f-string` định dạng kết quả để in hoặc ghi báo cáo.

Chuẩn hóa Unicode cho tiếng Việt: Cùng một ký tự có dấu có thể được biểu diễn bằng một code point dựng sẵn hoặc bằng chữ cái cơ sở kết hợp với dấu[cite: 1]. Trước khi so sánh, đếm hoặc tokenization văn bản tiếng Việt, nên đưa chuỗi về dạng NFC bằng `unicodedata.normalize("NFC", text)`[cite: 1].

### 1.5.2. Đọc ghi file văn bản, CSV, JSON

Context manager `with open(...)` đóng file kể cả khi có ngoại lệ[cite: 1]. Chỉ rõ `encoding="utf-8"` cho dữ liệu tiếng Việt

Chế độ `r` đọc, `w` tạo/ghi đè, `a` ghi tiếp[cite: 1]. Khi ghi CSV trên Windows, dùng `newline=""`.

---

## Tổng kết và bài tập chương

- Giải thích được vị trí của Python trong pipeline dữ liệu/AI.[cite: 1]

- Chạy được script, đọc traceback và phân biệt lỗi cú pháp, runtime, logic.[cite: 1]

- Dùng đúng kiểu, toán tử, rẽ nhánh, vòng lặp, break và continue.[cite: 1]

- Viết hàm có quy ước đầu vào đầu ra rõ ràng và hạn chế trạng thái toàn cục.[cite: 1]

- Chọn cấu trúc dữ liệu dựa trên thứ tự, tính duy nhất và khả năng thay đổi.[cite: 1]

- Chuẩn hóa Unicode NFC và đọc/ghi được TXT, CSV, JSON bằng UTF-8.[cite: 1]

---

[⬅️ Quay lại Mục lục chính](../README.md) | [Đi tới Bài tập ➡️](./bai_tap.md)
