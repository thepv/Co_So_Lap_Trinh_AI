[⬅️ Quay lại Mục lục chính](../README.md) | [Đi tới Bài tập ➡️](./bai_tap.md)

---

# CHƯƠNG 1: Nền tảng Python cho lập trình và trí tuệ nhân tạo

**Chuẩn đầu ra:** CLO1, CLO2, CLO3 | **15 tiết** | **30 giờ tự học**

**Khung bài giảng:** Từ yêu cầu bài toán đến chương trình xử lý dữ liệu

Chương này giúp người học đọc yêu cầu, biểu diễn dữ liệu, điều khiển luồng xử lý, phân rã chương trình thành hàm và lưu trữ kết quả. Các ví dụ sử dụng dữ liệu nhỏ để người học có thể dự đoán kết quả trước khi chạy mã.

## Mục tiêu chương
Sau khi hoàn thành chương, người học có thể:
* Giải thích vai trò của Python trong luồng dữ liệu của một hệ thống AI;
* Thiết lập môi trường, chạy chương trình và đọc traceback cơ bản;
* Sử dụng biến, kiểu dữ liệu, toán tử, rẽ nhánh và vòng lặp;
* Thiết kế hàm có đầu vào, đầu ra và phạm vi biến rõ ràng;
* Lựa chọn list, tuple, dictionary hoặc set theo yêu cầu bài toán;
* Xử lý chuỗi và đọc/ghi dữ liệu văn bản, CSV, JSON an toàn.

---

## 1.1. Giới thiệu Python và môi trường lập trình

### 1.1.1. Vai trò của Python trong AI và xử lý dữ liệu
Python được dùng rộng rãi trong AI vì cú pháp dễ đọc, hệ sinh thái thư viện phong phú và khả năng kết nối với các thành phần hiệu năng cao viết bằng C, C++ hoặc chạy trên phần cứng chuyên dụng. Người học có thể tập trung vào logic xử lý dữ liệu mà vẫn sử dụng được NumPy, Pandas và các công cụ AI ở những chương sau.

**Thông dịch, biên dịch và vai trò gắn kết.** Python thường được xếp vào nhóm ngôn ngữ thông dịch: người dùng chạy mã qua một trình thông dịch và nhiều lỗi kiểu dữ liệu hoặc lỗi logic chỉ xuất hiện khi luồng thực thi đi đến câu lệnh liên quan. Ngược lại, với quy trình điển hình của C hoặc Pascal, toàn bộ chương trình được biên dịch thành mã máy trước khi chạy, nên trình biên dịch có thể phát hiện một nhóm lỗi sớm hơn. Phân biệt này không tuyệt đối: CPython vẫn biên dịch mã nguồn thành bytecode, còn chương trình biên dịch vẫn có thể gặp lỗi lúc chạy.

Python đồng thời là một ngôn ngữ gắn kết (glue language). Mã Python cung cấp giao diện dễ đọc để kết nối thư viện viết bằng C/C++, mã chạy song song trên GPU, công cụ lưu trữ và dịch vụ triển khai. Vì vậy, tốc độ của một pipeline AI không chỉ đến từ trình thông dịch Python; các phép tính nặng thường được chuyển xuống thư viện đã tối ưu, trong khi Python điều phối luồng dữ liệu và thí nghiệm.

Một pipeline AI nhập môn có thể nhìn như chuỗi bốn bước:
1. Thu nhận dữ liệu: văn bản, CSV, JSON, ảnh hoặc cảm biến;
2. Kiểm tra và tiền xử lý: phát hiện lỗi, làm sạch, biến đổi;
3. Mô hình/thuật toán: học quy luật hoặc áp dụng quy tắc;
4. Đầu ra: dự đoán, phân loại, điểm số hoặc báo cáo.

Python là lớp kết nối các bước này. Tuy nhiên, một chương trình không trở thành AI chỉ vì được viết bằng Python: dữ liệu, mục tiêu và thuật toán mới quyết định vai trò của nó.

### 1.1.2. Cài đặt môi trường và chạy chương trình đầu tiên
Trong triển khai CPython phổ biến, mã nguồn được phân tích và biên dịch thành bytecode; máy ảo Python sau đó thực thi bytecode. Mô hình này giải thích vì sao Python có trải nghiệm tương tác linh hoạt nhưng vẫn phát hiện một số lỗi cú pháp trước khi chương trình bắt đầu chạy.

