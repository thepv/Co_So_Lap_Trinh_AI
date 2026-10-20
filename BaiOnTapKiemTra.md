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

Tham khảo:  
<img width="650" height="210" alt="image" src="https://github.com/user-attachments/assets/2f856ab2-76aa-473f-8352-b4dee228327e" />
---
