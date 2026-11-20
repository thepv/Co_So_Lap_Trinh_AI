# CHƯƠNG 5. XÂY DỰNG PIPELINE TIỀN XỬ LÝ DỮ LIỆU CHO AI  
 Chương tích hợp học phần thành một pipeline chạy lại được, học tham số chỉ từ tập train, áp dụng
  nhất quán cho dữ liệu mới và tạo bằng chứng chất lượng.

## Mục tiêu chương
Người học kiểm tra đầu vào và kết hợp làm sạch–biến đổi–mã hóa. Pipeline được tự động hóa bằng
Pipeline và ColumnTransformer, sau đó đánh giá đầu ra và bàn giao ma trận đặc trưng kèm siêu dữ
liệu mà chưa cần huấn luyện mô hình.

## 5.1. Quy trình tổng quát
Chương 5 kết hợp các kỹ năng đã học ở các chương trước thành một pipeline có thể chạy lại: nhận
dữ liệu thô, kiểm tra chất lượng đầu vào, làm sạch lỗi chắc chắn, chia tập train/tập test, học tham số
tiền xử lý trên tập train, áp dụng nhất quán cho tập test và dữ liệu mới, rồi bàn giao ma trận đặc trưng
kèm siêu dữ liệu. Điểm khác biệt giữa một đoạn code minh họa và một pipeline tiền xử lý dùng được
trong dự án AI là khả năng lặp lại, khả năng kiểm tra và khả năng giải thích từng quyết định xử lý.
     Bản thảo của mục này nhấn mạnh Pipeline và ColumnTransformer của scikit-learn. Đó là công
cụ rất hữu ích, nhưng trong khung đề cương hiện tại, mục 5.1 tập trung vào tư duy tổng quát trước
khi đóng gói bằng pipeline ở mục 5.2. Nếu chưa biết đơn vị quan sát là gì, cột nào là biến mục tiêu,
cột nào là mã định danh, quy tắc làm sạch nào được phép áp dụng và bước nào có học tham số từ dữ
liệu, thì việc đưa code vào Pipeline chỉ làm cho lỗi khó thấy hơn.

### 5.1.1. Nhập dữ liệu và kiểm tra ban đầu
Bước nhập dữ liệu không chỉ là gọi pd.read_csv hoặc nhận một DataFrame. Người lập trình cần
xác định bối cảnh của bảng dữ liệu trước khi xử lý: mỗi dòng đại diện cho đối tượng nào, biến mục
tiêu là gì, các cột định danh nào không được đưa vào mô hình, kiểu dữ liệu mong đợi của từng cột là
gì, và dữ liệu có thể chứa thông tin tương lai hay không. Nếu bỏ qua bước này, pipeline có thể chạy
thành công về mặt kỹ thuật nhưng học sai tín hiệu.
    Một kiểm tra đầu vào tối thiểu nên bao gồm:

            • Lược đồ (schema): tên cột bắt buộc, kiểu dữ liệu mong đợi, cột thừa hoặc cột thiếu.
            • Đơn vị quan sát: mỗi dòng là một khách hàng, một đơn hàng, một lượt học hay một bản ghi
              log?
            • Biến mục tiêu và mã định danh: biến mục tiêu dùng để dự đoán, mã định danh dùng để truy
              vết; cả hai cần được tách khỏi ma trận đặc trưng.
            • Khuyết thiếu và trùng lặp: tỷ lệ khuyết thiếu theo cột, mã định danh trùng, dòng trùng hoặc
              khóa nghiệp vụ trùng.
            • Miền giá trị: tuổi không âm, điểm trong khoảng hợp lệ, ngày tháng đúng thứ tự, danh mục
              nằm trong tập cho phép.
            • Phân bố biến mục tiêu: biến mục tiêu có quá lệch không, có nhóm nào thiếu mẫu không, có
              nguy cơ rò rỉ dữ liệu không?

**Mã nguồn 59. Tiếp nhận và kiểm tra dữ liệu thô**
```python
import pandas as pd

raw = pd.DataFrame({
...."customer_id": ["C01", "C02", "C03", "C04", "C05", "C06"],
...."age": [25, 31, None, 45, 38, 29],
...."income": [12.0, 18.5, 15.0, None, 27.0, 16.0],
...."region": ["Nam", "Bắc", "Nam", "Trung", None, "Bắc"],
...."signup_days": [120, 45, 200, 14, 88, 60],
...."bought": [0, 1, 0, 1, 1, 0],
})

required_columns = {
...."customer_id",
...."age",
...."income",
...."region",
...."signup_days",
...."bought",
}
missing_columns = required_columns.difference(raw.columns)
if missing_columns:
....raise ValueError(f"Thiếu cột: {sorted(missing_columns)}")

profile = pd.DataFrame({
...."dtype": raw.dtypes.astype(str),
...."missing_count": raw.isna().sum(),
...."missing_rate": raw.isna().mean().round(3),
...."unique_count": raw.nunique(dropna=True),
})

print("Kích thước:", raw.shape)
print(profile)
print("Mã định danh trùng:", \
        + int(raw["customer_id"].duplicated().sum()))
print("Phân bố biến mục tiêu:")

print(raw["bought"].value_counts(normalize=True).round(3))
# Kết quả mong muốn:
# Kích thước: (6, 6)
#                dtype missing_count missing_rate unique_count
# customer_id   object              0         0.000          6
# age          float64              1         0.167          5
# income       float64              1         0.167          5
# region        object              1         0.167          3
# signup_days    int64              0         0.000          6
# bought         int64              0         0.000          2
# Mã định danh trùng: 0
# Phân bố biến mục tiêu:
# bought
# 0    0.5
# 1    0.5
# Name: proportion, dtype: float64
    Sau khi có hồ sơ ban đầu, cần phân loại vấn đề thành lỗi chắc chắn, điểm cần xác minh và biến đổi
có thể tự động hóa. Ví dụ, thiếu region có thể được điền bằng giá trị phổ biến nhất trong pipeline;
nhưng tuổi âm, mã định danh trùng với thông tin mâu thuẫn, hoặc biến mục tiêu được tạo sau thời
điểm dự đoán là các vấn đề cần quyết định nghiệp vụ. Không nên để bộ điền khuyết (imputer) hoặc
bộ chuẩn hóa (scaler) che giấu lỗi lược đồ và lỗi miền.
```

