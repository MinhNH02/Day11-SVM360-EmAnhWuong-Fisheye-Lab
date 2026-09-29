# Thành viên và phân vai — Day11 SVM 360 Fisheye

Mô hình: **một repo Public chung**, ba vai cố định A/B/C suốt buổi (không luân phiên). `lab11.py mode` không hiểu vai A/B/C — chỉ vai A chạy lệnh này để chọn slice chung; B/C **không** chạy lại `mode --self <tên mình>` vì sẽ đổi slice đang chọn của hồ sơ chung.

## 1. Thông tin nhóm

- Khóa/lớp: LV2-LV3/D301
- Tên nhóm: EmAnhWuong
- Repo Public: https://github.com/MinhNH02/Day11-SVM360-2A202602074-NguyenHaiMinh-Fisheye-Lab (đã đổi tên hiển thị trên GitHub thành repo chung của nhóm — remote URL git vẫn giữ đường dẫn cũ, GitHub tự chuyển hướng)
- Máy giữ hồ sơ chính / người quản lý: máy của Minh
- Slice chung lấy từ mode.json: **B4-edge**
- Tên định danh vai A dùng cho `--self`: `minh`
- Kênh trao đổi nội bộ: discord
- Đại diện nộp (vai C): Nguyễn Đôn Quốc Tuấn (2A202602127)
- Commit chốt bài: [Điền sau khi chốt nộp]

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Nguyễn Hải Minh | 2A202602074 | `minh` | Tạo task, vẽ parking/C0/slice, self-QC, export, lock và rework | `submission/parking/`, `submission/p1_calib/` (lock 717A-5D16), `submission/r1_craft/` (lock C556-4C4B, `selfqc.md`), `submission/rework/` (lock2 D534-34D9); dòng `r1_craft` trong `findings.csv`; commit `<SHA>` |
| B · QA độc lập | Nguyễn Thế Anh | 2A202602138 | `theanh` | Review bản A đã khóa theo ảnh/guideline, chưa xem reference/model | `submission/r2_qa/qa_review.md`, 4 dòng `r2_qa` trong `findings.csv`, `submission/screenshots/qa_*.png` (3 ảnh), mục kiểm lại sau rework trong `qa_review.md`; commit `<SHA>` |
| C · Chẩn đoán & điều phối | Nguyễn Đôn Quốc Tuấn | 2A202602127 | `tuan` | Chạy báo cáo sau QA, phân xử, lập hồ sơ, kiểm và nộp bài | `submission/r3_diag/` (local quality, model compare, IoU sweep, nhận xét zone table), 22 dòng `r3_diag`, `40_decision_log.csv`, `10_error_card.md`, `20_guideline_patch.md`, `30_escalation_ticket.md`, `45_*`, `46_*`, `50_exit_ticket.md`, `manifest.json`; commit `<SHA>` |

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | mode.json (slice B4-edge), phân vai ở mục 2 | A: chạy `status` thấy đúng slice B4-edge; B: đọc `sensor_context.md` và `observations.md`, đối chiếu vạch với ảnh contrast | Doctor ✓, CVAT đang chạy |
| P2 · Khóa bản đầu | A → B, C | `r1_craft/annotations.xml`, `lock.txt`, slice B4-edge, mã **C556-4C4B** (C0 calib: 717A-5D16) | B: chạy `qa --code C556-4C4B` khớp mã, đủ 3 frame; C: đối chiếu slice trong `lock.txt` với `mode.json` | Khóa một lần, không relock |
| P3 · Chốt QA mù | B → C, A | `r2_qa/qa_review.md`, 4 dòng r2_qa trong findings, 3 ảnh `screenshots/qa_*.png` | C: kiểm mỗi nhận xét có frame/object_ref/rule_id/ảnh; `triage` hợp lệ; chưa mở reference trước mốc này | 2 ca chuyển C phân xử (L3/L4 frame 236370, L2 frame 258420) |
| P4 · Quyết định sửa | C → A, B | 22 dòng r3_diag, `40_decision_log.csv` (D1–D7), `20_guideline_patch.md`, `30_escalation_ticket.md` | A: lọc findings `action=rework` được 5 ca và mở lại từng ca trên ảnh; B: đối chiếu quyết định với nhận xét gốc, không sửa nhận xét ban đầu | 2 escalated (R03a, model ThreeWheeler), 1 mở (D7) |
| P5 · Kiểm bản sửa | A → B → C | `rework/annotations-v2.xml`, `lock2.txt` mã **D534-34D9**, `rework/delta.md` | B: kiểm lại 5 ca rework + ca D6 trên bản v2 (ghi trong `qa_review.md`); C: đọc `delta.md`, số khớp với quyết định sửa | mid: matched 6→9, missing 3→0, spurious 3→2 |
| P6 · Chốt nộp | A, B → C | `manifest.json` (failed_gates rỗng), commit `<SHA>` | A: nhãn/lock đúng phiên bản; B: 3 ảnh screenshots mở được, dẫn đúng nhận xét/ticket; C: `check` exit 0, repo Public | Còn mở D7, đã ghi người theo dõi |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử: Frame adasind_236370, L2 + L3 (R02). B nhận xét box L3 ôm lệch, có thể gộp hai xe. A ban đầu cho rằng L2 và L3 là hai xe khác nhau. C mở ảnh phóng to và đối chiếu reference R4 và model M1, thấy chỉ có một xe máy. Quyết định: gộp thành một box Bike (595,830)-(885,1018) ở bản v2 (`40_decision_log.csv` D1, ảnh `submission/screenshots/qa_236370_rider_bikes.png`). Ca liên quan về người đứng cầm ghi đông (L1) không thống nhất được với reference nên chuyển guideline (D4, `20_guideline_patch.md` R03a).
- Ca còn mở: D7, frame adasind_236370. Chưa chắc cậu bé L4 có dắt thêm một xe đạp riêng hay không. Người theo dõi: Nguyễn Thế Anh (B). Phép kiểm tiếp theo: xem các frame liền kề trong video gốc ADASIND; nếu thấy rõ xe đạp thứ hai thì thêm box Bike và ghi finding mới.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: C soạn `45_sampling_plan.csv`, `46_gold_set_plan.md`, `45_review_plan.md` và câu 1–2 exit ticket; A viết câu 3 exit ticket và phần gán nhãn trong `10_error_card.md`; B soát bằng chứng ảnh và góp ý hard case cho kế hoạch bốn camera. Cả ba đọc lại trước khi chốt.
- Thay đổi phân công nếu có: Không đổi; ba vai giữ nguyên suốt buổi.

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Nguyễn Hải Minh — `r1_craft/lock.txt` C556-4C4B, `rework/lock2.txt` D534-34D9
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Thế Anh — đã đọc và xác nhận `r2_qa/qa_review.md`, gồm mục kiểm lại sau rework (bản v2 D534-34D9)
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Nguyễn Đôn Quốc Tuấn — đã đọc `r2_qa/qa_review.md` và đối chiếu với `findings.csv`, `40_decision_log.csv`; `check` exit 0, `manifest.json` failed_gates rỗng
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.

Chỉ đánh dấu việc đã kiểm thật. Nhóm nộp một hồ sơ chung; `check` không tự chấm đóng góp từng người.
