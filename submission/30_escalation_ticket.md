# Escalation ticket

## Ticket 1

- **Frame:** adasind_258420.jpg (M4, M8, M11) và adasind_310008.jpg (M6, M7) — slice B4-edge
- **Ảnh chụp:** `screenshots/qa_258420_far_truck.png` (vùng xe ba bánh L3 bên trái), đối chiếu thêm `r3_diag/model_compare.html` và `r3_diag/local_quality_confusion.csv`
- **Expected impact:** Model YOLO26m đóng băng không ra được class `ThreeWheeler`: 5/5 xe ba bánh trong slice bị gán Truck (M4, M8, M6), Car (M11) hoặc vừa Truck vừa Bus trùng khít một chỗ (M6 + M7). Nếu dùng output này làm pre-label, mọi xe ba bánh sẽ sai class và người vẽ phải sửa tay; nếu dùng để đánh giá, recall ThreeWheeler của model gần như bằng 0 ở slice này. Ngoài ra model tách người lái khỏi xe máy (M5, M7, M10 frame 258420), trái R03, sinh thêm box thừa ở zone mid.
- **Owner:** `ai_team`
- **Recommendation:** Không sửa nhãn người để khớp model. Đề nghị ai_team (1) thêm bước ánh xạ class hoặc fine-tune có class ThreeWheeler trước khi dùng model làm pre-label cho dữ liệu đường Ấn Độ; (2) gộp box người lái vào box xe theo R03 ở bước hậu xử lý; (3) lọc box trùng khác class trên cùng vị trí (IoU > 0,9). Kiểm lại trên nhiều slice có xe ba bánh (không chỉ 3 frame này) trước khi kết luận tỉ lệ lỗi.

## Ticket 2

- **Frame:** adasind_236370.jpg (object L1 + L3, reference R4)
- **Ảnh chụp:** `screenshots/qa_236370_rider_bikes.png`
- **Expected impact:** Cùng một người đứng cầm ghi đông xe máy được A/B đếm là Pedestrian + Bike, reference đếm là một Bike, model đếm Pedestrian + Bike. Nếu không có luật rõ, ca này luôn bị tính SPURIOUS hoặc MISSING tùy người vẽ, làm nhiễu số ở zone mid.
- **Owner:** `guideline`
- **Recommendation:** Duyệt đề xuất R03a trong `20_guideline_patch.md` (bump v1.1.0), sau đó sửa lại vật R4 trong teaching reference cho khớp luật mới.