**Mã nguồn 60. Kiểm tra miền giá trị và khóa định danh**
```python
valid_age = raw["age"].dropna().between(0, 120)
valid_income = raw["income"].dropna().ge(0)
valid_signup_days = raw["signup_days"].dropna().ge(0)

checks = {
...."age_in_range": bool(valid_age.all()),
...."income_non_negative": bool(valid_income.all()),
...."signup_days_non_negative": bool(valid_signup_days.all()),
...."customer_id_unique": not raw["customer_id"].duplicated().any(),
...."target_binary": \
 + set(raw["bought"].dropna().unique()).issubset({0, 1}),
}

for name, passed in checks.items():
....print(f"{name}: {'OK' if passed else 'Cần kiểm tra'}")
# Kết quả mong muốn:
# age_in_range: OK
# income_non_negative: OK
# signup_days_non_negative: OK
# customer_id_unique: OK
# target_binary: OK
    Bản dữ liệu gốc nên được giữ bất biến. Mọi bước sửa dữ liệu cần tạo ra bản mới hoặc ghi rõ tác
động: bao nhiêu dòng bị ảnh hưởng, cột nào bị thay đổi và giả định nào được dùng. Đây là nhật ký
xử lý tối thiểu để người khác có thể tái hiện hoặc phản biện quyết định tiền xử lý.

      5.1.2. Làm sạch – biến đổi – mã hóa
      Sau kiểm tra đầu vào, pipeline tổng quát thường đi theo thứ tự: tách biến mục tiêu và mã định danh,
      sửa lỗi miền có quy tắc chắc chắn, chia tập train/tập test, rồi mới học các tham số điền khuyết, chuẩn
      hóa thang đo và mã hóa trên tập train. Tập test chỉ được biến đổi bằng tham số đã học từ tập train.
      Quy tắc này là trung tâm của tiền xử lý cho AI vì nó ngăn leakage: thông tin từ dữ liệu đánh giá không
      được tham gia vào quá trình xác định median, mean, min/max, độ lệch chuẩn hoặc tập danh mục.
```

**Mã nguồn 61. Tách mã định danh, biến mục tiêu và chia dữ liệu trước khi học tham số**
```python
from sklearn.model_selection import train_test_split

id_columns = ["customer_id"]
target_column = "bought"
feature_columns = ["age", "income", "region", "signup_days"]

X = raw[feature_columns].copy()
y = raw[target_column].copy()
ids = raw[id_columns].copy()

X_train, X_test, y_train, y_test, id_train, id_test = \
       + train_test_split(
....X,
....y,
....ids,
....test_size=0.33,
....random_state=42,
....stratify=y,
)

print("Train/Test:", X_train.shape, X_test.shape)
print("Train target:")
print(y_train.value_counts(normalize=True).round(3))
# Kết quả mong muốn:
# Train/Test: (4, 4) (2, 4)
# Train target:
# bought
# 0    0.5
# 1    0.5
# Name: proportion, dtype: float64
            Các bước tiền xử lý trong mục này có thể nhóm thành ba lớp:
            1. Làm sạch: xử lý lỗi miền, chuẩn hóa chuỗi danh mục, kiểm tra trùng lặp, quyết định cách xử
               lý dữ liệu khuyết thiếu.
            2. Biến đổi: chuẩn hóa thang đo cột số, phân khoảng nếu cần, tạo đặc trưng mới từ cột có sẵn.
            3. Mã hóa: chuyển biến phân loại thành biểu diễn số bằng mã hóa thứ bậc hoặc mã hóa one-hot
               theo đúng ý nghĩa dữ liệu.
         Trong dự án thật, một số bước làm sạch có thể thực hiện trước khi chia tập train/tập test nếu chúng
      không học tham số thống kê từ dữ liệu, ví dụ đổi tên cột, chuẩn hóa kiểu ngày tháng, sửa đơn vị theo

quy tắc chắc chắn hoặc loại dòng không thuộc phạm vi bài toán. Ngược lại, mọi bước học thống kê từ
dữ liệu như median để điền khuyết thiếu, trung bình/độ lệch chuẩn để chuẩn hóa, giá trị nhỏ nhất/lớn
nhất để đưa về miền chuẩn hoặc tập danh mục để one-hot phải được fit trên tập train.

Bước xử lý                  Có học tham số từ dữ liệu?        Thời điểm phù hợp
Đổi tên cột, chọn cột       Không                             Trước khi chia tập train/tập test
theo lược đồ
Sửa đơn vị theo quy tắc     Không                             Trước khi chia tập train/tập test, có
chắc chắn                                                     nhật ký xử lý
Điền giá trị khuyết thiếu   Có                                Khớp trên tập train, biến đổi tập
bằng median/trung bình                                        test
StandardScaler      hoặc    Có                                Khớp trên tập train, biến đổi tập
MinMaxScaler                                                  test
One-hot encoder học         Có                                Fit trên tập train, xử lý nhãn mới ở
tập danh mục                                                  tập test
Loại biến mục tiêu, mã      Không                             Trước mọi bước mô hình hóa
định danh khỏi ma trận
đặc trưng

                    Bảng 5.1. Phân biệt bước xử lý theo khả năng học tham
                              số từ dữ liệu.
    Ví dụ dưới đây minh họa thủ công ranh giới fit/transform cho dữ liệu số và phân loại. Mục 5.2
sẽ đóng gói các bước này bằng Pipeline và ColumnTransformer để mã ngắn hơn, nhất quán hơn và
giảm rủi ro thao tác nhầm.
```

