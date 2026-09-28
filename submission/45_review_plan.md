# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| **Frame có `ego_body` lớn hoặc người trên xe ego** (B4 `adasind_295948.jpg`, C0 `adasind_019560.jpg`) | 295948: 7 dòng IGNORE_SCOPE (L2, L3, L4, L7) do reference `ego_body` là hình chữ nhật; 1 ca rider sát camera (L6, R3) mà model bỏ sót | Lỗi phạm vi P0 làm sai **mọi** chỉ số của frame (4 vật bị loại khỏi phép đếm). Với 46/48 frame có thân xe ego, cùng kiểu lỗi có thể lặp ở nhiều slice khác | Polygon `ego_body` của L và R, overlay L/R/M, tỉ lệ box nằm trong ignore (≥ 50% theo R09), ảnh `screenshots/295948_ref_ego_body_qua_rong.png` |
| **Cảnh đông có xe ba bánh và người chồng nhau** (B4 `adasind_271039.jpg`, zone center) | 5/7 lỗi L ở center; 1 E0 (L7 thiếu trong R); 4 E5 (cụm L2/L14/R8, L3); 3 E4 (model gán auto thành Car) | Nơi người, reference và model bất đồng nhiều nhất; ThreeWheeler precision 0.600 và luật tách/gộp đám đông (R03, R06) còn mơ hồ | Crop độ phân giải gốc cho từng người trong cụm, object_ref L/R/M, `local_quality_conflicts.csv`, ảnh `screenshots/271039_L7_auto_R_thieu.png` |

Giới hạn của kết luận từ ba frame ADASIND: chỉ có 3 frame (20 vật reference) từ **một** camera nhìn trước. n ở edge chỉ
là 3, nên không so được tỉ lệ lỗi giữa các zone. Teaching reference có lỗi (E0 ở 3 ca) nên không phải gold. Chỉ số
local-quality phụ thuộc ngưỡng IoU: ở IoU 0.7, L ở edge từ matched 3 giảm còn 1 (`iou_sweep.md`). Các con số này chỉ
để chọn chỗ cần soi, không phải tỉ lệ lỗi của bộ dữ liệu.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: (1) **Phân tầng trước khi lấy mẫu:** theo camera × normal/hard,
rồi trong mỗi ô chia tiếp theo route, thời điểm (ngày/đêm/ngược sáng), mật độ vật và loại xe (có xe ba bánh, xe tải nhỏ,
rider). (2) **Chống đếm trùng cảnh:** gom frame thành "cảnh" theo khoảng thời gian liên tục (ví dụ cùng clip, cách nhau
< 5 s). Mỗi cảnh lấy tối đa 1–2 frame, cách nhau đủ xa để vật đã thay đổi, nên 35 frame hard của front phải đến từ ít
nhất khoảng 20 cảnh khác nhau. (3) **Bảng phủ:** sau khi chọn, đếm số cảnh, route, số frame có seam, có `ego_body`, có
xe ba bánh cho từng camera. Ô nào có 0 thì bổ sung trước khi review. (4) Ghi lại seed/quy tắc chọn để người khác lặp
lại được.

Vì sao chỉ giúp tìm ca cần soi, chưa đo được tỉ lệ lỗi: mẫu được **chọn có chủ đích**, dồn vào ca hard (115/200), nên
tỉ lệ lỗi trên mẫu cao hơn tỉ lệ trên 50.000 frame. Nếu muốn ước lượng tỉ lệ lỗi thật thì cần một mẫu ngẫu nhiên riêng
(hoặc gán trọng số theo tầng), và phải có gold đã phân xử, không phải teaching reference.
