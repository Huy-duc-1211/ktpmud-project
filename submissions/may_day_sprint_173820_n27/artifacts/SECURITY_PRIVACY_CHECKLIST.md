# Checklist bảo mật, quyền riêng tư và audit — bản PC-2

**Phạm vi:** review và test code synthetic tại mốc code sau PR #7; chưa phải kiểm định toàn bộ môi trường hoặc xác nhận của GV/TA. Người tổng hợp: Lead (SV1). [Nguồn test audit](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/tests/test_audit.py) · [Quyết định audit](https://github.com/Huy-duc-1211/ktpmud-bkt-ruleengine-starter/blob/7a8da1e9697359fbb884c911d744129c1a752455/DECISIONS.md) · [Báo cáo test](TEST_REPORT.md).

| Mục cần kiểm | Bằng chứng / trạng thái | Giới hạn và việc tiếp theo |
|---|---|---|
| Dữ liệu demo/test | Bộ ca trong repo mô tả dữ liệu synthetic; mục đích học tập. | Không nhập dữ liệu bệnh nhân thật; tiếp tục rà file trước nộp. |
| Schema audit | Writer chỉ dựng bốn khóa `Timestamp`, `User_UUID`, `Action_Type`, `Status`; test đọc và kiểm schema. | Không suy ra mọi file audit đã có trên máy người dùng đều sạch. |
| UUID | UUID giả lập theo phiên/temporary call, nằm ngoài patient input; có test context và khôi phục phiên. | Không dùng UUID này làm định danh bệnh nhân. |
| SUCCESS / INVALID_INPUT | Có test đường input hợp lệ, input sai và status tương ứng khi ghi audit thành công. | Không khẳng định record tồn tại nếu ghi thất bại. |
| INTERNAL_ERROR | Lỗi validator/rule bất ngờ thử ghi `INTERNAL_ERROR` rồi báo `RuntimeError`; nếu audit lỗi ưu tiên `AuditError`. Có test synthetic. | Đây là quyết định nhóm, không khẳng định thầy đã xác nhận riêng status này. |
| Fail-closed và lỗi kép | Có test inject lỗi mkdir/open/write/short write/flush/close; ưu tiên lỗi audit, không trả kết quả thành công. | Lỗi hệ điều hành, quyền và file system thực tế có thể khác test giả lập. |
| Không lộ input qua lỗi/log | Các test canary kiểm tra message, exception chain/traceback, stdout/stderr, log và audit trong các đường được test. | Kết luận: **chưa phát hiện lộ dữ liệu trong phạm vi test**, không bảo đảm mọi thư viện/môi trường. |
| Đường dẫn audit | Mặc định ngoài repo trong thư mục người dùng; `DR_MAY_DAY_AUDIT_PATH` chấp nhận đường dẫn tuyệt đối ngoài repo. | Thành viên tự quản lý file và quyền truy cập; không commit log hoặc payload. |
| JSONL có dòng hỏng | Ghi tiếp tách dòng nếu thiếu newline; công cụ đọc bỏ qua dòng sai/khóa trùng, báo vị trí và mã thoát 3 nếu đọc một phần. | Không sửa hoặc chứng thực phần dữ liệu cũ; không bảo đảm an toàn ghi đồng thời hay khôi phục sau mất điện. |
| Secret / PII | Chưa có bằng chứng rà soát độc lập toàn bộ repo và lịch sử tại SHA freeze. AI-use log không chứa PII; dữ liệu audit/demo không chứa thông tin định danh thực. | Rà cả mã, file cấu hình, file đầu ra và lịch sử trước Freeze; metadata nhóm do nhóm chủ động cung cấp nằm trong hồ sơ nộp công khai. |
| Bằng chứng cuối | CI và kết quả sơ bộ có trong [TEST_REPORT.md](TEST_REPORT.md). | Chạy lại các kiểm tra liên quan và cập nhật SHA freeze, môi trường, người review. |

**Cảnh báo:** educational/synthetic — not for clinical use.
