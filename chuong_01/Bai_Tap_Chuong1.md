# 📝 BÀI TẬP TỰ HỌC THEO BUỔI - CHƯƠNG 1

Năm bài tập dưới đây tương ứng 30 giờ tự học của Chương 1. Mỗi bài dự kiến 6 giờ và tạo một phần sản phẩm cho bài tập tích hợp cuối chương.

---

## 📅 Buổi 1: Môi trường Python và vai trò trong AI (6 giờ)

**🎯 Nhiệm vụ:** Cài đặt môi trường, tạo một script đọc ba giá trị đầu vào, tính thống kê đơn giản và ghi kết quả ra màn hình. Vẽ sơ đồ chỉ rõ dữ liệu đầu vào, bước xử lý Python và đầu ra có thể được dùng trong một pipeline AI.
**📥 Input:** Ba số thực nhập trên một dòng, cách nhau bởi khoảng trắng. 
**📤 Output:** Dictionary gồm `count`, `mean`, `min` và `max`.

**📝 Mã nguồn 18: Prototype và ví dụ input-output Buổi 1**
```python
def summarize(values: list[float]) -> dict[str, float]:
    ...

sample_input = [6.0, 8.0, 10.0]
expected_output = {"count": 3, "mean": 8.0, "min": 6.0, "max": 10.0}
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 1: Đáp án mẫu Buổi 1**
```python
def summarize(values: list[float]) -> dict[str, float | int]:
    if not values:
        raise ValueError("values không được rỗng")
    return {
        "count": len(values),
        "mean": sum(values) / len(values),
        "min": min(values),
        "max": max(values),
    }

sample_input = [6.0, 8.0, 10.0]
expected_output = {"count": 3, "mean": 8.0, "min": 6.0, "max": 10.0}
actual_output = summarize(sample_input)
assert actual_output == expected_output
print(actual_output)

# Kết quả mong muốn:
# {'count': 3, 'mean': 8.0, 'min': 6.0, 'max': 10.0}
```
</details>

**✅ Sản phẩm:** Nhật ký cài đặt có ảnh hoặc thông tin phiên bản, file mã nguồn, kết quả của hai lần chạy và sơ đồ pipeline một trang.
**🚀 Hoàn thành khi:** Script chạy lại không lỗi, đầu vào đầu ra được mô tả rõ và người học giải thích được vai trò của Python trong sơ đồ.

---

## 📅 Buổi 2: Biến, điều kiện và vòng lặp (6 giờ)

**🎯 Nhiệm vụ:** Viết chương trình kiểm tra một danh sách điểm dạng chuỗi, chuyển đổi giá trị hợp lệ, phân loại theo các khoảng điểm và ghi riêng các giá trị không hợp lệ. Dùng ít nhất một vòng lặp, một cấu trúc rẽ nhánh, break hoặc continue đúng ngữ cảnh.
**📥 Input:** List chuỗi; điểm hợp lệ thuộc đoạn `[0, 10]`. 
**📤 Output:** Dictionary gồm `valid`, `invalid` và `levels` có cùng thứ tự với các điểm hợp lệ.

**📝 Mã nguồn 19: Prototype và ví dụ input-output Buổi 2**
```python
def classify_scores(raw_scores: list[str]) -> dict[str, list]:
    ...

sample_input = ["8.5", "abc", "11", "6"]
expected_output = {
    "valid": [8.5, 6.0],
    "invalid": ["abc", "11"],
    "levels": ["gioi", "dat"],
}
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 2: Đáp án mẫu Buổi 2**
```python
def classify_scores(raw_scores: list[str]) -> dict[str, list]:
    result = {"valid": [], "invalid": [], "levels": []}
    for raw in raw_scores:
        try:
            score = float(raw)
        except (TypeError, ValueError):
            result["invalid"].append(raw)
            continue
            
        if not 0 <= score <= 10:
            result["invalid"].append(raw)
            continue
            
        result["valid"].append(score)
        result["levels"].append("gioi" if score >= 8 else "dat" if score >= 5 else "chua_dat")
        
    return result

sample_input = ["8.5", "abc", "11", "6"]
expected_output = {
    "valid": [8.5, 6.0],
    "invalid": ["abc", "11"],
    "levels": ["gioi", "dat"],
}
actual_output = classify_scores(sample_input)
assert actual_output == expected_output
assert classify_scores([]) == {"valid": [], "invalid": [], "levels": []}
print(actual_output)

# Kết quả mong muốn:
# {'valid': [8.5, 6.0], 'invalid': ['abc', '11'], 'levels': ['gioi', 'dat']}
```
</details>

