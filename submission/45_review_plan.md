# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Zone mid, cụm người + xe hai bánh chồng nhau (adasind_236370 quanh L1/L3/L4; adasind_258420 các xe máy phía xa) | 7 ca: 3 E1 của người vẽ (cắt một xe thành hai box, bỏ sót 2 vật sát ngưỡng 40 px), 1 E2 thiếu luật rider, 1 E5 xe đạp chưa chắc, 2 nhận xét QA | Đây là chỗ cả người, reference và model đếm khác nhau nhiều nhất (mid: L thiếu 3 thừa 3 trên 9 box reference trước rework). Lỗi ở đây đổi hẳn số đếm vật, không chỉ lệch vài pixel | Ảnh phóng to từng cụm (`screenshots/qa_236370_rider_bikes.png`, `qa_258420_far_truck.png`), dòng findings r2_qa và r3_diag, quyết định D1, D2, D4, D7 trong decision log |
| Mọi frame có xe ba bánh (ThreeWheeler), cả ba zone | 5/5 xe ba bánh bị model gán sai class (Truck, Car, Bus trùng box) + 1 ca calib C0 người vẽ nhầm Truck/ThreeWheeler | Nếu dùng model làm pre-label thì toàn bộ class này sai; người vẽ cũng từng nhầm ở C0, nên đây là class dễ lệch cả người lẫn máy | `r3_diag/model_compare.md`, `local_quality_confusion.csv`, ticket 1 trong `30_escalation_ticket.md`, dòng calib L3+R6 |

Giới hạn của kết luận từ ba frame ADASIND: Ba frame, một camera, ban ngày, khoảng 20 vật reference; mỗi ô zone chỉ vài vật nên một box sửa là đổi kết luận. Slice tên "edge" nhưng vật edge lại là nhóm người đi bộ to và rõ, nên số edge đẹp không chứng minh vùng rìa dễ. Teaching reference cũng có lỗi (bỏ sót xe tải L2 frame 258420), nên "khớp reference" chưa phải là đúng.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: Khi rút 200 frame, lấy tối đa một frame cho mỗi đoạn cảnh (ví dụ cách nhau ít nhất vài giây hoặc khác chuyến đi), rồi đếm độ phủ theo cảnh chứ không theo frame: mỗi ô camera × normal/hard phải có đủ số cảnh khác nhau, có xe ba bánh, cụm người + xe máy, ban đêm và vật sát rìa vòng kính. Frame liền nhau cùng cảnh gần như lặp lại cùng vật nên đếm là một ca. Vì hard slice được chọn cố ý dồn vào chỗ khó (35/200 cho front hard, 30 cho rear hard...), tỉ lệ lỗi đo trên 200 frame này sẽ cao hơn thực tế và không suy ra được tỉ lệ lỗi của 50.000 frame; muốn đo tỉ lệ phải có thêm một mẫu ngẫu nhiên riêng.
