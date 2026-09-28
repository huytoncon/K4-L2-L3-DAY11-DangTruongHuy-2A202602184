# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): (1) vạch chéo ở tiền cảnh giữa-dưới, từ (406,654) xuống mép
  dưới ảnh tại (534,720), là ranh giữa hai ô của dãy tiền cảnh; (2) vạch tiền cảnh bên phải, từ (698,623) tới mép phải
  (960,685), là ranh ô kế tiếp cùng dãy. Hai vạch này có đầu mút tự do hướng về lối xe và chạy song song nhau, cách nhau
  đúng một bề ngang ô, nên đó là vạch chia ô chứ không phải vạch dẫn hướng. Ngoài ra tôi vẽ các vạch chéo của dãy ô
  đôi (head-to-head) ở giữa ảnh, cắt tại chỗ giao với dải sơn dọc giữa dãy, ví dụ (247,564)→(196,535) và (194,536)→(170,520),
  cùng các vạch ngắn của dãy ô ở xa (y < 500). Tổng 38 polyline, 4 polygon.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: không vẽ **mép bãi / chân hàng rào** ở xa (y ≈ 460–470, chạy ngang
  cả ảnh) vì đó là biên mặt đường, không phải sơn chia ô. Cũng không vẽ **mảng sáng màu ở giữa mép dưới** (x ≈ 340–390,
  y ≈ 712–720) vì chỉ thấy một mẩu nhỏ bị khung cắt, không xác định được là vạch ô hay vết bẩn trên mặt đường.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: polygon chính là **lối xe giữa dãy ô đôi ở giữa và dãy ô
  tiền cảnh**. Mép trên đi qua đầu mút dưới của các vạch dãy giữa (y ≈ 524–577), mép dưới đi qua đầu mút trên của các vạch
  tiền cảnh (y ≈ 595–684), hai bên dừng ở biên ảnh. Không có xe hay vật che trong vùng này. Ba polygon mảnh còn lại nằm ở
  các lối xe xa hơn (y ≈ 463–522); polygon xa nhất dừng trước chiếc xe đỏ (x ≈ 193–221) và không vượt qua hàng rào.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): (a) Polyline dài (941,507)→(0,542) là **dải sơn dọc giữa dãy
  ô đôi**. Tôi gán `parking_line` vì nó tạo **đầu ô** (cạnh ngắn) của từng ô ở cả hai phía, nhưng nó cũng có thể bị xem là
  vạch dọc theo dãy chứ không chia riêng một ô; ngoài ra một số đoạn ở xa của dải này mờ. (b) Ranh giữa ô đỗ và lối xe
  **không có sơn**, nên biên `free_space` là đường tưởng tượng nối các đầu mút vạch. (c) Các vạch và polygon ở vùng xa
  (y < 500) chỉ dài vài chục pixel và mờ, độ chắc chắn thấp hơn vùng tiền cảnh.
