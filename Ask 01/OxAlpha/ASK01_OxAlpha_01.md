# ASK 01 — bộ hỏi cho OxAlpha — tệp 1/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 1 (A1-01)

### A1-01 — Windows + GPU AMD: ROCm hay DirectML? (MuseSpark / Nemotron / đề bài Ask 05 ↔ Grok / SpaceBunny)
**MuseSpark:** *«DML là đường duy nhất dùng GPU AMD trên Windows (`…:3972-3978`)»*; *«Trên Linux ROCm chạy được cả 3 trainer; trên Windows dùng `torch-directml`»*.
**Nemotron:** *«ROCm chỉ hỗ trợ AMD GPU discrete RDNA2+, KHÔNG hỗ trợ iGPU/APU cũ … Laptop Strix Halo (Radeon 8060S) chưa chắc đã hỗ trợ ROCm 6.x»*; *«Setup WSL2 Ubuntu 22.04 + ROCm 6.1 (1–2 tuần) → tất cả trainer nhận GPU»*.
**Bằng chứng đội:** `.venv` torch 2.12.0a0+**rocm7.13** trên Windows, `cuda=True`, device «AMD Radeon(TM) 8060S Graphics»; Docco trainer `--help` rc 0 trên venv này; docstring `train_nnue_gpu_v3.py:3934-3936` («trên Windows KHÔNG có ROCm») viết 25/07/2026 — đã lỗi thời. torch-directml: bản cuối 09/2024, ghim torch 2.4.1.
**Hỏi:** (1) Có lý do kỹ thuật nào còn ưu tiên DirectML/WSL2 thay vì ROCm-Windows có sẵn? (2) Với **máy đích 192 lõi có GPU AMD rời** (RDNA3/4) chạy Windows: ROCm Windows có cùng mức hỗ trợ như trên APU không, hay phải Linux? Dẫn phiên bản ROCm/torch cụ thể nếu bạn chắc.

---

## Câu 2 (A1-02)

### A1-02 — Trainer OngThan có «chạy được ngay trên CPU» không? (Nemotron / đề bài ↔ SpaceBunny)
**Nemotron (ma trận §2):** *«OngThan (pikafish-nnue-pytorch/train.py): CPU-only ✅ Chạy được (Lightning CPU)»*.
**SpaceBunny:** *«`OngThan/…/train.py:57` và `:67` hardcode `nnue.cuda()`. Trên máy 44 lõi không GPU thì dòng này ném exception … Sửa 1 dòng … Patch tối thiểu: `DEV = torch.device("cuda" if torch.cuda.is_available() else "cpu")`»*.
**Bằng chứng đội:** cả hai đều thiếu: `train.py --help` gãy **trước khi chọn device** ở `:28` (`pl.Trainer.add_argparse_args` bị bỏ từ Lightning 2.0; pin `pytorch-lightning==1.9.5`); `nnue_dataset.py:35-44` `.pin_memory()` cũng ném RuntimeError khi không có GPU (đo thật). Docco đã port Lightning 2 (`trainer_xiangqi/train.py:503-508`, loader `:50-53`).
**Hỏi:** (1) Cách vá đúng tối thiểu là gì — port sang Lightning 2 như Docco, hay giữ Lightning 1.9.5 trong venv riêng (pl 1.9.5 chỉ đòi `torch>=1.10`, nhưng có chạy nổi với torch 2.12/ROCm không)? (2) `feature_transformer_fallback.py` dùng `F.embedding_bag(mode='sum', per_sample_weights=…)` — trên CPU 192 lõi có nghẽn ở embedding_bag không, có kernel/cách nào tốt hơn cho FT thưa 10530×1024 không? (3) Lightning trên CPU có overhead đáng kể so với vòng train thuần cho net này không?

---

## Câu 3 (A1-03)

