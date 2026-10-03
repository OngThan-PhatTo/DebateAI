# ASK 01 — bộ hỏi cho OxAlpha — tệp 16/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 76 (LCAT-02)

**Mục LCAT-02 — Longcat (SAI):** AI Longcat nói: «DataLoader num_workers=192; torch.set_num_threads(192)». Bằng chứng của đội: train.py:18-19 (0 cho sparse); tu_canh_tai_nguyen.py:61-63 chừa 8 lõi. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 77 (MUSE-07)

**Mục MUSE-07 — MuseSpark (SAI):** AI MuseSpark nói: «'DML là đường duy nhất dùng GPU AMD trên Windows' (chép docstring :3943-3944)». Bằng chứng của đội: ĐO: ROCm chạy trên Windows (.venv rocm7.13, cuda=True); docstring lỗi thời. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 78 (MUSE-08)

**Mục MUSE-08 — MuseSpark (SAI):** AI MuseSpark nói: «'Linux ROCm chạy cả 3 trainer; Windows dùng torch-directml; HalfKAv2 dùng torch-directml nếu cài được'». Bằng chứng của đội: Docco train.py:50-51: Lightning KHÔNG có accelerator DirectML; PyPI torch-directml đòi torch==2.4.1 (venv 2.11/2.12 không cài được). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 79 (MUSE-09)

**Mục MUSE-09 — MuseSpark (SAI):** AI MuseSpark nói: «Lệnh CPU-NNUE cho OngThan 'python train.py … --batch-size 2048 --threads 64' chạy được». Bằng chứng của đội: ĐO: train.py --help trên .venv → AttributeError add_argparse_args (train.py:28, pin pytorch-lightning==1.9.5); :57/:67 nnue.cuda(); nnue_dataset.py:35-44 pin_memory. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 80 (NEMO-08)

**Mục NEMO-08 — Nemotron (BIA):** AI Nemotron nói: «pl.Trainer(gradient_clip_algorithm='checkpoint') để activation checkpointing». Bằng chứng của đội: Lightning chỉ nhận gradient_clip_algorithm 'norm'/'value'; không có 'checkpoint'. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
