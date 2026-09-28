# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   **Cần quy tắc riêng, không mặc định là `DUPLICATE`.** Mỗi camera có ảnh gốc riêng. Nếu output đích là ground truth
   theo từng camera thì vật thấy được trên camera nào phải có box trên camera đó, và hai box đều hợp lệ (có thể một box
   ở `edge`, một box ở `mid`). `DUPLICATE` chỉ đúng khi hai box nằm trên **cùng một ảnh** cho cùng một vật. Việc hợp nhất
   hai box thành một object (ví dụ trên BEV) là quyết định của policy output, và cần timestamp đồng bộ cùng calibration
   để chứng minh hai box là cùng vật. Chưa có các điều kiện đó thì giữ cả hai box và ghi ca vào decision log.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   **Giữ cùng track ID** khi vẫn là cùng một vật và còn quan sát được liên tục, kể cả khi bị che một phần ngắn hoặc đi
   từ center ra edge. **Thêm keyframe** khi hình học thay đổi lớn mà nội suy không bám được, ví dụ vật đi vào vùng rìa
   méo mạnh, đổi hướng, hoặc bị cắt bởi vòng kính (lúc đó bật `truncated`). **Đặt Outside** khi vật rời trường nhìn
   hoặc bị che hoàn toàn. Nếu vật quay lại mà không chắc là cùng vật thì mở track mới, không nối bừa. **Trước khi nối
   track qua hai camera** cần: timestamp đồng bộ giữa camera, calibration nội/ngoại để chiếu cả hai về cùng hệ tọa độ,
   vùng seam đã xác định, bằng chứng nhận dạng (vị trí chiếu khớp, class, hướng đi, ngoại hình), và policy output nói
   rõ track là theo camera hay toàn cục.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   `adasind_295948.jpg` **L1**: tôi gán `Truck` cho xe có thùng hàng phía sau cabin, còn reference R1 gán `Car` (IoU
   0.843, WRONG_CLASS). Tôi không sửa nhãn cho giống reference. Tôi ghi `E0_reference_defect`, owner `guideline`,
   action `escalate`, dẫn R04 ("pickup/xe tải nhỏ → Truck"), chụp `screenshots/295948_L1_truck_vs_R1_car.png` và đề xuất
   R04a trong guideline patch. Model M2 cũng gán Truck, là bằng chứng phụ. Nếu làm lại, tôi sẽ: tự vẽ trực tiếp trên
   CVAT thay vì soạn XML rồi upload (lock ghi `new=0`, và bản nháp đầu làm mất `lens_border` vì polygon vẽ lại bị để
   reason mặc định); soát riêng lớp `ignore_region` trước khi vẽ box; và với cụm người chồng nhau ở 271039, zoom ảnh gốc
   để quyết định tách/gộp ngay từ đầu thay vì để tới P4.