**Mã nguồn 62. Minh họa fit trên tập train và transform tập test thủ công**
```python
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, StandardScaler

numeric_columns = ["age", "income", "signup_days"]
categorical_columns = ["region"]

num_imputer = SimpleImputer(strategy="median")
scaler = StandardScaler()
cat_imputer = SimpleImputer(strategy="most_frequent")
encoder = OneHotEncoder(handle_unknown="ignore", \
 + sparse_output=False)

X_train_num = num_imputer.fit_transform(X_train[numeric_columns])
X_train_num = scaler.fit_transform(X_train_num)

X_test_num = num_imputer.transform(X_test[numeric_columns])
X_test_num = scaler.transform(X_test_num)

X_train_cat = \
 + cat_imputer.fit_transform(X_train[categorical_columns])
X_train_cat = encoder.fit_transform(X_train_cat)

X_test_cat = cat_imputer.transform(X_test[categorical_columns])
X_test_cat = encoder.transform(X_test_cat)

print(X_train_num.shape, X_train_cat.shape)
print(X_test_num.shape, X_test_cat.shape)
# Kết quả mong muốn:
# (4, 3) (4, 3)
# (2, 3) (2, 3)
           Một pipeline tốt không chỉ tạo ra ma trận số mà còn tạo bằng chứng rằng đầu ra đủ điều kiện bàn
      giao: số dòng tập train/tập test được bảo toàn, không có NaN hoặc giá trị vô hạn, thứ tự cột giữa tập
      train và tập test giống nhau, biến mục tiêu căn chỉnh đúng index, và mã định danh không lọt vào đặc
      trưng. Nếu một danh mục mới xuất hiện ở tập test hoặc dữ liệu mới, encoder cần có chính sách rõ
      ràng như handle_unknown="ignore" thay vì làm pipeline dừng đột ngột.
      Checklist cho mục 5.1
            • Đã xác định đơn vị quan sát, biến mục tiêu, mã định danh và lược đồ bắt buộc chưa?
            • Bản raw có được giữ bất biến và có hồ sơ chất lượng ban đầu chưa?
            • Đã tách tập train/tập test trước mọi bước học tham số thống kê chưa?
            • Cột mã định danh, biến mục tiêu và thông tin tương lai có bị loại khỏi ma trận đặc trưng chưa?
            • Các bước fit/transform có thể được đóng gói lại ở mục 5.2 mà không đổi logic không?
      Bài tập thực hành
            1. Cho một bảng khách hàng có cột mã định danh, tuổi, thu nhập, khu vực và biến mục tiêu mua
               hàng. Hãy viết đoạn kiểm tra lược đồ, dữ liệu khuyết thiếu, bản ghi trùng và phân bố biến mục
               tiêu.
            2. Giải thích vì sao không được tính median của cột thu nhập trên toàn bộ dữ liệu trước khi chia
               tập train/tập test.
            3. Tách mã định danh và biến mục tiêu khỏi ma trận đặc trưng, chia tập train/tập test, rồi minh
               họa cách khớp bộ điền khuyết trên tập train và biến đổi tập test.

      5.2. Kết hợp các bước thành pipeline
      5.2.1. Tự động hóa chu trình xử lý
      Mục 5.1 đã trình bày ranh giới fit/transform bằng các bước thủ công. Khi số cột tăng lên, cách viết
      thủ công rất dễ sai: quên dùng cùng imputer cho tập train và tập test, đổi thứ tự cột khi ghép ma trận,
      fit encoder lại trên dữ liệu mới hoặc đưa nhầm mã định danh vào ma trận đặc trưng. Pipeline và
      ColumnTransformer giải quyết vấn đề này bằng cách đóng gói các bước tiền xử lý thành một đối
      tượng có quy ước hoạt động rõ ràng: fit chỉ học tham số trên tập train, còn transform áp dụng
      đúng các tham số đó cho tập test và dữ liệu mới.
          Một dự án tổng hợp trong học phần có thể dùng bảng khách hàng, giá nhà hoặc doanh số. Bảng
      dữ liệu thô thường có đủ các vấn đề đã học: giá trị khuyết thiếu, cột số có outlier, cột phân loại chưa
      mã hóa, cột phân loại có thứ bậc, mã định danh cần giữ để truy vết nhưng không được đưa vào mô
      hình, và biến mục tiêu cần tách riêng. Trước khi xây pipeline, sinh viên cần tạo một hồ sơ ban đầu
      bằng info, describe, isna().sum và kiểm tra miền giá trị để xác định mỗi cột sẽ được xử lý thế

nào.
```

