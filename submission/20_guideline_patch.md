# Guideline patch

- **Rule mới đề xuất:**
  - **R03a — Rider dừng xe:** người **ngồi trên yên** xe hai bánh vẫn là một `Bike`, kể cả khi đang dừng và chân chống
    đất. Chỉ tách thành `Pedestrian` + `Bike` khi người **đứng hẳn trên đất, không ngồi trên yên** và đang dắt hoặc đứng
    cạnh xe. Ví dụ: C0 `adasind_019560.jpg` người áo vàng ngồi yên, chân chống đất, gán một `Bike` (reference R1). Tôi đã
    tách nhầm thành L6 `Pedestrian` + L7 `Bike`.
  - **R04a — Xe tải nhỏ và bán tải:** xe có **thùng hàng hở hoặc thành thùng phía sau cabin** là `Truck`, bất kể kích
    thước. `Car` chỉ dành cho xe có khoang chở người kín (sedan, hatchback, van chở người). Ví dụ: `adasind_295948.jpg` L1
    x460–658 có thùng hàng, nên là `Truck`; reference R1 đang gán `Car`.
  - **R07a — Hình dạng `ego_body`:** polygon `ego_body` phải bám viền phần thân/người trên xe ego nhìn thấy, **không dùng
    hình chữ nhật** trùm ra ngoài. Nếu polygon che ≥ 50% một vật ở xa thì đó là lỗi phạm vi P0. Ví dụ: `adasind_295948.jpg`
    reference `ego_body` x0–229, y915–1736 phủ người che ô L4 và người áo vàng L3 ở cách xa hàng chục mét.
  - **R06a — Cụm người chồng nhau:** nếu hai người chồng nhau mà mỗi người vẫn thấy được đầu và ít nhất một chân thì box
    riêng từng người. Nếu không tách được đầu/thân thì gộp bằng `ignore_region` `crowd_or_group`, **không** vẽ một box
    `Pedestrian` bao cả cụm. Ví dụ: `adasind_271039.jpg` cụm x391–462 (L2, L14 và R8).
- **Áp dụng cho:** class `Bike`/`Pedestrian` (R03a), `Car`/`Truck` (R04a), `ignore_region` reason `ego_body` (R07a),
  `ignore_region` reason `crowd_or_group` (R06a). Chỉ áp dụng cho 6 class động của lab. Đây là tập con, không phải toàn
  bộ taxonomy ADASIND gốc.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R03 chỉ nói "dắt xe (không ngồi lên)" mà không nói ca dừng
  xe chân chạm đất, nên hai người đọc khác nhau (C0 L6). R04 liệt kê "xe bán tải nhẹ chở người" vào `Car` và
  "pickup/xe tải nhỏ" vào `Truck` mà không có tiêu chí phân biệt, nên reference và người gán lệch nhau (295948 L1/R1). R07
  không nói polygon phải bám viền, nên reference có một hình chữ nhật loại mất 4 vật khỏi phép so sánh. R06 có
  `crowd_or_group` nhưng không có ngưỡng khi nào tách, khi nào gộp.
- **`rules_version` mới:** v1.0.0 → **v1.1.0**
- **Hiệu lực từ:** round kế tiếp sau P5 của lớp (lần gán nhãn/rework tiếp theo và khi dựng lại teaching reference). Các
  nhãn đã khóa ở `r1_craft` và `rework` của buổi này giữ `rules_version=v1.0.0`.
