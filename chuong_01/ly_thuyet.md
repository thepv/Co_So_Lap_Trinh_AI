[⬅️ Quay lại Mục lục chính](../README.md) | [Đi tới Bài tập ➡️](./bai_tap.md)

---

# 📘 CHƯƠNG 1: Nền tảng Python cho lập trình và trí tuệ nhân tạo

🎓 **Chuẩn đầu ra:** CLO1, CLO2, CLO3 

⏱️ **Thời lượng:** 15 tiết - 30 giờ tự học 

💡 **Khung bài giảng:** Từ yêu cầu bài toán đến chương trình xử lý dữ liệu .

Chương này giúp người học đọc yêu cầu, biểu diễn dữ liệu, điều khiển luồng xử lý, phân rã chương trình thành hàm và lưu trữ kết quả . 

Các ví dụ sử dụng dữ liệu nhỏ để người học có thể dự đoán kết quả trước khi chạy mã .

---

## 🎯 Mục tiêu chương

Sau khi hoàn thành chương, người học có thể :

- ✅ Giải thích vai trò của Python trong luồng dữ liệu của một hệ thống AI; 

- ✅ Thiết lập môi trường, chạy chương trình và đọc traceback cơ bản; 

- ✅ Sử dụng biến, kiểu dữ liệu, toán tử, rẽ nhánh và vòng lặp; 

- ✅ Thiết kế hàm có đầu vào, đầu ra và phạm vi biến rõ ràng; 

- ✅ Lựa chọn list, tuple, dictionary hoặc set theo yêu cầu bài toán; 

- ✅ Xử lý chuỗi và đọc/ghi dữ liệu văn bản, CSV, JSON an toàn. 

---

## 🐍 1.1. Giới thiệu Python và môi trường lập trình

### 🌟 1.1.1. Vai trò của Python trong AI và xử lý dữ liệu

Python được dùng rộng rãi trong AI vì cú pháp dễ đọc, hệ sinh thái thư viện phong phú và khả năng kết nối với các thành phần hiệu năng cao viết bằng C, C++ hoặc chạy trên phần cứng chuyên dụng . 

Người học có thể tập trung vào logic xử lý dữ liệu mà vẫn sử dụng được NumPy, Pandas và các công cụ AI ở những chương sau .

**Thông dịch, biên dịch và vai trò gắn kết:**

- 🔄 Python thường được xếp vào nhóm ngôn ngữ thông dịch: người dùng chạy mã qua một trình thông dịch và nhiều lỗi kiểu dữ liệu hoặc lỗi logic chỉ xuất hiện khi luồng thực thi đi đến câu lệnh liên quan . 

- ⚙️ Ngược lại, với quy trình điển hình của C hoặc Pascal, toàn bộ chương trình được biên dịch thành mã máy trước khi chạy, nên trình biên dịch có thể phát hiện một nhóm lỗi sớm hơn . 

- ⚖️ Phân biệt này không tuyệt đối: CPython vẫn biên dịch mã nguồn thành bytecode, còn chương trình biên dịch vẫn có thể gặp lỗi lúc chạy .

**Ngôn ngữ gắn kết (Glue language):**

- 🔗 Python đồng thời là một ngôn ngữ gắn kết (glue language) . 

- 🌉 Mã Python cung cấp giao diện dễ đọc để kết nối thư viện viết bằng C/C++, mã chạy song song trên GPU, công cụ lưu trữ và dịch vụ triển khai . 

- 🚀 Tốc độ của một pipeline AI không chỉ đến từ trình thông dịch Python; các phép tính nặng thường được chuyển xuống thư viện đã tối ưu, trong khi Python điều phối luồng dữ liệu và thí nghiệm .

**Một pipeline AI nhập môn có thể nhìn như chuỗi bốn bước** :

- 📥 1. Thu nhận dữ liệu: văn bản, CSV, JSON, ảnh hoặc cảm biến; 

- 🧹 2. Kiểm tra và tiền xử lý: phát hiện lỗi, làm sạch, biến đổi; 

- 🧠 3. Mô hình/thuật toán: học quy luật hoặc áp dụng quy tắc; 

- 📤 4. Đầu ra: dự đoán, phân loại, điểm số hoặc báo cáo. 

📌 Python là lớp kết nối các bước này . Tuy nhiên, một chương trình không trở thành AI chỉ vì được viết bằng Python: dữ liệu, mục tiêu và thuật toán mới quyết định vai trò của nó .

### 💻 1.1.2. Cài đặt môi trường và chạy chương trình đầu tiên

Trong triển khai CPython phổ biến, mã nguồn được phân tích và biên dịch thành bytecode; máy ảo Python sau đó thực thi bytecode . 

