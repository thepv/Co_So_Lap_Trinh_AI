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

## 📊 2. Bảng So Sánh Các Cách Biểu Diễn Hồ Sơ

| Tiêu Chí | Cách 1: List of Dictionaries (`List[dict]`) | Cách 2: List of Tuples (`List[tuple]`) |
| :--- | :--- | :--- |
| **Độ tường minh** | 🟢 **Rất cao**: Truy cập dữ liệu qua từ khóa rõ ràng như `sv["name"]`. | 🔴 **Thấp**: Truy cập qua chỉ số index cố định như `sv[1]`, dễ nhầm lẫn. |
| **Tính linh hoạt** | 🟢 **Cao**: Dễ dàng thêm, sửa hoặc xóa bớt các trường thông tin của sinh viên. | 🔴 **Thấp**: Tuple là immutable (bất biến), muốn sửa phải tạo mới hoàn toàn. |
| **Hiệu năng bộ nhớ**| 🟡 **Tốn nhiều bộ nhớ hơn** do phải lưu trữ các cặp key-value cho từng bản ghi. | 🟢 **Tối ưu hơn**, chiếm ít tài nguyên bộ nhớ hơn trong các mảng dữ liệu lớn. |

📌 **Lý do lựa chọn cấu trúc cuối (`List[dict]`):** Mặc dù tốn bộ nhớ hơn một chút so với Tuple, nhưng `List[dict]` mang lại sự cân bằng hoàn hảo cho các ứng dụng thực tế. Nó giúp mã nguồn cực kỳ dễ đọc, dễ bảo trì, và tích hợp tự nhiên với các định dạng dữ liệu phổ biến hiện nay như JSON hoặc bản ghi từ Cơ sở dữ liệu (Database rows).  
Sản phẩm. File mã nguồn, từ điển dữ liệu và bảng so sánh cấu trúc dữ liệu.  
Hoàn thành khi. Cấu trúc được chọn phù hợp với thứ tự, tính duy nhất và khả năng thay đổi;
chương trình phát hiện đúng bản ghi trùng.  
Tham khảo:  
<img width="1052" height="412" alt="image" src="https://github.com/user-attachments/assets/a55d1eea-a6e9-476e-b131-164c8e2c203b" />

---

