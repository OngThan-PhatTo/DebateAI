# ASK 01 — bộ hỏi cho OxAlpha — tệp 44/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 216 (B8-MSA-02)

**Mục B8-MSA-02 — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: ««Nút duy nhất nhận mọi nguồn» (ảnh/SVG/FEN)». Bằng chứng của đội: OneBtn_CSharp5.cs:6-11: Img.ChupVungVaNhan/NhanTuAnh trả "" conf 0; Vec.ParseSvg trả "" với conf = 1.0 ⇒ khung giả, nhánh SVG tự tin 100 % với kết quả rỗng (PA…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 217 (T13-12)

### T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle — SAI 11 · BỊA 1 (phần 1/2)
Hỏi lại **đúng BigPickle**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | BigPickle nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| C18-BP-02 | **BỊA** | Link HF «HF gated» pony-diffusion-v6/pony-diffusion-v6 và lllyasviel/control_v11f1p_sd15_clip | HF API 02/10 → 401 cả hai (repo gated vẫn trả 200 metadata, vd FLUX.1-dev 200 gated=auto); repo thật gần tên: lllyasviel/control_v11f1p_sd15_depth 200; 22/24 l… | COMFY18_2909 |
| BP-0.2b/0.2c | **SAI** | 0.2b/0.2c: COUNT(DISTINCT) đắt; `PRAGMA count_changes` cho changes(); canon FEN bỏ số nước | `Shared/DataFen/nap_fendb.py:85-126` `khoa_the` = `chessdb_nhan.chuan_hoa_fen` (bỏ số nước) | ASK01_2609 |
| BP-A2-0105A2-35 | **SAI** | A2-01…05, A2-35: Template matching lớp 1; 2 điểm đủ nếu cùng hàng, điểm 3 khi méo; full/one; hồ sơ JSON 9 trường; 4 khú… | hồ sơ v2 đã có (`BocCoManHinh.cs:1027`) | ASK01_2609 |
| BP-A3-05 | **SAI** | A3-05: (a) `dtc>remain` ⇒ trả cận HOÀ, không bỏ hẳn — «lỗi thật»; (b) mate-score qua TT, GHI với chiếu mãi; (c) KHÔNG B… | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| BP-A3-07 | **SAI** | A3-07: Nguy cơ va chạm khoá theo bộ đếm; luật ở search không ở eval; 1 hằng `RULE_NO_CAPTURE_PLY` + CI quét literal | đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma… | ASK01_2609 |
| BP-ASK12-2(0.2) | **SAI** | ASK12-2 (0.2): Tách hàng đợi ↔ kho đúng; UNIQUE trên `vkey`; chuyển dòng trong 1 giao dịch | `Backend.cs:4323` `datanoscore(... PRIMARY KEY(vkey,vmove)) WITHOUT ROWID` ⇒ khoá là (vkey,vmove), UNIQUE(vkey) đơn sẽ phá lược đồ | ASK01_2609 |
| BP-ASK12-3(0.3) | **SAI** | ASK12-3 (0.3): «bind_double KHÔNG phải gốc; cột không có INTEGER affinity; bảng không có UNIQUE» + 7 phép thử SQLite 3.… | `Backend.cs:351-359`: REAL là **bit-pattern** int64 (vd −5,63e−279), không phải số nguyên chính xác ⇒ affinity không đổi; PK(vkey,vmove) có sẵn nhưng REAL≠INT… | ASK01_2609 |
| BP-ASK12-5(0.5)/K4 | **SAI** | ASK12-5 (0.5)/K4: 3 tầng T1–T3, T2 hỏng ⇒ chặn T3; template matching lớp 1; ngưỡng «đã bắt» = 32 quân + đối xứng loại +… | `BocCoManHinh.cs:1447 BocOnDinh(soLanGiong)` (đã có 2 khung); `:1289-1313` thừa quân từng loại (đã có); `TuChoi.cs:70-75 TheVuaHoc` (khung đầu). S27 xem §B | ASK01_2609 |
| BP-A2-07 | **SAI** | A2-07: bài BigPickle — sai số học | ô «◐ (**sai số học**)» | ASK02_A2_2309 |
| BP-A2-21 | **SAI** | A2-21: bài BigPickle — 6.200 sai | ô «◐ (**6.200 sai**)» | ASK02_A2_2309 |
| BP-AUTO-VERIFY | **SAI** | A2-02: tự xác nhận bằng cách đọc lại 32 quân thế khai cuộc trên khung vừa học | §8.6 `DEBATE_AUTOPLAY_NHAN_BAN_CO_2026-09-21.md:167`: đọc lại trên CHÍNH khung vừa học là tự-so-với-mình — đo được `up1` (cờ úp) vẫn PASS với nhãn cờ thường; đ… | ASK02_A2_2309 |
| A1-06-BP | **SAI** | A1-06: giao nguồn theo §6(c) (offer kèm) | CHAM_ANSWER01_A1-01A1-18_2026-09-21.md §5: §6(c) nguyên văn «only occasionally and noncommercially, and only if you received the object code with such an offer… | ASK_A1_2109 |

---

## Câu 218 (C18-BP-02)

**Mục C18-BP-02 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (BỊA):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «Link HF «HF gated» pony-diffusion-v6/pony-diffusion-v6 và lllyasviel/control_v11f1p_sd15_clip». Bằng chứng của đội: HF API 02/10 → 401 cả hai (repo gated vẫn trả 200 metadata, vd FLUX.1-dev 200 gated=auto); repo thật gần tên: lllyasviel/control_v11f1p_sd15_depth 200; 22/24 l…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 219 (BP-0.2b/0.2c)

**Mục BP-0.2b/0.2c — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «0.2b/0.2c: COUNT(DISTINCT) đắt; `PRAGMA count_changes` cho changes(); canon FEN bỏ số nước». Bằng chứng của đội: `Shared/DataFen/nap_fendb.py:85-126` `khoa_the` = `chessdb_nhan.chuan_hoa_fen` (bỏ số nước). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 220 (BP-A2-0105A2-35)

**Mục BP-A2-0105A2-35 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A2-01…05, A2-35: Template matching lớp 1; 2 điểm đủ nếu cùng hàng, điểm 3 khi méo; full/one; hồ sơ JSON 9 trường; 4 khú…». Bằng chứng của đội: hồ sơ v2 đã có (`BocCoManHinh.cs:1027`). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
