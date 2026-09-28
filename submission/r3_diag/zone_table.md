# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 12 | 2 | 5 | 4 | 7 | SPURIOUS (3) |
| mid | 5 | 1 | 2 | 2 | 4 | SPURIOUS (1) |
| edge | 3 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **center**. Có 12/20 vật reference nằm ở
  center; L có 5 spurious + 2 missing, M có 4 missing + 7 thừa. Ở **edge**, L khớp cả 3/3, M chỉ thiếu 1 và thừa 1.
  Nhưng center có nhiều lỗi chủ yếu vì có nhiều vật hơn, và các vật đó dồn vào một cụm đông ở frame 271039 (5/7 lỗi L
  của center nằm trong frame này: L2, L3, L7, L14 và cụm auto đỗ), chứ không phải vì vùng giữa ống kính méo hơn.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame:
  (1) Lỗi L ở center là do **đám đông và vật che nhau** (R03/R06: tách hay gộp người, rider bị che nửa người), cộng với
  hai chỗ reference có vấn đề (thiếu auto L7, sai class L1 Truck/Car). (2) Lỗi M lặp lại theo **class**, không theo zone:
  model không có ThreeWheeler nên gán 5 auto thành Car/Truck và box người ngồi trong xe (E4, nhiều ca). (3) Có 4 box L ở
  frame 295948 bị tính IGNORE_SCOPE vì `ego_body` của reference là một hình chữ nhật trùm ra các vật ở xa. Các vật này
  bị loại khỏi phép đếm, nên hàng mid bị đếm thiếu. **Giới hạn:** chỉ có 3 frame từ một camera, n_ref ở edge chỉ là 3.
  Zone là khoảng cách tới tâm vòng kính, không phải khoảng cách tới xe. Không đủ để kết luận edge "dễ" hơn center, cũng
  không suy rộng được cho bốn camera SVM.
