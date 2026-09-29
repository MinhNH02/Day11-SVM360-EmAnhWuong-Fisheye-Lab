# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | MISSING | 2 |
| center | B4 | SPURIOUS | 4 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 2 |
| edge | B4 | ATTRIBUTE | 1 |
| edge | B4 | BOX_GEOMETRY | 2 |
| edge | B4 | MISSING | 1 |
| edge | B4 | SPURIOUS | 1 |
| edge | C0 | SPURIOUS | 1 |
| mid | B4 | BOX_GEOMETRY | 2 |
| mid | B4 | MISSING | 9 |
| mid | B4 | SPURIOUS | 12 |
| mid | B4 | WRONG_CLASS | 2 |
| mid | C0 | SPURIOUS | 2 |
| mid | C0 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 22 (ví dụ frame adasind_019560.jpg)
- MISSING: 13 (ví dụ frame adasind_019560.jpg)
- BOX_GEOMETRY: 4 (ví dụ frame adasind_236370.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

Lỗi nổi bật nhất là SPURIOUS/MISSING dồn ở ô **mid × B4** (12 SPURIOUS, 9 MISSING). Tách ra thì đây là hai nhóm khác nhau, không phải một lỗi:

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:
  - Phần lớn SPURIOUS ở mid là của model (M_only frame 258420: M3, M5, M7, M10 là người lái bị tách khỏi xe; M4, M8, M11 là xe ba bánh bị gán Truck/Car). Tôi xếp `E4_model_domain` vì cùng một kiểu sai lặp lại ở mọi xe ba bánh (5/5) và mọi xe máy có người lái trong slice, trong khi nhãn L và R ở các vật này khớp nhau.
  - Ba ca MISSING/SPURIOUS thật sự của người vẽ là `E1_annotator_error`: frame 258420 bỏ sót xe con trắng R5 (cao 42 px) và người đi bộ R7 (51 px) vì tôi ước lượng nhầm là dưới ngưỡng 40 px; frame 236370 cắt một xe máy thành hai box L2 + L3.
  - Một ca còn lại (frame 236370 L1+M2, người đứng cầm ghi đông) là `E2_guideline_gap`: R03 chỉ nói "người lái" và "người dắt", không nói ca đứng sát xe chân chạm đất.
- Cách sửa và ai nhận việc (`owner`): A (`annotator`) đã sửa 5 ca rework ở `rework/annotations-v2.xml` — mid từ 6 matched / 3 missing / 3 spurious lên 9 / 0 / 2 (`rework/delta.md`). Ca người đứng cầm ghi đông chuyển `guideline` qua `20_guideline_patch.md`. Nhóm lỗi model chuyển `ai_team` qua `30_escalation_ticket.md`; không sửa nhãn cho khớp model.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/qa_236370_rider_bikes.png` (R02/R03, dòng r3_diag L2, L3+M1, R4, L1+M2), `screenshots/qa_258420_far_truck.png` (R01/R04, dòng r3_diag R5, R7+M12, L2), `r3_diag/model_compare.md` và `local_quality_confusion.csv` cho nhóm ThreeWheeler → Truck/Car/Bus.