Mô hình này giải thích vì sao Python có trải nghiệm tương tác linh hoạt nhưng vẫn phát hiện một số lỗi cú pháp trước khi chương trình bắt đầu chạy .

Hai môi trường phù hợp cho học phần là :

- 🖥️ **Visual Studio Code:** Quản lý thư mục dự án, file py, terminal, breakpoint và debugger . Đây là phần mềm được nêu trong đề cương .

- 📓 **Jupyter Notebook/Google Colab:** Tổ chức mã thành cell và hiển thị kết quả ngay bên dưới, phù hợp cho khám phá dữ liệu . Người học phải chú ý thứ tự chạy cell để tránh trạng thái ẩn .

🔍 **Đọc thông báo lỗi:** Traceback nên được đọc từ dòng cuối để xác định loại lỗi, sau đó đi ngược lên để tìm file và dòng mã của người học . 

Ba nhóm lỗi nhập môn :

- ❌ SyntaxError: mã không tuân theo ngữ pháp; 

- ⚠️ Lỗi khi chạy như NameError, TypeError, ValueError; 

- 🐛 Lỗi logic: chương trình chạy nhưng kết quả sai. 

---

## 🧱 1.2. Thành phần cơ bản của ngôn ngữ

### 🏷️ 1.2.1. Biến, kiểu dữ liệu, toán tử

Biến là tên tham chiếu đến một đối tượng trong bộ nhớ . Khi viết `sample_count = 50`, Python tạo hoặc lấy đối tượng số nguyên 50 rồi gắn tên `sample_count` với đối tượng đó . 

Dấu `=` trong Python là toán tử gán, không phải dấu bằng toán học . Câu lệnh gán được đọc từ phải sang trái: tính giá trị ở vế phải trước, sau đó gắn kết quả cho tên ở vế trái . 

Python dùng kiểu động: kiểu dữ liệu gắn với giá trị tại thời điểm chạy, không gắn cố định với tên biến . Quy ước phổ biến là dùng `snake_case` cho tên biến và hàm .

📊 **Một số kiểu dữ liệu nền tảng** :

- 🔢 `int`: Số mẫu, số vòng lặp, số dòng dữ liệu, chỉ số nguyên. 

- 🎯 `float`: Tỷ lệ, điểm số, giá trị đo, kết quả trung bình. 

- 🔤 `str`: Nhãn, tên cột, đường dẫn file, dữ liệu văn bản. 

- 🔘 `bool`: Kết quả điều kiện, cờ hợp lệ không hợp lệ, trạng thái bật/tắt. 

- 🕳️ `NoneType`: Chưa có giá trị, dữ liệu thiếu, kết quả không xác định. 

🧮 **Các nhóm toán tử cơ bản trong Python** :

- ➕ Số học (`+ - * /`): Cộng, trừ, nhân, chia. Toán tử `/` luôn trả về kết quả dạng số thực. 

- ➗ Số học (`// % **`): Chia lấy phần nguyên, chia lấy phần dư, lũy thừa. 

- ⚖️ So sánh (`== != > < >= <=`): Trả về True hoặc False; dùng trong rẽ nhánh, lọc dữ liệu và kiểm tra điều kiện. 

- 🔣 Logic (`and or not`): Kết hợp nhiều điều kiện; `and` yêu cầu cả hai đúng, `or` chỉ cần ít nhất một đúng, `not` đảo kết quả. 

### 🔀 1.2.2. Cấu trúc rẽ nhánh và vòng lặp

Rẽ nhánh chọn khối lệnh dựa trên điều kiện . Python dùng ba từ khóa chính: `if` mở điều kiện đầu tiên, `elif` kiểm tra điều kiện thay thế khi các điều kiện trước sai, và `else` bắt mọi trường hợp còn lại . Mỗi dòng điều kiện kết thúc bằng dấu hai chấm `:` . 

Vòng lặp thực hiện lặp lại một khối lệnh . Vòng lặp `for` phù hợp khi cần duyệt qua từng phần tử của một đối tượng có thể lặp, chẳng hạn danh sách, chuỗi, dòng dữ liệu hoặc dãy số tạo bởi `range()` . 

Vòng lặp `while` phù hợp khi chưa biết trước số lần lặp, nhưng biết điều kiện để tiếp tục . 

### 🛑 1.2.3. Điều khiển dòng thực thi: break, continue

`continue` bỏ phần còn lại của lượt hiện tại; `break` kết thúc vòng lặp gần nhất . Dùng chúng khi điều kiện bỏ qua dừng rõ ràng, tránh tạo luồng điều khiển khó theo dõi .

---

## 🛠️ 1.3. Hàm và thiết kế chương trình

