# ASK 01 — bộ hỏi cho OxAlpha — tệp 45/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 221 (BP-A3-05)

**Mục BP-A3-05 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A3-05: (a) `dtc>remain` ⇒ trả cận HOÀ, không bỏ hẳn — «lỗi thật»; (b) mate-score qua TT, GHI với chiếu mãi; (c) KHÔNG B…». Bằng chứng của đội: đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 222 (BP-A3-07)

**Mục BP-A3-07 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A3-07: Nguy cơ va chạm khoá theo bộ đếm; luật ở search không ở eval; 1 hằng `RULE_NO_CAPTURE_PLY` + CI quét literal». Bằng chứng của đội: đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 223 (BP-ASK12-2(0.2))

**Mục BP-ASK12-2(0.2) — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «ASK12-2 (0.2): Tách hàng đợi ↔ kho đúng; UNIQUE trên `vkey`; chuyển dòng trong 1 giao dịch». Bằng chứng của đội: `Backend.cs:4323` `datanoscore(... PRIMARY KEY(vkey,vmove)) WITHOUT ROWID` ⇒ khoá là (vkey,vmove), UNIQUE(vkey) đơn sẽ phá lược đồ. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 224 (BP-ASK12-3(0.3))

**Mục BP-ASK12-3(0.3) — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «ASK12-3 (0.3): «bind_double KHÔNG phải gốc; cột không có INTEGER affinity; bảng không có UNIQUE» + 7 phép thử SQLite 3.…». Bằng chứng của đội: `Backend.cs:351-359`: REAL là **bit-pattern** int64 (vd −5,63e−279), không phải số nguyên chính xác ⇒ affinity không đổi; PK(vkey,vmove) có sẵn nhưng REAL≠INT…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 225 (BP-ASK12-5(0.5)/K4)

**Mục BP-ASK12-5(0.5)/K4 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «ASK12-5 (0.5)/K4: 3 tầng T1–T3, T2 hỏng ⇒ chặn T3; template matching lớp 1; ngưỡng «đã bắt» = 32 quân + đối xứng loại +…». Bằng chứng của đội: `BocCoManHinh.cs:1447 BocOnDinh(soLanGiong)` (đã có 2 khung); `:1289-1313` thừa quân từng loại (đã có); `TuChoi.cs:70-75 TheVuaHoc` (khung đầu). S27 xem §B. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
