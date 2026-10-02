# ASK 01 — bộ hỏi cho OxAlpha — tệp 2/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 6 (A1-06)

### A1-06 — N job ít luồng hay 1 job nhiều luồng? (SpaceBunny / BigPickle ↔ Grok)
**SpaceBunny:** *«Chạy N job 1 luồng, không 1 job N luồng … High-end GPUs usually require 2 runs in parallel on each GPU to saturate it»*. **BigPickle:** *«8 job × 24 luồng thường tốt hơn 1 job 192 luồng, vì gradient của mô hình 35K tham số không đủ bão hòa 192 luồng GEMM nhỏ»*.
**Grok:** *«cách này train ra N net khác nhau, không làm một net nhanh hơn. Nó chỉ có ích khi quét siêu tham số … gộp nhiều net bằng average trọng số là sai (`TRAIN_METHODOLOGY_Bodetosu.md:22-27`)»*.
**Hỏi:** Với net 1260/10530 → 256/512 (FT thưa, GEMM nhỏ), trên 192 lõi: (1) điểm bão hòa luồng của 1 job ước ở đâu và đo thế nào; (2) có cách gộp N job thành MỘT net hợp lệ (DDP gloo, gradient averaging, hay «train N seed rồi SPRT chọn 1») — ưu nhược; (3) ca thử ngắn để quyết (tổng mẫu/s + A/B).

---

## Câu 7 (A1-07)

### A1-07 — Hàm mất của đội khác upstream Stockfish 3 chỗ: có đáng đổi không? (SpaceBunny)
**SpaceBunny:** *«1. `sigmoid(x)` một vế vs upstream `0.5·(1+σ(x)−σ(−x))` hai vế … 2. offset = 0 vs 270 … 3. `.square()` vs `pow(abs, 2.5)` … GIẢ THUYẾT CẦN ĐO»*.
**Bằng chứng đội:** `model.py:313-323` (OngThan) và Docco `model.py:322-339` đúng như trích (Docco nới in/out_scaling 1000/880 vì điểm quyết định cờ tướng lớn, `:323-325`); Docco có `--dtm-cp` ánh xạ điểm mate trước sigmoid (`:331-334`).
**Hỏi:** (1) Với phân bố điểm cờ tướng (p95 ≈ 1781 cp, 12 % > 900 cp), vế đối xứng/offset/pow 2.5 có cơ sở lý thuyết nào để kỳ vọng +Elo, hay chỉ là chi tiết cờ vua? (2) Thiết kế A/B 1 đêm: dữ liệu, seed, số ván, đối chứng A=A, ngưỡng.

---

## Câu 8 (A1-08)

### A1-08 — QAT int16/int8: dùng hay bỏ? (BigPickle ↔ SpaceBunny / MuseSpark / Grok)
**BigPickle:** *«Đừng huấn luyện INT8/QAT rồi hy vọng Pikafish dùng được … trainer chính thức huấn luyện FP32»*.
**SpaceBunny:** *«`train_nnue_gpu_v3.py` `ACC_INT16_TRAN = 32767` + `can_tran_acc_int16()` … Bạn đã có fake-quant int16 để train đúng thứ engine chạy»*.
**Bằng chứng đội:** engine nhà (Bodetosu/Nữ Oa/NhuLai) chạy accumulator int16, scale FT 64/L1 64/OUT 16 (`nnue.h:135-137`); trainer có `_fake_quant_act` (:2913, clamp 0..127 + STE) và kẹp acc int16 (:3080-3084); Pikafish/OngThan: FP32 rồi lượng tử ở `serialize.py`.
**Hỏi:** Với net nhỏ L1=256 int16 (không phải Stockfish), QAT fake-quant có giúp +Elo đo được so với «FP32 rồi quantize sau» không? Có bẫy nào của STE với clamp 0..127 làm gradient chết? Thiết kế đối chứng.

---

## Câu 9 (A1-09)

### A1-09 — Kích thước net Pikafish và «16 net song song» (BigPickle ↔ Grok)
**BigPickle:** *«mô hình Pikafish chỉ có khoảng 35 nghìn tham số … ≈ 137 KB ở FP32»*; *«`LayerStacks = 16` … Engine nạp được 16 net cùng lúc → Bạn có thể train 16 net song song, mỗi net một bucket, rồi đánh giá chéo»*.
**Bằng chứng đội:** tệp net 50.706.378 B; feature transformer `FullThreats 45649 + HalfKAv2_hm` × 1024 int16 chiếm gần hết; `LayerStacks` là 16 đầu ra trong MỘT net chọn theo bucket số quân (`model.py:38-50` `nn.Linear(2*L1, L2*count)`), train chung một lần.
**Hỏi:** Bạn có đồng ý BigPickle sai 2 điểm này không? Nếu có ý tưởng «train riêng từng bucket» thật sự (vd. freeze FT, fine-tune từng stack theo pha ván), hãy mô tả cách làm + cách đo.

---

## Câu 10 (A1-10)

### A1-10 — Net HalfKAv2 (10530×1024/512) train CPU «không thực tế»? (Grok ↔ MuseSpark / SpaceBunny)
**Grok:** *«Riêng net cỡ Pikafish/ThanDieuDaiHiep (HalfKAv2_hm + FullThreats, L1 1024) thì train từ đầu bằng CPU không thực tế. Muốn làm thì thuê GPU theo giờ»*.
**MuseSpark:** *«Chậm hơn GPU khoảng 5-15x mỗi epoch (ước) nhưng epoch chỉ là phần nhỏ so với tháng datagen»*.
**Bằng chứng đội:** chưa đo mẫu/s CPU cho FT 10530/45649; iGPU 10.540 vị trí/s là net 1260.
**Hỏi:** Ước lượng có căn cứ (FLOP/byte của embedding_bag thưa với ~32 feature hoạt động/bên, L1 1024, batch 16384) cho 1 job CPU 24 luồng AVX-512: bao nhiêu mẫu/s? Ở mức nào thì «20 M mẫu/epoch × 100 epoch» còn chấp nhận được? Chỉ ra phép đo 10 phút để kiểm ước lượng của bạn.
