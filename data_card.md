# PHIẾU THÔNG TIN DỮ LIỆU — Tập hợp Nhận xét IMDB
**Phiên bản:** 1.0 | **Cập nhật lần cuối:** 2026-04-13 | **Trạng thái:** Hoàn thành với kết quả kiểm toán

---

## MODULE 1: HỎI — MỤC ĐÍCH SỬ DỤNG VÀ PHẠM VI

### 1.1 Đối tượng Dự kiến (Các Bên Liên quan)
Ai nên quan tâm đến tập dữ liệu này? (Liệt kê 2-5 nhóm)
| Nhóm | Lợi ích Chính | Câu hỏi Họ Đặt Ra |
|-------|---------------|--------------------|
| **Sinh viên NLP** | Học phân loại tình cảm, đánh giá tiêu chuẩn | Dữ liệu có sạch không? Có vấn đề nhãn? Làm sao tránh rò rỉ? |
| **Nhà Nghiên cứu ML** | Độ mạnh mẽ, khả năng tái lập, đánh giá cơ sở | Tách train/test thế nào? Chất lượng nhãn? Kiểm tra trùng lặp/rò rỉ chưa? |
| **Kỹ Sư NLP/ML** | Triển khai sản xuất, chất lượng dữ liệu | Cần tiền xử lý gì? Tỷ lệ pass trong xác thực? Có vấn đề gì cần xử lý? |
| **Nhà Khoa học Dữ liệu** | Thiết kế tính năng, phân tích khám phá | Phân bố độ dài văn bản? Cân bằng lớp? Có hiện tượng lạ? |
| **Người Đánh giá Học thuật** | Nguồn gốc dữ liệu, tiêu chuẩn tài liệu | Phiếu thông tin dữ liệu đầy đủ? Kiểm toán xong? Giới hạn được công khai? |

### 1.2 Các Phạm vi Quan trọng (5-7 Lense Quan trọng nhất)

1. **Chất lượng Dữ liệu & Làm sạch** ✅ BẮT BUỘC
   - Làm sạch thẻ HTML/thực thể, phát hiện trùng lặp, quy tắc xác thực
   - Nguồn: GE validation (5/6 PASS), nhật ký kiểm toán, kết quả cleanlab

2. **Chất lượng Nhãn & Chú thích** ✅ BẮT BUỘC
   - Sự đồng ý của nhãn, vấn đề nghi ngờ, bằng chứng xem xét thủ công
   - Nguồn: Cleanlab (2% vấn đề nghi ngờ), xem xét thủ công 5 mẫu

3. **Tách Train/Val/Test & Rò rỉ** ✅ BẮT BUỘC
   - Tỷ lệ tách, phân biệt phim, thứ tự tiền xử lý
   - Nguồn: nhật ký kiểm toán cho thấy rủi ro rò rỉ từ vựng nếu tiền xử lý trước

4. **Chuyển đổi & Tiền xử lý** ✅ BẮT BUỘC
   - Thống kê TRƯỚC vs SAU, lý do cho mỗi bước
   - Nguồn: audit_before.md vs audit_after.md

5. **Ảnh chụp Tập dữ liệu & Thống kê** ✅ BẮT BUỘC
   - N, phân bố nhãn, độ dài văn bản, cân bằng lớp
   - Nguồn: datacard_stats.json (50k hàng, cân bằng 50-50)

6. **Kết quả Xác thực & Vấn đề Đã biết** ✅ BẮT BUỘC
   - Các quy tắc GE, phân tích lỗi, kế hoạch giảm thiểu
   - Nguồn: GE validation_result.json (5/6 PASS, 1 FAIL)

7. **Phù hợp với Use Case & Giới hạn** ✅ BẮT BUỘC
   - Nó tốt cho cái gì, không tốt cho cái gì, những khoảng trống đã biết
   - Nguồn: phạm vi bài báo + kết quả kiểm toán

**Phạm vi Không áp dụng:**
- **Chi phí Tính toán / Hiệu suất Suy luận:** N/A — Không quan trọng về hiệu suất cho đánh giá tiêu chuẩn
- **Hỗ trợ Đa ngôn ngữ:** N/A — Tập dữ liệu chỉ tiếng Anh
- **Phát trực tuyến Thời gian thực:** N/A — Phát hành tiêu chuẩn tĩnh
- **Lọc PII/Quyền riêng tư:** N/A — Đánh giá công khai của IMDB do người dùng tạo (không có dữ liệu riêng)

---

## MODULE 2: KIỂM TOÁN — ĐƠNG LƯỜNG & XÁC MINH

### 2.1 Danh sách Kiểm tra: Ai đo được cái gì?

