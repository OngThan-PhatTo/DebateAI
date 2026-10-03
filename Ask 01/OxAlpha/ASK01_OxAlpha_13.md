# ASK 01 — bộ hỏi cho OxAlpha — tệp 13/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 61 (UTPC-05)

**Mục UTPC-05 — unknown (SAI):** AI unknown nói: «DDP 2 GPU (torchrun) là tối ưu, code sẵn hỗ trợ (train.py:98, Docco train.py:175-177)». Bằng chứng của đội: Windows: .venv dist=False; loader nnue_dataset.py:167-168 không chia rank ⇒ 2 rank đọc cùng stream; Docco :175-177 là cây PHỤ SourceCode/Engine_Xiangqi/OngThan_variants (P-150), cây SHIP là :520. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 62 (UDPC-05)

**Mục UDPC-05 — unknown (SAI):** AI unknown nói: «Phân lõi: 150–180 self-play + 8–16 chuyển đổi + phần còn lại train/match». Bằng chứng của đội: 180 + 8 + … ≥ 188/192 > trần 80 % (153) luật 03/09. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 63 (OXPC-02)

**Mục OXPC-02 — OxAlpha (SAI):** AI OxAlpha nói: «5080 TDP thấp hơn 3090 (~2×360 W vs ~2×350 W)». Bằng chứng của đội: tự mâu thuẫn: 360 W > 350 W (spec NVIDIA, Codex/BigPickle cùng lô). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 64 (OXPC-03)

**Mục OXPC-03 — OxAlpha (SAI):** AI OxAlpha nói: «Gán nhãn thầy bằng `./pikafish analysis threads=192 depth=22 input=… out=…`». Bằng chứng của đội: SourceCode/6_Engine_SOURCE_D_20260806/Xiangqi/OngThan/pikafish-src/uci.cpp:103-160 bảng lệnh (quit/uci/setoption/go/position/isready/flip/bench/d/eval/compiler/export_net/help) — không có «analysis» kiểu này. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 65 (OXPC-04)

**Mục OXPC-04 — OxAlpha (SAI):** AI OxAlpha nói: «Self-play 192 thread, mỗi thread 1 engine». Bằng chứng của đội: 192 = 100 % > trần 80 %/chừa ≥ 8 (luật 03/09). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
