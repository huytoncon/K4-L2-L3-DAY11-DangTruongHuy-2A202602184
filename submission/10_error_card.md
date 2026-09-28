# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | BOX_GEOMETRY | 1 |
| center | B4 | MISSING | 5 |
| center | B4 | SPURIOUS | 13 |
| center | B4 | WRONG_CLASS | 2 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | SPURIOUS | 2 |
| edge | B4 | MISSING | 1 |
| edge | B4 | SPURIOUS | 1 |
| mid | B4 | BOX_GEOMETRY | 3 |
| mid | B4 | IGNORE_SCOPE | 7 |
| mid | B4 | MISSING | 2 |
| mid | B4 | SPURIOUS | 8 |
| mid | B4 | WRONG_CLASS | 1 |
| mid | C0 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 25 (ví dụ frame adasind_019560.jpg)
- MISSING: 8 (ví dụ frame adasind_270517.jpg)
- IGNORE_SCOPE: 7 (ví dụ frame adasind_295948.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: lỗi nổi bật nhất là **SPURIOUS (25)**, nhưng phần lớn không phải
  người gán vẽ thừa. Tách theo nguồn: (a) **E4_model_domain** chiếm nhiều nhất ở B4. Model YOLO26m gán 5 xe ba bánh thành
  Car/Truck (270517 M6/M7/M9, 271039 M9/M11/M12) và box người ngồi trong auto (270517 M5/M10/M11). Lỗi lặp theo class
  trên nhiều frame, không theo zone, nên đây là lệch miền chứ không phải một box lệch. (b) **E0_reference_defect**:
  271039 L7 là auto cao 78 px thấy rõ, model cũng thấy (M11), nhưng reference không có. Thêm vào đó 7 dòng IGNORE_SCOPE
  ở 295948 là do `ego_body` của reference là một hình chữ nhật x0–229, y915–1736 trùm lên người đi bộ ở xa (L3, L4).
  (c) **E1_annotator_error** thật chỉ có 270517 L4 (vật 12 px ở mép, không đọc được class) và 3 ca ở C0 (xích lô,
  rider dừng xe). (d) Cụm người ở 271039 (L2/L14 và R8) để **E5_unresolved**: L và M tách hai người, R gộp một.
- Cách sửa và ai nhận việc (`owner`): `annotator` đã rework 270517 L4 (delta: spurious ở mid từ 2 giảm còn 1). `data_ops`
  sửa polygon `ego_body` 295948 và bổ sung auto 271039 vào reference (ticket 1, 2). `guideline` chốt ranh giới xe tải
  nhỏ/bán tải (R04) và cách xử lý rider dừng xe, người trong cụm đông (patch v1.1.0). `ai_team` đánh giá model có class
  ThreeWheeler hoặc fine-tune trên ảnh fisheye có auto trước khi dùng làm pre-label (ticket 3). `qa` cho người soát thứ
  hai xem lại cụm 271039.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/295948_ref_ego_body_qua_rong.png` (R07/R09),
  `screenshots/271039_L7_auto_R_thieu.png` (R01/R04), `screenshots/295948_L1_truck_vs_R1_car.png` (R04); các dòng
  `r3_diag` trong `findings.csv` với object_ref tương ứng; `r3_diag/local_quality.md` (ThreeWheeler precision 0.600, Truck
  0.500, mean IoU TP 0.829).
