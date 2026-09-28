# QA review · B4-center

Mã khóa: 05A1-1EDA

**Hình thức:** QA do **Hùng** thực hiện trên bản r1_craft do Huy gán nhãn và đã khóa (xem `00_setup/roles.md`). Khóa
lúc 17:05, bắt đầu review lúc 17:10. Chỉ dùng ảnh gốc, `qa_overlay.html` và rules v1.0.0; **chưa** mở reference,
model overlay hay worked HTML của slice.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_295948.jpg | L6 | R07;R03 | Người quấn khăn caro chiếm gần hết nửa phải ảnh (box x572–1080, y608–1723), rất sát camera, thấy bánh xe đạp và chân trên bàn đạp. Đang gán `Bike` (rider). Nếu đây là người đạp chính xe gắn camera (xích lô ego) thì phải là `ignore_region` `ego_body` (R07) chứ không phải box. Ảnh đơn không đủ để khẳng định bên nào. |
| adasind_295948.jpg | L7 + L2 | R03 | Người áo xanh bị cắt mép trái (L7 `Pedestrian` x0–100, y897–1035) và xe đạp chở thùng trắng (L2 `Bike` x0–140, y1005–1200) đang tách thành hai box. Tay người đặt trên thùng hàng, không thấy rõ người ngồi trên yên hay đứng dắt; nếu đang ngồi thì R03 yêu cầu gộp thành một `Bike`. |
| adasind_271039.jpg | L2, L3, L14 | R06;R03 | Cụm ở x395–462, y820–947: hai người (L2, L14) và một xe máy có người (L3) chồng nhau, sau ô tô đỏ L1. Ranh giới từng người khó tách; cần xét có nên dùng `ignore_region` `crowd_or_group` (R06). Với L3, không thấy rõ người ngồi trên xe hay đứng cạnh (R03). |
| adasind_270517.jpg | L5 | R02 | Box scooter trắng đỗ lề (x805–876, y816–913) rộng hơn vật: mép trên ở y816 trong khi yên/đầu xe bắt đầu khoảng y845, phần trên box là nền cửa hàng. Box nên bám phần nhìn thấy. |
| adasind_270517.jpg | L4 | R01;R06 | Vật ở mép trái (x0–12, y798–885) cao 87 px nhưng chỉ rộng 12 px, bị biên khung cắt. Chỉ đoán được là người; không đọc được class chắc chắn. Có thể là `unreadable` (R06) thay vì box `Pedestrian`. |
| adasind_271039.jpg | L12 | R01;R05 | Người áo vàng (x267–285, y827–906) bị người áo đen L11 che gần hết, phần nhìn thấy rộng khoảng 18 px. Chiều cao đạt H nhưng cần xác nhận đây là một người riêng, không phải một phần của L11 hoặc auto L7 phía sau. `occluded=true` đúng theo R05. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
