# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_270517.jpg
- L4 mid SPURIOUS
## adasind_271039.jpg
- L9 mid IGNORE_SCOPE
- L2+R8 center BOX_GEOMETRY
- L3 center SPURIOUS
- L7 center SPURIOUS
- L8+R6 mid BOX_GEOMETRY
- L14 center SPURIOUS
## adasind_295948.jpg
- L2 mid IGNORE_SCOPE
- L3 mid IGNORE_SCOPE
- L4 mid IGNORE_SCOPE
- L7 mid IGNORE_SCOPE
- L1+R1 center WRONG_CLASS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 12 | 10 | 2 | 5 |
| mid | 5 | 4 | 1 | 2 |
| edge | 3 | 3 | 0 | 0 |
