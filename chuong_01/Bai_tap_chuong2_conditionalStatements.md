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
### Bài tập 3: Cổng kiểm soát chất lượng dữ liệu trước huấn luyện (Quality Gate)

**Mục tiêu:** Áp dụng rẽ nhánh lồng nhau hoặc kết hợp điều kiện phức hợp để đưa ra quyết định chấp thuận hay từ chối một tập dữ liệu đầu vào.

**Mô tả:** Một tập dữ liệu khách hàng được đánh giá bởi 3 chỉ số:
* `missing_rate`: Tỷ lệ khuyết thiếu của toàn bảng (giá trị thực từ 0.0 đến 1.0).
* `duplicate_id_count`: Số lượng mã định danh khách hàng bị trùng lặp (số nguyên).
* `row_count`: Số dòng dữ liệu thực tế thu thập được.

**Quy tắc quyết định:**
* Nếu `row_count < 100`: Từ chối ngay lập tức với lý do `"Dữ liệu quá ít để huấn luyện"`.
* Nếu `duplicate_id_count > 0`: Từ chối với lý do `"Vi phạm tính duy nhất của ID: cần lọc trùng trước"`.
* Nếu `missing_rate > 0.2` (>20%): Từ chối với lý do `"Tỷ lệ khuyết thiếu quá cao"`.
* Nếu `missing_rate > 0.05` (từ 5% đến 20%): Cảnh báo `"Chấp thuận có điều kiện: Cần kích hoạt bộ điền khuyết (Imputer)"`.
* Ngược lại: Thông báo `"Chấp thuận: Dữ liệu sạch, sẵn sàng bàn giao cho mô hình"`.

**Yêu cầu:** Viết chương trình kiểm tra cho bộ chỉ số: `row_count = 250`, `duplicate_id_count = 0`, `missing_rate = 0.08`.

---
### Bài tập 4: Tối ưu hóa phân phối tác vụ xử lý bất đồng bộ (Task Worker Dispatcher)

**Mục tiêu:** Áp dụng cấu trúc `match-case` (Python 3.10+) để định tuyến tác vụ và sử dụng toán tử ternary (`if-else` một dòng) để gán trạng thái ưu tiên tốc độ xử lý.

**Mô tả:** Hệ thống xử lý dữ liệu nhận vào các gói tác vụ dưới dạng một từ điển (dictionary). Mỗi tác vụ gồm hai thông tin: `type` (loại tác vụ: `"ETL"`, `"TRAIN"`, `"INFERENCE"`) và `size` (dung lượng dữ liệu tính bằng MB).

**Quy tắc xử lý:**
1. Định tuyến worker dựa trên `type` bằng `match-case`:
   * `"ETL"`: Định tuyến đến `"Spark-Worker"`.
   * `"TRAIN"`: Định tuyến đến `"GPU-Worker"`.
   * `"INFERENCE"`: Định tuyến đến `"CPU-Worker"`.
   * Các giá trị khác: Định tuyến đến `"Unknown-Worker"`.
2. Xác định chế độ hàng đợi bằng toán tử ternary (một dòng): Nếu `size > 500` thì chế độ là `"High-Priority"`, ngược lại là `"Standard"`.

**Yêu cầu:** Khai báo tác vụ `task = {"type": "TRAIN", "size": 650}`. Viết chương trình xuất ra thông tin định tuyến theo cấu trúc: `"[<CHẾ_ĐỘ_HÀNG_ĐỢI>] Gửi tác vụ đến <TÊN_WORKER>"`

---



