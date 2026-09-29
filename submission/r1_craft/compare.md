# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_236370.jpg
- L1 mid SPURIOUS
- L2 center SPURIOUS
- L3+R4 mid BOX_GEOMETRY
## adasind_258420.jpg
- L2 mid SPURIOUS
- R5 mid MISSING
- R7 mid MISSING
## adasind_310008.jpg

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 4 | 4 | 0 | 1 |
| mid | 9 | 6 | 3 | 3 |
| edge | 7 | 7 | 0 | 0 |
