### Bài tập 1: Phân tầng cảnh báo tài nguyên máy chủ huấn luyện (GPU Monitoring)

**Mục tiêu:** Rèn luyện thiết kế thứ tự điều kiện trong chuỗi `if-elif-else` để cảnh báo theo cấp độ nghiêm trọng.

**Mô tả:** Trong quá trình huấn luyện mô hình học sâu, nhiệt độ GPU (`gpu_temp` tính theo độ C) được giám sát liên tục:
* Nếu \(gpu\_temp \ge 90^\circ\text{C}\): Phát cảnh báo `"NGUY HIỂM: Dừng khẩn cấp tiến trình!"`.
* Nếu \(80^\circ\text{C} \le gpu\_temp < 90^\circ\text{C}\): Cảnh báo `"CẢNH BÁO: Bật quạt tối đa và hạ xung"`.
* Nếu \(65^\circ\text{C} \le gpu\_temp < 80^\circ\text{C}\): Thông báo `"HOẠT ĐỘNG: Tải cao nhưng an toàn"`.
* Dưới 65°C: Thông báo `"HOẠT ĐỘNG: Tải bình thường"`.

**Yêu cầu:** Khai báo biến `gpu_temp = 83.5` và viết cấu trúc rẽ nhánh phù hợp để xuất thông điệp tương ứng.

---
### Bài tập 2: Xác thực và xử lý nhãn phân loại giao dịch (Transaction Verification)

**Mục tiêu:** Kết hợp rẽ nhánh với toán tử kiểm tra tập hợp (`in`) và xử lý dữ liệu khuyết thiếu (`None`).

**Mô tả:** Bảng dữ liệu giao dịch có trường phân loại hình thức thanh toán `payment_method`. Hệ thống chỉ chấp nhận 3 hình thức hợp lệ: `"MOMO"`, `"VNPAY"`, `"BANK_TRANSFER"`. Dữ liệu có thể bị khuyết (`None`), chứa chuỗi rỗng `""`, hoặc chứa hình thức lạ chưa hỗ trợ (ví dụ `"CASH"`).

**Yêu cầu:** Viết đoạn mã kiểm tra biến `payment_method`:
* Nếu là `None` hoặc chuỗi rỗng: Phân loại là `"Thiếu thông tin"`.
* Nếu nằm trong 3 hình thức được hỗ trợ: Chuẩn hóa và thông báo `"Hợp lệ: <TÊN_PHƯƠNG_THỨC>"`.
* Mọi trường hợp còn lại: Thông báo `"Phương thức không được hỗ trợ: <GIÁ_TRỊ>"`.

---

