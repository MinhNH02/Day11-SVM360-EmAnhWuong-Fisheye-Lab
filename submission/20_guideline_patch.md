# Guideline patch

- **Rule mới đề xuất:** R03a — Người đứng sát xe hai bánh, **chân chạm đất**, tay cầm ghi đông nhưng không ngồi trên yên được coi là người dắt xe: vẽ một box `Pedestrian` cho người và một box `Bike` cho xe (theo phần nhìn thấy của riêng chiếc xe). Chỉ khi người ngồi trên yên hoặc một chân đã gác lên xe mới gộp thành một `Bike` duy nhất. Khi không nhìn thấy chân (bị che), ghi `occluded=true` và mặc định coi là người lái, ghi rõ trong `note`.
- **Áp dụng cho:** class `Bike` và `Pedestrian`, cả ba zone; liên quan R03 (rider) và R02 (bám phần nhìn thấy).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R03 chỉ tách hai ca "người lái" và "người dắt (không ngồi lên)". Ở frame adasind_236370 (object L1 + L3, xem `screenshots/qa_236370_rider_bikes.png`) người áo xám đứng cạnh xe máy, tay cầm ghi đông, chân chạm đất: A và QA (B) đọc là người dắt → Pedestrian + Bike; teaching reference gộp thành một Bike (595,731)-(882,1018) như rider; model YOLO26m lại tách người riêng. Ba nguồn ra ba cách đếm khác nhau cho cùng một cảnh nên phép so L/R ở vật này không còn đo được đúng sai.
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round `rework` của slice này trở đi, sau khi Lab Coach duyệt; teaching reference cần sửa lại vật R4 frame adasind_236370 theo luật mới rồi mới dùng để chấm các slice sau.