**Mã nguồn 63. Dữ liệu dự án thô và hồ sơ kiểm tra ban đầu**
```python
import numpy as np
import pandas as pd

rng = np.random.default_rng(42)
n_samples = 100

df_project = pd.DataFrame({
...."customer_id": [f"KH_{i:03d}" for i in range(1, n_samples + 1)],
...."age": rng.choice([22, 28, 35, np.nan, 45, 50, 120], \
 + size=n_samples),
...."income": rng.choice([15.0, 25.0, np.nan, 40.0, 80.0, 200.0],
.........................size=n_samples),
...."region": rng.choice(["Ha Noi", "TPHCM", "Da Nang", None],
.........................size=n_samples),
...."card_tier": rng.choice(["Bac", "Vang", "Bach kim"], \
 + size=n_samples),
...."spending": rng.normal(loc=5.0, scale=2.0, size=n_samples),
})

print(df_project.info())
print(df_project.isna().sum())
print(df_project[["age", "income", "spending"]].describe())
print("Mã định danh trùng:", \
 + int(df_project["customer_id"].duplicated().sum()))
# Kết quả mong muốn:
# <class 'pandas.core.frame.DataFrame'>
# RangeIndex: 100 entries, 0 to 99
# Data columns (total 6 columns):
#  #   Column       Non-Null Count Dtype
# ---  ------        -------------- -----
#  0   customer_id 100 non-null     object
#  1   age           80 non-null    float64
#  2   income       80 non-null     float64
#  3   region       80 non-null     object
#  4   card_tier    100 non-null    object
#  5   spending      100 non-null   float64
# dtypes: float64(3), object(3)
# memory usage: 4.8+ KB
# None
# customer_id     0
# age            20
# income         20
# region         20
# card_tier       0
# spending        0
# dtype: int64

#              age      income                 spending
# count   80.00000   80.000000               100.000000
# mean    50.21250   63.437500                 4.907026
# std     31.12925   59.770626                 2.022644
# min     22.00000   15.000000                -0.133317
# 25%     28.00000   25.000000                 3.595307
# 50%     45.00000   40.000000                 4.799225
# 75%     50.00000   80.000000                 5.796153
# max    120.00000 200.000000                 10.810134
# Mã định danh trùng: 0
          Từ hồ sơ trên, ta phân loại cột theo vai trò thay vì xử lý toàn bộ bảng bằng một công thức chung.
      customer_id là khóa truy vết, không phải đặc trưng. spending là biến mục tiêu cần dự đoán. age
      và income là biến số nên cần điền khuyết và chuẩn hóa thang đo. region là biến phân loại không
      thứ bậc nên phù hợp với mã hóa one-hot. card_tier có thứ bậc nghiệp vụ nên có thể mã hóa bằng
      thứ tự do người thiết kế quy định. Giá trị age = 120 được xem là outlier cần quy tắc rõ: loại bỏ, sửa
      theo nguồn dữ liệu gốc, hoặc giữ lại nếu miền nghiệp vụ cho phép. Trong bài học nhập môn, ta minh
      họa phương án loại các bản ghi có tuổi lớn hơn 100 trước khi chia tập train/tập test vì đây là lỗi miền
      xác định, không phải tham số thống kê học từ dữ liệu.
```

**Mã nguồn 64. Tách mã định danh, biến mục tiêu, đặc trưng và chia tập train/tập test**
```python
from sklearn.model_selection import train_test_split

clean_project = df_project[df_project["age"].isna() |
...........................df_project["age"].le(100)].copy()

id_column = "customer_id"
target_column = "spending"
feature_columns = ["age", "income", "region", "card_tier"]

X = clean_project[feature_columns].copy()
y = clean_project[target_column].copy()
ids = clean_project[[id_column]].copy()

X_train, X_test, y_train, y_test, id_train, id_test = \
        + train_test_split(
....X,
....y,
....ids,
....test_size=0.2,
....random_state=42,
)

print("Raw/Clean:", df_project.shape, clean_project.shape)
print("Train/Test:", X_train.shape, X_test.shape)
# Kết quả mong muốn:
# Raw/Clean: (100, 6) (88, 6)
# Train/Test: (70, 4) (18, 4)
            Sau khi có tập train/tập test, ta mới định nghĩa các pipeline con. Pipeline số gồm bước điền khuyết

và bước chuẩn hóa; cả hai đều học tham số từ tập train. Pipeline chữ gồm bước điền khuyết và mã hóa
one-hot. Với dữ liệu vận hành, tập test hoặc dữ liệu mới có thể xuất hiện khu vực chưa từng có trong
tập train; vì vậy encoder cần handle_unknown="ignore" để không làm toàn bộ pipeline dừng đột
ngột.
```