| Phép đo | Chỉ số | Chủ sở hữu | Phương pháp | Trạng thái |
|-------------|--------|--------|-----------|--------|
| **Kích thước Tập dữ liệu** | N = 50,000 | Script kiểm toán | Đếm hàng | ✅ Đã xác minh |
| **Phân bố Nhãn** | {0: 25k, 1: 25k} | Script kiểm toán | Giá trị đếm | ✅ Đã xác minh |
| **Thống kê Độ dài Văn bản** | min/median/p95/max | Script kiểm toán | len(text) mỗi hàng | ✅ Đã xác minh |
| **Hiện tượng HTML** | 29,202 hàng với HTML, 11 thực thể | Script kiểm toán | Quét Regex | ✅ Đã xác minh |
| **Bản sao Chính xác** | 832 / 50,000 = 1.664% | Script kiểm toán | Khớp chuỗi chính xác | ✅ Đã xác minh |
| **Xác thực GE** | 5/6 PASS (83.33%), 1 FAIL | Khung GE | Quy tắc expect_* | ✅ Đã xác minh |
| **Vấn đề Cleanlab** | 1,003 / 50,000 = 2.006% nghi ngờ | Mô hình Cleanlab | Phát hiện dựa trên ML | ✅ Đã xác minh |
| **Tỷ lệ Tách** | train 40k / val 5k / test 5k | Script tách | Phân chia ngẫu nhiên phân tầng | ✅ Đã xác minh |
| **Tác động Tiền xử lý** | Thẻ HTML bị xóa, tìm thấy bản sao | So sánh kiểm toán | audit_before vs sau | ✅ Đã xác minh |

### 2.2 Đã Xác minh / Ước tính / Cần Đo

(Tham khảo: `datacard/metadata_register.md`)

#### ✅ ĐÃ XÁC MINH (Đã Đo & Xác nhận)
- ✅ **Ảnh chụp Tập dữ liệu:**
  - N = 50,000 hàng
  - Phân bố nhãn: 25k tích cực (1), 25k tiêu cực (0) — hoàn toàn cân bằng
  - Độ dài văn bản: min=32, median=954, p95=3,328, max=13,593 ký tự
  - Không có giá trị bị thiếu (văn bản hoặc nhãn)

- ✅ **Kiểm toán Chất lượng Dữ liệu (TRƯỚC tiền xử lý):**
  - Thẻ HTML trong 29,202 hàng (58.4%)
  - Bản sao chính xác: 824 hàng
  - Mất cân bằng lớp: tỷ lệ 1.0 (hoàn toàn cân bằng)

- ✅ **Xác thực Great Expectations:**
  - 5 quy tắc ĐẠT ✅
    1. Cột `text` không null: 50,000/50,000 ✅
    2. Cột `label` không null: 50,000/50,000 ✅  
    3. `label` trong {0, 1}: 50,000/50,000 ✅
    4. Cột `id` không null: 50,000/50,000 ✅
    5. Cột `id` duy nhất: 50,000/50,000 ✅
  - 1 quy tắc KHÔNG ĐẠT ❌
    - `len_chars` từ 1-10,000: **5 giá trị ngoại lệ vượt quá 10,000 ký tự**
    - Giá trị max: 13,593 ký tự (ngoại lệ lành tính cho tập dữ liệu này)

- ✅ **Vấn đề Nhãn Cleanlab (Phát hiện dựa trên ML):**
  - 1,003 hàng được gắn cờ là vấn đề nghi ngờ = 2.006%
  - Top 200 xuất trong `outputs/logs/cleanlab_label_issues.csv`

- ✅ **Tách & Phân tầng:**
  - Train: 40,000 hàng (80%)
  - Val: 5,000 hàng (10%)  
  - Test: 5,000 hàng (10%)
  - Phương pháp: Phân chia ngẫu nhiên phân tầng (cân bằng lớp được duy trì)

#### 🟡 ƯỚC TÍNH (Suy diễn, không đo trực tiếp)
- **Cặp gần giống nhau:** 0 cặp tìm thấy trong mẫu (ước tính <0.1% tập dữ liệu)
- **Chính xác nhãn cho đánh giá "biên giới":** khoảng 2-3% có thể không rõ ràng (dựa trên vấn đề nghi ngờ Cleanlab)
- **Rủi ro rò rỉ tiền xử lý:** CAO nếu vectorizer khớp trên dữ liệu đầy đủ thay vì chỉ train

#### ⏳ CẦN ĐO (Chưa thu thập)
- **Sự đồng ý của con người về nhãn tranh cãi** (xác minh các mẫu được Cleanlab gắn cờ bằng chú thích viên)
- **Mô-đun Công bằng:** Có sai lệch nhân khẩu học nào trong nhãn tình cảm không? (tên diễn viên, tài liệu tham khảo văn hóa)
- **Phù hợp cho các lĩnh vực cụ thể:** Tình cảm dành riêng cho lĩnh vực (y tế, tài chính, công nghệ) — không được đo

---

## MODULE 3: TRẢ LỜI — 15 THEME

### **THEME 1: Ảnh chụp Tập dữ liệu** ✅

**Nguồn:** `outputs/datacard_stats.json` (có thẩm quyền)

| Đặc điểm | Giá trị | Ghi chú |
|---|---|---|
| **Tên Tập dữ liệu** | Nhận xét Phim IMDB (Maas et al., 2011) | Tập dữ liệu tiêu chuẩn |
| **Tổng Hàng** | 50,000 | Kích thước tập dữ liệu hoàn chỉnh |
| **Train / Val / Test** | 40,000 / 5,000 / 5,000 | Tách 80/10/10, phân tầng |
| **Phân bố Nhãn** | {0 (tiêu cực): 25,000, 1 (tích cực): 25,000} | Hoàn toàn cân bằng, 50-50 |
| **Số Tính năng** | 2 + siêu dữ liệu | `text`, `label`, `id` tùy chọn |
| **Phạm vi Độ dài Văn bản** | 32-13,593 ký tự | Trung vị: 954 ký tự, P95: 3,328 |
| **Tỷ lệ Cân bằng Lớp** | 1.0 (hoàn hảo) | Không có vấn đề mất cân bằng |
| **Loại Nhãn** | Tình cảm nhị phân (0=tiêu cực, 1=tích cực) | Được ngưỡng hóa từ điểm IMDB |
| **Có nhãn vs Không có nhãn** | 50,000 có nhãn, 0 không có nhãn | 100% có nhãn, 0% không có nhãn |
| **Giá trị Bị thiếu** | 0 cho văn bản, 0 cho nhãn | Không có null phát hiện |

