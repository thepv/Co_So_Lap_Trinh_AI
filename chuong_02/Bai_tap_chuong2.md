# 📝 BÀI TẬP TỰ HỌC THEO BUỔI - CHƯƠNG 2

Hai bài tập tương ứng 12 giờ tự học của Chương 2 và cùng sử dụng một bộ dữ liệu cảm biến để sản phẩm của Buổi 6 được tiếp tục ở Buổi 7.

---

## 📅 Buổi 6: NumPy và tính toán vector hóa (6 giờ)

**🎯 Nhiệm vụ:** Tạo hoặc nạp ma trận nhiệt độ của ba trạm trong bảy ngày; dự đoán rồi kiểm tra `shape` của từng phép index, slice và broadcasting. Tính trung bình theo trạm/ngày bằng phép vector hóa, sau đó so sánh thời gian với một phiên bản dùng vòng lặp trên dữ liệu lặp đủ lớn.
**📥 Input:** Mảng NumPy float có `shape (n_stations, n_days)`; hàng là trạm, cột là ngày.
**📤 Output:** Hai vector trung bình theo trạm và theo ngày.

**📝 Mã nguồn 32: Prototype và ví dụ input-output Buổi 6**
```python
import numpy as np

def temperature_means(values: np.ndarray) -> tuple[np.ndarray, np.ndarray]:
    ...

sample_input = np.array([[30.0, 32.0], [28.0, 30.0], [26.0, 28.0]])
expected_station_means = np.array([31.0, 29.0, 27.0])
expected_day_means = np.array([28.0, 30.0])
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 6: Đáp án mẫu Buổi 6**
```python
import numpy as np

def temperature_means(values: np.ndarray) -> tuple[np.ndarray, np.ndarray]:
    data = np.asarray(values, dtype=float)
    if data.ndim != 2 or data.size == 0:
        raise ValueError("values phải là ma trận 2 chiều không rỗng")
    return data.mean(axis=1), data.mean(axis=0)

sample_input = np.array([[30.0, 32.0], [28.0, 30.0], [26.0, 28.0]])
expected_station_means = np.array([31.0, 29.0, 27.0])
expected_day_means = np.array([28.0, 30.0])

station_means, day_means = temperature_means(sample_input)

np.testing.assert_allclose(station_means, expected_station_means)
np.testing.assert_allclose(day_means, expected_day_means)
print(station_means, day_means)

# Kết quả mong muốn:
# [31. 29. 27.] [28. 30.]
```
</details>

**✅ Sản phẩm:** Notebook có dự đoán kích thước trước khi chạy, kết quả tính, bảng thời gian và nhận xét về kiểu dữ liệu bộ nhớ.
**🚀 Hoàn thành khi:** Mọi phép toán có kích thước được giải thích, kết quả vector hóa khớp phiên bản vòng lặp và phép đo hiệu năng có quy trình tái lập.

---

## 📅 Buổi 7: Pandas và khảo sát dữ liệu dạng bảng (6 giờ)

**🎯 Nhiệm vụ:** Dùng DataFrame cho bộ dữ liệu Buổi 6; khảo sát lược đồ, đặt MultiIndex, lọc/sắp xếp, thống kê tỷ lệ khuyết thiếu và chọn cách xử lý có lập luận. Hoàn thiện bài tập tích hợp về chất lượng cảm biến và xuất báo cáo JSON.
**📥 Input:** DataFrame có các cột `time: datetime`, `station: str`, `temperature: float` và `humidity: float`; hai cột số có thể chứa `NaN`.
**📤 Output:** DataFrame đã kiểm tra và dictionary tỷ lệ thiếu theo cột.

**📝 Mã nguồn 33: Prototype và ví dụ input-output Buổi 7**
```python
import pandas as pd

def audit_sensors(df: pd.DataFrame) -> tuple[pd.DataFrame, dict[str, float]]:
    ...

sample_input = pd.DataFrame({
    "time": ["2026-01-01", "2026-01-01"],
    "station": ["A", "B"],
    "temperature": [30.0, None],
    "humidity": [70.0, 80.0],
})

expected_missing_rate = {
    "time": 0.0, "station": 0.0, 
    "temperature": 0.5, "humidity": 0.0
}
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 7: Đáp án mẫu Buổi 7**
```python
import pandas as pd

REQUIRED_SENSOR_COLUMNS = ["time", "station", "temperature", "humidity"]

def audit_sensors(df: pd.DataFrame) -> tuple[pd.DataFrame, dict[str, float]]:
    missing_columns = set(REQUIRED_SENSOR_COLUMNS) - set(df.columns)
    if missing_columns:
        raise ValueError(f"Thiếu cột: {sorted(missing_columns)}")
        
    audited = df.loc[:, REQUIRED_SENSOR_COLUMNS].copy()
    audited["time"] = pd.to_datetime(audited["time"], errors="raise")
    
    for column in ["temperature", "humidity"]:
        audited[column] = pd.to_numeric(audited[column], errors="coerce")
        
    missing_rate = audited.isna().mean().astype(float).to_dict()
    audited = audited.set_index(["station", "time"]).sort_index()
    
    return audited, missing_rate

sample_input = pd.DataFrame({
    "time": ["2026-01-01", "2026-01-01"],
    "station": ["A", "B"],
    "temperature": [30.0, None],
    "humidity": [70.0, 80.0],
})

expected_missing_rate = {
    "time": 0.0, "station": 0.0, 
    "temperature": 0.5, "humidity": 0.0
}

audited, actual_missing_rate = audit_sensors(sample_input)

assert actual_missing_rate == expected_missing_rate
assert audited.index.names == ["station", "time"]
print(actual_missing_rate)

# Kết quả mong muốn:
# {'time': 0.0, 'station': 0.0, 'temperature': 0.5, 'humidity': 0.0}
```
</details>

**✅ Sản phẩm:** Notebook khảo sát, từ điển dữ liệu, bảng trước/sau xử lý và báo cáo JSON; phần kết luận tối đa một trang.
**🚀 Hoàn thành khi:** Các thao tác theo nhãn/vị trí đúng, không che giấu giá trị khuyết thiếu, báo cáo có số liệu kiểm chứng và đủ để ôn kiểm tra lần 1.
