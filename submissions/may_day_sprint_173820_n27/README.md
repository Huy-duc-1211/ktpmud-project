# Hồ sơ nộp bài — Track A · Dr May Day Rule Engine Sprint

- Nhóm `173820_n27`, lớp `173820`; thành viên và MSSV trong [submission.json](submission.json) và [CONTRIBUTIONS.md](artifacts/CONTRIBUTIONS.md).
- [Draft PR #39](https://github.com/fossbk-spec/ktpmud-project/pull/39) được mở cho PC-2 ngày 17/11/2026. Nội dung dưới đây đối chiếu **ứng viên trước Freeze 19/11**, chưa phải SHA/tag cuối.
- Fork code: [Huy-duc-1211/ktpmud-bkt-ruleengine-starter](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter); code/test ứng viên ở [`4be2751`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/commit/4be2751f76e540a563de9f09f24935753f80a2e7), tài liệu QA/AI-use ở [PR #10, `33c5a63`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/commit/33c5a63e9e4993c3d17adc93b0dc0d8b5e63e4ed).
- Starter: tag [`bkt-2026-v1`](https://github.com/fossbk-spec/ktpmud-bkt-ruleengine-starter/tree/bkt-2026-v1), SHA `1c0a5b11fe8ea2b8a3126665cfe13ff551ad3469`.

Mô-đun Python nhận input synthetic theo Decision Table, `classify(patient)` trả mức/mã rule/lý do và ghi audit JSONL bốn trường. Không dùng dữ liệu bệnh nhân thật và không đưa lời khuyên điều trị.

**educational/synthetic — not for clinical use**

## Xem và chạy mã nguồn

Mã nguồn/test nằm ở fork code. Ví dụ kiểm ứng viên hiện tại trong PowerShell; sau Freeze thay SHA bằng `final_commit` đã xác nhận trong [source-ref.json](source-ref.json):

```powershell
git clone https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter.git
cd ktpmud-bkt-ruleengine-starter
git checkout 33c5a63e9e4993c3d17adc93b0dc0d8b5e63e4ed
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m pytest -q
```

Trên Bash dùng `source .venv/bin/activate`. Có thể chạy `python run_case.py MISS-009` để xem ca mã đại diện; [hướng dẫn runner](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/33c5a63e9e4993c3d17adc93b0dc0d8b5e63e4ed/runcase.md). Để xem audit, đặt `DR_MAY_DAY_AUDIT_PATH` thành đường dẫn tuyệt đối ngoài repo rồi dùng `python query_audit.py <đường-dẫn-audit>`. [README repo code](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/33c5a63e9e4993c3d17adc93b0dc0d8b5e63e4ed/README.md) có chi tiết.

## Hồ sơ

| Nội dung | File |
|---|---|
| Metadata nhóm và nguồn code | [submission.json](submission.json), [source-ref.json](source-ref.json) |
| Yêu cầu và quyết định thiết kế | [Requirements](artifacts/REQUIREMENTS.md), [Decisions](artifacts/DECISIONS.md) |
| Test, QA, CI và benchmark | [Test report](artifacts/TEST_REPORT.md), [QA Matrix tại commit ứng viên](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/33c5a63e9e4993c3d17adc93b0dc0d8b5e63e4ed/docs/qa/QA_MATRIX.xlsx) |
| Audit và privacy | [Security/privacy checklist](artifacts/SECURITY_PRIVACY_CHECKLIST.md) |
| Sử dụng AI và đóng góp | [AI use](artifacts/AI_USE.md), [Contributions](artifacts/CONTRIBUTIONS.md) |
| Demo | [Kịch bản demo](slides/DEMO_SCRIPT.md) |

[CI PR #10 tại `33c5a63`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44517747720) thành công; lượt kiểm local 490 passed, 3 test Windows bị skip trên Linux. Hai lượt benchmark ứng viên đạt p95 0.301703/0.349306 ms trong `/tmp` tmpfs; xem [Test report](artifacts/TEST_REPORT.md) về môi trường và giới hạn.

**Trước Ready for review:** `source-ref.json` vẫn giữ metadata PC-2 và tên tag dự kiến, chưa chứng minh tag tồn tại. Leader chốt/merge PR code, tạo tag theo quy định, thay bằng `final_commit`, `release_tag`, `ci_run_url` thật; đồng bộ phần Freeze trong progress và các link SHA nếu cần. Sau đó kiểm `validate` xanh trên chính PR #39. Việc GV/TA nghiệm thu là bước riêng.