Hai môi trường phù hợp cho học phần là:
* **Visual Studio Code:** Quản lý thư mục dự án, file py, terminal, breakpoint và debugger. Đây là phần mềm được nêu trong đề cương.
* **Jupyter Notebook/Google Colab:** Tổ chức mã thành cell và hiển thị kết quả ngay bên dưới, phù hợp cho khám phá dữ liệu. Người học phải chú ý thứ tự chạy cell để tránh trạng thái ẩn.

Tạo file `bai_hoc_dau_tien.py`:

**Mã nguồn 1. Chương trình Python đầu tiên**

course = "Cơ sở lập trình cho Trí tuệ nhân tạo"
print("Hello, AI World!")
print(f"Học phần: {course}")
# Kết quả mong muốn:
# Hello, AI World!
# Học phần: Cơ sở lập trình cho Trí tuệ nhân tạo
Chạy trong terminal bằng python bai_hoc_dau_tien.py (Windows cũng có thể dùng py tùy cách cài đặt). Trước khi chạy, kiểm tra phiên bản bằng python --version và bảo đảm terminal đang đứng đúng thư mục.  
   

Đọc thông báo lỗi. Traceback nên được đọc từ dòng cuối để xác định loại lỗi, sau đó đi ngược lên để tìm file và dòng mã của người học. Ba nhóm lỗi nhập môn:  
   

SyntaxError: mã không tuân theo ngữ pháp;  
   

Lỗi khi chạy như NameError, TypeError, ValueError;  
   

Lỗi logic: chương trình chạy nhưng kết quả sai.  
   

Câu hỏi dẫn nhập: Cùng một chương trình Python có thể được dùng trong pipeline AI và trong ứng dụng thông thường hay không? Hãy nêu một ví dụ cho mỗi trường hợp.  
   

Mã nguồn 2. Mã chứa lỗi dùng cho hoạt động gỡ lỗi

  
   

Python
# Mã này cố ý không chạy. Hãy tìm và sửa ba lỗi.
Print("Chào mừng đến với môn Cơ sở lập trình cho AI")