**Mã nguồn 65. Pipeline số, phân loại và ColumnTransformer**
```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, OrdinalEncoder, \
 + StandardScaler

numeric_columns = ["age", "income"]
categorical_columns = ["region"]
ordinal_columns = ["card_tier"]

numeric_pipeline = Pipeline([
....("imputer", SimpleImputer(strategy="median")),
....("scaler", StandardScaler()),
])
categorical_pipeline = Pipeline([
....("imputer", SimpleImputer(strategy="most_frequent")),
....("onehot", OneHotEncoder(handle_unknown="ignore", \
 + sparse_output=False)),
])
ordinal_pipeline = Pipeline([
....("imputer", SimpleImputer(strategy="most_frequent")),
....("ordinal", OrdinalEncoder(
........categories=[["Bac", "Vang", "Bach kim"]],
........handle_unknown="use_encoded_value",
........unknown_value=-1,
....)),
])

preprocessor = ColumnTransformer([
....("numeric", numeric_pipeline, numeric_columns),
....("categorical", categorical_pipeline, categorical_columns),
....("ordinal", ordinal_pipeline, ordinal_columns),
], verbose_feature_names_out=False)
     ColumnTransformer là lớp ghép nối: nó gửi mỗi nhóm cột vào pipeline phù hợp rồi ghép kết
quả thành một ma trận số. Cách làm này tốt hơn việc tự ghép bằng np.concatenate vì thứ tự cột,
tên đặc trưng và ranh giới fit/transform được quản lý cùng một nơi. Khi cần thay đổi chiến lược xử
lý, ví dụ đổi median thành most_frequent hoặc thêm một cột số mới, ta chỉnh cấu hình pipeline
thay vì sửa nhiều đoạn code rời rạc.

### 5.2.2. Đánh giá chất lượng dữ liệu sau xử lý
```
Sau khi pipeline chạy thành công, không nên kết luận ngay rằng dữ liệu đã đạt chất lượng. Thành
công kỹ thuật chỉ có nghĩa là chương trình không phát sinh exception. Dữ liệu đầu ra vẫn cần được

      kiểm tra theo các tiêu chí cụ thể: số dòng có được bảo toàn không, tập train và tập test có cùng thứ tự
      đặc trưng không, mọi giá trị có phải là số hữu hạn không, còn NaN hay giá trị vô hạn không, tên đặc
      trưng có truy vết được về cột gốc không, biến mục tiêu có căn chỉnh đúng index không, và mã định
      danh có bị lọt vào ma trận đặc trưng không.

**Mã nguồn 66. Khớp trên tập train, biến đổi tập test và chạy các cổng kiểm soát chất lượng**
```python
import numpy as np
import pandas as pd

X_train_ready = preprocessor.fit_transform(X_train)
X_test_ready = preprocessor.transform(X_test)
feature_names = preprocessor.get_feature_names_out()

train_ready = pd.DataFrame(X_train_ready, columns=feature_names,
...........................index=X_train.index)
test_ready = pd.DataFrame(X_test_ready, columns=feature_names,
..........................index=X_test.index)

assert len(train_ready) == len(X_train)
assert list(train_ready.columns) == list(test_ready.columns)
assert np.isfinite(train_ready.to_numpy()).all()
assert np.isfinite(test_ready.to_numpy()).all()
assert train_ready.index.equals(y_train.index)
assert test_ready.index.equals(y_test.index)
assert "customer_id" not in train_ready.columns

print(train_ready.head())
print(test_ready.head())
# Kết quả mong muốn:
#          age    income region_Da Nang                  ...    region_TPHCM        \
       +    region_None card_tier
# 64 -0.207963 -0.250475            0.0                  ...                 0.0            \
       +      1.0        0.0
# 84 0.824479 -0.250475             0.0                  ...                 1.0            \
       +      0.0        0.0
# 13 1.340701 -0.250475             0.0                  ...                 1.0            \
       +      0.0        0.0
# 33 -0.207963 -0.250475            0.0                  ...                 0.0            \
       +      1.0        2.0
# 46 -0.207963 -0.751424            0.0                  ...                 0.0            \
       +      1.0        2.0
#
# [5 rows x 7 columns]
#          age    income region_Da Nang                  ...    region_TPHCM        \
       +    region_None card_tier
# 87 -0.207963 -0.250475            1.0                  ...                 0.0            \
       +      0.0        2.0
# 0 -1.550139 0.551044              0.0                  ...                 1.0            \
       +      0.0        1.0

# 29 -0.207963 -0.250475                             0.0    ...                 0.0            \
 +   0.0        1.0
# 25  0.824479 -0.250475                             0.0    ...                 1.0            \
 +   0.0        0.0
# 14  1.340701 -0.250475                             1.0    ...                 0.0            \
 +   0.0        0.0
#
# [5 rows x 7 columns]

    Một báo cáo chất lượng tốt nên tách hai lớp kiểm tra. Lớp thứ nhất là kiểm tra máy có thể tự quyết
định: kích thước, NaN, giá trị vô hạn, tên cột, số dòng, chỉ mục và lược đồ. Lớp thứ hai là kiểm tra
cần diễn giải nghiệp vụ: tỷ lệ khuyết thiếu trước xử lý có quá cao không, số dòng bị loại vì outlier có
hợp lý không, phân bố biến mục tiêu có thay đổi mạnh sau làm sạch không, và danh mục mới có phải
dấu hiệu thay đổi lược đồ hay chỉ là dữ liệu hợp lệ mới phát sinh. Assert là cổng kỹ thuật; báo cáo
chất lượng là bằng chứng để người khác phản biện được quyết định tiền xử lý.
```

