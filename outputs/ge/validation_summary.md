# Great Expectations — Validation Summary
- evaluated_expectations: 6
- successful_expectations: 5
- unsuccessful_expectations: 1
- success_percent: 83.33333333333334

Nếu FAIL: ghi nguyên nhân và kế hoạch xử lý trong Data Card (Validation Types).
FAIL
Nguyên nhân FAIL:
Có 1 expectation không đạt
Cách xử lý 
Rà soát expectation bị fail
Nếu là duplicate:
Loại bỏ hoàn toàn trước khi train
Nếu là format/text issue:
Bổ sung bước preprocessing 
Chạy lại validation sau khi fix để đảm bảo 100% PASS