**✅ Sản phẩm:** File mã nguồn, bảng ít nhất 12 ca kiểm thử và bản giải thích ngắn cho ba quyết định điều khiển dòng thực thi.
**🚀 Hoàn thành khi:** Chương trình xử lý được biên, dữ liệu rỗng và dữ liệu sai; kết quả của các ca kiểm thử khớp với dự đoán.

---

## 📅 Buổi 3: Hàm và tổ chức chương trình (6 giờ)

**🎯 Nhiệm vụ:** Tái cấu trúc chương trình của Buổi 2 thành các hàm đơn nhiệm cho nhập, kiểm tra, phân loại, tóm tắt và hiển thị. Loại bỏ trạng thái toàn cục, bổ sung docstring và chủ động tạo một lỗi để đọc traceback trước khi sửa.
**📥 Input:** List chuỗi điểm như Buổi 2. 
**📤 Output:** Báo cáo tổng hợp gồm số hợp lệ, số không hợp lệ, điểm trung bình và số lượng theo mức.

**📝 Mã nguồn 20: Prototype và ví dụ input-output Buổi 3**
```python
def parse_score(raw: str) -> float | None:
    ...

def build_score_report(raw_scores: list[str]) -> dict[str, object]:
    ...

sample_input = ["8", "6", "x"]
expected_output = {
    "valid_count": 2, "invalid_count": 1,
    "mean": 7.0, "level_counts": {"gioi": 1, "dat": 1},
}
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 3: Đáp án mẫu Buổi 3**
```python
def parse_score(raw: str) -> float | None:
    try:
        score = float(raw)
    except (TypeError, ValueError):
        return None
    return score if 0 <= score <= 10 else None

def score_level(score: float) -> str:
    return "gioi" if score >= 8 else "dat" if score >= 5 else "chua_dat"

def build_score_report(raw_scores: list[str]) -> dict[str, object]:
    scores = [score for raw in raw_scores if (score := parse_score(raw)) is not None]
    level_counts: dict[str, int] = {}
    
    for score in scores:
        level = score_level(score)
        level_counts[level] = level_counts.get(level, 0) + 1
        
    return {
        "valid_count": len(scores),
        "invalid_count": len(raw_scores) - len(scores),
        "mean": sum(scores) / len(scores) if scores else None,
        "level_counts": level_counts,
    }

sample_input = ["8", "6", "x"]
expected_output = {
    "valid_count": 2, "invalid_count": 1,
    "mean": 7.0, "level_counts": {"gioi": 1, "dat": 1},
}
actual_output = build_score_report(sample_input)
assert actual_output == expected_output
print(actual_output)

# Kết quả mong muốn:
# {'valid_count': 2, 'invalid_count': 1, 'mean': 7.0, 'level_counts': {'gioi': 1, 'dat': 1}}
```
</details>

**✅ Sản phẩm:** Phiên bản trước/sau tái cấu trúc, sơ đồ lời gọi hàm và nhật ký phân tích một traceback.
**🚀 Hoàn thành khi:** Mỗi hàm có đầu vào đầu ra rõ, có thể kiểm thử độc lập và kết quả cuối không đổi so với phiên bản đúng ban đầu.

---

## 📅 Buổi 4: Cấu trúc dữ liệu cơ bản (6 giờ)

**🎯 Nhiệm vụ:** Biểu diễn hồ sơ sinh viên bằng list, tuple, dictionary và set; tính thống kê mô tả nhỏ, phát hiện mã trùng và lập từ điển dữ liệu cho các trường. So sánh ít nhất hai cách biểu diễn và nêu lý do chọn cấu trúc cuối.
**📥 Input:** List dictionary, mỗi bản ghi có `student_id: str`, `name: str` và `score: float`. 
**📤 Output:** Dictionary gồm danh sách mã trùng, số sinh viên và điểm trung bình.

**📝 Mã nguồn 21: Prototype và ví dụ input-output Buổi 4**
```python
def summarize_students(records: list[dict[str, object]]) -> dict[str, object]:
    ...

sample_input = [
    {"student_id": "S01", "name": "An", "score": 8.0},
    {"student_id": "S01", "name": "An", "score": 9.0},
]
expected_output = {
    "student_count": 2, "duplicate_ids": ["S01"], "mean_score": 8.5,
}
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 4: Đáp án mẫu Buổi 4**
```python
def summarize_students(records: list[dict[str, object]]) -> dict[str, object]:
    seen: set[str] = set()
    duplicate_ids: set[str] = set()
    scores: list[float] = []
    
    for record in records:
        student_id = str(record["student_id"])
        if student_id in seen:
            duplicate_ids.add(student_id)
        seen.add(student_id)
        scores.append(float(record["score"]))
        
    return {
        "student_count": len(records),
        "duplicate_ids": sorted(duplicate_ids),
        "mean_score": sum(scores) / len(scores) if scores else None,
    }

sample_input = [
    {"student_id": "S01", "name": "An", "score": 8.0},
    {"student_id": "S01", "name": "An", "score": 9.0},
]
expected_output = {
    "student_count": 2, "duplicate_ids": ["S01"], "mean_score": 8.5,
}
actual_output = summarize_students(sample_input)
assert actual_output == expected_output
print(actual_output)

# Kết quả mong muốn:
# {'student_count': 2, 'duplicate_ids': ['S01'], 'mean_score': 8.5}
```
</details>

