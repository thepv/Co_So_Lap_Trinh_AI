Bài tập 1: Xây dựng hàm kiểm định và làm sạch danh sách điểm số (Data Cleaning Function)  
🎯 Mục tiêu
Rèn luyện viết hàm nhận tham số đầu vào, kiểm tra kiểu dữ liệu, bắt lỗi miền và trả về danh sách đã làm sạch cùng số bản ghi bị loại bỏ.  
📋 Mô tả bài toán
Trong module tiền xử lý dữ liệu, cần một hàm nhận vào danh sách các quan sát thô raw_scores (chứa số thực, chuỗi lạ hoặc điểm âm/lớn hơn 10).  
⚙️ Yêu cầu
1. Viết hàm clean_scores(raw_scores) nhận vào danh sách raw_scores.
2. Tạo danh sách valid_scores chứa các điểm hợp lệ (phải là kiểu int hoặc float, nằm trong đoạn \([0.0, 10.0]\)).
3. Đếm số lượng quan sát bị loại bỏ rejected_count.
4. Hàm trả về một tuple gồm: (valid_scores, rejected_count).
5. Thử nghiệm với danh sách: [8.5, 9.0, "-1.0", "N/A", 7.5, 12.0, 6.0] (Lưu ý sửa lỗi cú pháp chuỗi -1.0, thành "-1.0").  
💡 Gợi ý  
• Dùng isinstance(score, (int, float)) để lọc đúng kiểu số (lưu ý trong Python, bool là subclass của int, nên cần lưu ý thêm not isinstance(score, bool) nếu cần thiết).  
• Dùng điều kiện 0 <= score <= 10 để kiểm tra miền điểm hợp lệ.  
• Đếm số phần tử bị loại: rejected_count = len(raw_scores) - len(valid_scores).    

Bài tập 2: Thiết kế hàm chuẩn hóa min-max (Min-Max Scaling Function)  
🎯 Mục tiêu  
Viết hàm toán học cho tiền xử lý đặc trưng với tham số tùy chọn (default parameter) và xử lý trường hợp ngoại lệ chia cho 0.  
📋 Mô tả bài toán  
Chuẩn hóa min-max đưa mảng giá trị số về một khoảng xác định \([min\_range, max\_range]\) (mặc định là \([0, 1]\)). Công thức cơ bản về đoạn \([0, 1]\) là:
\(x_{norm}=\frac{x-x_{min}}{x_{max}-x_{min}}\)
⚙️ Yêu cầu  
1. Viết hàm min_max_scale(values, target_min=0.0, target_max=1.0) nhận vào:  
	• values: Danh sách các số thực cần chuẩn hóa.
	• target_min, target_max: Cận dưới và cận trên sau biến đổi (mặc định lần lượt là 0.0 và 1.0).
2. Bắt lỗi dữ liệu:
	• Nếu values rỗng, trả về danh sách rỗng [].
	• Nếu \(x_{min} == x_{max}\) (danh sách chứa các giá trị hằng số, không có độ biến thiên), in ra cảnh báo và trả về danh sách có độ dài tương đương nhưng toàn bộ giá trị là target_min.
3. Dùng List Comprehension để tính toán hiệu quả và trả về danh sách kết quả.  
💡 Gợi ý  
• Tìm \(x_{min} = \min(values)\) và \(x_{max} = \max(values)\).  
• Kiểm tra if x_max - x_min == 0: để tránh lỗi chia cho số không (ZeroDivisionError).  
• Áp dụng công thức co giãn tổng quát trên một đoạn bất kỳ:  
\(x_{scaled}=target\_min+\frac{x-x_{min}}{x_{max}-x_{min}}\times (target\_max-target\_min)\)  

Bài tập 3: Tổ chức Pipeline tiền xử lý theo kiến trúc hàm hoàn chỉnh  
🎯 Mục tiêu  
Phân rã chương trình thành nhiều hàm đơn nhiệm độc lập và tổ chức hàm điều phối main() có khối bảo vệ if __name__ == "__main__":.  
📋 Mô tả bài toán  
Chuỗi xử lý (Pipeline) dữ liệu sẽ bao gồm 3 hàm độc lập phối hợp với nhau để hoàn thành bài toán:
• Hàm 1: parse_dataset(raw_data): Nhận danh sách các chuỗi số bất kỳ (ví dụ: ["8.5", "10", "abc", "7.0", "-2"]), chuyển đổi các phần tử hợp lệ thành số thực float, bỏ qua các phần tử lỗi định dạng bằng cấu trúc try-except.
• Hàm 2: compute_summary(numbers): Nhận danh sách số đã được chuyển đổi, trả về một từ điển (dict) gồm các thông số: "count", "mean" (trung bình cộng), "min", "max". Nếu danh sách rỗng, trả về None.
• Hàm 3: main(): Điều phối toàn bộ luồng dữ liệu: Khai báo danh sách thô \(\rightarrow \) gọi parse_dataset \(\rightarrow \) gọi compute_summary \(\rightarrow \) in báo cáo kết quả ra màn hình.  
⚙️ Yêu cầu
1. Cấu trúc mã nguồn rõ ràng, tường minh.
2. Sử dụng khối lệnh chuẩn if __name__ == "__main__": main() để thực thi chương trình một cách an toàn khi chạy trực tiếp.
3. Các hàm chỉ giao tiếp với nhau bằng đối số truyền vào và giá trị trả về (return), tuyệt đối không sử dụng từ khóa global.
4. Trong hàm compute_summary, chú ý kiểm tra điều kiện danh sách rỗng để tránh lỗi chia cho 0 khi tính toán giá trị trung bình (mean).  
💡 Gợi ý  
• Sử dụng try-except ValueError trong hàm parse_dataset để bắt lỗi khi hàm float(item) gặp chuỗi không thể chuyển đổi (như "abc").  
• Sử dụng cấu trúc if not numbers: return None để bảo vệ mã nguồn trước danh sách trống.  
• Tham khảo mô hình phân rã bài toán và thiết lập cấu trúc mã của bài học trước để trình bày đúng chuẩn Clean Code.
