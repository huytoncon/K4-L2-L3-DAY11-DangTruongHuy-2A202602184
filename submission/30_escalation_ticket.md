# Escalation ticket

## Ticket 1

- **Frame:** `adasind_295948.jpg`, reference `ignore_region` `ego_body` x0–229, y915–1736 (dòng findings `r3_diag` L4
  IGNORE_SCOPE).
- **Ảnh chụp:** `submission/screenshots/295948_ref_ego_body_qua_rong.png`
- **Expected impact:** polygon là hình chữ nhật, phủ 100% box L2 (xe đạp chở hàng), L3 (người áo vàng), L4 (người che ô)
  và 87% L7. Theo R09, 4 vật hợp lệ bị coi là don't-care, nên không được đếm TP/FP. Recall/precision của frame này đang bị
  đo trên 3 vật thay vì khoảng 7. Model cũng thấy các vật này (M3, M5, M7), nên so sánh L/R/M ở frame bị lệch. Mức P0 (R10).
- **Owner:** `data_ops`
- **Recommendation:** vẽ lại `ego_body` bám viền người áo caro ngồi trên xe ego (khoảng x0–230, y1195–1700). Xác nhận
  xe đạp chở hàng ở x0–140, y1005–1200 là vật ngoài hay một phần xe ego. Chạy lại `compare`/`local-quality` sau khi sửa.
  Đề xuất R07a trong `20_guideline_patch.md`.

## Ticket 2

- **Frame:** `adasind_271039.jpg`, L7 `ThreeWheeler` x199–284, y821–899 (dòng findings `r3_diag` L7 SPURIOUS, cell L_only).
- **Ảnh chụp:** `submission/screenshots/271039_L7_auto_R_thieu.png`
- **Expected impact:** auto mui vàng cao 78 px (≥ H=40, R01), thân xe thấy rõ sau đám người. Reference không có box hay
  ignore cho vật này. Model thấy vật (M11 Car x192–314) dù sai class. Nếu không sửa, mọi người gán đúng vật này đều bị tính
  FP, và ThreeWheeler precision bị kéo xuống (hiện 0.600 trong `local_quality.md`).
- **Owner:** `data_ops`
- **Recommendation:** thêm box `ThreeWheeler` `occluded=true` vào reference. Kiểm tra thêm auto xám x160–200 mà reference
  đang để `unreadable`.

## Ticket 3

- **Frame:** `adasind_270517.jpg` (M6, M7, M9, M5, M10, M11) và `adasind_271039.jpg` (M9, M11, M12)
- **Ảnh chụp:** `submission/screenshots/271039_L7_auto_R_thieu.png` (vùng auto đỗ); `submission/r3_diag/model_compare.html`
- **Expected impact:** YOLO26m gán **5/5** xe ba bánh trong slice thành `Car`/`Truck` (270517 còn có hai box khác class
  cho cùng một auto: M6 Car và M9 Truck). Model cũng box người ngồi trong auto thành `Pedestrian`, trái với R03. Nếu dùng
  model làm pre-label, người gán phải sửa class mọi xe ba bánh. Recall theo class ThreeWheeler của model trên slice này
  bằng 0.
- **Owner:** `ai_team`
- **Recommendation:** không dùng output model cho class ThreeWheeler làm pre-label. Đánh giá trên nhiều frame hơn (không
  chỉ 3 frame) để xác nhận E4. Cân nhắc fine-tune với ảnh fisheye có xe ba bánh và luật R03 (không box người trong xe).
- **Ghi chú escalation khác trong findings:** 295948 L1/R1 Truck vs Car (owner `guideline`, R04a), 271039 L3 rider bị
  che nửa người (owner `qa`).