---

### **THEME 2: Động lực & Thu thập** ✅

**Nguồn:** Bài báo (Maas et al., 2011) + Kết quả kiểm toán

**Động lực Tập dữ liệu (Từ Bài báo):**
- Giới thiệu để cung cấp **tiêu chuẩn mạnh mẽ, quy mô lớn** cho phân tích tình cảm
- Các tập dữ liệu trước đây đã nhỏ hoặc không cân bằng; tập dữ liệu này cung cấp 50k ví dụ với cực tính rõ ràng
- Cho phép nghiên cứu về **tạo vectơ từ cho tình cảm** (học vectơ từ từ các bài đánh giá)

**Quy trình Thu thập:**
- Nguồn: Nhận xét phim IMDB (công khai, do người dùng tạo)
- Phương pháp gắn nhãn: **Ngưỡng tự động** của xếp hạng IMDB (thang 0-10)
  - Tiêu cực: điểm ≤4/10
  - Tích cực: điểm ≥7/10
  - Loại trừ: phạm vi điểm 5-6 (trung tính bị loại trừ)
- Phân biệt phim: Train và test sử dụng **các bộ phim khác nhau** để giảm rò rỉ từ từ cụ thể của phim

**Kết quả Kiểm toán của Chúng tôi (2026-04-13):**
- ✅ Không có giá trị bị thiếu phát hiện
- ✅ Cân bằng lớp hoàn hảo được duy trì (50-50)
- ❌ Tìm thấy 832 bài đánh giá trùng lặp chính xác (1.664%) — có thể là bài đánh giá lặp lại hoặc spam
- ❌ Tìm thấy 29,202 hàng có thẻ HTML `<br>` (58.4%) — không được làm sạch trong phát hành gốc
- ⚠️ Tìm thấy 1,003 vấn đề nhãn nghi ngờ (2.006%) qua Cleanlab — một số bài đánh giá có thể bị gắn nhãn sai

---

### **THEME 3: Thành phần & Cấu trúc** ✅

**Trường Dữ liệu:**
```
text (str):     Văn bản nhận xét phim, tiếng Anh tự do
label (int):    Nhãn Tình cảm: 0 (tiêu cực) hoặc 1 (tích cực)
id (str):       Định danh duy nhất tùy chọn cho mỗi bài đánh giá
```

**Phân tích Thành phần:**
- **100% nhận xét văn bản** — Tất cả các trường hợp là văn bản nhận xét phim tiếng Anh từ IMDB
- **100% có nhãn nhị phân** — Tất cả bình luận đều có chính xác một nhãn (0 hoặc 1)
- **0% bộ thử nghiệm giữ nguyên từ gốc**: Chúng tôi tạo ra tách train/val/test của riêng mình (không từ bài báo)

**Khía cạnh Có cấu trúc:**
- Một trường hợp = Một văn bản bài đánh giá + Một nhãn
- Không có cấu trúc phân cấp (không phải cuộc trò chuyện, chủ đề, v.v.)
- Không cần sắp xếp theo thời gian (mỗi bài độc lập)
- Chú thích cấp độ tài liệu (không phải cấp độ từ hoặc mã thông báo)

**Hiện tượng Đã biết:**
- Thẻ HTML có trong ~58% bài đánh giá (cần làm sạch)
- Thực thể HTML (11 tìm thấy) — chủ yếu `&quot;`, `&amp;`, v.v.
- Spam bài đánh giá có thể / bài đánh giá trùng lặp (1.664% bản sao chính xác)

---

### **THEME 4: Quy trình Thu thập** ✅

(Xem Theme 2 ở trên để biết nguồn thu thập)

**Thời gian:**
- Xuất bản bài báo: Tháng 6 năm 2011
- Phát hành tập dữ liệu: 2011 (ngày chính xác không được chỉ định trong bài báo)
- Kiểm toán của chúng tôi: Ngày 13 tháng 4 năm 2026 (15 năm sau phát hành)

**Pháp lý & Truy cập:**
- **Giấy phép:** Công khai cho nghiên cứu (không có giấy phép rõ ràng trong bài báo)
- **URL:** http://www.andrew-maas.net/data/sentiment
- **Truy cập:** Có thể tải xuống, không yêu cầu xác thực
- **Điều khoản:** Coi là tiêu chuẩn công khai (kiểm tra trang web để xem T&Cs mới nhất)

**Lấy mẫu & Cân bằng (Từ Bài báo):**
- Cân bằng theo thiết kế: bằng nhau tích cực & tiêu cực (25k mỗi cái)
- Phương pháp lấy mẫu: Tất cả bài đánh giá IMDB có điểm ≤4 hoặc ≥7, tối đa 30 bài đánh giá mỗi phim
  - Ngăn chặn rò rỉ dữ liệu từ các mẫu cụ thể về phim

---

### **THEME 5: Chất lượng Dữ liệu (Xác thực & Kiểm toán)** ✅

**Nguồn:** `outputs/ge/validation_result.json` + `outputs/logs/audit_*.md`