**✅ Sản phẩm:** File mã nguồn, từ điển dữ liệu và bảng so sánh cấu trúc dữ liệu.
**🚀 Hoàn thành khi:** Cấu trúc được chọn phù hợp với thứ tự, tính duy nhất và khả năng thay đổi; chương trình phát hiện đúng bản ghi trùng.

---

## 📅 Buổi 5: Chuỗi Unicode và file dữ liệu (6 giờ)

**🎯 Nhiệm vụ:** Hoàn thiện bài tập tích hợp cuối chương: chuẩn hóa họ tên về Unicode NFC, kiểm tra điểm, đọc CSV và ghi báo cáo JSON bằng UTF-8. Thử với file hợp lệ, file có lỗi và file rỗng.
**📥 Input:** File CSV UTF-8 có header `student_id`, `name`, `score`; mỗi dòng là một sinh viên. 
**📤 Output:** File JSON gồm `total_rows`, `valid_rows`, `rejected_rows` và `mean_score`.

**📝 Mã nguồn 22: Prototype và ví dụ input-output Buổi 5**
```python
def process_student_csv(input_path: str, output_path: str) -> dict[str, object]:
    ...

# input.csv:
# student_id, name, score
# S01," Nguyen An ",8
# S02, Bình, abc

expected_output = {
    "total_rows": 2, "valid_rows": 1,
    "rejected_rows": 1, "mean_score": 8.0,
}
```

<details>
<summary>💡 <b>Xem đáp án mẫu (Nhấn để mở rộng)</b></summary>

**Mã nguồn GV 5: Đáp án mẫu Buổi 5**
```python
import csv
import json
import unicodedata
from pathlib import Path
from tempfile import TemporaryDirectory

def normalize_name(value: str) -> str:
    normalized = unicodedata.normalize("NFC", value)
    return " ".join(normalized.split()).title()

def process_student_csv(input_path: str, output_path: str) -> dict[str, object]:
    valid_rows: list[dict[str, object]] = []
    total_rows = 0
    
    with open(input_path, newline="", encoding="utf-8") as source:
        for row in csv.DictReader(source):
            total_rows += 1
            try:
                score = float(row["score"])
            except (TypeError, ValueError):
                continue
                
            if not 0 <= score <= 10:
                continue
                
            valid_rows.append({
                "student_id": row["student_id"].strip(),
                "name": normalize_name(row["name"]),
                "score": score,
            })
            
    report = {
        "total_rows": total_rows,
        "valid_rows": len(valid_rows),
        "rejected_rows": total_rows - len(valid_rows),
        "mean_score": (
            sum(row["score"] for row in valid_rows) / len(valid_rows)
            if valid_rows else None
        ),
    }
    
    Path(output_path).write_text(
        json.dumps(report, ensure_ascii=False, indent=2),
        encoding="utf-8",
    )
    return report

with TemporaryDirectory() as temp_dir:
    input_path = Path(temp_dir) / "input.csv"
    output_path = Path(temp_dir) / "report.json"
    input_path.write_text(
        'student_id, name, score\nS01," Nguyen An ",8\nS02, Binh, abc\n',
        encoding="utf-8",
    )
    
    expected_output = {
        "total_rows": 2, "valid_rows": 1,
        "rejected_rows": 1, "mean_score": 8.0,
    }
    actual_output = process_student_csv(str(input_path), str(output_path))
    
    assert actual_output == expected_output
    assert json.loads(output_path.read_text(encoding="utf-8")) == expected_output
    print(actual_output)

# Kết quả mong muốn:
# {'total_rows': 2, 'valid_rows': 1, 'rejected_rows': 1, 'mean_score': 8.0}
```
</details>

**✅ Sản phẩm:** Mã nguồn hoàn chỉnh, ba file kiểm thử, báo cáo JSON và hướng dẫn chạy ngắn.
**🚀 Hoàn thành khi:** Chương trình chạy lại cho kết quả nhất quán, không làm mất dấu tiếng Việt và báo cáo ghi đúng số dòng hợp lệ/bị loại.
