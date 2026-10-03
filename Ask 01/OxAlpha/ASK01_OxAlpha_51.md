# ASK 01 — bộ hỏi cho OxAlpha — tệp 51/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 251 (LC-A3-01)

**Mục LC-A3-01 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A3-01: (a) nén thang; (b) mate vào WDL, search lo DTM; (c) UCI mate = NƯỚC, chessdb = ply; **(d) `cp = max(0, 3000 − 5×…». Bằng chứng của đội: (c) trùng SB-2 (+1 phiếu). (d) **thiếu dấu bên thua**, không kẹp dưới, mâu thuẫn chính (b) của nó; dtm > 600 ⇒ 0 = hoà. Nguồn Fairy-Stockfish repo không trỏ dò…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 252 (LC-A3-05)

**Mục LC-A3-05 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A3-05: cận hoà khi dtc > remain; cẩn thận graph-history; chặn chiếu/đuổi mãi». Bằng chứng của đội: đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 253 (LC-A3-07)

**Mục LC-A3-07 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A3-07: 120→80 «có thể» va chạm TT; tăng eval theo dtc». Bằng chứng của đội: đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 254 (LC-A3-10)

**Mục LC-A3-10 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A3-10: «M-nnnn đếm từ thế hiện tại»; hạn API không biết». Bằng chứng của đội: đo D3 27/09 (3 lượt queryall): mặc định egtbmetric=dtm trả W-M-0008 (CHẴN) cho bên đang thắng ⇒ không thể là DTM tính từ thế hiện tại; dtc cho W-00-009 lẻ, khớ…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 255 (LC-A3-11)

**Mục LC-A3-11 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A3-11: thầy báo mate giả; trọng số theo đồng thuận; **(c) «UCCI mate = nửa nước»**». Bằng chứng của đội: họ Stockfish/Pikafish in `mate` theo **NƯỚC** — `pikafish_cf_rebuild/src/uci.cpp:552-553` `(plies+1)/2` (SB kiểm), Fairy `uci.cpp:489`; engine TQ thương mại **…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
