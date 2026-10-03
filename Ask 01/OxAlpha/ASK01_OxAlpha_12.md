# ASK 01 — bộ hỏi cho OxAlpha — tệp 12/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 56 (GRVPC-01)

**Mục GRVPC-01 — unknown (SAI):** AI unknown nói: «So sánh «2 × Intel Arc A-5080 (≈12 TFLOPS, 16 GB GDDR6)» với 2×3090». Bằng chứng của đội: câu hỏi gốc AI_Debate/Discussion/Askfull/ASK_PC_192_CORE_TRICH_NGUYEN_VAN_2026-09-29.md hỏi RTX 5080 (NVIDIA); không có Intel Arc A-5080. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 57 (GRVPC-03)

**Mục GRVPC-03 — unknown (SAI):** AI unknown nói: «2×3090 data-parallel ⇒ ≈0,5 tháng tới sức Pikafish (2M bước, ~70 TB self-play)». Bằng chứng của đội: bỏ qua khâu sinh dữ liệu: đội đo 432–552 thế/s/32 luồng (_SOI/B1_ASK05_FABLE_2026-10-01.md:60); «70 TB» không nguồn; các AI khác cùng lô ≥ 5 tháng. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 58 (GRVPC-05)

**Mục GRVPC-05 — unknown (BỊA):** AI unknown nói: ««All example scripts are placed under scripts/» trong C:\Users\HaTrungTin\.gemini\antigravity\scratch\pc_engine_analysis». Bằng chứng của đội: Get-ChildItem -Recurse 02/10: thư mục có nhưng 0 tệp (không scripts/, configs/, data/). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 59 (UTPC-03)

**Mục UTPC-03 — unknown (SAI):** AI unknown nói: «Datagen deep_distill «200k games × 1,2M nodes ≈ 3–4 ngày»». Bằng chứng của đội: chính code bài dẫn: ongthan_train_methods.py:55 `games=200` và :122 `games * 2` ⇒ 200 ván (không phải 200k); bài tự ghi `--games 200` ở §3.1. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 60 (UTPC-04)

**Mục UTPC-04 — unknown (SAI):** AI unknown nói: «Bối cảnh OngThan: features=1260, net ongthan/pikafish.nnue ~10–15 MB; HalfKAv2 features=40960». Bằng chứng của đội: 1260 là Bodetosu (train_nnue_gpu_v3.py bản 29/09 (git show ce08b540, SHA E323B86A) :141); OngThan HalfKAv2 10530 (OngThan/tools/ongthan/pikafish-nnue-pytorch/variant.py:44); net official 50.706.378 B (CLAUDE.md luật 20/09). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