### A1-03 — Nút thắt: train hay sinh nhãn? (đề bài / Nemotron ↔ Grok / SpaceBunny / BigPickle)
**Đề bài Ask 05 & Nemotron:** *«CPU-only … chậm 20-50× so với GPU»* là vấn đề cần giải.
**Grok:** *«Nút thắt thật là dữ liệu và nhãn, không phải phép nhân ma trận»*. **SpaceBunny:** *«Mua GPU sửa 8,8 ngày → 1,5 giờ, còn 168 ngày [datagen] không đổi một mili giây»*.
**Bằng chứng đội:** iGPU train 10.540 vị trí/s; datagen ≈ 549 thế/s (×19–24); RMSE 587 (10 % dữ liệu) → 489 (100 %) (`DATA_VOLUME_FINDING_sonnet_2026-07-18.md:10`).
**Hỏi:** Với 192 lõi dành cho datagen (giả sử tuyến tính ≈ 6× máy 32 luồng ⇒ ~3.300 thế/s) và train CPU (chưa đo), **tỷ lệ đúng để chia lõi** giữa datagen và train là bao nhiêu? Có cách đo nào cho thấy tại điểm nào GPU bắt đầu đáng tiền? Đưa công thức theo mẫu/s, không đưa số tháng.

---

## Câu 4 (A1-04)

### A1-04 — «8 tỷ mẫu ⇒ 168 ngày datagen» có đúng không? (SpaceBunny ↔ Grok)
**SpaceBunny:** *«Mục tiêu thực tế của bạn = 400 × 20M = 8 tỷ mẫu … 8 tỷ / 552,5/s = 168 NGÀY … Nút thắt ở sinh nhãn»*.
**Grok:** *«40 tỷ là số lượt mẫu (400 epoch × 100 triệu), không phải 40 tỷ thế khác nhau. Mỗi epoch lấy lại mẫu từ cùng kho … "very competitive even after only 100 epochs"»*.
**Bằng chứng đội:** `train.py:43` `--epoch-size 20000000` = số mẫu đi qua mỗi epoch, kho NoBook chỉ 2.010.659 mẫu (loader lặp lại kho); 552,5/s là tổng máy 32 luồng, chưa scale 192 lõi.
**Hỏi:** Với NNUE HalfKAv2 cờ tướng (10530 feature), **kho bao nhiêu thế KHÁC NHAU** là đủ để 100–400 epoch × 20 M không overfit (có số liệu/kinh nghiệm Stockfish/Pikafish: thế/tham số)? Dấu hiệu đo được của «kho quá nhỏ» là gì (val loss, A/B)?

---

## Câu 5 (A1-05)

### A1-05 — Luồng CPU cho trainer Bodetosu: `--threads`, `--luong-train`, hay để OpenMP tự lấy? (Grok ↔ SpaceBunny ↔ code)
**Grok:** *«Không có `--threads`: tôi không tìm thấy `set_num_threads` trong cây chính … khi train bằng CPU, torch tự lấy hết số lõi»*. **SpaceBunny:** *«python tools/train_nnue_gpu_v3.py --device cpu --threads 64»*.
**Bằng chứng đội:** không có `--threads`, nhưng `:39-70` gọi `tu_canh_tai_nguyen.dat_luong_thu_vien` đặt `OMP_NUM_THREADS` **trước `import numpy`** (trần 80 %, chừa 8 lõi, `tu_canh_tai_nguyen.py:61-63`), `:5308-5311` `torch.set_num_threads`; cờ ghi đè là `--luong-train N`. Án lệ 03/09: torch-CPU không đặt OMP ⇒ ~28 luồng mỗi lượt, RAM phình.
**Hỏi:** Trên máy **192 lõi Windows** (nhiều processor group > 64 luồng logic): (1) `torch.set_num_threads(150)` có thật sự dùng được lõi ở group thứ 2/3 không, hay OpenMP/MKL chỉ ở 1 group? (2) Cách đặt affinity/CPU Sets đúng cho python+torch; (3) Nên 1 job 150 luồng hay nhiều job (xem A1-06)? Dẫn tài liệu nếu chắc.
