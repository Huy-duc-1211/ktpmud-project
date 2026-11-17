# Kịch bản demo nháp — tối đa 7 phút

Mục tiêu: giải thích Rule Engine chạy được trên dữ liệu synthetic, minh họa kết quả, audit và test. Đây là **kịch bản chuẩn bị**, chưa xác nhận nhóm đã trình diễn hoặc chốt người thuyết trình. Chỉ chạy bằng dữ liệu synthetic.

| Thời gian dự kiến | Nội dung |
|---|---|
| 0:00–0:45 | Giới thiệu phạm vi, Decision Table, hàm `classify(patient)` và cảnh báo educational/synthetic — not for clinical use. |
| 0:45–2:30 | Trong checkout code, chạy `python run_case.py RULE-021` để minh họa mức sốc; chạy `python run_case.py MISS-009` để giải thích mã đại diện khi thiếu dữ liệu; chạy `python run_case.py INV-005` để xem lỗi validation. |
| 2:30–3:30 | Giải thích ba trường output và vì sao `ly_do` ở MISS-009 nói rõ mã đại diện. Không diễn giải kết quả thành tư vấn y tế. |
| 3:30–4:30 | Mở audit JSONL đã chỉ định bằng đường dẫn tuyệt đối ngoài repo; chỉ ra bốn trường, status phù hợp, không có input lâm sàng thô. Có thể chạy `python query_audit.py <đường-dẫn-audit>`. |
| 4:30–5:30 | Trình bày `pytest -q` và link [CI](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44114089165); nêu ba test Windows bị skip trên CI Linux. |
| 5:30–6:30 | Trình bày benchmark sơ bộ và giới hạn: chưa là kết quả SHA freeze; nêu p50/p95 từ [báo cáo](../artifacts/TEST_REPORT.md). |
| 6:30–7:00 | Nêu những việc còn lại, nguồn code và chuẩn bị chuyển sang hỏi đáp. |

Trước khi demo, chạy thử lại từng lệnh và chuẩn bị đường dẫn audit có quyền ghi. Nếu dùng PowerShell, ví dụ: `$env:DR_MAY_DAY_AUDIT_PATH = "$env:TEMP\pc2_demo_audit.jsonl"`; kiểm tra file tồn tại/quyền ghi trước, rồi chạy ba ca. `INV-005` là ca lỗi dự kiến và có thể trả mã thoát khác 0. Trên Windows có thể dùng `run_case.cmd` và nhập mã ca; tham khảo [hướng dẫn](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/runcase.md).

Thành viên cần tự hiểu phần phụ trách để trả lời; không sử dụng AI trong phần vấn đáp trực tiếp theo đề.