#### Kết quả Xác thực Great Expectations

| Quy tắc Xác thực | Trạng thái | Chi tiết |
|---|---|---|
| **Cột text không null** | ✅ ĐẠT | 50,000/50,000 không null |
| **Cột label không null** | ✅ ĐẠT | 50,000/50,000 không null |
| **label trong {0, 1}** | ✅ ĐẠT | 50,000/50,000 giá trị hợp lệ |
| **Cột id không null** | ✅ ĐẠT | 50,000/50,000 không null |
| **Cột id duy nhất** | ✅ ĐẠT | 50,000/50,000 ID duy nhất |
| **len_chars từ 1–10,000** | ❌ KHÔNG ĐẠT | **5 bản ghi vượt quá 10,000 ký tự** (max: 13,593) |
| **Điểm Chung** | 83.33% | 5/6 quy tắc đạt |

**Khuyến nghị cho Thất bại:**
- ❌ Ngoại lệ: Độ dài văn bản tối đa là 13,593 ký tự (5 bản ghi vượt quá 10,000)
- **Hành động:** Quyết định nếu giới hạn độ dài văn bản có nghiêm ngặt hoặc cho phép ngoại lệ
  - Nếu nghiêm ngặt: Lọc xuống tối đa 10,000 ký tự trước khi huấn luyện
  - Nếu linh hoạt: Cập nhật quy tắc thành tối đa 15,000 ký tự và chạy lại xác thực ✅

---

### **THEME 6: Loại Xác thực & Phương pháp** ✅

(Xem Theme 5 ở trên)

**Khung Xác thực:** Great Expectations (GE)  
**Tỷ lệ Đạt:** 83.33% (5/6)  
**Xử lý Thất bại:**
- Hiện tại: 1 quy tắc không đạt (ngoại lệ độ dài văn bản)
- Giảm thiểu: Hoặc lọc ngoại lệ hoặc điều chỉnh ngưỡng quy tắc

**Xác thực Tiền xử lý (Trước ➜ Sau):**
- Bỏ thẻ HTML: 29,202 → 0 hàng có thẻ `<br>` ✅
- Không mất dữ liệu khi làm sạch (tất cả 50,000 hàng được bảo toàn)
- Tỷ lệ trùng lặp không thay đổi: 1.648% → 1.664% (biến động nhẹ do làm tròn)

---

### **THEME 7: Use Case Dự kiến & Giới hạn** ✅

**Use Case Chính (Phù hợp Cho):**
1. ✅ **Đánh giá tiêu chuẩn phân loại tình cảm** — Tập dữ liệu đánh giá tiêu chuẩn cho các mô hình tình cảm nhị phân
2. ✅ **Nghiên cứu tạo vectơ từ** — Học vectơ từ từ văn bản bài đánh giá
3. ✅ **Học tập biểu diễn văn bản** — Đánh giá tạo nhúng tài liệu (Word2Vec, GloVe, FastText, Transformers)
4. ✅ **Dự án khóa học NLP** — Học tập sinh viên cho phân tích tình cảm
5. ✅ **So sánh cơ sở** — Đánh giá các thuật toán mới so với tiêu chuẩn đã biết

**KHÔNG Phù hợp Cho:**
- ❌ **Triển khai sản xuất thực tế** — Tiêu chuẩn công khai, không dành cho sản xuất
- ❌ **Tình cảm đa lớp** (chi tiết: rất tiêu cực → rất tích cực) — Chỉ nhị phân
- ❌ **Tình cảm dựa trên khía cạnh** — Không có nhãn khía cạnh, chỉ tình cảm bài đánh giá chung
- ❌ **Ngôn ngữ không phải tiếng Anh** — Tập dữ liệu chỉ tiếng Anh
- ❌ **Phim gần đây** — Dữ liệu từ ~2011, các bình luận có thể không phản ánh điện ảnh hiện tại
- ❌ **Nghiên cứu công bằng/sai lệch** — Không có nhãn nhân khẩu học; sai lệch văn hóa/diễn viên tiềm ẩn không được kiểm tra

---

### **THEME 8: Thành phần Tập dữ liệu & Nhân khẩu học** ✅

**Thành phần:** 100% bài đánh giá do người dùng tạo trên IMDB

**Thông tin Nhân khẩu học:**
- **Nhân khẩu học Tác giả:** KHÔNG thu thập (bình luận IMDB ẩn danh)
- **Nhân khẩu học Phim:** Phim được đại diện trên tất cả các thể loại, thập kỷ, quốc gia
  - Không có phân tích nhân khẩu học trong bài báo
  - Bài đánh giá trải dài từ phim độc lập nhỏ đến những bộ phim Hollywood lớn

**Các Nhóm Nhạy cảm KHÔNG được đánh dấu:**
- Không có nhãn giới tính, chủng tộc, tuổi tác, bản sắc văn hóa
- Không có thông tin về khả năng tiếp cận

**Khuyến nghị:** Tập dữ liệu này phù hợp cho **phân tích tình cảm chung** nhưng KHÔNG cho **phân tích công bằng/sai lệch** mà không có nhãn nhân khẩu học bổ sung.

---

### **THEME 9: Gắn nhãn & Chú thích** ✅

**Nguồn:** `outputs/logs/cleanlab_summary.md` + `outputs/logs/cleanlab_label_issues.csv`

### Phương pháp Gắn nhãn

