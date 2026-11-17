# Hồ sơ nộp bài — Track A · Dr May Day Rule Engine Sprint

- Nhóm: `173820_n27`, lớp `173820`.
- Mốc hồ sơ: bản chuẩn bị PC-2 ngày 17/11/2026, **chưa phải bản freeze hoặc bài nộp cuối**.
- Fork code: [https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter); commit PC-2 dự kiến: [`d00216053c206435f370c977c51b42d711623861`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/commit/d00216053c206435f370c977c51b42d711623861).
- Starter: [`bkt-2026-v1`](https://github.com/fossbk-spec/ktpmud-bkt-ruleengine-starter/tree/bkt-2026-v1), SHA `1c0a5b11fe8ea2b8a3126665cfe13ff551ad3469`.

Nhóm xây dựng mô-đun Python nhận input synthetic theo Decision Table, phân loại bằng `classify(patient)`, trả mức, mã rule và lý do, sau đó ghi audit JSONL. Chương trình không sử dụng dữ liệu bệnh nhân thật và không đưa khuyến nghị điều trị.

**educational/synthetic — not for clinical use**

## Mã nguồn, cài đặt và kiểm thử

Mã nguồn/test nằm ở fork code, không nằm trong thư mục hồ sơ này. Ví dụ sau checkout đúng commit PC-2 dự kiến; cập nhật SHA khi Freeze. Thực hiện trong PowerShell:

```powershell
git clone https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter.git
cd ktpmud-bkt-ruleengine-starter
git checkout d00216053c206435f370c977c51b42d711623861
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
pytest -q
```

Trên Bash thay bước kích hoạt bằng `source .venv/bin/activate`. Có thể chạy `python run_case.py MISS-009` để xem một ca thiếu dữ liệu synthetic; xem [hướng dẫn chạy ca và xuất CSV](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/runcase.md). Muốn xem audit của phiên demo, đặt biến `DR_MAY_DAY_AUDIT_PATH` tới đường dẫn **tuyệt đối ngoài repository** trước khi chạy rồi dùng `python query_audit.py <đường-dẫn-audit>`. Hướng dẫn chi tiết ở [README repo code](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/README.md).

## Hồ sơ và bằng chứng

| Nội dung | File |
|---|---|
| Metadata nhóm và nguồn code | [submission.json](submission.json), [source-ref.json](source-ref.json) |
| Yêu cầu và quyết định thiết kế đầy đủ | [Requirements](artifacts/REQUIREMENTS.md), [Decisions](artifacts/DECISIONS.md) |
| Kết quả, giới hạn test và benchmark | [Test report](artifacts/TEST_REPORT.md) |
| Audit, security và privacy | [Security/privacy checklist](artifacts/SECURITY_PRIVACY_CHECKLIST.md) |
| Sử dụng AI và đóng góp | [AI use](artifacts/AI_USE.md), [Contributions](artifacts/CONTRIBUTIONS.md) |
| Demo | [Kịch bản demo nháp](slides/DEMO_SCRIPT.md) |

CI trên nhánh tiến độ tại `7a8da1e`: [run 44114089165](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44114089165), `490 passed, 3 skipped` trên Linux. Ba ca bỏ qua yêu cầu Windows; kết quả này không chứng minh chúng đã chạy trên Windows. Benchmark đã có kết quả sơ bộ, còn cần chạy và lưu bằng chứng trên SHA freeze.

## Các giá trị cần cập nhật trước khi nộp

- `case_study_id = BKT-1` được dùng làm giá trị làm việc, dựa trên ví dụ trong [test validator chính thức](https://github.com/fossbk-spec/ktpmud-project/blob/main/scripts/test_validate_submission.py) và các PR Track A; Leader cần đối chiếu mã chính thức trước khi nộp.
- `submitted_at` hiện là ngày lập bản nháp `2026-11-17`; nếu mở PR vào ngày khác, sửa thành ngày nộp thực tế.
- `release_tag` là **tên dự kiến**, chưa khẳng định tag đã được tạo. Sau Freeze, điền SHA freeze, tag đã tạo và CI của SHA đó.
- Khi có Draft PR và cập nhật `progress/PROGRESS.md`, đối chiếu lại commit PC-2, CI gần nhất và trạng thái hồ sơ trong `source-ref.json` trước xác nhận.
