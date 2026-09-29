# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 1 | 2 | 2 | SPURIOUS (1) |
| mid | 9 | 3 | 3 | 4 | 9 | SPURIOUS (2) |
| edge | 7 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: Cả hai cùng gãy ở **mid**. Ở mid, L thiếu 3 và thừa 3 trên 9 box reference, còn M bỏ sót 4 và thừa 9. Center gần như sạch (L thừa 1, M thiếu 2 thừa 2) và edge của L khớp cả 7/7. Tức là dù slice tên "edge", chỗ khó thật lại là dải mid ở giữa bán kính vòng kính, nơi tập trung vật nhỏ ở xa và cụm người + xe chồng lên nhau.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: Với L, lỗi mid không do méo mà do vật nhỏ sát ngưỡng 40 px (xe con 42 px, người 51 px bị bỏ) và cụm xe máy + người đứng cạnh ở frame 236370 bị cắt sai thành hai box. Với M, phần lớn lỗi là lỗi miền: model không có khái niệm ThreeWheeler (5/5 xe ba bánh bị gán Truck/Car/Bus) và tách người lái khỏi xe máy, nên nhiều box "thừa" thực ra là cách đếm khác R03 chứ không phải nhìn nhầm. Edge ít lỗi hơn dự đoán có thể chỉ vì ở slice này vật edge là nhóm người đi bộ to và rõ. Ba frame, một camera, khoảng 20 vật reference là quá ít để kết luận zone nào khó hơn cho cả hệ SVM; mỗi ô trong bảng chỉ vài vật nên đổi một box là đổi hẳn kết luận.
