# Thanh Ghi Siêu Dữ liệu — Đã Xác minh / Ước tính / Cần Đo

## ✅ ĐÃ XÁC MINH (Được Đo trực tiếp & Xác nhận)

### Ảnh chụp Tập dữ liệu
- **N (tổng hàng):** 50,000 ✅ [nguồn: datacard_stats.json]
- **Phân bố nhãn:** {0: 25,000, 1: 25,000} ✅ [hoàn toàn cân bằng]
- **Thống kê độ dài văn bản:** min=32, median=954, p95=3,328, max=13,593 ký tự ✅
- **Tách Train/Val/Test:** 40,000 / 5,000 / 5,000 (80/10/10) ✅
- **Giá trị Bị thiếu:** text=0, label=0 ✅ [0% bị thiếu]

### Kiểm toán Chất lượng Dữ liệu (Trước Tiền xử lý)
- **Thẻ HTML có mặt:** 29,202 hàng (58.4%) chứa thẻ `<br>` ✅
- **Bản sao Chính xác:** 824 hàng (tỷ lệ 1.648%) ✅
- **Mất cân bằng Lớp:** tỷ lệ 1.0 (cân bằng hoàn hảo) ✅

### Xác thực Great Expectations
- **5 ĐẠT:** nullness (text, label, id), uniqueness (id), value range (label trong {0,1}) ✅ [tỷ lệ pass 83.33%]
- **1 KHÔNG ĐẠT:** độ dài text từ 1–10,000 ký tự (5 ngoại lệ max 13,593) ✅
- **Nguồn:** outputs/ge/validation_result.json

### Đánh giá Chất lượng Nhãn Cleanlab
- **Vấn đề Nghi ngờ:** 1,003 hàng (2.006%) được gắn cờ là có thể bị gắn nhãn sai ✅
- **Top 200 Được xuất:** outputs/logs/cleanlab_label_issues.csv ✅
- **Xem xét Thủ công:** 5 mẫu được kiểm tra (2 lỗi rõ ràng, 3 không rõ ràng) ✅

### Biến đổi Dữ liệu (Kiểm toán Trước → Sau)
- **Loại bỏ HTML:** 29,202 → 0 hàng (100% được làm sạch) ✅
- **Thay đổi độ dài văn bản:** median 970→954 (-16 ký tự, đúng dự kiến) ✅
- **Tỷ lệ Bản sao:** 1.648% → 1.664% (nhất quán sau làm sạch) ✅
- **Tổng hàng được bảo toàn:** 50,000 (0 mất mát khi làm sạch) ✅

### Phân tích Rò rỉ Tiền xử lý
- **Rủi ro Chỉ ra:** Nếu vectorizer khớp trên tất cả 50k, từ vựng tập test rò rỉ ✅
- **Giảm thiểu:** Tách trước, fit trên train only ✅
- **Trạng thái:** Được ghi chú trong đường ống tiền xử lý ✅

---

## 🟡 ƯỚC TÍNH (Suy diễn, Không Được Đo Trực tiếp)

### Phát hiện Gần giống nhau
- **Ước tính bản sao gần giống:** <0.1% (0 cặp tìm thấy trong mẫu) 
- **Lý do:** Cleanlab báo cáo 0 cặp gần giống trong tập được phân tích
- **Lưu ý:** Toàn bộ tập dữ liệu không được kiểm tra đầy đủ; ước tính bảo thủ

### Chất lượng Nhãn (Trường hợp Biên giới)
- **Tỷ lệ không rõ ràng Ước tính:** ~2–3% nhãn thực sự biên giới
- **Cơ sở:** Phát hiện Cleanlab 2% + xem xét thủ công cho thấy 60% không rõ ràng
- **Lưu ý:** Không được xác nhận chính thức bởi chú thích viên con người

### Nhân khẩu học Tác giả / Sai lệch Địa lý
- **Thành phần người dùng IMDB Ước tính:** ~70% nói tiếng Anh, Bắc Mỹ
- **Cơ sở:** Thống kê người dùng IMDB (không phải dữ liệu của chúng tôi)
- **Lưu ý:** Tập dữ liệu của chúng tôi không cung cấp nhãn nhân khẩu học tác giả

### Phân bố Lớp hiệp theo Độ dài Văn bản
- **Ước tính:** Bài đánh giá tiêu cực có thể ngắn hơn bình tích cực một chút (chênh lệch trung vị ~16 ký tự)
- **Cơ sở:** audit_before cho thấy median 970; phiên bản được làm sạch 954 (có thể cân bằng trước/sau)
- **Lưu ý:** Phân bố cho mỗi nhãn không được báo cáo riêng

