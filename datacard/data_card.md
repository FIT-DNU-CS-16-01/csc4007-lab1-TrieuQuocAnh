# Data Card (copy từ Google Docs template)

Link Google Docs: ...

## Version
- v1.0 — ...
- v1.1 — ...

Dán nội dung Data Card bên dưới.
Dataset gồm 50,000 văn bản phục vụ bài toán phân loại nhị phân (binary classification) với hai nhãn cân bằng (0 và 1). Dữ liệu được sử dụng cho mục đích huấn luyện và đánh giá các mô hình NLP.
Audit BEFORE Preprocessing
Schema & Missingness
n_rows: 50,000
Không có missing text hoặc label
Label cân bằng (25k mỗi lớp)
Dataset có cấu trúc tốt, không cần xử lý missing hoặc imbalance
HTML Noise
~58.4% dữ liệu chứa HTML tags
29,200 mẫu chứa <br>
11 mẫu chứa HTML entities
Distribution / Length
Median: 970 ký tự
P95: 3391 ký tự
Max: 13,704 ký tự
Duplicates
824 duplicate (~1.65%)

Preprocessing Pipeline
Các bước tiền xử lý được áp dụng:
Loại bỏ HTML tags (e.g. <br>)
Chuẩn hóa văn bản (lowercase, strip)
Xử lý HTML entities
Loại bỏ duplicate
Chia dữ liệu thành train/val/test
Fit TF-IDF trên tập train
Transform val/test

Audit AFTER Preprocessing
Schema & Missingness
Không còn missing hoặc empty text
Label vẫn cân bằng
HTML Noise
Đã loại bỏ hoàn toàn HTML tags
Còn 11 HTML entities (rất nhỏ)
Distribution / Length
Median: 954 ký tự
P95: 3328 ký tự
Max: 13,593 ký tự
Duplicates
832 duplicate (~1.66%)
Leakage Fix
Áp dụng đúng quy trình:
Split trước
Fit trên train
Transform trên val/test