**Mã nguồn 67. Tạo báo cáo chất lượng sau xử lý**
```python
quality_report = {
...."raw_rows": len(df_project),
...."clean_rows": len(clean_project),
...."train_rows": len(X_train),
...."test_rows": len(X_test),
...."removed_rows": len(df_project) - len(clean_project),
...."train_missing_before": X_train.isna().sum().to_dict(),
...."test_missing_before": X_test.isna().sum().to_dict(),
...."train_missing_after": train_ready.isna().sum().to_dict(),
...."test_missing_after": test_ready.isna().sum().to_dict(),
...."feature_count": train_ready.shape[1],
...."feature_names": feature_names.tolist(),
}

print(pd.Series(quality_report, dtype="object"))
# Kết quả mong muốn:
# raw_rows                                                          \
 +        100
# clean_rows                                                        \
 +         88
# train_rows                                                        \
 +         70
# test_rows                                                         \
 +         18
# removed_rows                                                      \
 +         12
# train_missing_before    {'age': 14, 'income': 13, 'region': 14, \
 +   'card_...
# test_missing_before     {'age': 6, 'income': 5, 'region': 4, \
 +   'card_tie...
# train_missing_after     {'age': 0, 'income': 0, 'region_Da \
 +   Nang': 0, '...

# test_missing_after                   {'age': 0, 'income': 0, 'region_Da \
        + Nang': 0, '...
# feature_count                                                                                   \
        +        7
# feature_names                        [age, income, region_Da Nang, region_Ha \
        + Noi, r...
# dtype: object

          Riêng với mã hóa one-hot, cần kiểm tra tình huống danh mục mới. Nếu dữ liệu mới có
      region = "Can Tho" trong khi tập train chưa có, encoder với handle_unknown="ignore" sẽ
      tạo các cột one-hot của region bằng 0 thay vì báo lỗi. Đây là lựa chọn an toàn ở mức kỹ thuật, nhưng
      báo cáo vận hành vẫn nên ghi nhận danh mục chưa biết để người quản trị dữ liệu xem xét cập nhật
      lược đồ hoặc huấn luyện lại pipeline.

      5.2.3. Xây dựng pipeline hoàn chỉnh từ dữ liệu thô
      Pipeline hoàn chỉnh không chỉ là một biến preprocessor. Nó là một chuỗi có đầu vào, đầu ra, quy
      tắc kiểm tra và hồ sơ bàn giao. Từ bản draft, nội dung thực hành dự án tổng hợp cần được hiểu là
      pipeline end-to-end: tiếp nhận dữ liệu thô, làm sạch lỗi miền xác định, tách tập train/tập test, khớp bộ
      tiền xử lý trên tập train, biến đổi tập test, thực hiện các cổng kiểm soát chất lượng, rồi lưu lại file kết
      quả đủ để chạy cho dữ liệu mới. Trong phạm vi học phần này, ta dừng ở tiền xử lý và chưa bắt buộc
      huấn luyện mô hình.
            Một pipeline hoàn chỉnh cần đáp ứng năm nhóm yêu cầu:
            1. Lược đồ đầu vào và đơn vị quan sát;
            2. Quy tắc làm sạch có lý do và nhật ký xử lý;
            3. Ranh giới tập train/tập test trước mọi thao tác học tham số;
            4. Transformer có thể fit/transform lặp lại;
            5. Các cổng kiểm soát chất lượng và siêu dữ liệu đầu ra.
          Các nhóm yêu cầu trên có thể được gói vào các hàm nhỏ. Hàm kiểm tra lược đồ chịu trách nhiệm
      từ chối bảng thiếu cột bắt buộc. Hàm làm sạch theo quy tắc xác định chỉ áp dụng những quy tắc không
      học thống kê từ dữ liệu, ví dụ loại tuổi lớn hơn 100 nếu đã được xác định là lỗi nhập liệu. Hàm fit chỉ
      dùng tập train. Hàm transform chỉ dùng preprocessor đã fit. Cách tổ chức này giúp sinh viên thấy rõ
      mỗi bước có đầu vào và đầu ra riêng, đồng thời dễ kiểm thử hơn so với một notebook dài chạy tuần
      tự.
```

**Mã nguồn 68. Hàm kiểm tra lược đồ và làm sạch theo quy tắc xác định**
```python
REQUIRED_COLUMNS = {
...."customer_id",
...."age",
...."income",
...."region",
...."card_tier",
...."spending",
}

def validate_schema(df):
....missing = REQUIRED_COLUMNS.difference(df.columns)

....if missing:
........raise ValueError(f"Thieu cot bat buoc: {sorted(missing)}")
....if df["customer_id"].duplicated().any():
........raise ValueError("customer_id bi trung")

def clean_domain_errors(df):
....validate_schema(df)
....cleaned = df[df["age"].isna() | df["age"].between(0, \
 + 100)].copy()
....audit = {
........"input_rows": len(df),
........"output_rows": len(cleaned),
........"removed_age_outliers": len(df) - len(cleaned),
....}
....return cleaned, audit

    Khi dữ liệu mới đến, không gọi fit_transform lần nữa; chỉ gọi transform trên bộ tiền xử lý
đã khớp. Nếu lược đồ thay đổi, pipeline phải báo lỗi rõ thay vì âm thầm chọn sai cột. Nếu cần triển
khai lại trong buổi học hoặc trong bài nộp, có thể lưu preprocessor đã fit bằng joblib để bảo đảm
cùng một file kết quả được dùng cho dữ liệu mới.
```

**Mã nguồn 69. Áp dụng pipeline đã fit cho dữ liệu mới**
```python
import pandas as pd

new_customers = pd.DataFrame({
...."customer_id": ["KH_101", "KH_102"],
...."age": [34, None],
...."income": [19.0, 14.5],
...."region": ["Can Tho", "Ha Noi"],  # "Can Tho" chưa xuất hiện \
 + trong train
...."card_tier": ["Vang", "Bac"],
...."spending": [0.0, 0.0],  # cột giả nếu schema yêu cầu target \
 + khi tiếp nhận
})

validated_new, new_audit = clean_domain_errors(new_customers)
new_ready = preprocessor.transform(validated_new[feature_columns])

assert new_ready.shape[1] == len(feature_names)
print(pd.DataFrame(new_ready, columns=feature_names))
print(new_audit)
# Kết quả mong muốn:
#         age    income region_Da Nang ... region_TPHCM                           \
 + region_None card_tier
# 0 -0.311208 -0.671272             0.0 ...           0.0                                   \
 + 0.0         1.0
# 1 -0.207963 -0.761443             0.0 ...           0.0                                   \
 + 0.0         0.0
#

# [2 rows x 7 columns]
# {'input_rows': 2, 'output_rows': 2, 'removed_age_outliers': 0}
```