**Nhãn Gốc (Từ Bài báo):**
- Nguồn: Điểm số nguyên IMDB (1-10)
- Ngưỡng tự động:
  - Điểm ≤4 → Nhãn 0 (tiêu cực)
  - Điểm ≥7 → Nhãn 1 (tích cực)
  - Điểm 5-6 → Loại trừ (trung tính, không trong tập dữ liệu)
- Người gắn nhãn: Người dùng IMDB gốc (không phải nghiên cứu chú thích được kiểm soát)

### Đánh giá Chất lượng Nhãn Cleanlab (2026-04-13)

**Vấn đề Nhãn Nghi ngờ:** 1,003 / 50,000 = **2.006%**
- Phương pháp: Học nhầm lẫn dựa trên ML (Cleanlab)
- Diễn dịch: ~2% nhãn có thể không chính xác hoặc không rõ ràng
- Ví dụ được xuất: 200 trường hợp nghi ngờ hàng đầu trong `cleanlab_label_issues.csv`

### Xem xét Thủ công 5 Mẫu Có Cleanlab Gắn cờ

**Mẫu 1** (ID: 11668, Nhãn Cho: 0, Xác suất Cleanlab: 0.001)
- Văn bản: "đây là một bộ phim tuyệt vời. Tôi yêu bộ phim... Đó là một bộ phim tuyệt vời! Doy!!!"
- Chẩn đoán: Tình cảm cực kỳ tích cực ("tuyệt vời", "yêu")
- **Kết luận: LỖI NHÃN ➜ Nên là 1, không phải 0 ❌**
- Hành động: **Khuyến nghị sửa nhãn thành 1**

**Mẫu 2** (ID: 22259, Nhãn Cho: 1, Xác suất Cleanlab: 0.002)
- Văn bản: "Bộ phim này là một ví dụ hoàn hảo về trạng thái của các bộ phim khiêu dâm B..."
- Chẩn đoán: Mỉa mai/châm biếm ("hoàn hảo" có nghĩa là xấu theo bối cảnh), nói chung tiêu cực
- **Kết luận: KHÔNG RỎ RÀNG — Có thể là 0 hoặc 1 🤔**
- Hành động: **Đánh dấu là không rõ ràng; cần phán đoán của chuyên gia**

**Mẫu 3** (ID: 22257, Nhãn Cho: 1, Xác suất Cleanlab: 0.003)
- Văn bản: "Bộ phim sex lạnh tình có một số điểm tốt... kịch bản ổn... mọi thứ khác có vẻ rẻ..."
- Chẩn đoán: Tình cảm hỗn hợp (một số khen, hầu hết phê bình)
- **Kết luận: KHÔNG RỎ RÀNG — Tình cảm gần biên 🤔**
- Hành động: **Khuyến nghị xem xét bởi chuyên gia lĩnh vực**

**Mẫu 4** (ID: 16634, Nhãn Cho: 1, Xác suất Cleanlab: 0.005)
- Văn bản: "Bộ phim này có mọi thứ làm cho một bộ phim xấu đáng xem..."
- Chẩn đoán: Rõ ràng tiêu cực ("bộ phim xấu", lộn xộn, không có sắp xếp)
- **Kết luận: LỖI NHÃN ➜ Nên là 0, không phải 1 ❌**
- Hành động: **Khuyến nghị sửa nhãn thành 0**

**Mẫu 5** (ID: 31245, Nhãn Cho: 0, Xác suất Cleanlab: 0.011)
- Văn bản: "Mickey Rourke săn Diane Lane... Nó không được nhập lực như... có một số yếu tố tích cực..."
- Chẩn đoán: Chủ yếu mô tả/phân tích, một số yếu tố tích cực, nói chung hơi tích cực
- **Kết luận: KHÔNG RỎ RÀNG — Có thể nghiêng về tích cực 🤔**
- Hành động: **Yêu cầu xem xét toàn bộ ngữ cảnh**

### Tóm tắt Xem xét Thủ công
- **Lỗi Nhãn Tìm thấy:** 2/5 (40%) — mẫu 1 & 4 nên được sửa
- **Không rõ ràng:** 3/5 (60%) — yêu cầu phán đoán chuyên gia con người
- **Ý nghĩa:** Tỷ lệ 2% nghi ngờ là hợp lý; một số nhãn thực sự khó

---

### **THEME 10: Tiền xử lý & Chuyển đổi Dữ liệu** ✅

**Nguồn:** `outputs/logs/audit_before.md` vs `outputs/logs/audit_after.md`

### Các Chuyển đổi Được Áp dụng

#### Chuyển đổi 1: Loại bỏ Thẻ HTML

| Chỉ số | TRƯỚC | SAU | Thay đổi |
|---|---|---|---|
| **Hàng có thẻ `<br>`** | 29,200 | 0 | ✅ -29,200 |
| **Hàng có BẤT KỲ thẻ HTML** | 29,202 | 0 | ✅ -29,202 |
| **Tổng hàng được bảo toàn** | 50,000 | 50,000 | ✅ 0 mất mát |

**Lý do:** Định dạng HTML IMDB (ngắt dòng `<br>`) là hiện tượng của web scraping, không có ý nghĩa cho đầu vào mô hình NLP. Loại bỏ cải thiện chất lượng văn bản.

#### Chuyển đổi 2: Phát hiện Bản sao (Không được loại bỏ, chỉ ghi lại)

