# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_019560.jpg
- L2 edge SPURIOUS
- L3+R6 mid WRONG_CLASS
- L4 center SPURIOUS
- L5 mid SPURIOUS
- L7 center SPURIOUS
- L10 mid SPURIOUS
- R5 center MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 3 | 2 | 1 | 2 |
| mid | 2 | 1 | 1 | 3 |
| edge | 1 | 1 | 0 | 1 |
