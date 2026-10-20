# 📝 Bài 1: Kiểm Tra và Phân Loại Danh Sách Điểm

## 🎯 Nhiệm Vụ
Viết một chương trình thực hiện các công việc sau với một danh sách điểm dạng chuỗi (String):
1. **Kiểm tra** tính hợp lệ của từng phần tử.
2. **Chuyển đổi** các giá trị hợp lệ sang dạng số.
3. **Phân loại** các điểm hợp lệ theo từng khoảng điểm cụ thể.
4. **Ghi nhận riêng** các giá trị không hợp lệ (không phải là số hoặc nằm ngoài phạm vi cho phép).

> ⚠️ **Yêu cầu kỹ thuật:** Sử dụng ít nhất một vòng lặp (`for`/`while`), một cấu trúc rẽ nhánh (`if-elif-else`), và từ khóa kiểm soát luồng `break` hoặc `continue` đúng ngữ cảnh.

---

## 📥 Đầu Vào (Input)
* Một danh sách các chuỗi ký tự biểu diễn điểm số (Ví dụ: `["8.5", "10", "abc", "-2", "7.0", "12"]`).
* Điểm số hợp lệ phải thuộc đoạn **`[0; 10]`**.

## 📤 Đầu Ra (Output)
Một cấu trúc dữ liệu dạng **Dictionary** chứa 3 danh sách thành phần:
* `valid`: Danh sách các điểm số hợp lệ (đã chuyển sang kiểu số thực).
* `invalid`: Danh sách các chuỗi không hợp lệ được giữ nguyên bản.
* `levels`: Danh sách xếp loại học lực tương ứng, có **cùng thứ tự** và kích thước với danh sách `valid`.

---

## 📊 Tiêu Chí Phân Loại Gợi Ý (`levels`)
Dưới đây là bảng phân loại khoảng điểm tham khảo:

| Khoảng Điểm | Xếp Loại 🏆 |
| :--- | :--- |
| `[9.0 - 10.0]` | Xuất Sắc 🔴 |
| `[8.0 - 9.0)` | Giỏi 🟢 |
| `[6.5 - 8.0)` | Khá 🟡 |
| `[5.0 - 6.5)` | Trung Bình 🔵 |
| `[0.0 - 5.0)` | Yếu 🟤 |  

Sản phẩm. File mã nguồn, bảng ít nhất 12 ca kiểm thử và bản giải thích ngắn cho ba quyết định
điều khiển dòng thực thi.  
Hoàn thành khi. Chương trình xử lý được biên, dữ liệu rỗng và dữ liệu sai; kết quả của các ca
kiểm thử khớp với dự đoán  

Tham khảo:  
<img width="650" height="210" alt="image" src="https://github.com/user-attachments/assets/2f856ab2-76aa-473f-8352-b4dee228327e" />
---  
# 🛠️ Bài 2: Tái Cấu Trúc Chương Trình (Refactoring) & Xử Lý Lỗi

## 🎯 Nhiệm Vụ
Tái cấu trúc lại toàn bộ chương trình xử lý điểm số của bài trước để nâng cao tính mô-đun, khả năng bảo trì và kiểm thử:
1. **Chia nhỏ chương trình** thành các hàm đơn nhiệm (Single Responsibility): Nhập dữ liệu, kiểm tra hợp lệ, phân loại, tóm tắt thống kê và hiển thị.
2. **Loại bỏ hoàn toàn trạng thái toàn cục** (Global State), truyền và nhận dữ liệu nghiêm ngặt qua tham số (`arguments`) và giá trị trả về (`return`).
3. **Bổ sung Docstring** chuẩn hóa cho từng hàm để giải thích rõ ràng chức năng, đầu vào và đầu ra.
4. **Chủ động tạo một lỗi hệ thống** để quan sát, phân tích lỗi thông qua `Traceback` trước khi tiến hành sửa lỗi.

---

## 📥 Đầu Vào (Input)
* Danh sách các chuỗi ký tự biểu diễn điểm số tương tự như bài trước.

## 📤 Đầu Đầu Ra (Output)
Một **Báo cáo tổng hợp** (Summary Report) hiển thị trực quan các thông tin:
* Tổng số lượng điểm hợp lệ và không hợp lệ.
* Điểm trung bình của các sinh viên có điểm hợp lệ.
* Số lượng chi tiết sinh viên đạt được theo từng mức phân loại (Xuất sắc, Giỏi, Khá...).
Tham khảo:  
<img width="1047" height="372" alt="image" src="https://github.com/user-attachments/assets/e997b66e-3261-4ddb-a201-3dfb8a45ee32" />
---  