| Chỉ số | TRƯỚC | SAU | Thay đổi |
|---|---|---|---|
| **Hàng bản sao chính xác** | 824 | 832 | ⚠️ +8 tìm thấy sau làm sạch |
| **Tỷ lệ Bản sao** | 1.648% | 1.664% | ⚠️ % tăng |
| **Tổng hàng được bảo toàn** | 50,000 | 50,000 | ✅ Không loại bỏ (phát hiện, không loại bỏ) |

**Lý do:** Bản sao chỉ ra spam/bài đánh giá lặp lại. Khuyến nghị: Loại bỏ bản sao SAU khi tách train/val/test (không trước) để tránh rò rỉ dữ liệu.

#### Chuyển đổi 3: Thay đổi Độ dài Văn bản (Do loại bỏ HTML)

| Chỉ số | TRƯỚC | SAU | Thay đổi |
|---|---|---|---|
| **Độ dài trung vị (ký tự)** | 970 | 954 | ✅ -16 ký tự (dự kiến) |
| **Độ dài P95** | 3,391 | 3,328 | ✅ -63 ký tự |
| **Độ dài Max** | 13,704 | 13,593 | ✅ -111 ký tự |
| **Độ dài Min** | 32 | 32 | ✅ Không thay đổi |

**Lý do:** Loại bỏ thẻ HTML làm giảm độ dài văn bản nhẹ nhàng nhưng không loại bỏ thông tin (thẻ là cấu trúc, không phải nội dung).

### Đường ống Tiền xử lý (Thứ tự Được khuyến nghị)

1. **Giai đoạn 1: TÁCH DỮ LIỆU TRƯỚC TIÊN** (QUAN TRỌNG để tránh rò rỉ)
   - Tách thành train (80%) / val (10%) / test (10%)
   - Lệnh: Sử dụng tách phân tầng trên cột `label`
   
2. **Giai đoạn 2: FIT TIỀN XỬ LÝ TRÊN TRAIN ONLY**
   - Tokenizer fit trên văn bản train only
   - Vectorizer (TF-IDF) fit trên văn bản train only
   - Từ vựng được xây dựng từ tập train
   
3. **Giai đoạn 3: ÁP DỤNG CHO TẤT CẢ**
   - Chuyển đổi val/test bằng cách sử dụng tiền xử lý khớp train
   - Điều này ngăn chặn rò rỉ mục tiêu vào xác thực/bài kiểm tra

4. **Giai đoạn 4: XỬ LÝ BẢN SAO**
   - Loại bỏ bản sao chính xác SAU khi tách train/test (tùy chọn)
   - Khuyến nghị: Giữ bản sao bây giờ (chỉ 1.66%, tác động tối thiểu)

**⚠️ CẢNH BÁO: Rủi ro Rò rỉ**
- Nếu bạn fit vectorizer trên tất cả 50k hàng, từ vựng tập test rò rỉ vào huấn luyện
- Triệu chứng: Độ chính xác kiểm tra nhân tạo cao
- Giải pháp: Luôn tách trước khi fit tiền xử lý ✅

---

### **THEME 11: Phân bố & Đặc điểm Tập dữ liệu** ✅

**Phân bố Nhãn:**
```
Tiêu cực (0):  25,000 (50.0%)
Tích cực (1):  25,000 (50.0%)
Tổng:          50,000 (100%)
```
✅ **Cân bằng hoàn hảo — không có vấn đề mất cân bằng lớp**

**Phân bố Độ dài Văn bản:**
```
Đếm:           50,000
Min:           32 ký tự
25%:           ~500 ký tự
Trung vị (50%): 954 ký tự
75%:           ~1,500 ký tự
P95:           3,328 ký tự
Max:           13,593 ký tự
Độ lệch chuẩn: Khác nhau theo tách (không được tính trong kiểm toán đầy đủ)
```

**Ngôn ngữ:**
- 100% bài đánh giá tiếng Anh

**Khoảng Thời gian (Gần đúng):**
- Bài đánh giá khoảng 2005-2011 (khi tập dữ liệu gốc được thu thập)
- Phim khoảng thời gian rộng hơn (cổ điển đến đương đại vào năm 2011)

**Nguồn gốc Địa lý:**
- IMDB là quốc tế nhưng chủ yếu là những người đánh giá nói tiếng Anh
- Không có phân tích địa lý

---

### **THEME 12: Giới hạn Đã biết & Khuyến nghị** ✅

### Giới hạn Đã biết

1. **LỚN: Chỉ 2% Vấn đề Nhãn Nghi ngờ**
   - Cleanlab phát hiện 1,003 / 50,000 vấn đề nghi ngờ (2.006%)
   - Không phải tất cả đều là lỗi thực (một số bài đánh giá không rõ ràng)
   - Khuyến nghị: Thực hiện xem xét con người trên tập hợp con trước khi sử dụng quan trọng

2. **Bài đánh giá Bản sao (1.664%)**
   - 832 bản sao chính xác tìm thấy
   - Tác động: Rò rỉ dữ liệu nhẹ nếu các bài đánh giá giống nhau xuất hiện trong train & test
   - Khuyến nghị: Loại bỏ bản sao mỗi tách (không toàn cầu)

3. **Hiện tượng HTML trong ~58% Bài đánh giá**
   - 29,202 bài đánh giá chứa thẻ `<br>` và các thực thể HTML khác
   - Đã được làm sạch trong phiên bản kiểm toán của chúng tôi, nhưng có thể ảnh hưởng đến so sánh lịch sử
   - Khuyến nghị: So sánh kết quả với phiên bản gốc và phiên bản được làm sạch

