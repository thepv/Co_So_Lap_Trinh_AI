### Bài tập 1: Phân tầng cảnh báo tài nguyên máy chủ huấn luyện (GPU Monitoring)

**Mục tiêu:** Rèn luyện thiết kế thứ tự điều kiện trong chuỗi `if-elif-else` để cảnh báo theo cấp độ nghiêm trọng.

**Mô tả:** Trong quá trình huấn luyện mô hình học sâu, nhiệt độ GPU (`gpu_temp` tính theo độ C) được giám sát liên tục:
* Nếu \(gpu\_temp \ge 90^\circ\text{C}\): Phát cảnh báo `"NGUY HIỂM: Dừng khẩn cấp tiến trình!"`.
* Nếu \(80^\circ\text{C} \le gpu\_temp < 90^\circ\text{C}\): Cảnh báo `"CẢNH BÁO: Bật quạt tối đa và hạ xung"`.
* Nếu \(65^\circ\text{C} \le gpu\_temp < 80^\circ\text{C}\): Thông báo `"HOẠT ĐỘNG: Tải cao nhưng an toàn"`.
* Dưới 65°C: Thông báo `"HOẠT ĐỘNG: Tải bình thường"`.

**Yêu cầu:** Khai báo biến `gpu_temp = 83.5` và viết cấu trúc rẽ nhánh phù hợp để xuất thông điệp tương ứng.

---
