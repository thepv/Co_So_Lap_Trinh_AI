### Bài tập 1: Lọc dữ liệu khuyết thiếu và tính điểm trung bình (Data Aggregation)

**Mục tiêu:** Áp dụng vòng lặp `for`, câu lệnh `continue`, và xử lý dữ liệu hỗn hợp (chuỗi, số, `None`).

**Mô tả bài toán:** Cho danh sách điểm số thu thập được từ một bảng khảo sát khách hàng hoặc người dùng:
`raw_scores = [8.5, None, 9.0, "N/A", 7.0, -1.0, 11.5, 6.5]`

**Yêu cầu:**
* Dùng vòng lặp `for` duyệt qua từng giá trị trong danh sách `raw_scores`.
* Bỏ qua các giá trị không phải số (không phải kiểu dữ liệu `int` hoặc `float`) hoặc bị khuyết thiếu (`None`) bằng câu lệnh `continue`.
* Bỏ qua các điểm số nằm ngoài miền quy định từ 0 đến 10 (\([0, 10]\)) bằng câu lệnh `continue`.
* Lưu các điểm hợp lệ lọc được vào một danh sách mới tên là `valid_scores`.
* Tính tổng và điểm trung bình của các điểm hợp lệ sau khi vòng lặp kết thúc.

---

### Gợi ý lời giải (Python)

```python
# Danh sách dữ liệu thô đầu vào
raw_scores = [8.5, None, 9.0, "N/A", 7.0, -1.0, 11.5, 6.5]

# Khởi tạo danh sách chứa các điểm hợp lệ
valid_scores = []

# Duyệt và lọc dữ liệu bằng vòng lặp
for score in raw_scores:
    # 1. Kiểm tra kiểu dữ liệu (chỉ chấp nhận int hoặc float, loại bỏ None và chuỗi)
        ............
    # 2. Kiểm tra miền giá trị hợp lệ [0, 10]
   ...........
        
    # Thêm giá trị hợp lệ vào danh sách mới
    ........

# Tính toán kết quả sau khi kết thúc vòng lặp
.........
```
### Bài tập 2: Cơ chế dừng sớm trong huấn luyện mô hình (Early Stopping Simulation)

**Mục tiêu:** Rèn luyện vòng lặp `for`, lệnh `break`, và kỹ thuật so sánh giá trị giữa các bước lặp liên tiếp.

**Mô tả bài toán:** Trong quá trình huấn luyện mạng nơ-ron qua các epoch, hàm mất mát trên tập kiểm định (`val_loss`) được ghi nhận qua một danh sách:
`val_losses = [0.95, 0.78, 0.65, 0.64, 0.66, 0.68, 0.71]`  
*(Mô hình huấn luyện tối đa 7 epoch, tương ứng từ epoch 1 đến epoch 7)*.

**Yêu cầu:**
Mô phỏng cơ chế dừng sớm (Early Stopping): 
* Nếu `val_loss` ở epoch hiện tại tăng cao hơn `val_loss` của epoch ngay trước đó (dấu hiệu mô hình bắt đầu bị *overfitting*), chương trình sẽ kích hoạt lệnh `break` để ngắt vòng lặp ngay lập tức và in thông báo dừng ở epoch nào với giá trị tổn thất tương ứng.
* Nếu không bị tăng, tiếp tục duyệt qua các epoch tiếp theo cho đến hết danh sách.

---
### Bài tập 3: Mô phỏng thuật toán tối ưu Gradient Descent 1D (Vòng lặp while)

**Mục tiêu:** Vận dụng vòng lặp `while` có điều kiện hội tụ và bộ đếm giới hạn số bước lặp (*safety limit*) để tránh vòng lặp vô hạn.

**Mô tả bài toán:** Tìm giá trị nhỏ nhất của hàm số \(f(x) = x^2\) bằng thuật toán Gradient Descent.
* Đạo hàm (gradient): \(g = 2x\).
* Công thức cập nhật: \(x_{new} = x - \alpha \cdot g\) (với tốc độ học \(\alpha = 0.1\)).
* Điểm bắt đầu: \(x = 10.0\).

