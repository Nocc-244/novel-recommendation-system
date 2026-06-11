# Hệ Thống Gợi Ý Truyện Chữ Trên WikiDich Bằng Machine Learning

## Giới thiệu

Đây là dự án Mini Project môn Học Máy, xây dựng hệ thống gợi ý truyện chữ dựa trên dữ liệu thực tế được thu thập từ nền tảng WikiDich.

Trong bối cảnh số lượng truyện trên các nền tảng đọc truyện trực tuyến ngày càng lớn, người đọc thường gặp khó khăn trong việc tìm kiếm những bộ truyện phù hợp với sở thích hoặc có chất lượng tốt. Dự án này áp dụng các kỹ thuật Machine Learning để phân tích dữ liệu truyện, xây dựng điểm đánh giá tổng hợp và hỗ trợ quá trình gợi ý truyện.

---

## Mục tiêu dự án

* Thu thập dữ liệu truyện từ WikiDich.
* Xây dựng tập dữ liệu có cấu trúc phục vụ huấn luyện mô hình.
* Tiền xử lý và chuẩn hóa dữ liệu.
* Xây dựng các đặc trưng từ thể loại và trạng thái truyện.
* Huấn luyện các mô hình Machine Learning để dự đoán điểm chất lượng của truyện.
* So sánh hiệu quả của các mô hình khác nhau.
* Xây dựng hệ thống gợi ý dựa trên các đặc trưng đã học được.

---

## Bộ dữ liệu

Dữ liệu được thu thập trực tiếp từ WikiDich thông qua chương trình Data Crawling tự xây dựng.

### Quy mô dữ liệu

* 2934 bộ truyện (dữ liệu thô)
* Nhiều thể loại khác nhau như:

  * Bách hợp
  * Đam mỹ
  * Ngôn tình
  * Hệ thống
  * Xuyên không
  * Hiện đại
  * Cổ đại
  * Và nhiều thể loại khác

### Các trường dữ liệu chính

* Tên truyện
* Tác giả
* Thể loại
* Tag
* Tình trạng truyện
* Lượt xem
* Lượt sao
* Lượt bình luận

Sau khi thu thập, dữ liệu được làm sạch và chuyển đổi thành dạng phù hợp để huấn luyện mô hình Machine Learning.

---

## Cấu trúc dự án

```text
.
├── Wikidich_bachhop_cao_data.ipynb
├── REC_SYS_Truyen.ipynb
├── data.csv
└── README.md
```

### Mô tả

**Wikidich_bachhop_cao_data.ipynb**

Notebook dùng để:

* Kết nối WikiDich
* Thu thập dữ liệu truyện
* Chuyển đổi dữ liệu sang định dạng CSV
* Lưu dữ liệu phục vụ huấn luyện

**REC_SYS_Truyen.ipynb**

Notebook dùng để:

* Đọc dữ liệu
* Tiền xử lý dữ liệu
* Feature Engineering
* Huấn luyện mô hình
* Đánh giá mô hình
* Sinh kết quả gợi ý

---

## Data Crawling

Dữ liệu được thu thập bằng Python sử dụng:

* requests / curl_cffi
* BeautifulSoup
* Pandas

### Lưu ý quan trọng

Notebook crawl dữ liệu yêu cầu Cookie đăng nhập WikiDich hợp lệ.

Trước khi chạy chương trình, người dùng cần thay thế Cookie trong phần cấu hình bằng Cookie của tài khoản WikiDich của mình.

Cookie cá nhân không được đưa lên repository vì lý do bảo mật.

Nếu không cung cấp Cookie hợp lệ, chương trình có thể không truy cập được dữ liệu hoặc trả về kết quả không đầy đủ.

---

## Tiền xử lý dữ liệu

Các bước xử lý dữ liệu bao gồm:

### Làm sạch dữ liệu

* Loại bỏ dữ liệu lỗi
* Thay thế giá trị thiếu
* Chuẩn hóa định dạng dữ liệu

### Log Transform

Do các chỉ số như lượt xem có giá trị rất lớn so với lượt sao và bình luận nên thực hiện:

```text
x' = log(1 + x)
```

nhằm giảm ảnh hưởng của các giá trị ngoại lai.

### Chuẩn hóa dữ liệu

Sử dụng MinMaxScaler để đưa dữ liệu về khoảng:

```text
[0, 1]
```

---

## Feature Engineering

Các đặc trưng đầu vào được xây dựng từ:

### Thể loại truyện

Sử dụng One-Hot Encoding để chuyển đổi dữ liệu thể loại thành dữ liệu số.

### Trạng thái truyện

Ví dụ:

* Đang ra
* Hoàn thành
* Tạm ngưng
* Và các trạng thái khác

Sau quá trình xử lý, tập dữ liệu bao gồm các đặc trưng dạng số phục vụ huấn luyện mô hình.

---

## Xây dựng biến mục tiêu

Điểm chất lượng của truyện được xây dựng từ các chỉ số tương tác:

* Rating (Star)
* Comment
* View

Sau khi chuẩn hóa:

```text
Y = 0.4 × Star + 0.35 × Comment + 0.25 × View
```

Điểm số này được sử dụng làm biến mục tiêu trong bài toán hồi quy.

---

## Các mô hình được sử dụng

### 1. Lasso Regression

Ưu điểm:

* Tự động chọn đặc trưng
* Giảm độ phức tạp của mô hình
* Phù hợp với dữ liệu One-Hot Encoding

### 2. Ridge Regression

Ưu điểm:

* Giữ lại toàn bộ đặc trưng
* Ổn định hơn trong nhiều trường hợp

### 3. Random Forest Regressor

Ưu điểm:

* Học được các mối quan hệ phi tuyến
* Ít bị Overfitting hơn cây quyết định đơn lẻ
* Khả năng dự đoán tốt trên dữ liệu thực tế

---

## Đánh giá mô hình

Các chỉ số đánh giá được sử dụng:

* MAE (Mean Absolute Error)
* MSE (Mean Squared Error)
* RMSE (Root Mean Squared Error)
* R² Score

Kết quả thực nghiệm cho thấy mô hình đạt:

* MAE ≈ 0.059
* RMSE ≈ 0.077
* R² ≈ 0.56

---

## Kết quả đạt được

* Thu thập thành công 2934 bộ truyện từ WikiDich.
* Xây dựng bộ dữ liệu phục vụ Machine Learning.
* Hoàn thành quy trình từ Data Crawling đến Model Training.
* So sánh nhiều mô hình học máy khác nhau.
* Xây dựng hệ thống gợi ý dựa trên các đặc trưng của truyện.

---

## Hạn chế

* Chưa sử dụng NLP để phân tích nội dung truyện.
* Chưa xây dựng hệ thống cá nhân hóa theo từng người dùng.
* Chưa khai thác nội dung văn bản của truyện.

---

## Hướng phát triển

Trong tương lai có thể mở rộng theo các hướng:

* Ứng dụng NLP để phân tích nội dung truyện.
* Sử dụng Deep Learning nhằm nâng cao độ chính xác.
* Xây dựng hệ thống Recommendation cá nhân hóa.
* Triển khai giao diện Web hoàn chỉnh cho người dùng cuối.

---

## Công nghệ sử dụng

* Python
* Pandas
* NumPy
* Scikit-learn
* BeautifulSoup
* curl_cffi
* Google Colab

---

## Thành viên thực hiện

* Nguyễn Thị Hà
* Phạm Việt Huy
* Lê Đình Huy

Mini Project môn Học Máy – Trường Đại học Khoa học Tự nhiên, Đại học Quốc gia Hà Nội.