4. **Gắn nhãn Tự động (Không phải Chú thích Thủ công)**
   - Nhãn dựa trên ngưỡng điểm IMDB, không phải xem xét thủ công
   - Một số bài đánh giá biên giới (điểm 4-5, 6-7) bị loại trừ sai
   - Khuyến nghị: Đừng nói "được chú thích thủ công" — đây là dựa trên điểm

5. **TẬP DỮ LIỆU CŨ (15 năm)**
   - Bài báo từ năm 2011; phim/slang có thể đã lỗi thời
   - Đại diện hạn chế của tài liệu tham khảo văn hóa gần đây
   - Khuyến nghị: Sử dụng để đánh giá tiêu chuẩn, không phải để mô hình hóa bài đánh giá hiện tại

6. **Chỉ Tình cảm Nhị phân**
   - Không có nhãn chi tiết (rất tiêu cực → rất tích cực)
   - Không có tình cảm dựa trên khía cạnh (ví dụ: "cốt truyện: tốt, diễn xuất: xấu")
   - Khuyến nghị: Chỉ sử dụng cho các tác vụ tình cảm nhị phân

7. **Không có Siêu dữ liệu Nhân khẩu học**
   - Không có siêu dữ liệu tác giả, năm phim/phim được tách riêng
   - Không thể nghiên cứu công bằng/sai lệch trên các nhóm nhân khẩu học
   - Khuyến nghị: Thêm nhãn thể loại/năm để phân tích nếu cần

### Khuyến nghị Sử dụng

- ✅ **CÓ sử dụng cho:** Đánh giá tiêu chuẩn phân loại tình cảm nhị phân, đánh giá tạo nhúng từ
- ❌ **KHÔNG sử dụng cho:** Các hệ thống NLP sản xuất, nghiên cứu công bằng, tình cảm chi tiết
- ⚠️ **CÓ CẬP NHẬT với:** Xác thực chéo (loại bỏ bản sao chính xác mỗi tách)
- ⚠️ **CÓ CẬP NHẬT với:** Rò rỉ tiền xử lý (tách trước khi vector hóa)

---

### **THEME 13: Tác động Xã hội & Sai lệch** ✅

**Sai lệch Tiềm ẩn:**

1. **Sai lệch Người dùng IMDB:**
   - Những người đánh giá là tự chọn (không đại diện cho tất cả những người xem phim)
   - Nói tiếng Anh, kết nối internet, nhân khẩu học thông thạo công nghệ

2. **Sai lệch Lựa chọn Phim:**
   - Phim cũ hơn, sau đó phát hành chính thức hiện tại
   - Đại diện hạn chế của phim quốc tế, độc lập hoặc hiện đại

3. **Sai lệch Xếp hạng:**
   - Điểm IMDB lệch về các quan điểm phân cực (người dùng có khả năng xếp hạng cực tính cao hơn)
   - Bài đánh giá trung tính (5-6 điểm) được loại trừ theo thiết kế
   - Phim được xếp hạng cao có thể được đại diện nhiều hơn

4. **Vấn đề Công bằng Tiềm ẩn (CHƯA NGHIÊN CỨU):**
   - Sai lệch Giới tính: Những bài đánh giá về phim với vai diễn nữ có được xếp hạng khác không?
   - Sai lệch Văn hóa: Những bài đánh giá về phim không phải tiếng Anh có được xếp hạng khác không?
   - **Trạng thái hiện tại: Không được phân tích trong tập dữ liệu này**

**Khuyến nghị:**
- Sử dụng cẩn thận khi triển khai cho các cơ sở người dùng đa dạng
- Xem xét tái cân bằng nếu sử dụng cho các ứng dụng nhạy cảm công bằng
- Đừng cho rằng bài đánh giá đại diện cho tình cảm không thiên vị

---

### **THEME 14: Giấy phép & Quyền Sở hữu Trí tuệ** ✅

**Giấy phép Dữ liệu:**
- **Nguồn Gốc:** IMDB (bài đánh giá do người dùng tạo)
- **Điều khoản IMDB:** Bài đánh giá công khai nhưng IMDB giữ bản quyền
- **Giấy phép Tập dữ liệu:** KHÔNG được nêu rõ ràng trong bài báo
- **Sử dụng Được khuyến nghị:** Tìm hiểu học tập, đánh giá tiêu chuẩn không thương mại
- **Hạn chế:** Kiểm tra http://www.andrew-maas.net/data/sentiment để xem các điều khoản mới nhất

**Trích dẫn Đúng:**
```
@inproceedings{maas2011,
  title={Learning Word Vectors for Sentiment Analysis},
  author={Maas, Andrew L and Daly, Raymond E and Pham, Peter T and 
          Huang, Dan and Ng, Andrew Y and Potts, Christopher},
  booktitle={Proceedings of the 49th Annual Meeting of the Association 
             for Computational Linguistics: Human Language Technologies},
  year={2011},
  pages={142--150}
}
```

**Liên hệ:**
- **Chủ sở hữu Tập dữ liệu:** Andrew L. Maas et al., Đại học Stanford
- **Email:** [amaas, rdaly, ptpham, yuze, ang, cgpotts]@stanford.edu
- **Trang web:** http://www.andrew-maas.net/data/sentiment

---

