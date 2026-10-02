# ASK 01 — bộ hỏi cho OxAlpha — tệp 18/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 86 (NEMO-07)

**Mục NEMO-07 — Nemotron (SAI):** AI Nemotron nói: «'ROCm không hỗ trợ iGPU/APU; Strix Halo chưa chắc ROCm 6.x' + 'WSL2+ROCm 1–2 tuần để tất cả trainer nhận GPU'». Bằng chứng của đội: ĐO .venv rocm7.13 trên gfx1151 Windows cuda=True; OngThan vướng Lightning 1.9.5 chứ không vướng ROCm. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 87 (OXAL-02)

**Mục OXAL-02 — OxAlpha (SAI):** AI OxAlpha nói: «'data augmentation: mirror, rotate, symmetries của bàn cờ → nhân 8× data'». Bằng chứng của đội: cờ tướng chỉ lật trái-phải ×2 (ongthan_train_methods.py:78; chính OxAlpha phần 2 ghi ×2). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 88 (OXAL-06)

**Mục OXAL-06 — OxAlpha (SAI):** AI OxAlpha nói: «Đề xuất 'chuyển sang kiến trúc Pikafish-style'». Bằng chứng của đội: đã là αβ+NNUE (OngThan lõi Pikafish, Docco Fairy, Bodetosu/Nữ Oa αβ+NNUE search.cpp:585). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 89 (SBUN-03)

**Mục SBUN-03 — SpaceBunny (SAI):** AI SpaceBunny nói: «'Sửa 2–3 dòng (cuda() có điều kiện + batch) là OngThan chạy CPU'». Bằng chứng của đội: ĐO: train.py:28 add_argparse_args gãy trước (pin pytorch-lightning==1.9.5, máy chỉ có 2.5.5/2.6.5); nnue_dataset.py:35-44 pin_memory → RuntimeError khi không GPU. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 90 (SBUN-08)

**Mục SBUN-08 — SpaceBunny (SAI):** AI SpaceBunny nói: «8 tỷ mẫu = 400×20M ⇒ trainer 8,8 ngày, datagen 168 ngày; 'mua GPU không đụng 168 ngày'». Bằng chứng của đội: epoch = lượt qua CÙNG kho (train.py:43; kho NoBook 2.010.659 mẫu); 10.540/s là số GPU ROCm (CHAM_ANSWER01:209); 552,5/s = tổng 32 luồng chưa scale 192 lõi. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
