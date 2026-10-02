# ASK 01 — bộ hỏi cho OxAlpha — tệp 7/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 31 (D-03)

### D-03 — Kho nhỏ, epoch lớn: lặp kho bao nhiêu lần thì hết lợi?
**BigPickle:** net «chín» sau ~400 epoch × 10⁸ = 4×10¹⁰ mẫu. **Đội:** epoch 20M nhưng kho chỉ 2,01M mẫu ⇒ mỗi epoch lặp kho ~10 lần.
**Hỏi:** Có bằng chứng (code/thí nghiệm công khai) về số lượt lặp tối đa trên cùng một kho trước khi NNUE quá khớp? Nên dừng theo val loss trên **ván tách riêng** hay theo A/B Elo? Đề xuất ca thử rẻ (vd 5/20/80 lượt lặp, cùng kho).

---

## Câu 32 (D-04)

### D-04 — Bản Pikafish mới nhất và số Elo giữa hai bản
**BigPickle:** trang chủ ghi 2026.09.25 vs 2026.01.31 = 2000 ván, +17,6 ± 3,8 Elo. **Codex** chỉ thấy release `Pikafish-2026-09-06` (commit `4c17cee1…`).
**Hỏi:** URL release/commit của bản mới nhất bạn xác nhận được, và điều kiện đấu của con số +17,6 (TC, số luồng, sách khai cuộc). Không chắc thì ghi «không chắc».

---

## Câu 33 (D-05)

### D-05 — Thầy hợp lệ cho net để BÁN
**BigPickle/OxAlpha/DeepSeek:** distill từ Pikafish là đường nhanh nhất. **Luật owner:** net bán không được học nhãn Pikafish/OngThan official, không trộn dump Pika Zero (ODbL).
**Hỏi:** Có bộ dữ liệu cờ tướng **FEN + điểm/WDL** công khai với giấy phép **CC0/MIT** (không phải ODbL, không sinh bằng Pikafish official) không? Dẫn URL + giấy phép nguyên văn. Nếu không có: chiến lược bootstrap tốt nhất từ net CC0 `C07E94A5` (Fairy, 11,26 MB) + tự đánh NoBook là gì?

---

## Câu 34 (D-06)

### D-06 — px0data: định dạng chunk và giấy phép khi train
**SpaceBunny:** train NNUE trên px0data (Kaggle 11.115.786.240 B) trong vài ngày. **Đội đo:** tar `./run1/training.<N>.gz` = chunk self-play kiểu lc0.
**Hỏi:** (1) Phiên bản định dạng chunk của Px0 (V6/V7…? cấu trúc bản ghi) và có công cụ công khai nào đổi chunk → FEN + Q/kết quả để làm nhãn NNUE không (dẫn tệp:dòng)? (2) Theo ODbL, một **net** train từ cơ sở dữ liệu ODbL là «Produced Work» hay «Derivative Database»? Có nghĩa vụ share-alike với net không? (chỉ trả lời nếu dẫn được điều khoản nguyên văn).

---

## Câu 35 (D-07)

### D-07 — 2 GPU với loader C++ dạng stream (và trên Windows)
**Codex:** DDP không tự chia dữ liệu; NCCL không có trên Windows. **unknown/TAI/gravity:** dùng `torchrun`/DDP. **Đội:** `nnue_dataset.py:167-168` bỏ `idx`; máy owner `torch.distributed.is_available()` = False.
**Hỏi:** (1) Trainer `nnue-pytorch` chính thức (hoặc `easy_train.py`) chia dữ liệu cho nhiều GPU thế nào khi loader là stream C++ — có chia theo rank (skip/offset) không? Dẫn tệp:dòng. (2) Trên Windows + CUDA, DDP backend gloo cho 2 GPU có đáng dùng so với 2 job tách (`CUDA_VISIBLE_DEVICES`)? Có số đo công khai không?