### 🏗️ 1.3.1. Định nghĩa hàm, tham số, giá trị trả về

Hàm gom một tác vụ có tên để tái sử dụng và kiểm thử . Tham số mô tả dữ liệu đầu vào; `return` kết thúc hàm và gửi kết quả về nơi gọi . Nếu không có `return`, hàm trả về `None` .

### 🌍 1.3.2. Phạm vi biến: local, global

Tên được tra cứu theo quy tắc LEGB: local, enclosing, global, built-in . Trong phạm vi đề cương, trọng tâm là local và global :

- 🏠 Tên tạo trong hàm là local nếu không khai báo khác; 

- 🌐 Tên ở cấp module là global và có thể đọc từ trong hàm; 

- 🔑 Muốn gán lại tên global trong hàm phải khai báo `global`. 

### 🧩 1.3.3. Tổ chức chương trình theo cấu trúc hàm

Phân rã một pipeline nhỏ thành các bước đơn nhiệm: kiểm tra, biến đổi, tổng hợp và trình bày . Mỗi hàm có thể được kiểm tra độc lập .

---

## 📦 1.4. Cấu trúc dữ liệu cơ bản

### 🍡 1.4.1. List, tuple và kỹ thuật xử lý

`List` và `tuple` đều là sequence có thứ tự, hỗ trợ index (chỉ mục) và slice (lát cắt) . 

`List` mutable, phù hợp collection thay đổi; `tuple` có cấu trúc ngoài immutable, phù hợp nhóm giá trị có ý nghĩa vị trí ổn định . 

Comprehension phù hợp cho biến đổi ngắn, rõ ràng . Nếu cần nhiều nhánh, nhiều side effect hoặc logic khó diễn đạt, dùng vòng lặp thường để tăng khả năng đọc .

### 📚 1.4.2. Dictionary, set và thao tác tập hợp

Dictionary ánh xạ key duy nhất đến value . Key phải hashable; truy cập bằng `data[key]` phát sinh KeyError nếu thiếu, còn `data.get(key)` cho phép cung cấp giá trị mặc định .

Set lưu phần tử duy nhất và hỗ trợ hợp, giao, hiệu . Set không bảo toàn số lần xuất hiện và không cam kết thứ tự trình bày, nên chỉ dùng để “loại trùng” khi hai đặc tính đó không quan trọng .

---

## 📝 1.5. Xử lý chuỗi và đọc/ghi file

### 🔤 1.5.1. Hàm xử lý chuỗi và lỗi thường gặp

Chuỗi Python là dãy Unicode immutable . Các thao tác thường dùng gồm :

- ✂️ `strip()` loại whitespace ở hai đầu chuỗi; 

- 🔠 `lower()` và `upper()` đổi kiểu chữ; 

- 🔪 `split()` tách chuỗi theo whitespace; 

- 🔗 `separator.join(parts)` ghép các phần tử bằng một chuỗi phân tách; 

- ✒️ `f-string` định dạng kết quả để in hoặc ghi báo cáo. 

🌐 **Chuẩn hóa Unicode cho tiếng Việt:** Cùng một ký tự có dấu có thể được biểu diễn bằng một code point dựng sẵn hoặc bằng chữ cái cơ sở kết hợp với dấu . Trước khi so sánh, đếm hoặc tokenization văn bản tiếng Việt, nên đưa chuỗi về dạng NFC bằng `unicodedata.normalize("NFC", text)` .

### 💾 1.5.2. Đọc ghi file văn bản, CSV, JSON

Context manager `with open(...)` đóng file kể cả khi có ngoại lệ . Chỉ rõ `encoding="utf-8"` cho dữ liệu tiếng Việt . 

Chế độ `r` đọc, `w` tạo/ghi đè, `a` ghi tiếp . Khi ghi CSV trên Windows, dùng `newline=""` .

---

## 🏁 Tổng kết và bài tập chương

- ✔️ Giải thích được vị trí của Python trong pipeline dữ liệu/AI. 

- ✔️ Chạy được script, đọc traceback và phân biệt lỗi cú pháp, runtime, logic. 

- ✔️ Dùng đúng kiểu, toán tử, rẽ nhánh, vòng lặp, break và continue. 

- ✔️ Viết hàm có quy ước đầu vào đầu ra rõ ràng và hạn chế trạng thái toàn cục. 

- ✔️ Chọn cấu trúc dữ liệu dựa trên thứ tự, tính duy nhất và khả năng thay đổi. 

- ✔️ Chuẩn hóa Unicode NFC và đọc/ghi được TXT, CSV, JSON bằng UTF-8. 

---

[⬅️ Quay lại Mục lục chính](../README.md) | [Đi tới Bài tập ➡️](./bai_tap.md)
