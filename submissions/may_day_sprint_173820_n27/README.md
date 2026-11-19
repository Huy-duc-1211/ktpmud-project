# Hồ sơ nộp bài — Track A · Dr May Day Rule Engine Sprint

- Nhóm `173820_n27`, lớp `173820`; thành viên và MSSV trong [submission.json](submission.json) và [CONTRIBUTIONS.md](artifacts/CONTRIBUTIONS.md).
- [Draft PR #39](https://github.com/fossbk-spec/ktpmud-project/pull/39) được mở cho PC-2 ngày 17/11/2026. Nội dung đã đối chiếu với commit merge PR #10 và CI của commit này; tag `rc1` còn chờ tạo trước khi Freeze.
- Fork code: [Huy-duc-1211/ktpmud-bkt-ruleengine-starter](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter); commit sau merge [PR #10 `7af8d47`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/commit/7af8d47fd410b96dcdb4b7caccd48580ce07cf4a), gồm mã/test đã có từ `4be2751` và hồ sơ QA/AI-use.
- Starter: tag [`bkt-2026-v1`](https://github.com/fossbk-spec/ktpmud-bkt-ruleengine-starter/tree/bkt-2026-v1), SHA `1c0a5b11fe8ea2b8a3126665cfe13ff551ad3469`.

Mô-đun Python nhận input synthetic theo Decision Table, `classify(patient)` trả mức/mã rule/lý do và ghi audit JSONL bốn trường. Không dùng dữ liệu bệnh nhân thật và không đưa lời khuyên điều trị.

**educational/synthetic — not for clinical use**

## Xem và chạy mã nguồn

Mã nguồn/test nằm ở fork code. Ví dụ kiểm chính commit sau merge trong PowerShell; SHA này đã được ghi tại [source-ref.json](source-ref.json):

```powershell
git clone https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter.git
cd ktpmud-bkt-ruleengine-starter
git checkout 7af8d47fd410b96dcdb4b7caccd48580ce07cf4a
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m pytest -q
```

Trên Bash dùng `source .venv/bin/activate`. Có thể chạy `python run_case.py MISS-009` để xem ca mã đại diện; [hướng dẫn runner](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7af8d47fd410b96dcdb4b7caccd48580ce07cf4a/runcase.md). Để xem audit, đặt `DR_MAY_DAY_AUDIT_PATH` thành đường dẫn tuyệt đối ngoài repo rồi dùng `python query_audit.py <đường-dẫn-audit>`. [README repo code](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7af8d47fd410b96dcdb4b7caccd48580ce07cf4a/README.md) có chi tiết.

## Hồ sơ

| Nội dung | File |
|---|---|
| Metadata nhóm và nguồn code | [submission.json](submission.json), [source-ref.json](source-ref.json) |
| Yêu cầu và quyết định thiết kế | [Requirements](artifacts/REQUIREMENTS.md), [Decisions](artifacts/DECISIONS.md) |
| Test, QA, CI và benchmark | [Test report](artifacts/TEST_REPORT.md), [QA Matrix tại commit merge](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7af8d47fd410b96dcdb4b7caccd48580ce07cf4a/docs/qa/QA_MATRIX.xlsx) |
| Audit và privacy | [Security/privacy checklist](artifacts/SECURITY_PRIVACY_CHECKLIST.md) |
| Sử dụng AI và đóng góp | [AI use](artifacts/AI_USE.md), [Contributions](artifacts/CONTRIBUTIONS.md) |
| Demo | [Kịch bản demo](slides/DEMO_SCRIPT.md) |

[CI sau merge tại `7af8d47`](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/actions/runs/44525947180) thành công; lượt kiểm local 490 passed, 3 test Windows bị skip trên Linux. Hai lượt benchmark ứng viên đạt p95 0.301703/0.349306 ms trong `/tmp` tmpfs; xem [Test report](artifacts/TEST_REPORT.md) về môi trường và giới hạn.

**Trước Ready for review:** `source-ref.json` đã ghi commit merge `7af8d47…` và CI tương ứng. Trường `release_tag` vẫn là tên dự kiến, **tag chưa được tạo**. Sau khi tạo tag trỏ đúng SHA này, cập nhật phần Freeze trong progress, kiểm lại tag và `validate` trên chính PR #39 rồi chuyển Ready ngày 20/11. Việc GV/TA nghiệm thu là bước riêng.