**Mã nguồn 70. Lưu và nạp lại pipeline tiền xử lý**
```python
import joblib

joblib.dump(preprocessor, "preprocessor.joblib")
loaded_preprocessor = joblib.load("preprocessor.joblib")

loaded_ready = \
      + loaded_preprocessor.transform(validated_new[feature_columns])
assert loaded_ready.shape == new_ready.shape

      5.2.4. Chuẩn bị dữ liệu đưa vào mô hình học máy
      Đầu ra sẵn sàng cho mô hình khi tập train/tập test có cùng thứ tự đặc trưng, chỉ chứa giá trị số hữu
      hạn, biến mục tiêu căn chỉnh đúng index và không chứa mã định danh hay thông tin tương lai. “Sẵn
      sàng” không có nghĩa là mô hình sẽ tốt; nó nghĩa là các yêu cầu về dữ liệu đã được kiểm tra và ma
      trận có thể được đưa vào thuật toán học máy mà không cần thao tác thủ công bổ sung.
           Hồ sơ bàn giao nên bao gồm: kích thước của tập train/tập test, danh sách tên đặc trưng sau mã hóa
      one-hot, số dòng của biến mục tiêu, hạt giống ngẫu nhiên, quy tắc loại dòng, danh sách cột gốc, tham
      số mà các bộ biến đổi đã học, phiên bản lược đồ và đường dẫn file kết quả. Trong đó, biên outlier cho
      biết quy tắc xác định nào đã loại bản ghi; median, trung bình và hệ số tỷ lệ cho biết pipeline đã học gì
      từ tập train; còn tập danh mục cho biết bộ mã hóa đã biết những giá trị nào. Siêu dữ liệu là bản tóm
      tắt để kiểm tra và truy vết, không thay thế file kết quả đã khớp. Với sinh viên năm nhất, hồ sơ này
      giúp nối giữa lập trình cơ bản và tư duy dự án AI: người nhận không chỉ nhận một mảng NumPy vô
      danh mà nhận cả bằng chứng về cách mảng đó được tạo ra.
```

