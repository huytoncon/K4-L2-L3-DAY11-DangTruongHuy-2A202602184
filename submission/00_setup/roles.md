# Phân công nhóm

Nhóm 3 người, nộp trong repo của Huy (`dangtruonghuy`), slice **B4-center**.

| Thành viên | Vai | Phần việc và file tương ứng |
|---|---|---|
| Huy | Gán nhãn (Annotator) | Parking (`parking/`), hiệu chuẩn C0 (`p1_calib/`), gán nhãn và khóa slice B4-center (`r1_craft/annotations.xml`, mã khóa `05A1-1EDA`), rework (`rework/`, mã khóa `8EFD-D034`) |
| Hùng | QA / QC | Tự soát checklist (`r1_craft/selfqc.md`), review bản đã khóa chỉ bằng ảnh và rule (`r2_qa/qa_review.md`, các dòng `r2_qa` trong `findings.csv`) |
| Việt Anh | Viết report | Chẩn đoán và báo cáo (`r3_diag/zone_table.md`, các dòng `r3_diag` trong `findings.csv`, `10_error_card.md`), bàn giao (`20_guideline_patch.md`, `30_escalation_ticket.md`, `40_decision_log.csv`, `45_review_plan.md`, `46_gold_set_plan.md`, `50_exit_ticket.md`) |

**Ghi chú quy trình:** công cụ được chạy ở chế độ một người (`00_setup/mode.json` chỉ có `dangtruonghuy`), nên slice và
các mã khóa đều thuộc repo này. Hùng review bản `r1_craft` sau khi đã khóa và trước khi mở reference.