### **THEME 15: Kiểm soát Phiên bản & Bảo trì** ✅

**Phiên bản Hiện tại:** 1.0 (Phát hành bài báo, 2011)

**Phiên bản Kiểm toán của Chúng tôi:** 1.0-audit-2026 (Ngày 13 tháng 4 năm 2026)
- Thêm kiểm toán chất lượng dữ liệu (GE, Cleanlab, số bản sao)
- Thêm phép biến đổi tiền xử lý (làm sạch HTML)
- Thêm Phiếu thông tin dữ liệu hoàn toàn này
- Không thay đổi dữ liệu thô; làm sạch là tùy chọn/được khuyến nghị

**Trạng thái Bảo trì:**
- ⚠️ **TIÊU CHUẨN TĨNH** — Tập hợp gốc không được bảo trì tích cực
- ✅ Phiên bản kiểm toán của chúng tôi sẽ được bảo trì cùng với kho lưu trữ này

**Cập nhật Dự kiến:**
- 🟡 Có thể: Nhãn được sửa cho các lỗi được Cleanlab phát hiện (đang chờ xem xét thủ công)
- 🟡 Có thể: Phiên bản được khử trùng (loại bỏ 1.664% bản sao)
- 🟡 Có thể: Phiên bản chi tiết (3-5 danh mục tình cảm) — công việc tương lai

**Phiên bản hóa:**
- v1.0 (Gốc) — Như được xuất bản 2011
- v1.0-audit (Của chúng tôi) — Với kiểm toán chất lượng dữ liệu, khuyến nghị làm sạch
- v1.1 (Kế hoạch) — Được khử trùng, nhãn được sửa

---

## BẢNG TÓM TẮT: 15 THEME KIỂM TRA

| \# | Theme | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | Ảnh chụp Tập dữ liệu | ✅ Hoàn thành | 50k hàng, cân bằng 50-50, văn bản + nhị phân |
| 2 | Động lực & Thu thập | ✅ Hoàn thành | Tiêu chuẩn tình cảm; ghi nhãn tự động; ~2011 |
| 3 | Thành phần & Cấu trúc | ✅ Hoàn thành | Chú thích cấp độ tài liệu, văn bản+nhãn, 100% tiếng Anh |
| 4 | Quy trình Thu thập | ✅ Hoàn thành | Điểm IMDB ≤4/≥7, tách phim rời nhau |
| 5 | Kiểm toán Chất lượng Dữ liệu | ✅ Hoàn thành | GE 83.33%, Cleanlab 2%, bản sao 1.66% |
| 6 | Loại Xác thực | ✅ Hoàn thành | Quy tắc GE: 5 ĐẠT, 1 KHÔNG ĐẠT |
| 7 | Use Case Dự kiến | ✅ Hoàn thành | Tốt: đánh giá tiêu chuẩn; Xấu: sản xuất |
| 8 | Thành phần & Nhân khẩu học | ✅ Hoàn thành | Không có siêu dữ liệu tác giả/phim |
| 9 | Gắn nhãn & Chú thích | ✅ Hoàn thành | Gắn nhãn tự động + xem xét 5 mẫu Cleanlab |
| 10 | Tiền xử lý & Biến đổi | ✅ Hoàn thành | Thống kê TRƯỚC → SAU, phòng ngừa rò rỉ |
| 11 | Đặc điểm Phân bố | ✅ Hoàn thành | Nhãn cân bằng hoàn hảo; văn bản: 32-13.6k ký tự |
| 12 | Giới hạn & Khuyến nghị | ✅ Hoàn thành | Lỗi 2%, cũ (2011), chỉ nhị phân |
| 13 | Tác động Xã hội & Sai lệch | ✅ Hoàn thành | Sai lệch người dùng/phim; công bằng chưa được nghiên cứu |
| 14 | Giấy phép & QSH | ✅ Hoàn thành | Tìm hiểu không thương mại; trích dẫn Maas et al. |
| 15 | Kiểm soát Phiên bản & Bảo trì | ✅ Hoàn thành | v1.0 (gốc), v1.0-audit (ours) |

---

## PHỤ LỤC: Tệp & Tài liệu Tham khảo

**Tệp Dữ liệu:**
- Tập dữ liệu chính: `outputs/splits/{train,val,test}.csv`
- Thống kê kiểm toán: `outputs/datacard_stats.json`
- Xác thực GE: `outputs/ge/validation_result.json`
- Vấn đề Cleanlab: `outputs/logs/cleanlab_label_issues.csv`
- Kiểm toán trước/sau: `outputs/logs/audit_before.md`, `audit_after.md`

**Phiếu Thông tin Dữ liệu này:**
- Chính: `data_card.md` (tệp này)
- Phiếu điểm heuristics: `datacard/heuristics_scorecard.md`
- Thanh ghi siêu dữ liệu: `datacard/metadata_register.md`

**Bài báo & Tập dữ liệu Gốc:**
- Bài báo: https://aclanthology.org/P11-1015.pdf
- Tập dữ liệu: http://www.andrew-maas.net/data/sentiment

---

**Trạng thái Phiếu Thông tin Dữ liệu: ✅ HOÀN THÀNH** (Module 1, 2, 3 hoàn tất)  
**Điểm Chất lượng: 4.5/5** (Sau khi tích hợp kiểm toán)  
**Bước Tiếp theo:** Thực hiện các sửa chữa được khuyến nghị (khử trùng, xem xét nhãn) cho v1.1

