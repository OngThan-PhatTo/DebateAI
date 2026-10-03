# ASK 01 — bộ hỏi cho OxAlpha — tệp 9/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 41 (GRKPC-07)

**Mục GRKPC-07 — Grok47 (SAI):** AI Grok47 nói: «192 lõi sinh ~2 tỷ thế/ngày». Bằng chứng của đội: như GLMPC-10 (cùng câu): cận trên ≈ 42× số đo 552,5/s. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 42 (MSPC-09)

**Mục MSPC-09 — MuseSpark (SAI):** AI MuseSpark nói: «192 lõi ~2 tỷ thế/ngày; tháng 5–8/5–9 (cùng khung GLM)». Bằng chứng của đội: như GLMPC-10: cận trên ≈ 42× số đo 552,5/s. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 43 (SBPC-02)

**Mục SBPC-02 — SpaceBunny (SAI):** AI SpaceBunny nói: «Đường B: train NNUE trên px0data ⇒ có engine bằng Pikafish trong 3–10 ngày». Bằng chứng của đội: data.bin = tar `./run1/training.<N>.gz` (đọc header tar 02/10) = chunk self-play kiểu lc0 (policy/value), KHÔNG phải FEN+điểm; SourceCode không có bộ đọc chunk (grep V6TrainingData\|px0data chỉ ra tài liệu + Shared/LuatCo/xoa_sau_nap.py — script xoá nguồn sau nạp, không đọc chunk) ⇒ không train NNUE «vài ngày» được; «bằng Pikafish» không có số đo. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 44 (SBPC-03)

**Mục SBPC-03 — SpaceBunny (SAI):** AI SpaceBunny nói: «Đường B «hợp pháp, rẻ», net train trên px0data bán được». Bằng chứng của đội: luật owner 20/09 15:1x mục 3: net để BÁN «không trộn dump Pika Zero (ODbL)»; E:\DuLieuCo\px0data\MANIFEST_tai.json ghi giấy phép «chua_ro (Kaggle Unknown, README khai ODbL)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 45 (SBPC-04)

**Mục SBPC-04 — SpaceBunny (SAI):** AI SpaceBunny nói: «Bố trí 2 worker px0 --threads=88 + 16 lõi datagen trên máy 192 lõi». Bằng chứng của đội: 88×2 + 16 = 192 = 100 % > trần 80 %/chừa ≥ 8 (CLAUDE.md 03/09, 10/09) ⇒ tối đa ~153. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
