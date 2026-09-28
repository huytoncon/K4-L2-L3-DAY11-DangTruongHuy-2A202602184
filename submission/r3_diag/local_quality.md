# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `05a11edacba4b382386395bd556a306b466145241cb322d22b19edf7ef0e11e2`; slice `B4-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_270517.jpg, adasind_271039.jpg, adasind_295948.jpg. Frame thiếu trong export: không.
TP=17; FP=7; FN=3; số lần đối chiếu=26; mean IoU của TP=0.829.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.654 | 0.923 | 0.846 |
| precision | 0.708 | 0.703 | 0.500 |
| recall | 0.850 | 0.881 | 0.750 |
| jaccard | 0.630 | 0.630 | 0.500 |
| dice | 0.773 | 0.766 | 0.667 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 1 | 0 | 0.962 | 0.750 | 1.000 | 0.750 | 0.857 |
| Car | 4 | 0 | 1 | 0.962 | 1.000 | 0.800 | 0.800 | 0.889 |
| Pedestrian | 6 | 3 | 1 | 0.846 | 0.667 | 0.857 | 0.600 | 0.750 |
| ThreeWheeler | 3 | 2 | 1 | 0.885 | 0.600 | 0.750 | 0.500 | 0.667 |
| Truck | 1 | 1 | 0 | 0.962 | 0.500 | 1.000 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_270517.jpg | 7 | 1 | 0 | 0.875 | 0.875 | 1.000 |
| adasind_271039.jpg | 8 | 5 | 2 | 0.533 | 0.615 | 0.800 |
| adasind_295948.jpg | 2 | 1 | 1 | 0.667 | 0.667 | 0.667 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 4 | 0 | 0 | 1 | 0 |
| Pedestrian | 0 | 0 | 6 | 0 | 0 | 1 |
| ThreeWheeler | 0 | 0 | 0 | 3 | 0 | 1 |
| Truck | 0 | 0 | 0 | 0 | 1 | 0 |
| <extra> | 1 | 0 | 3 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
