# Cleanlab — Label Issues Summary
- suspected_label_issues_count: 1003
- suspected_label_issues_ratio: 0.0201
- export_top_k: 200

Student task: chọn 5 mẫu trong cleanlab_label_issues.csv để review (giữ/sửa/ambiguous) và ghi vào Data Card.
Sample 1
Text (rút gọn): “this is a great movie. I love the series...”
Label hiện tại: 0
Nhận xét: Nội dung thể hiện cảm xúc rất tích cực (“great”, “love”)
Kết luận: Sửa nhãn → 1
Sample 2
Text (rút gọn): “This flick is sterling example of the state of...”
Label hiện tại: 1
Nhận xét: Câu mang sắc thái mỉa mai/tiêu cực, cụm “sterling example” thường dùng theo nghĩa châm biếm
Kết luận: Ambiguous (có thể là negative nhưng cần đọc full context)
Sample 3
Text (rút gọn): “This low-budget erotic thriller that has some ...”
Label hiện tại: 1
Nhận xét: Nội dung chưa rõ ràng, có thể vừa khen vừa chê
Kết luận: Ambiguous
Sample 4
Text (rút gọn): “This movie has everything that makes a bad movie...”
Label hiện tại: 1
Nhận xét: Rõ ràng là tiêu cực (“bad movie”)
Kết luận: Sửa nhãn → 0
Sample 5
Text (rút gọn): “Mickey Rourke hunts Diane Lane in ...”
Label hiện tại: 0
Nhận xét: Nội dung mang tính mô tả, không rõ sentiment
Kết luận: Ambiguous