# 🗂️ Bài 3: Cấu Trúc Dữ Liệu Nâng Cao & Quản Lý Hồ Sơ Sinh Viên

## 🎯 Nhiệm Vụ
Nâng cấp hệ thống quản lý để xử lý hồ sơ sinh viên toàn diện bằng cách phối hợp nhiều cấu trúc dữ liệu nền tảng:
1. **Thử nghiệm biểu diễn** hồ sơ sinh viên bằng 4 cấu trúc: `List`, `Tuple`, `Dictionary`, và `Set`.
2. **Tính toán thống kê mô tả cơ bản** (Số lượng sinh viên, điểm trung bình).
3. **Phát hiện mã sinh viên trùng lặp** (`student_id`) bằng cách tận dụng đặc tính của `Set`.
4. **Lập từ điển dữ liệu** (Data Dictionary) để đặc tả thuộc tính của các trường thông tin.
5. **So sánh ít nhất hai cách biểu diễn** và lập luận lý do lựa chọn cấu trúc tối ưu cuối cùng.

---

## 📘 1. Từ Điển Dữ Liệu (Data Dictionary)

Bảng đặc tả cấu trúc của một bản ghi học viên trong hệ thống:

| Tên Trường (Field) | Kiểu Dữ Liệu | Ràng Buộc (Constraints) | Mô Tả |
| :--- | :--- | :--- | :--- |
| `student_id` | `str` | Duy nhất (Unique), không rỗng | Mã định danh sinh viên (Ví dụ: "SV001") |
| `name` | `str` | Không chứa ký tự số | Họ và tên đầy đủ của sinh viên |
| `score` | `float` | Thuộc đoạn `[0.0; 10.0]` | Điểm số tổng kết của sinh viên |

--- 
Sản phẩm. File mã nguồn, từ điển dữ liệu và bảng so sánh cấu trúc dữ liệu.  
Hoàn thành khi. Cấu trúc được chọn phù hợp với thứ tự, tính duy nhất và khả năng thay đổi;
chương trình phát hiện đúng bản ghi trùng.  
Tham khảo:  
<img width="1052" height="412" alt="image" src="https://github.com/user-attachments/assets/a55d1eea-a6e9-476e-b131-164c8e2c203b" />
---  
# 🌡️ Bài 4: Xử Lý Mảng Nâng Cao Với NumPy & Tối Ưu Hóa Hiệu Năng Vector Hóa

## 🎯 Nhiệm Vụ
Ứng dụng thư viện NumPy để quản lý, thao tác dữ liệu nhiệt độ đa chiều và đánh giá hiệu năng giữa tư duy lập trình tối ưu (Vectorization) với vòng lặp truyền thống:
1. **Khởi tạo mảng dữ liệu**: Tạo mảng NumPy 2D mô phỏng nhiệt độ của **3 trạm khí tượng** trong vòng **7 ngày**.
2. **Khảo sát thuộc tính mảng (Shape Inspection)**: Dự đoán và kiểm tra hình dạng (`shape`) của mảng sau các thao tác truy xuất dữ liệu nâng cao gồm: Indexing (Chỉ mục), Slicing (Cắt mảng), và Broadcasting (Lan truyền).
3. **Tính toán thống kê tốc độ cao**: Sử dụng các hàm toán học tích hợp sẵn của NumPy (Vectorization) để tính nhiệt độ trung bình theo từng trạm và theo từng ngày.
4. **Đánh giá hiệu năng (Benchmarking)**: Mở rộng quy mô dữ liệu đủ lớn, viết phiên bản dùng vòng lặp lồng nhau (`for`), đo lường và so sánh thời gian thực thi với phiên bản vector hóa của NumPy để thấy rõ sự khác biệt.

---

## 📥 Đầu Vào (Input)
* Mảng NumPy (`np.ndarray`) kiểu số thực (`float`) có cấu trúc `shape = (n_stations, n_days)`.
* Trong đó: Mỗi **hàng** đại diện cho một trạm khí tượng, mỗi **cột** đại diện cho một ngày trong tuần.

## 📤 Đầu Ra (Output)
* Hai vector kết quả:
  * Vector trung bình theo **trạm** (Chiều dài bằng số trạm).
  * Vector trung bình theo **ngày** (Chiều dài bằng số ngày).
* Kết quả đo lường thời gian chạy (Báo cáo Benchmark tốc độ giữa Vòng lặp vs Vectorization).

---

## 🔬 1. Ma Trận Dự Đoán Hình Dạng (Shape) Khi Thao Tác Mảng

