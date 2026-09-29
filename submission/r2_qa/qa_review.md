# QA review · B4-edge

Mã khóa: C556-4C4B

- Reviewer độc lập (vai B): Thế Anh
- Chủ nhãn (vai A): Nguyễn Hải Minh
- Slice: B4-edge (adasind_236370, adasind_258420, adasind_310008)
- File đã soát: `submission/r1_craft/annotations.xml`, mã khóa khớp `lock.txt`
- Soát trên `qa_overlay.html` và ảnh gốc, **chưa mở reference hay output model** của slice này.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_236370.jpg | L3 | R02 | Box Bike (660,830)-(890,1015) đang ôm hai xe: xe máy mà người áo xám L1 cầm ghi đông, và chiếc xe đạp đứng trước cậu bé L4 (bánh trước ở khoảng x 815–880). Cần tách thành hai box theo từng xe. Ảnh: `screenshots/qa_236370_rider_bikes.png` |
| adasind_236370.jpg | L4 | R03 | Cậu bé áo xanh đang cầm tay lái xe đạp, không ngồi lên → theo R03 phải có Pedestrian **và** một Bike riêng. Hiện chiếc xe đạp này không có box riêng (bị gộp vào L3). Cần kiểm lại khi tách L3 |
| adasind_258420.jpg | L2 | R04 | Xe tối màu, cao, phía sau hai xe máy; chỉ thấy phần thùng phía sau, không thấy đầu xe. Nhãn Truck hợp lý theo dáng thùng, nhưng A tự ghi là chưa chắc; tôi cũng không loại trừ được Bus. Đề nghị giữ và đưa vào ca cần phân xử. Ảnh: `screenshots/qa_258420_far_truck.png` |
| adasind_310008.jpg | L3 | R02 | Nhóm 4 người đi bộ tách được từng người nên vẽ từng box là đúng, không cần `crowd_or_group`. Riêng L3 đáy box (y 1036) kéo xuống cả phần bóng dưới chân, thấp hơn gót khoảng 10–15 px; nên kéo lên sát chân. Ảnh: `screenshots/qa_310008_group.png` |
| adasind_310008.jpg | L2, L4 | R05 | Đã kiểm thuộc tính: L2 và L4 bị người đi cạnh che một phần → `occluded=true` là đúng. Không có vật nào bị vòng kính cắt trong nhóm này. Không cần sửa |

Các điểm tôi thấy ổn: `lens_border` khớp vòng kính cả 3 frame; `ego_body` có ở cả 3 frame và phủ đúng chân/tay người lái; hai xe máy đang chạy frame 258420 (L1, L7) là một box Bike gồm cả người lái, đúng R03; không thấy box nào nằm quá nửa trong vùng ignore.

Ca chưa rõ chuyển cho C phân xử: L3/L4 frame 236370 (tách xe) và L2 frame 258420 (Truck hay Bus).

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.

## Kiểm lại sau rework (bản v2, mã khóa D534-34D9)

Soát trên `submission/rework/annotations-v2.xml` sau khi A sửa, chỉ các ca đã quyết định trong decision log.

| Quyết định | frame | object | Kết quả kiểm lại |
|---|---|---|---|
| D1 | adasind_236370.jpg | Bike gộp từ L2 + L3 | Đạt: còn một box Bike (595,830)-(885,1018), ôm trọn xe máy, không còn box thừa L2 |
| D2 | adasind_258420.jpg | Car (R5) | Đạt: đã thêm box Car (219,782)-(257,824) cho xe con trắng phía xa |
| D2 | adasind_258420.jpg | Pedestrian (R7) | Đạt: đã thêm box Pedestrian (92,792)-(109,843) |
| D6 | adasind_310008.jpg | L3 Pedestrian | Đạt: đáy box kéo lên y 1024, sát gót chân, không còn ôm bóng |
| D7 | adasind_236370.jpg | L4 + xe đạp | Chưa đóng: vẫn chờ xem frame liền kề, không đổi nhãn ở v2 |

Không phát hiện thay đổi nào ngoài các ca trên. Delta ở `rework/delta.md` khớp: zone mid matched 6→9, missing 3→0.


