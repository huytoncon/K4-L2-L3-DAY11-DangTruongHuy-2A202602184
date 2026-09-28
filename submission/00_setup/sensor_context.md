# Sensor context

Slice được giao: **B4-center** (`adasind_270517.jpg`, `adasind_271039.jpg`, `adasind_295948.jpg`); frame hiệu chuẩn C0: `adasind_019560.jpg`.

- **Rig (theo quan sát, không có tài liệu rig):** một camera fisheye duy nhất, ảnh dọc 1080×1920, nhìn **về phía trước** theo
  hướng di chuyển, đặt thấp ở độ cao ngồi của người. Trong khung thấy chân/đầu gối, áo, dép của người ngồi cạnh và tay/lưng
  người phía trước (vd. `adasind_295948.jpg` thấy lưng người đội khăn caro ở phải, người mặc áo caro ở trái) → nhiều khả năng
  camera gắn/cầm trong một xe nhỏ không có vỏ kín (xe lôi/xe ba bánh hoặc xe hai bánh). Đây là suy đoán từ ảnh, **không** khẳng
  định loại xe, độ cao, góc nghiêng hay thông số calibration vì ADASIND không cung cấp.
- **`ego_body` nhìn thấy ở đâu:** chủ yếu ở **góc dưới-trái** và **mép dưới** trong vòng kính: đầu gối/chân/áo người ngồi trên
  xe ego (`adasind_270517.jpg`, `adasind_019560.jpg`). Ở `adasind_295948.jpg` phần xe ego/người trên xe ego chiếm cả hai bên dưới
  (trái: người áo caro; phải: lưng người phía trước). `adasind_271039.jpg` gần như không thấy thân xe ego, chỉ một mảng tối nhỏ ở
  mép trái-dưới → phải xem kỹ từng frame, không vẽ mặc định. Cần thống nhất khi gán: người ngồi trên chính xe ego thuộc
  `ego_body`, không box thành `Pedestrian`.
- **Vòng kính (lens circle):** theo `assets/frames.csv`, tâm lệch **trái và hơi dưới** tâm ảnh (cx ≈ 417–496, cy ≈ 892–987;
  tâm ảnh là 540, 960), bán kính R ≈ 790–815 px. Vòng tròn **tràn ra ngoài biên trái và phải** của khung (x từ khoảng −390
  đến 1290) nhưng nằm trong khung theo chiều dọc (y khoảng 100–1800), nên phần đen `lens_border` nằm ở **trên và dưới** ảnh
  cùng các góc. Phần ảnh hợp lệ trong vòng kính chiếm khoảng **75–78%** diện tích khung (C0: ~73%). Tâm/bán kính đổi nhẹ giữa
  các frame, nên bin `center/mid/edge` phải tính theo vòng kính của từng frame.
- **Giới hạn một camera:** dữ liệu chỉ có một camera nhìn trước, không có front/rear/left/right đồng bộ, timestamp chung hay
  calibration. Không suy ra được seam giữa camera, khoảng cách thật hay track ID xuyên camera; phần bốn camera ở P4 là kế
  hoạch trên tình huống giả lập.