**Yêu cầu:**
Viết vòng lặp `while` để cập nhật \(x\) tuân theo các quy tắc sau:
* Vòng lặp tiếp tục khi độ lớn của gradient \(\vert{}g\vert{} > 0.01\) (chưa hội tụ).
* Đồng thời, số bước lặp `iterations` không được vượt quá `100` bước (điều kiện dừng khẩn cấp tránh lặp vô hạn).
* Ở mỗi bước lặp, tăng biến đếm `iterations += 1`.
* Khi kết thúc vòng lặp, in ra số bước lặp đã thực hiện và giá trị \(x\) tối ưu tìm được.

*Gợi ý: Điều kiện vòng lặp sẽ có dạng `while abs(gradient) > 0.01 and iterations < max_iterations:`. Hãy nhớ cập nhật lại giá trị `gradient` sau mỗi lần thay đổi \(x\).*

---

### Gợi ý lời giải
Gợi ý:Điều kiện vòng lặp: while abs(gradient) > 0.01 and iterations < 100: (với gradient = 2 * x).Nhớ cập nhật lại biến gradient sau mỗi lần đổi $x$.
```
### Bài tập 9: Hệ thống đề xuất mã giảm giá phù hợp với giỏ hàng (Nested Loops)

**Mục tiêu:** Vận dụng vòng lặp lồng nhau (*nested loops*) để ghép cặp dữ liệu từ hai danh sách độc lập, áp dụng điều kiện rẽ nhánh để kiểm tra tính hợp lệ và tối ưu hóa lựa chọn.

**Mô tả bài toán:** Một khách hàng có danh sách các sản phẩm đang có trong giỏ hàng (mỗi sản phẩm là một từ điển chứa thông tin tên và giá tiền). Hệ thống có một danh sách các mã giảm giá (voucher) đang áp dụng, mỗi mã yêu cầu giá trị đơn hàng tối thiểu để được kích hoạt.

Cho dữ liệu đầu vào:
```python
cart_items = [
    {"name": "Chuột máy tính", "price": 250},
    {"name": "Bàn phím cơ", "price": 850},
    {"name": "Tai nghe Gaming", "price": 400}
]

vouchers = [
    {"code": "WELCOME50", "min_spend": 200, "discount": 50},
    {"code": "TECHMAX150", "min_spend": 1000, "discount": 150},
    {"code": "VIPSUPER500", "min_spend": 2000, "discount": 500}
]
```
### Bài tập 4: bài toán thực tế trong lĩnh vực Thương mại điện tử (E-commerce)
**Yêu cầu:**
* Duyệt qua từng sản phẩm trong giỏ hàng bằng vòng lặp để tính tổng giá trị đơn hàng (`total_bill`).
* Sử dụng vòng lặp lồng nhau để kiểm tra xem với tổng giá trị đơn hàng đó, khách hàng có thể áp dụng được những mã giảm giá nào (điều kiện: `total_bill >= min_spend`).
* In ra tổng tiền của giỏ hàng và danh sách các mã giảm giá hợp lệ kèm số tiền thực tế khách hàng cần trả sau khi giảm giá (`total_bill - discount`).

---

### Gợi ý lời giải (Python)

```python
# Dữ liệu giỏ hàng của khách hàng
cart_items = [
    {"name": "Chuột máy tính", "price": 250},
    {"name": "Bàn phím cơ", "price": 850},
    {"name": "Tai nghe Gaming", "price": 400}
]

# Dữ liệu các mã giảm giá hiện có trên hệ thống
vouchers = [
    {"code": "WELCOME50", "min_spend": 200, "discount": 50},
    {"code": "TECHMAX150", "min_spend": 1000, "discount": 150},
    {"code": "VIPSUPER500", "min_spend": 2000, "discount": 500}
]

# 1. Tính tổng giá trị đơn hàng
total_bill = 0
for item in cart_items:
    total_bill += item["price"]

print(f"Tổng giá trị đơn hàng hiện tại: {total_bill}k VNĐ")
print("-" * 50)
print("Các mã giảm giá bạn có thể áp dụng:")

# 2. Vòng lặp lồng nhau (ở đây lồng sau khi đã có tổng bill) 
# để so khớp điều kiện của từng voucher
has_voucher = False
for voucher in vouchers:
    if total_bill >= voucher["min_spend"]:
        final_price = total_bill - voucher["discount"]
        print(f"  - Mã [{voucher['code']}]: Giảm {voucher['discount']}k -> Số tiền còn lại: {final_price}k VNĐ")
        has_voucher = True

if not has_voucher:
    print("  Không có mã giảm giá nào phù hợp với đơn hàng của bạn.")
```


