# Báo cáo kiểm thử và hiệu năng — bản chuẩn bị PC-2

## 1. Phạm vi và mốc

- Nhóm `173820_n27`; bản lập 17/11/2026. Báo cáo mô tả bằng chứng tại từng SHA, chưa phải nghiệm thu bài cuối hay phán quyết của GV/TA.
- Code/test đã tích hợp: [merge PR #7](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/commit/925cea676f4962f9318e7dc35aad6017d4f80bd3). Bản PC-2 dự kiến [`7a8da1e9697359fbb884c911d744129c1a752455`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/commit/7a8da1e9697359fbb884c911d744129c1a752455) đi tiếp từ commit đó và chỉ đổi `progress/PROGRESS.md`.
- Nguồn thiết kế: [Decision Table](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/docs/decision-table.md), [Test Plan](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/docs/testing/TEST_PLAN.md), [Integration Test Plan](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/docs/testing/INTEGRATION_TEST_PLAN.md), [QA Matrix](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/docs/qa/QA_MATRIX.xlsx) và [CSV ca](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/TEST_CASES.csv).

## 2. Kết quả đã ghi nhận

| Loại kiểm tra | Môi trường, lệnh và nguồn | Kết quả / giới hạn |
|---|---|---|
| CI sau cập nhật tiến độ | GitHub Actions Linux, Python 3.12.16, `pytest -q`; [run 44114089165](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44114089165), commit `7a8da1e` | **490 passed, 3 skipped**. Ba test bỏ qua yêu cầu môi trường Windows. CI còn có cảnh báo action Node.js; không đánh dấu ba test đó đã đạt trên Windows. |
| CI trên PR nội bộ | [PR #7, run 44106490534](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44106490534), HEAD `59f5ac4d77fed04aabc59dc1ed92bd706533d203` | Workflow chạy thành công trong ngữ cảnh PR; bằng chứng riêng cho A-09. |
| Chạy lại trong môi trường làm việc | Linux, Python 3.12.14, pytest 9.2.0; checkout tách tại `925cea6`; `PYTHONPATH=<thư mục chứa pytest đã có> python -m pytest -q` | **490 passed, 3 skipped, 1 warning**. Không trình bày đây là clean checkout với môi trường cài mới vì pytest được lấy từ thư mục phụ thuộc sẵn có. |
| Checkout sạch trước đó | Theo [báo cáo PR #7](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/pull/7), commit `0d00584`, Python 3.12.14 | 483 test đạt ở mốc đó; không thay cho chạy lại trên SHA freeze. |
| Audit và lỗi | [tests/test_audit.py](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/tests/test_audit.py); 61 test audit đạt ở commit `274b3bf` theo [báo cáo benchmark sơ bộ](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/docs/testing/BENCHMARK_REPORT.md) | Kiểm tra ban đầu trong phạm vi test: schema, session UUID, lỗi input/nội bộ/audit, ưu tiên lỗi, canary privacy, file ghi dở và query dòng lỗi. Không suy rộng thành không có mọi lỗi privacy. |
| Chạy thử các lệnh demo | Checkout `925cea6`, Python 3.12.14, audit tạm ngoài repo, dùng dữ liệu synthetic. | `RULE-021` trả `SOC_PHAN_VE/R-SOC`; `MISS-009` trả `PHAN_VE/R01-PV`; `INV-005` báo `ValueError` dự kiến. Công cụ query đọc ba record, không bỏ qua dòng nào; status lần lượt `SUCCESS`, `SUCCESS`, `INVALID_INPUT`. |

Các ca kiểm thử có tham số tạo ra nhiều mục pytest. Tổng số `passed` không đồng nghĩa số yêu cầu độc lập đã nghiệm thu.

## 3. Nội dung bộ kiểm thử

- **Phân loại và validation:** [tests/test_classifier.py](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/tests/test_classifier.py) đọc 175 ca chính, kiểm 81 ca logic nội bộ riêng, và có kiểm biên, missing/null, invalid, overlap, 729 tổ hợp boolean trong một test, cùng phản ví dụ/cách diễn đạt tương đương của `ly_do`.
- **Audit và privacy:** [tests/test_audit.py](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/tests/test_audit.py) kiểm ghi `SUCCESS`, `INVALID_INPUT`, `INTERNAL_ERROR`, lỗi kép, message/traceback/log và các đường đọc JSONL lỗi bằng synthetic canary.
- **Công cụ chạy ca:** [tests/test_run_case.py](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/tests/test_run_case.py) kiểm nhập/xuất CSV và lỗi đường dẫn; ba test đặc thù Windows bị skip trong CI Linux.
- **175 file `docs/qa/evidence/`:** là output của các ca do thành viên QA đưa lên, dùng để đối chiếu/truy vết; không tự chứng minh 175 test độc lập hoặc nghiệm thu. So với mã nguồn và kết quả chạy, cần giữ mốc SHA riêng.

## 4. Đối chiếu A-01–A-11

Trạng thái này theo bản progress chuẩn bị PC-2, không thay thế đánh giá cuối:

| AC | Trạng thái | Bằng chứng / việc còn lại |
|---|---|---|
| A-01 | Đang | `classify()` đã triển khai; tiếp tục review contract và SHA cuối. |
| A-02 | Đang | Classifier và fixture runner tách riêng; bổ sung đối chiếu input synthetic độc lập. |
| A-03 | Đang | Có test ba mức; hoàn tất mapping và review. |
| A-04 | Đang | Có chính sách và test missing/null/invalid; đối chiếu SHA cuối. |
| A-05 | Đang | Có test và review lời giải thích; đối chiếu nghiệm thu. |
| A-06 | Đang | Có audit và test lỗi/privacy; giới hạn theo phạm vi đã kiểm. |
| A-07 | Đang | Có pytest nghiệp vụ; hoàn thiện ánh xạ QA và evidence. |
| A-08 | Đang | CI checkout chạy thành công; lần clean checkout độc lập nêu trên ở SHA cũ, cần chạy SHA freeze. |
| A-09 | Xong | Có [CI PR #7](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44106490534); kiểm lại khi chốt freeze. |
| A-10 | Đang | Có benchmark sơ bộ; chưa đối chiếu và lưu mẫu trên SHA freeze. |
| A-11 | Xong | [README](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/README.md) có disclaimer yêu cầu; đối chiếu lại SHA freeze. |

## 5. Benchmark sơ bộ

Theo [BENCHMARK_REPORT.md](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/docs/testing/BENCHMARK_REPORT.md), code/fixture ở `274b3bf`; script cùng nội dung được đưa vào PR sau phép đo. Chạy 100 warm-up, 1.000 lần `classify()` gồm ghi audit thật; nearest-rank. Một lượt kiểm script có **p50 0,070977 ms, p95 0,175715 ms**, thấp hơn 100 ms trên môi trường được báo cáo. Mẫu thô chưa được lưu trong repo và benchmark chưa chạy trên SHA freeze; vì vậy **chưa kết luận A-10 hoàn tất cho bài nộp cuối**.

## 6. Cần hoàn thiện trước Freeze

1. Chạy lại clean checkout, CI và ba test Windows trên SHA dự kiến freeze; ghi rõ hệ điều hành, Python, lệnh, kết quả và skip.
2. Chạy benchmark theo protocol trên SHA freeze, lưu timings và metadata môi trường ngoài repo; kiểm audit và báo cáo p50/p95.
3. Hoàn thiện mapping QA, review ngữ nghĩa `ly_do`, link bằng chứng; xác nhận rõ reviewer và phạm vi.
4. Cập nhật báo cáo này và `source-ref.json` theo SHA/tag/CI cuối. `validate` xanh của PR nộp bài chỉ chứng minh kiểm tra cấu trúc tự động, không chứng minh toàn bộ nghiệp vụ đúng.
