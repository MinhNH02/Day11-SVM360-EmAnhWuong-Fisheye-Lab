# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao? Không phải `DUPLICATE`. `DUPLICATE` là hai box cho cùng một vật **trên cùng một ảnh**. Ở seam, mỗi camera là một ảnh riêng, nên vẽ box ở cả hai camera là đúng với từng ảnh (có thể là edge ở camera này, mid ở camera kia). Cái cần là một quy tắc riêng cho bước sau: giữ cả hai box theo từng camera khi gán nhãn, rồi quyết định ghép hay không ở tầng hợp nhất (BEV / đầu ra chung), dựa trên timestamp, calibration và policy output. Nếu gán nhãn mà tự xóa một box để "khỏi trùng" thì camera kia sẽ bị tính thiếu vật.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera. Giữ cùng track ID khi vẫn là một vật và còn thấy được liên tục, kể cả khi bị che một phần vài frame. Thêm keyframe khi hình học đổi nhiều (vật vào sát rìa vòng kính bị méo, bị che rồi hiện lại, đổi hướng) để nội suy không trôi. Đặt Outside khi vật ra khỏi vòng kính hoặc bị che hoàn toàn; khi quay lại mới bật lại cùng ID nếu chắc là cùng vật. Trước khi nối track qua hai camera cần: timestamp đồng bộ giữa hai camera, calibration ngoại (vị trí tương đối hai camera) để chiếu vị trí về chung một hệ, và policy nói đầu ra giữ track riêng từng camera hay một track chung. Thiếu một trong ba thì để hai track riêng và ghi chú seam.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm? Frame adasind_258420, L2: tôi vẽ một Truck ở xa mà reference và model đều không có, QA (B) còn nghi class. Nhóm không xóa cho khớp reference mà phóng to lại ảnh, thấy rõ đuôi xe tải thùng tối màu cao 66 px, nên ghi `E0_reference_defect`, giữ box với lý do (D3). Ngược lại ở frame 236370 tôi tin L2 và L3 là hai xe, nhưng khi so với reference, model và nhận xét của B thì đó là một xe máy tôi cắt đôi — tôi nhận lỗi và gộp lại ở bản v2. Nếu làm lại: đo chiều cao bằng công cụ của CVAT thay vì ước lượng bằng mắt cho vật sát ngưỡng 40 px (đã bỏ sót hai vật 42 và 51 px), và với cụm người + xe chồng nhau thì phóng to, vẽ từng xe một trước rồi mới vẽ người.

Đóng góp: A (Minh) viết câu 3 và phần gán nhãn nêu trong đó; B (Thế Anh) soát lại bằng chứng QA; C (Tuấn) tổng hợp câu 1–2 sau khi cả nhóm đọc `docs/10-svm360-reading-vi.md`. Cả ba cần xác nhận lại trước khi nộp.
