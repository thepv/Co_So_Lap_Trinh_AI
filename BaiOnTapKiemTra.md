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