**Mã nguồn 71. Hồ sơ bàn giao ma trận đặc trưng**
```python
numeric_fitted = preprocessor.named_transformers_["numeric"]
categorical_fitted = preprocessor.named_transformers_["categorical"]

num_imputer = numeric_fitted.named_steps["imputer"]
num_scaler = numeric_fitted.named_steps["scaler"]
cat_encoder = categorical_fitted.named_steps["onehot"]

handoff = {
...."schema_version": "1.0",
...."train_shape": train_ready.shape,
...."test_shape": test_ready.shape,
...."feature_names": feature_names.tolist(),
...."train_target_rows": len(y_train),
...."test_target_rows": len(y_test),
...."random_state": 42,
...."source_features": feature_columns,
...."id_column": id_column,
...."target_column": target_column,
...."artifact": "preprocessor.joblib",
...."learned_parameters": {

........"numeric_medians": dict(zip(
............numeric_columns, num_imputer.statistics_.tolist()
........)),
........"scaler_mean": dict(zip(
............numeric_columns, num_scaler.mean_.tolist()
........)),
........"scaler_scale": dict(zip(
............numeric_columns, num_scaler.scale_.tolist()
........)),
........"known_categories": {
............column: values.tolist()
............for column, values in zip(
................categorical_columns, cat_encoder.categories_
............)
........},
....},
...."outlier_rules": {
........"age": {
............"lower": 0,
............"upper": 100,
............"action": "remove",
........},
....},
...."audit": {
........"removed_age_outliers": len(df_project) - \
 + len(clean_project),
....},
}

assert train_ready.index.equals(y_train.index)
assert test_ready.index.equals(y_test.index)
assert handoff["train_shape"][1] == len(handoff["feature_names"])
assert handoff["test_shape"][1] == len(handoff["feature_names"])
print(handoff)
# Kết quả mong muốn:
# {'schema_version': '1.0', 'train_shape': (70, 7), 'test_shape': \
 +   (18, 7), 'feature_names': ['age', 'income', 'region_Da Nang', \
 +   'region_Ha Noi', 'region_TPHCM', 'region_None', 'card_tier'], \
 +   'train_target_rows': 70, 'test_target_rows': 18, 'random_state': \
 +   42, 'source_features': ['age', 'income', 'region', 'card_tier'], \
 +   'id_column': 'customer_id', 'target_column': 'spending', \
 +   'artifact': 'preprocessor.joblib', 'learned_parameters': \
 +   {'numeric_medians': {'age': 35.0, 'income': 40.0}, 'scaler_mean': \
 +   {'age': 37.01428571428571, 'income': 52.5}, 'scaler_scale': \
 +   {'age': 9.685766961508678, 'income': 49.9052674002025}, \
 +   'known_categories': {'region': ['Da Nang', 'Ha Noi', 'TPHCM', \
 +   None]}}, 'outlier_rules': {'age': {'lower': 0, 'upper': 100, \
 +   'action': 'remove'}}, 'audit': {'removed_age_outliers': 12}}
     Khi thay đổi lược đồ, danh sách cột hoặc quy tắc xử lý outlier, cần tăng schema_version và

      tạo lại cả file kết quả lẫn siêu dữ liệu trong cùng một lần chạy. Không nên chép tay các tham số này
      sang báo cáo vì báo cáo rất dễ lệch khỏi pipeline thực tế. Trong dự án lớn hơn, có thể bổ sung phiên
      bản thư viện, thời điểm tạo và mã kiểm tra của file kết quả để xác nhận đúng file khi triển khai.
      Câu hỏi thảo luận
           • Vì sao customer_id hoặc họ tên khách hàng không được đưa vào ma trận đặc trưng dù chúng
             có vẻ giúp phân biệt từng dòng?
           • Điều gì xảy ra nếu gọi fit_transform trên toàn bộ dữ liệu trước khi chia tập train/tập test?
           • Khi tập test có một khu vực mới chưa xuất hiện trong tập train, handle_unknown="ignore"
             bảo vệ chương trình như thế nào và giới hạn của cách bảo vệ đó là gì?
           • Khi nào nên dùng mã hóa thứ bậc cho một cột phân loại, và khi nào mã hóa one-hot phù hợp
             hơn?
      Bài tập thực hành
          1. Từ df_project, bổ sung kiểm tra loại khách hàng có age > 100 và ghi lại số dòng bị loại
             trong audit report.
          2. Thêm một cột phân loại mới vào df_project, cập nhật ColumnTransformer và kiểm tra
             lại danh sách tên đặc trưng.
          3. Lưu preprocessor bằng joblib, nạp lại, rồi transform một bảng dữ liệu mới có region chưa
             từng xuất hiện trong tập train.
          4. Viết một hàm build_handoff_report nhận train_ready, test_ready, y_train,
             y_test và trả về hồ sơ bàn giao ở dạng dictionary.

      Nhiệm vụ tổng kết
      Xây dựng pipeline đầu cuối cho một bảng nhỏ: kiểm tra lược đồ, làm sạch theo quy tắc xác định
      và có nhật ký xử lý, chia tập train/tập test, khớp bộ biến đổi trên tập train, biến đổi tập test và thực
      hiện các cổng kiểm soát chất lượng. Nộp sơ đồ luồng, mã nguồn, từ điển dữ liệu, bảng chất lượng
      trước/sau, hai ma trận của tập train/tập test sẵn sàng cho mô hình, file preprocessor.joblib và
      handoff_report. Hồ sơ bàn giao phải ghi được tên đặc trưng sau mã hóa, số bản ghi bị loại, hạt
      giống ngẫu nhiên, tham số chuẩn hóa thang đo và biên outlier, kèm lập luận vì sao đầu ra đủ điều kiện
      bàn giao.
       Pipeline chạy lại cho kết quả nhất quán.
       Mọi bước có đầu vào, đầu ra và giả định rõ ràng.
       Không khớp tham số từ tập validation/tập test hoặc dữ liệu mới.
       Có các cổng kiểm soát chất lượng, tên đặc trưng và nhật ký xử lý.
       Mã định danh, biến mục tiêu và thông tin tương lai không lọt vào ma trận đặc trưng.
       Tệp kết quả và hồ sơ bàn giao được tạo trong cùng một lần chạy.
       Siêu dữ liệu ghi đúng kích thước, hạt giống ngẫu nhiên, tham số đã học và biên outlier.

## Bài tập tự học theo buổi
Buổi 10: Pipeline tiền xử lý hoàn chỉnh (6 giờ)
```
Nhiệm vụ. Hoàn thiện nhiệm vụ tổng kết bằng cách kết nối kiểm tra lược đồ, làm sạch, biến đổi, mã
hóa và các cổng kiểm soát chất lượng thành một pipeline chạy lại được. Lưu bộ tiền xử lý, tạo hồ sơ
bàn giao và thử ít nhất một trường hợp lược đồ thay đổi mà pipeline phải từ chối hoặc cảnh báo.
     Input. Hai DataFrame train/test theo schema gồm customer_id, các cột số, cột phân loại và
target; schema mới thiếu cột bắt buộc phải bị từ chối. Output. Ma trận đặc trưng train/test, file bộ
tiền xử lý và dictionary handoff_report ghi schema, shape, tên đặc trưng, tham số và audit.

**Mã nguồn 72. Prototype và ví dụ input–output Buổi 10**
```python
import pandas as pd

def build_preprocessing_handoff(
....train_df: pd.DataFrame,
....test_df: pd.DataFrame,
....target_column: str,
....id_column: str,
) -> tuple[object, pd.DataFrame, pd.DataFrame, dict[str, object]]:
.......

# Input: train (8 rows), test (2 rows), 4 đặc trưng sau biến đổi
expected_handoff = {
...."train_shape": [8, 4], "test_shape": [2, 4],
...."target_column": "target", "id_column": "customer_id",
}
    Sản phẩm. Sơ đồ luồng, mã nguồn, file preprocessor.joblib, từ điển dữ liệu, bảng chất
lượng trước/sau, hai ma trận của tập train/tập test và handoff_report.
    Hoàn thành khi. Pipeline cho kết quả nhất quán với cùng hạt giống, không học tham số từ tập
validation/tập test, không đưa mã định danh/biến mục tiêu vào ma trận đặc trưng và hồ sơ bàn giao
khớp với file kết quả.
    Thời lượng dự kiến. 6 giờ, tương ứng toàn bộ thời lượng tự học của Chương 5 và là sản phẩm
tổng hợp cuối học phần.
```
