# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front (25 normal / 35 hard) | Đám đông người + xe ba bánh chồng nhau; rider dừng xe chân chống đất; xe tải nhỏ vs bán tải; ngược sáng | Trên ADASIND, lỗi dồn vào cụm đông (271039: 5/7 lỗi L ở center) và luật R03/R04/R06 còn mơ hồ | Ảnh fisheye gốc (không undistort), vòng kính (cx, cy, R) theo frame, timestamp, bản `rules_version` | 2 người gán độc lập + 1 người phân xử theo rule v1.1.0; mọi ca bất đồng class/tách-gộp đưa vào decision log |
| rear (20 / 30) | Lùi và đỗ xe: người/xe rất gần bị cắt bởi vòng kính và `ego_body` (cản sau); rider sát xe | Ranh giới `ego_body` là lỗi P0 (295948: một hình chữ nhật loại 4 vật); truncated nhiều | Mask `ego_body` cố định theo rig (đo một lần mỗi xe/camera), vòng kính, timestamp | Soát riêng polygon `ego_body`/`lens_border` trước box; kiểm không box nào ≥ 50% trong ignore (R09) |
| left (20 / 25) | Vật ở seam front-left/rear-left thấy trên hai camera; xe chạy song song bị cắt ở biên | Cùng vật hai box, dễ bị đánh DUPLICATE sai hoặc ghép sai | Calibration ngoại/nội của cả hai camera, timestamp đồng bộ giữa camera, policy output đích (per-camera hay BEV) | Reviewer thứ hai xem **cả hai** camera cùng timestamp; chưa ghép box cross-camera khi thiếu calibration |
| right (20 / 25) | Lề đường: người đi bộ, xe đỗ, sạp hàng sát xe; seam front-right/rear-right khi tấp lề | Vật sát xe méo mạnh ở rìa fisheye; biển/sạp che người (271039 L4 occluded) | Như left, cộng vùng lề trong ảnh gốc | Như left; thêm kiểm `occluded` và `truncated` bằng mắt vì hai attribute hay bị lẫn (R05) |

**Quy trình chung:** chọn mẫu theo `45_sampling_plan.csv` (phân tầng, mỗi cảnh tối đa 1–2 frame). Gán độc lập 2 lần, mù
với model và nhãn người kia. Tính bất đồng theo từng object. Người phân xử là người thứ ba có quyền chốt rule. Ca không
chốt được thì `E5_unresolved`, loại khỏi gold cho tới khi guideline có luật mới. Chỉ gọi là gold khi mọi bất đồng đã có
quyết định ghi trong log, và một mẫu kiểm tra lại (ví dụ 10% frame) cho kết quả khớp.

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): khi thay camera, lens, vị trí lắp hoặc thân xe (làm
  đổi vòng kính và `ego_body`); khi calibration được đo lại; khi bump `rules_version` (ví dụ v1.0.0 → v1.1.0 đổi R03,
  R04 thì phải gán lại các ca rider/xe tải nhỏ); khi thêm class (ví dụ vật tĩnh); và định kỳ khi phân bố dữ liệu
  mới (route, mùa, giờ) lệch khỏi tập gold.
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: một người đi xe máy ở góc trước-trái xuất hiện
  ở `edge` của front và ở `mid` của left cùng lúc. Trước khi coi hai box là cùng một vật (hoặc xóa một box), cần
  timestamp đồng bộ giữa hai camera, calibration để chiếu hai box về cùng hệ tọa độ (BEV/xe), và policy output: giữ box
  trên mỗi camera (per-camera ground truth) hay hợp nhất một object trên BEV. Nếu thiếu, giữ cả hai box, không đánh
  DUPLICATE, và ghi vào decision log.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: ADASIND
  chỉ có một camera nhìn trước, không có seam, không có rear/side, không có timestamp đa camera. Hai người đồng ý nhau
  vẫn có thể cùng sai (C0: tôi và prefill có thể cùng sai nếu không có reference). Teaching reference cũng có lỗi (3 ca
  E0 ở B4). Report chỉ đo độ khớp với một reference trên 3 frame, không đo đúng/sai của nhãn và không phủ các tình huống
  riêng của rear/left/right.
