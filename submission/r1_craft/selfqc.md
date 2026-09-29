# Tự soát


## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

Ghi chú khi soát:
- Phạm vi: người/xe nhỏ ở cuối đường frame 258420 (cao dưới 40 px) không vẽ. Người áo đỏ bên trái frame 236370 cao khoảng 68 px nên có vẽ.
- lens_border: 2 polygon import sẵn mỗi frame khớp vòng kính, không sửa. ego_body: vẽ ở cả 3 frame (chân + dép frame 236370, tay và thân người lái frame 258420 và 310008); frame 310008 bản nháp vẽ hẹp quá, đã nới ra cho hết cánh tay.
- Class: xe tải thùng bạt → Truck; các xe vàng đen ba bánh → ThreeWheeler; xe bán tải trắng phía xa frame 258420 tạm gán Truck, chưa chắc.
- Rider: người mặc áo xám frame 236370 đang đứng dắt xe máy nên tách Pedestrian + Bike; hai xe máy đang chạy frame 258420 là một box Bike gồm cả người lái.
- Geometry: prefill Bike và Pedestrian frame 236370 bị hụt một chút, đã kéo lại theo phần nhìn thấy; không nắn thẳng vật ở rìa.
- truncated/occluded: xe con sát mép trái frame 236370 bị biên ảnh cắt → truncated; người đi sau người khác trong nhóm 4 người frame 310008 → occluded.
- K12: vẽ polygon theo viền cho 4 vật lệnh cvat gợi ý, group với box tương ứng; fill ratio vùng edge thấp hơn center (0,82 so với 0,96).

## Fill ratio (K12)
- adasind_236370.jpg box 6 edge: 0.837
- adasind_236370.jpg box 5 center: 0.959
- adasind_310008.jpg box 2 edge: 0.812
- adasind_310008.jpg box 3 edge: 0.810
mean edge: 0.820 (n=3)
mean center: 0.959 (n=1)
