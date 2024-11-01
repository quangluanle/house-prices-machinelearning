# Mô hình MLP được tăng cường bằng cách sử dụng dự đoán từ mô hình Random Forest như một đặc trưng bổ sung.

## Các bước thực hiện

### 1. Cài đặt môi trường
- Cài thư viện cần thiết: `tensorflow`, `scikit-learn`, `pandas`, `numpy`.

### 2. Xử lý dữ liệu

#### a. Nạp dữ liệu
- Dữ liệu huấn luyện `X_train.csv` và nhãn mục tiêu `y_train.csv` được nạp, cùng với dữ liệu kiểm thử `X_test.csv`.
- Loại bỏ cột `ID`.

#### b. Xử lý các giá trị thiếu
- Các cột `buildingType`, `elevator`, `fiveYearsProperty`, `subway`, `livingRoom`, và `bathRoom` được chuyển sang kiểu số (numeric) và thay thế giá trị thiếu bằng giá trị mode (giá trị xuất hiện nhiều nhất).
- Với các cột có giá trị ngoài phạm vi hợp lệ, giá trị đó được thay thế bằng giá trị mode (chẳng hạn như `buildingType` và `bathRoom`).
  
#### c. Tính toán khoảng cách tới thủ đô
- Sử dụng công thức địa lý `haversine` để tính khoảng cách giữa vị trí của bất động sản và thủ đô dựa trên tọa độ `Lat` và `Lng`.

#### d. Xử lý thời gian
- Chuyển đổi cột `tradeTime` và `constructionTime` sang kiểu ngày tháng và số.
- Tính toán tuổi của tòa nhà (`ageOfBuilding`) và chia thành các cột như `year`, `month`, và `day` theo `tradeTime`.

#### e. Xử lý dữ liệu dạng text
- Các giá trị trong cột `floor` và `drawingRoom` được chuyển đổi thành các giá trị số dựa trên ánh xạ đặc biệt (mapping).

### 3. Huấn luyện mô hình

#### a. Chia tập dữ liệu
- Chia tập dữ liệu thành tập huấn luyện và tập validation với tỷ lệ 80:20.

#### b. Huấn luyện mô hình Random Forest
- Sử dụng mô hình `RandomForestRegressor` với các tham số tùy chỉnh (như `n_estimators=900`, `max_depth=20`) để huấn luyện.
- In ra độ lỗi RMSE trên tập validation.

#### c. Dự đoán từ Random Forest
- Dự đoán trên tập huấn luyện, validation, và tập kiểm thử từ mô hình Random Forest.
- Sử dụng các dự đoán này làm đặc trưng bổ sung để huấn luyện mô hình MLP.

### 4. Huấn luyện mô hình MLP

#### a. Xây dựng mô hình MLP
- Tạo mô hình MLP với nhiều tầng `Dense`, `BatchNormalization`, và `Dropout` nhằm giảm thiểu overfitting.
- Mô hình sử dụng cấu trúc 512-256-128-1 với các tầng ẩn kích hoạt hàm `relu`.

#### b. Huấn luyện và tinh chỉnh mô hình MLP
- Sử dụng `EarlyStopping` và `ReduceLROnPlateau` để dừng sớm quá trình huấn luyện nếu độ lỗi không cải thiện.
- Huấn luyện mô hình MLP với các dự đoán từ mô hình Random Forest như một đặc trưng bổ sung.

### 5. Dự đoán và lưu kết quả
- Dự đoán trên tập validation và tập kiểm thử bằng mô hình MLP, sau đó tính RMSE cho tập validation.
- Lưu kết quả dự đoán trên tập kiểm thử vào file `Latest_submission.csv`.

### 6. Lưu mô hình
- Mô hình MLP đã được huấn luyện.