Giả sử mảng gốc `weather_data` có `shape = (3, 7)`. Dưới đây là bảng phân tích, giải thích bản chất kích thước trước khi thực thi mã nguồn:

| Thao Tác Thí Nghiệm | Cú Pháp Minh Họa | Shape Dự Đoán | Shape Thực Tế | Giải Thích Bản Chất Kích Thước 💡 |
| :--- | :--- | :--- | :--- | :--- |
| **Scalar Indexing** | `weather_data` | `()` | `()` | Lấy ra 1 phần tử đơn lẻ (hàng 2, cột 3) nên giảm toàn bộ số chiều về một giá trị vô hướng (0D Scalar). |
| **Slicing (Cắt mảng)** | `weather_data[0:2, 1:4]` | `(2, 3)` | `(2, 3)` | Giữ nguyên số chiều (2D). Trích xuất vùng giao giữa hàng 0,1 và cột 1,2,3. |
| **Mixed (Index + Slice)**| `weather_data[1, :]` | `(7,)` | `(7,)` | Trích xuất toàn bộ cột thuộc hàng số 2. Chiều nào nhận chỉ mục đơn lẻ cố định sẽ bị triệt tiêu, mảng hạ xuống thành 1D. |
| **Broadcasting** | `weather_data + vector_1D` | `(3, 7)` | `(3, 7)` | Cộng mảng `(3, 7)` với một mảng 1D `(7,)`. NumPy tự động nhân bản (stretch) dòng dữ liệu dọc theo trục đứng (trục trạm) để khớp kích thước thực hiện phép toán song song. |

---
# 🐼 Bài 5: Phân Tích Dữ Liệu Khí Tượng Với Pandas & Kỹ Thuật Xử Lý Dữ Liệu Khuyết Thiếu

## 🎯 Nhiệm Vụ
Sử dụng thư viện Pandas để thực hiện quy trình chuẩn hóa, phân tích cấu trúc và làm sạch một tập dữ liệu khí tượng thực tế được đọc từ tệp tin CSV:
1. **Khảo sát lược đồ (Schema Inspection)**: Đọc dữ liệu, kiểm tra các kiểu dữ liệu (`dtypes`), số lượng bản ghi và phát hiện các cột chứa giá trị rỗng (`Null/NaN`).
2. **Thiết lập Chỉ mục đa cấp (MultiIndex)**: Định hình lại cấu trúc bảng bằng cách đặt cấu trúc phân cấp chỉ mục dựa trên hai trường định danh: Trạm khí tượng (`Station_ID`) và Thời gian (`Year`, `Month`).
3. **Truy vấn nâng cao (Lọc & Sắp xếp)**: Sử dụng phương thức `.loc` để trích xuất dữ liệu theo MultiIndex và sắp xếp lại chỉ mục theo thứ tự logic nhằm tối ưu hóa hiệu năng truy vấn.
4. **Thống kê tỷ lệ khuyết thiếu (Missing Data Analysis)**: Tính toán chính xác tỷ lệ phần trăm dữ liệu bị trống trên từng thuộc tính đo lường.
5. **Xử lý dữ liệu thiếu có lập luận**: Đề xuất giải pháp loại bỏ hoặc nội suy dữ liệu dựa trên bản chất khoa học của từng chỉ số khí tượng.

---

## 📥 Đầu Vào (Input)
* Tệp dữ liệu cấu trúc mẫu mang tên `weather_stations_data.csv` (Sinh viên tải về theo liên kết được cung cấp cuối bài).
* Tập dữ liệu bao gồm các trường thông tin: `Station_ID` (Mã trạm), `Year` (Năm), `Month` (Tháng), `Temperature` (Nhiệt độ), `Humidity` (Độ ẩm), và `Precipitation` (Lượng mưa). Trong đó một số ô dữ liệu bị trống do lỗi thiết bị cảm biến.

## 📤 Đầu Ra (Output)
* Mã nguồn Python triển khai đầy đủ các bước khảo sát, đặt MultiIndex, lọc và xử lý khuyết thiếu.
* Bảng thống kê chi tiết tỷ lệ khuyết thiếu của từng thuộc tính kèm theo lập luận khoa học cho phương án xử lý được lựa chọn.

  ## 📥 Tệp Dữ Liệu Thực Hành Cho Sinh Viên
Sinh viên tải tệp dữ liệu mẫu để thực hiện bài tập này tại liên kết dưới đây:
* **Tải xuống tệp tin**: [weather_stations_data.csv](weather_stations_data.csv) *(lưu tệp tin này vào cùng thư mục chứa file mã nguồn Python để chương trình nhận diện chính xác).*

---

