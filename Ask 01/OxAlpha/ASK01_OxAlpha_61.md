# ASK 01 — bộ hỏi cho OxAlpha — tệp 61/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 301 (T13-23)

### T13-23 — Phần I bổ sung (lô B2, B4–B9): DeepSeek — SAI 2 · BỊA 0
Hỏi lại **đúng DeepSeek**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | DeepSeek nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| DS-A3-05 | **SAI** | A3-05: (a) đoạn «diff» `dungDuoc` + «return VALUE_DRAW EXACT khi wdl≠0 && dtc>remain»; (b) 3 lỗi kinh điển: mate qua TT… | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| DS-A3-07 | **SAI** | A3-07: (a) 14/120 = 11,7 % ↔ 14/80 = 17,5 %; **«rule60 = 6 và 14 cùng hash»**; dịch nấc về 10 hoặc /4, đo 100 ván; (b)… | `ThanDieuDaiHiep/src/types.h:383 make_key(0) = 1442695040888963407 ≠ 0` ⇒ rule60 ∈ [0,13] dùng `k`, [14,21] dùng `k ^ make_key(0)` ⇒ 6 và 14 **khác** hash; đún… | ASK01_2609 |

---

## Câu 302 (DS-A3-05)

**Mục DS-A3-05 — T13-23 — Phần I bổ sung (lô B2, B4–B9): DeepSeek (SAI):** AI T13-23 — Phần I bổ sung (lô B2, B4–B9): DeepSeek nói: «A3-05: (a) đoạn «diff» `dungDuoc` + «return VALUE_DRAW EXACT khi wdl≠0 && dtc>remain»; (b) 3 lỗi kinh điển: mate qua TT…». Bằng chứng của đội: đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 303 (DS-A3-07)

**Mục DS-A3-07 — T13-23 — Phần I bổ sung (lô B2, B4–B9): DeepSeek (SAI):** AI T13-23 — Phần I bổ sung (lô B2, B4–B9): DeepSeek nói: «A3-07: (a) 14/120 = 11,7 % ↔ 14/80 = 17,5 %; **«rule60 = 6 và 14 cùng hash»**; dịch nấc về 10 hoặc /4, đo 100 ván; (b)…». Bằng chứng của đội: `ThanDieuDaiHiep/src/types.h:383 make_key(0) = 1442695040888963407 ≠ 0` ⇒ rule60 ∈ [0,13] dùng `k`, [14,21] dùng `k ^ make_key(0)` ⇒ 6 và 14 **khác** hash; đún…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 304 (T13-24)

### T13-24 — Phần I bổ sung (lô B2, B4–B9): Astra 6 — SAI 1 · BỊA 0
Hỏi lại **đúng Astra 6**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Astra 6 nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| AS6-D5 | **SAI** | D-5: khoá khác nhau chưa đủ — TT có thể trả mate/bound của mốc cũ; thử bộ đếm 0/72/76/79 vs mốc 400, có TT tắt | A01-16 ĐÓNG 27/09 23:0x: nấc /4 vs /8 ab 0 [−36; +36]; lõi đã hạ mate TT theo ply còn tới mốc + không cắt TT khi bộ đếm ≥ mốc − 4 (D5, `search.cpp:1938,2929,29… | ASK02_2709 |

---

## Câu 305 (AS6-D5)

**Mục AS6-D5 — T13-24 — Phần I bổ sung (lô B2, B4–B9): Astra 6 (SAI):** AI T13-24 — Phần I bổ sung (lô B2, B4–B9): Astra 6 nói: «D-5: khoá khác nhau chưa đủ — TT có thể trả mate/bound của mốc cũ; thử bộ đếm 0/72/76/79 vs mốc 400, có TT tắt». Bằng chứng của đội: A01-16 ĐÓNG 27/09 23:0x: nấc /4 vs /8 ab 0 [−36; +36]; lõi đã hạ mate TT theo ply còn tới mốc + không cắt TT khi bộ đếm ≥ mốc − 4 (D5, `search.cpp:1938,2929,29…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