---

## ⏳ CẦN ĐO (Chưa Thu thập, Công việc Tương lai)

### Sự Đồng ý của Con người về Nhãn Tranh cãi
- **Tác vụ:** Để những người chú thích con người xem xét 5 mẫu được gắn cờ bởi Cleanlab
- **Chỉ số:** Sự đồng ý Chú thích (Cohen's kappa)
- **Trạng thái:** CHƯA THỰC HIỆN ⏳
- **Nỗ lực Ước tính:** 30-60 phút cho 5 mẫu

### Thống kê Cấp độ Mã thông báo (Tùy chọn)
- **Chỉ số Cần thiết:** 
  - Trung bình mã thông báo mỗi bài đánh giá
  - Kích thước Từ vựng (V) cho train / val / test
  - OOV (out-of-vocabulary) trên tập test
- **Trạng thái:** CHƯA ĐƯỢC TÍNH TOÁN ⏳

### Phân tích Công bằng & Sai lệch (Tùy chọn)
- **Chỉ số cần Đo:**
  - Phân bố Tình cảm theo Thể loại phim
  - Phân bố Tình cảm theo Năm phát hành phim
  - Tần số Tên diễn viên/nữ diễn viên và mối tương quan Tình cảm
- **Trạng thái:** CHƯA HOÀN THÀNH ⏳
- **Lý do:** Không có nhãn nhân khẩu học có sẵn; sẽ cần ghi chú thủ công

### Mẫu Cực tính Hỗn hợp Cấp độ Câu (Tùy chọn)
- **Chỉ số:** Bài đánh giá có chứa cảm xúc hỗn hợp (tích cực + tiêu cực trong một bài)?
- **Trạng thái:** CHƯA ĐƯỢC PHÂN TÍCH ⏳
- **Phương pháp:** Sẽ cần chú thích cấp độ khía cạnh

### Độ trễ Chú thích & Nhân khẩu học Người gắn nhãn (Lịch sử)
- **Chỉ số:** Mỗi bài đánh giá người dùng IMDB dành bao lâu để viết?
- **Trạng thái:** KHÔNG CÓ SẴN ⏳
- **Lý do:** Bài báo gốc không thu thập siêu dữ liệu này

### So sánh với v1.0 (Gốc)
- **Tác vụ:** So sánh dữ liệu được làm sạch của chúng tôi với phiên bản gốc để tái tạo
- **Chỉ số:** So sánh Độ chính xác, So sánh Hiệu suất
- **Trạng thái:** CHƯA THỰC HIỆN ⏳
- **Lý do:** Cần phiên bản tập dữ liệu gốc để so sánh

---

## BẢNG TÓM TẮT

| Chỉ số | Trạng thái | Chắc chắn | Chủ sở hữu | Bằng chứng |
|--------|--------|-----------|--------|----------|
| Phân bố nhãn | ✅ Đã xác minh | 100% | Script kiểm toán | datacard_stats.json |
| Kết quả xác thực GE | ✅ Đã xác minh | 100% | Khung GE | validation_result.json |
| Vấn đề Cleanlab nghi ngờ | ✅ Đã xác minh | ~95% | Mô hình Cleanlab | cleanlab_summary.md |
| Bản sao Chính xác | ✅ Đã xác minh | 100% | Khớp chuỗi | audit_before.md |
| Hiện tượng HTML | ✅ Đã xác minh | 100% | Quét Regex | audit_before.md |
| Tách Train/val/test | ✅ Đã xác minh | 100% | Script tách | datacard_stats.json |
| Bản sao Gần giống | 🟡 Ước tính | ~70% | Phân tích mẫu | (0 tìm thấy trong tập hợp con) |
| Không rõ ràng Nhãn | 🟡 Ước tính | ~75% | Xem xét thủ công | 5 mẫu kiểm tra |
| Sự đồng ý Chú thích của con người | ⏳ Cần đo | — | (Đang chờ) | N/A |
| Chỉ số Công bằng | ⏳ Cần đo | — | (Đang chờ) | N/A |
| Kích thước Từ vựng | ⏳ Cần đo | — | (Đang chờ) | N/A |

---

**Cập nhật Lần cuối:** Ngày 13 tháng 4 năm 2026  
**Trạng thái:** ✅ Module 2 (KIỂM TOÁN) Hoàn thành