x = 100
y = 200
print(x + y
# Kết quả mong muốn:
# IndentationError: unexpected indent
Hoạt động trên lớp: Người học sửa mã thành phiên bản chạy được và giải thích vì sao Python phân biệt print với Print.  
   

1.2. Thành phần cơ bản của ngôn ngữ
1.2.1. Biến, kiểu dữ liệu, toán tử
Trong mọi chương trình xử lý dữ liệu, ba câu hỏi xuất hiện ngay từ đầu là: dữ liệu được đặt tên như thế nào, dữ liệu thuộc kiểu gì, và ta được phép thực hiện phép toán nào trên dữ liệu đó. Với một pipeline AI, các giá trị như số mẫu, nhãn lớp, điểm đánh giá, đường dẫn file hoặc cờ kiểm tra chất lượng đều phải được biểu diễn bằng biến trước khi có thể làm sạch, biến đổi hay đưa vào mô hình. Vì vậy, biến, kiểu dữ liệu và toán tử là nền tảng để người học đọc được mã nguồn ở các chương NumPy, Pandas và pipeline sau này.  
   

Biến là tên tham chiếu đến một đối tượng trong bộ nhớ. Khi viết sample_count = 50, Python tạo hoặc lấy đối tượng số nguyên 50 rồi gắn tên sample_count với đối tượng đó. Dấu = trong Python là toán tử gán, không phải dấu bằng toán học. Câu lệnh gán được đọc từ phải sang trái: tính giá trị ở vế phải trước, sau đó gán kết quả cho tên ở vế trái. Tên biến nên mô tả ý nghĩa dữ liệu, chẳng hạn accuracy rõ hơn a khi biểu diễn độ chính xác của một mô hình.  
   

Python dùng kiểu động: kiểu dữ liệu gắn với giá trị tại thời điểm chạy, không gắn cố định với tên biến. Cùng một tên có thể được gán sang giá trị thuộc kiểu khác, nhưng trong bài học nhập môn không nên lạm dụng điều này vì làm chương trình khó đọc và dễ sinh lỗi logic. Tên biến phải bắt đầu bằng chữ cái hoặc dấu gạch dưới, không bắt đầu bằng số, không chứa khoảng trắng, không chứa ký tự đặc biệt như -, và không trùng từ khóa của Python như if, for, class. Quy ước phổ biến là dùng snake_case cho tên biến và hàm.  
   

Bảng 1.1. Một số kiểu dữ liệu nền tảng.

  
   

Kiểu	Ví dụ	Ý nghĩa thường gặp trong xử lý dữ liệu
int	50	
Số mẫu, số vòng lặp, số dòng dữ liệu, chỉ số nguyên.  
   

float	0.85	
Tỷ lệ, điểm số, giá trị đo, kết quả trung bình.  
   

str	"cat"	
Nhãn, tên cột, đường dẫn file, dữ liệu văn bản.  
   

bool	True	
Kết quả điều kiện, cờ hợp lệ/không hợp lệ, trạng thái bật/tắt.  
   

NoneType	None	
Chưa có giá trị, dữ liệu thiếu, kết quả không xác định.  
   

Mã nguồn 3. Biến, kiểu dữ liệu và toán tử

  
   

Python
sample_count = 50
accuracy = 0.85
model_name = "Linear Regression"
is_acceptable = accuracy >= 0.80
print(type(sample_count))
print(f"Mô hình: {model_name}")
print(f"Số mẫu sau khi nhân đôi: {sample_count * 2}")
print(f"Đạt yêu cầu: {is_acceptable}")
# Kết quả mong muốn:
# <class 'int'>
# Mô hình: Linear Regression
# Số mẫu sau khi nhân đôi: 100
# Đạt yêu cầu: True
Kiểu dữ liệu quyết định phép toán nào hợp lệ. Hai số có thể cộng, trừ, nhân, chia; hai chuỗi có thể ghép bằng +; nhưng cộng trực tiếp chuỗi "10" với số 5 sẽ gây TypeError vì Python không tự đoán người lập trình muốn ghép văn bản hay tính toán số học. Khi dữ liệu nhập từ bàn phím hoặc đọc từ file CSV, nhiều giá trị ban đầu là chuỗi; cần chuyển kiểu bằng int(), float() hoặc str() trước khi tính toán. Chuyển kiểu có thể phát sinh ValueError nếu nội dung không đúng định dạng, ví dụ float("12.5a").  
   

Bảng 1.2. Các nhóm toán tử cơ bản trong Python.

  
   

Nhóm toán tử	Toán tử	Ý nghĩa và lưu ý
Số học	+ - * /	
Cộng, trừ, nhân, chia. Toán tử / luôn trả về kết quả dạng số thực.  
   

Số học	// % **	
Chia lấy phần nguyên, chia lấy phần dư, lũy thừa. Hữu ích khi tách nhóm, kiểm tra chẵn lẻ hoặc tính chỉ số.  
   

So sánh	== != > < >= <=	
Trả về True hoặc False; dùng trong rẽ nhánh, lọc dữ liệu và kiểm tra điều kiện.  
   

Logic	and or not	
Kết hợp nhiều điều kiện; and yêu cầu cả hai đúng, or chỉ cần ít nhất một đúng, not đảo kết quả.  
   

Gán	=	
Gán giá trị cho biến; không dùng để so sánh. So sánh bằng phải viết ==.  
   

Các biểu thức điều kiện nên được đọc theo nghĩa dữ liệu, không chỉ theo ký hiệu:  
   

0 <= score <= 10 kiểm tra điểm nằm trong thang hợp lệ;  
   

score >= 0 and score <= 10 là cách viết tách rời của cùng kiểm tra;  
   

Điều kiện kết hợp bằng and chỉ đúng khi mọi điều kiện thành phần đều đúng.  
   

Khi biểu thức dài, nên đặt từng điều kiện vào biến có tên rõ ràng để chương trình dễ đọc hơn.  
   

Mã nguồn 4. Chuyển kiểu và kiểm tra điều kiện trước khi tính toán

  
   

Python
raw_score = "8.5"
score = float(raw_score)
is_valid_score = 0 <= score <= 10
is_passed = score >= 5.0
print(f"Điểm hợp lệ: {is_valid_score}")
print(f"Qua ngưỡng: {is_passed}")
print(f"Phần nguyên của điểm: {score // 1}")
print(f"Điểm nhân hệ số 2: {score * 2}")
# Kết quả mong muốn:
# Điểm hợp lệ: True
# Qua ngưỡng: True
# Phần nguyên của điểm: 8.0
# Điểm nhân hệ số 2: 17.0
Nhập liệu an toàn bằng try-except. Dữ liệu từ input() luôn là chuỗi và có thể không chuyển được sang kiểu số. Khối try chứa thao tác có thể phát sinh lỗi; khối except ValueError xử lý riêng trường hợp sai định dạng thay vì để chương trình dừng đột ngột. Sau khi chuyển kiểu thành công, chương trình vẫn phải kiểm tra miền giá trị vì một số hợp lệ về cú pháp chưa chắc hợp lệ về nghiệp vụ.  
   

Mã nguồn 5. Nhập điểm an toàn cho pipeline dữ liệu

  
   

Python
raw_score = input("Nhập điểm số (0-10): ")
try:
    score = float(raw_score)
except ValueError:
    score = None
    print("Lỗi: dữ liệu nhập vào không phải là số")

if score is not None:
    if 0 <= score <= 10:
        print(f"Điểm hợp lệ: {score}")
    else:
        print("Lỗi: điểm phải nằm trong đoạn [0, 10]")
# Kết quả mong muốn:
# Điểm hợp lệ: 8.5
Ví dụ trên không âm thầm thay dữ liệu lỗi bằng 0 và phân biệt hai vấn đề: sai định dạng được bắt bởi except, còn sai miền giá trị được xử lý bằng điều kiện. Cấu trúc rẽ nhánh được giải thích chi tiết ở mục kế tiếp.  
   

1.2.2. Cấu trúc rẽ nhánh và vòng lặp
Sau khi có biến và điều kiện logic, chương trình cần quyết định sẽ chạy nhánh lệnh nào hoặc lặp lại một thao tác bao nhiêu lần. Đây là phần điều khiển luồng thực thi. Nếu không có rẽ nhánh, chương trình chỉ chạy tuần tự từ trên xuống dưới; nếu không có vòng lặp, người lập trình phải sao chép cùng một đoạn mã nhiều lần để xử lý nhiều bản ghi.  
   

Rẽ nhánh chọn khối lệnh dựa trên điều kiện. Python dùng ba từ khóa chính: if mở điều kiện đầu tiên, elif kiểm tra điều kiện thay thế khi các điều kiện trước sai, và else bắt mọi trường hợp còn lại. Mỗi dòng điều kiện kết thúc bằng dấu hai chấm :. Các câu lệnh thuộc cùng một nhánh phải được thụt vào cùng mức; bốn dấu cách là quy ước phổ biến. Python dùng thụt dòng để xác định khối lệnh, nên sai thụt dòng không chỉ làm xấu mã mà có thể làm thay đổi ý nghĩa chương trình.  
   

Mã nguồn 6. Khuôn mẫu if-elif-else

  
   

Python
temperature = 31
if temperature >= 35:
    action = "Bật cảnh báo nhiệt độ cao"
elif temperature >= 30:
    action = "Theo dõi nhiệt độ"
else:
    action = "Nhiệt độ ổn định"
print(action)
# Kết quả mong muốn:
# Theo dõi nhiệt độ
Trong một chuỗi if-elif-else, Python kiểm tra từ trên xuống và chỉ chạy nhánh đầu tiên có điều kiện đúng. Vì vậy, thứ tự điều kiện rất quan trọng. Các trường hợp đặc biệt như dữ liệu ngoài miền nên đặt trước các nhánh phân loại thông thường để tránh kết luận sai.  
   

Mã nguồn 7. Phân loại điểm bằng rẽ nhánh

  
   

Python
score = 8.5
if not 0 <= score <= 10:
    category = "Không hợp lệ"
elif score >= 9.0:
    category = "Xuất sắc"
elif score >= 7.0:
    category = "Khá"
else:
    category = "Cần cố gắng"
print(f"Kết quả đánh giá: {category}")
# Kết quả mong muốn:
# Kết quả đánh giá: Khá
Ở ví dụ trên, điều kiện not 0 <= score <= 10 được đặt đầu tiên để loại điểm không hợp lệ trước khi xếp loại. Nếu bỏ nhánh này, một giá trị như 12 có thể bị xếp loại “Xuất sắc” dù nằm ngoài thang điểm. Nhánh else không có điều kiện vì nó là lựa chọn cuối cùng khi mọi điều kiện trước đều sai.  
   

Vòng lặp thực hiện lặp lại một khối lệnh. Vòng lặp for phù hợp khi cần duyệt qua từng phần tử của một đối tượng có thể lặp, chẳng hạn danh sách, chuỗi, dòng dữ liệu hoặc dãy số tạo bởi range(). Cú pháp for item in collection: được đọc là: với mỗi item trong collection, chạy khối lệnh bên dưới một lần.  
   

Vòng lặp while phù hợp khi chưa biết trước số lần lặp, nhưng biết điều kiện để tiếp tục. Python kiểm tra điều kiện ở đầu mỗi lượt; nếu điều kiện còn True thì chạy thân vòng lặp, nếu False thì dừng. Với while, người lập trình phải chỉ ra cách điều kiện sẽ thay đổi để tránh vòng lặp vô hạn.  
   

Mã nguồn 8. Hai dạng vòng lặp

  
   

Python
for index in range(1, 4):
    print(f"Lần lặp: {index}")

countdown = 3
while countdown > 0:
    print(countdown)
    countdown -= 1
# Kết quả mong muốn:
# Lần lặp: 1
# Lần lặp: 2
# Lần lặp: 3
# 3
# 2
# 1
Hàm range(1, 4) tạo dãy 1, 2, 3; điểm dừng 4 không được lấy. Trong vòng lặp while, câu countdown -= 1 là bước cập nhật điều kiện. Nếu quên dòng này, countdown luôn lớn hơn 0 và chương trình sẽ lặp mãi. Khi xử lý dữ liệu, mỗi vòng lặp nên có mục tiêu rõ ràng: duyệt từng bản ghi, tính tổng, tìm giá trị đầu tiên thỏa điều kiện, hoặc tạo một danh sách kết quả mới.  
   

1.2.3. Điều khiển dòng thực thi: break, continue
continue bỏ phần còn lại của lượt hiện tại; break kết thúc vòng lặp gần nhất. Dùng chúng khi điều kiện bỏ qua/dừng rõ ràng, tránh tạo luồng điều khiển khó theo dõi.  
   

Mã nguồn 9. Bỏ dữ liệu không hợp lệ và dừng tại tín hiệu kết thúc

  
   

Python
values = [1, 2, -5, 4, 0, 6]
valid_values = []
for value in values:
    if value < 0:
        continue
    if value == 0:
        break
    valid_values.append(value)
print(valid_values) # [1, 2, 4]
# Kết quả mong muốn:
# [1, 2, 4]
Bài tập tự kiểm tra.

  
   

Giải thích khác biệt giữa /, // và % bằng ba ví dụ.  
   

Dùng for và continue để tính tổng số dương trong [10, -3, 25, 12, -5, 8, -30].  
   

Viết điều kiện xác nhận một tỷ lệ nằm trong đoạn từ 0 đến 1.  
   

1.3. Hàm và thiết kế chương trình
1.3.1. Định nghĩa hàm, tham số, giá trị trả về
Hàm gom một tác vụ có tên để tái sử dụng và kiểm thử. Tham số mô tả dữ liệu đầu vào; return kết thúc hàm và gửi kết quả về nơi gọi. Nếu không có return, hàm trả về None.  
   

Một hàm nên có đặc tả rõ ràng: miền đầu vào, kết quả đầu ra và lỗi dự kiến.  
   

Mã nguồn 10. Hàm tính độ chính xác có kiểm tra đầu vào

  
   

Python
def calculate_accuracy(correct_count, total_count=100):
    """Return accuracy in [0, 1] for valid counts."""
    if total_count <= 0:
        raise ValueError("total_count phải lớn hơn 0")
    if not 0 <= correct_count <= total_count:
        raise ValueError("correct_count nằm ngoài miền hợp lệ")
    return correct_count / total_count

accuracy_1 = calculate_accuracy(85)
accuracy_2 = calculate_accuracy(450, 500)
print(f"{accuracy_1:.1%}; {accuracy_2:.1%}")
# Kết quả mong muốn:
# 85.0%; 90.0%
Tham số vị trí phụ thuộc thứ tự; đối số theo tên tăng khả năng đọc, chẳng hạn calculate_accuracy(correct_count=90, total_count=120). Tham số mặc định phải đặt sau tham số không có mặc định.  
   

1.3.2. Phạm vi biến: local, global
Tên được tra cứu theo quy tắc LEGB: local, enclosing, global, built-in. Trong phạm vi đề cương, trọng tâm là local và global:  
   

Tên tạo trong hàm là local nếu không khai báo khác;  
   

Tên ở cấp module là global và có thể đọc từ trong hàm;  
   

Muốn gán lại tên global trong hàm phải khai báo global.  
   

Đối tượng của biến local không nhất thiết bị hủy ngay khi hàm kết thúc: nó còn tồn tại nếu nơi khác vẫn giữ tham chiếu. Trong thiết kế chương trình, ưu tiên truyền dữ liệu qua tham số và return thay vì sửa trạng thái global.  
   

Mã nguồn 11. Local và global không cần thay đổi trạng thái chung

  
   

Python
language_name = "Python" # global
def format_language(version):
    display_name = f"{language_name} {version}" # local
    return display_name

print(format_language("3.12"))
# print(display_name) # NameError nếu bỏ dấu #
# Kết quả mong muốn:
# Python 3.12
1.3.3. Tổ chức chương trình theo cấu trúc hàm
Phân rã một pipeline nhỏ thành các bước đơn nhiệm: kiểm tra, biến đổi, tổng hợp và trình bày. Mỗi hàm có thể được kiểm tra độc lập.  
   

Mã nguồn 12. Chương trình xử lý điểm theo cấu trúc hàm

  
   

Python
def clean_scores(raw_scores):
    """Keep numeric scores in the closed interval [0, 10]."""
    clean = []
    for score in raw_scores:
        if isinstance(score, (int, float)) and 0 <= score <= 10:
            clean.append(float(score))
    return clean

def mean(values):
    """Return None when no observation is available."""
    if not values:
        return None
    return sum(values) / len(values)

def main():
    raw_scores = [8.5, 9.0, -1.0, "N/A", 7.5, 12.0, 6.0]
    scores = clean_scores(raw_scores)
    average = mean(scores)
    print(f"Dữ liệu hợp lệ: {scores}")
    print("Không có dữ liệu" if average is None else f"Trung bình: {average:.2f}")

if __name__ == "__main__":
    main()
# Kết quả mong muốn:
# Dữ liệu hợp lệ: [8.5, 9.0, 7.5, 6.0]
# Trung bình: 7.75
Bài tập thiết kế. Viết hai hàm chuyển độ C sang độ F và phát cảnh báo nhiệt độ. Hàm chuyển đổi phải return giá trị; hàm cảnh báo nhận giá trị đã chuyển đổi, không đọc biến global.  
   

1.4. Cấu trúc dữ liệu cơ bản
1.4.1. List, tuple và kỹ thuật xử lý
List và tuple đều là sequence có thứ tự, hỗ trợ index (chỉ mục) và slice (lát cắt). List mutable, phù hợp collection thay đổi; tuple có cấu trúc ngoài immutable, phù hợp nhóm giá trị có ý nghĩa vị trí ổn định. Tuple vẫn có thể chứa phần tử mutable, vì vậy không nên hiểu “immutable” là mọi dữ liệu lồng nhau đều bất biến.  
   

Mã nguồn 13. List, tuple, slicing và comprehension

  
   

Python
scores = [7.5, 8.0, 6.5, 9.0]
scores.append(9.5)
first_three = scores[:3]
image_shape = (224, 224, 3)
scores_on_100 = [score * 10 for score in scores]

print(first_three)
print(f"Kích thước ảnh: {image_shape}")
print(scores_on_100)
# Kết quả mong muốn:
# [7.5, 8.0, 6.5]
# Kích thước ảnh: (224, 224, 3)
# [75.0, 80.0, 65.0, 90.0, 95.0]
Comprehension phù hợp cho biến đổi ngắn, rõ ràng. Nếu cần nhiều nhánh, nhiều side effect hoặc logic khó diễn đạt, dùng vòng lặp thường để tăng khả năng đọc.  
   

1.4.2. Dictionary, set và thao tác tập hợp
Dictionary ánh xạ key duy nhất đến value. Key phải hashable; truy cập bằng data[key] phát sinh KeyError nếu thiếu, còn data.get(key) cho phép cung cấp giá trị mặc định.  
   

Mã nguồn 14. Bản ghi dữ liệu bằng dictionary

  
   

Python
model_info = {
    "name": "Decision Tree",
    "accuracy": 0.88,
    "iteration_count": 100,
}
model_info["accuracy"] = 0.91
for key, value in model_info.items():
    print(f"{key}: {value}")
# Kết quả mong muốn:
# name: Decision Tree
# accuracy: 0.91
# iteration_count: 100
Set lưu phần tử duy nhất và hỗ trợ hợp, giao, hiệu. Set không bảo toàn số lần xuất hiện và không cam kết thứ tự trình bày, nên chỉ dùng để “loại trùng” khi hai đặc tính đó không quan trọng.  
   

Mã nguồn 15. Tính duy nhất và phép tập hợp

  
  
raw_labels = ["Mèo", "Chó", "Mèo", "Chim", "Chó"]
unique_labels = set(raw_labels)
train_labels = {"Mèo", "Chó", "Chim"}
test_labels = {"Chó", "Cá", "Rùa"}

# ổn định thứ tự khi hiển thị
print(sorted(unique_labels))
print(train_labels & test_labels) # giao
print(test_labels - train_labels) # nhãn mới trong test
# Kết quả mong muốn:
# ['Chim', 'Chó', 'Mèo']
# {'Chó'}
# {'Cá', 'Rùa'}
Hoạt động lựa chọn cấu trúc. Với từng tình huống – chuỗi thời gian, kích thước ảnh, hồ sơ sinh viên, tập nhãn duy nhất – hãy chọn list, tuple, dictionary hoặc set và giải thích bằng yêu cầu thay đổi, truy cập và thứ tự.  

1.5. Xử lý chuỗi và đọc/ghi file
  
1.5.1. Hàm xử lý chuỗi và lỗi thường gặp
  
Chuỗi Python là dãy Unicode immutable. Các thao tác thường dùng gồm:  

strip() loại whitespace ở hai đầu chuỗi;  

lower() và upper() đổi kiểu chữ;  

split() tách chuỗi theo whitespace;  

separator.join(parts) ghép các phần tử bằng một chuỗi phân tách;  

F-string định dạng kết quả để in hoặc ghi báo cáo.  

Chuẩn hóa Unicode cho tiếng Việt. Cùng một ký tự có dấu có thể được biểu diễn bằng một code point dựng sẵn hoặc bằng chữ cái cơ sở kết hợp với dấu. Hai chuỗi nhìn giống nhau vì thế vẫn có thể so sánh khác nhau và tạo token khác nhau. Trước khi so sánh, đếm hoặc tokenization văn bản tiếng Việt, nên đưa chuỗi về dạng NFC bằng unicodedata.normalize("NFC", text).  

Mã nguồn 16. Chuẩn hóa Unicode, khoảng trắng và chuyển đổi có kiểm soát

  
import unicodedata

raw_text = " Ngôn ngữ Python cho AI "
unicode_text = unicodedata.normalize("NFC", raw_text)
normalized_text = " ".join(unicode_text.strip().lower().split())
tokens = normalized_text.split()

print(normalized_text)
print(tokens)

raw_number = "12.5a"
try:
    numeric_value = float(raw_number)
except ValueError:
    numeric_value = None
    print(f"Không thể chuyển '{raw_number}' thành số thực")
# Kết quả mong muốn:
# ngôn ngữ python cho ai
# ['ngôn', 'ngữ', 'python', 'cho', 'ai']
# Không thể chuyển '12.5a' thành số thực
Không nên luôn thay dữ liệu lỗi bằng 0 vì 0 có thể là giá trị hợp lệ và làm sai phân bố. Tùy bài toán, dùng None, bỏ bản ghi, yêu cầu nhập lại hoặc ghi lỗi để xử lý sau.  

1.5.2. Đọc ghi file văn bản, CSV, JSON
  
Context manager with open(...) đóng file kể cả khi có ngoại lệ. Chỉ rõ encoding="utf-8" cho dữ liệu tiếng Việt. Chế độ r đọc, w tạo/ghi đè, a ghi tiếp. Khi ghi CSV trên Windows, dùng newline="".  

Mã nguồn 17. Ghi và đọc lại TXT, CSV, JSON

  

Python
import csv
import json
from pathlib import Path
from tempfile import TemporaryDirectory

with TemporaryDirectory() as temp_dir:
    folder = Path(temp_dir)
    
    # TXT
    text_path = folder / "nhat_ky.txt"
    text_path.write_text("Khởi tạo pipeline thành công!\n", encoding="utf-8")
    log_text = text_path.read_text(encoding="utf-8")
    
    # CSV
    csv_path = folder / "ket_qua.csv"
    rows = [["id", "model", "accuracy"], [1, "SVM", 0.85], [2, "RF", 0.92]]
    with csv_path.open("w", newline="", encoding="utf-8") as file:
        csv.writer(file).writerows(rows)
    with csv_path.open("r", newline="", encoding="utf-8") as file:
        loaded_rows = list(csv.DictReader(file))
        
    # JSON
    json_path = folder / "config.json"
    config = {"project": "Nhận dạng", "epochs": 50}
    with json_path.open("w", encoding="utf-8") as file:
        json.dump(config, file, ensure_ascii=False, indent=2)
    with json_path.open("r", encoding="utf-8") as file:
        loaded_config = json.load(file)
        
    print(log_text.strip())
    print(loaded_rows)
    print(loaded_config)
# Kết quả mong muốn:
# Khởi tạo pipeline thành công!
# [{'id': '1', 'model': 'SVM', 'accuracy': '0.85'}, {'id': '2', 'model': 'RF', 'accuracy': '0.92'}]
# {'project': 'Nhận dạng', 'epochs': 50}
Ví dụ dùng thư mục tạm để không để lại file sau khi chạy. Trong bài tập thực tế, người học có thể thay folder bằng thư mục dữ liệu của dự án. Không ghép đường dẫn bằng dấu gạch thủ công; pathlib.Path giúp mã chạy nhất quán trên Windows, macOS và Linux.  

Tổng kết và bài tập chương
    
   
+ 4
Giải thích được vị trí của Python trong pipeline dữ liệu/AI.  

Chạy được script, đọc traceback và phân biệt lỗi cú pháp, runtime, logic.  

Dùng đúng kiểu, toán tử, rẽ nhánh, vòng lặp, break và continue.  

Viết hàm có quy ước đầu vào đầu ra rõ ràng và hạn chế trạng thái toàn cục.  

Chọn cấu trúc dữ liệu dựa trên thứ tự, tính duy nhất và khả năng thay đổi.  

Chuẩn hóa Unicode NFC và đọc/ghi được TXT, CSV, JSON bằng UTF-8.  

Bài tập tích hợp. Tạo chương trình đọc một file CSV gồm mã sinh viên, họ tên và điểm dạng chuỗi. Chương trình phải:  

Tách thành các hàm đọc, kiểm tra, tóm tắt và ghi kết quả;  

Chuẩn hóa họ tên về Unicode NFC, loại khoảng trắng thừa và viết hoa nhất quán;  

Bỏ qua điểm không chuyển được thành số hoặc ngoài đoạn [0, 10] và ghi rõ số dòng bị loại;  

Tính điểm trung bình của dữ liệu hợp lệ;  

Ghi báo cáo JSON bằng UTF-8, kèm metadata gồm tổng số dòng, số dòng hợp lệ và số dòng bị loại; sau đó đọc lại để xác nhận.  

Sản phẩm gồm mã nguồn, ba file dữ liệu thử (hợp lệ, có lỗi, rỗng) và mô tả ngắn về cấu trúc dữ liệu đã chọn.  

⬅️ Quay lại Mục lục chính | Đi tới Bài tập (Bài tập tự học theo buổi) ➡️
