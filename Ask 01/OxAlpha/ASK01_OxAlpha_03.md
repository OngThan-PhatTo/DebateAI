# ASK 01 — bộ hỏi cho OxAlpha — tệp 3/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 11 (A1-11)

### A1-11 — BF16 autocast trên CPU: nhanh hay chậm? (Longcat / Nemotron / bài 29/09 ↔ BigPickle)
**Longcat:** *«Mixed Precision (Nhanh 1.5-2×) … `torch.autocast(device_type='cpu', dtype=torch.bfloat16)`»*. **BigPickle:** *«autocast BF16 trên CPU chỉ nhanh thật nếu CPU có AMX hoặc AVX512-BF16. Trên phần cứng không có, nó có thể chậm hơn FP32»*.
**Bằng chứng đội:** autocast bf16 chạy được trên Zen 5 (có AVX512-BF16) — tốc độ chưa đo; trainer có fake-quant int16 (bf16 có thể đổi kết quả lượng tử).
**Hỏi:** Với FT thưa (embedding_bag) + GEMM nhỏ, bf16 có lợi ở đâu (băng thông FT?) và có hại gì cho fake-quant int16/độ chính xác accumulator? Ca thử A=A: loss on/off lệch bao nhiêu là chấp nhận?

---

## Câu 12 (A1-12)

### A1-12 — DirectML còn đáng giữ không? (MuseSpark / Nemotron / GLM-5.3 ↔ Grok)
**GLM-5.3 (unknown):** *«Windows mọi GPU: torch-directml — chậm hơn CUDA nhưng đủ cho net nhỏ»*. **Nemotron:** *«DirectML ~1/2-1/3 RTX 3090»*.
**Grok:** *«torch-directml ra bản cuối 0.2.5.dev240914 ngày 15/09/2024 … chỉ dùng làm dự phòng khi ROCm hỏng»*.
**Bằng chứng đội:** PyPI xác nhận bản cuối 09/2024, `requires torch==2.4.1` ⇒ phải venv riêng torch 2.4.1; không venv nào của đội cài; trainer có nhánh `dml` (:3972-3978) **chưa test thật** (`TRAIN_METHODOLOGY_Bodetosu.md:14-16`).
**Hỏi:** Có GPU Windows nào (Intel Arc, AMD RDNA2 cũ, Qualcomm) mà ROCm/XPU không phủ nhưng DirectML phủ, đáng để đội giữ nhánh này? Nếu không, đề xuất xoá nhánh `dml` để giảm bề mặt bảo trì?

---

## Câu 13 (A1-13)

### A1-13 — Jieqi: trainer riêng hay dùng chung `pick_device`? (Nemotron / đề bài)
**Nemotron:** *«Jieqi (`train_nnue_jieqi.py`): ✅ Kế thừa Bodetosu [CPU/DirectML/ROCm/XPU]»*.
**Bằng chứng đội:** `train_nnue_jieqi.py:368` `choices=["auto","cpu","cuda"]`, `:391-392` tự chọn; không chốt chặn; chỉ tồn tại ở cây DEV (`KHONG_PHAI_CAY_CHINH.md:11-12`), cây canonical không có trainer. Cờ úp đang HOÃN (owner 12/09).
**Hỏi:** Khi mở lại cờ úp: nên **import `pick_device` + bộ tự canh từ trainer Bodetosu** (một nguồn) hay gộp hẳn jieqi thành `--variant jieqi` của `train_nnue_gpu_v3.py` (F=1452)? Rủi ro trôi hai bản?

---

## Câu 14 (A1-14)

### A1-14 — Chạy bằng Lightning hay vòng train thuần cho CPU 192 lõi? (đề bài Q4 «Dài hạn» ↔ BigPickle / Nemotron)
**Đề bài Ask 05:** *«Thay Lightning → PyTorch thuần + `torch.compile()` + `DistributedDataParallel` (gloo backend)»*. **Nemotron:** *«`torchrun --nproc_per_node=192` … Scale gần tuyến tính tới 64-128 cores; >128 cores diminishing returns do sync»*. **BigPickle:** *«Trong một máy 192 core thì nhiều instance độc lập vẫn hơn DDP»*.
**Bằng chứng đội:** Bodetosu trainer đã là PyTorch thuần; OngThan/Docco là Lightning (Docco chạy CPU rc 0). Chưa có số DDP-gloo nào.
**Hỏi:** Với net < 1 M tham số dense (+ FT thưa), DDP gloo 8–16 process trên 1 node có lợi hơn 1 process đa luồng không, hay tốn sync hơn tính? Có số đo công khai nào (nnue-pytorch, bullet) về CPU-DDP cho NNUE? `torch.compile` trên CPU với embedding_bag có tác dụng không?

---

## Câu 15 (A1-15)

### A1-15 — Thầy chấm nhãn và giấy phép dữ liệu (SpaceBunny / Grok / BigPickle)
**SpaceBunny:** *«Tải px0data 11,1 GB (ODbL) từ Kaggle … bỏ hẳn 168 ngày sinh nhãn»*. **Grok:** *«Dữ liệu ODbL có điều khoản share-alike cho cơ sở dữ liệu dẫn xuất. Nếu engine vào gói thương mại thì cần hỏi tư vấn»*. **BigPickle:** *«Chưng cất từ engine mạnh … net Pikafish phân phối dưới giấy phép riêng. Đọc kỹ license trước khi phát hành net dẫn xuất»*.
**Bằng chứng đội:** luật owner 20/09 15:1x: net để bán chỉ CC0 (`xiangqi-c07e94a5c7cb.nnue`) hoặc tự train; thầy cho net bán không dùng Pikafish official; chessdb.cn không có trang điều khoản (404), hạn mức 100k/IP/ngày chưa xác minh.
**Hỏi:** Theo hiểu biết của bạn (ghi rõ mức chắc): (1) net train từ dữ liệu ODbL (px0data) có bị share-alike kéo theo không — dữ liệu dẫn xuất là «database» hay «produced work»? (2) nhãn từ **output** của Pikafish (không chép weight) có ràng buộc gì theo license của Pikafish? Không tư vấn pháp lý — chỉ nêu điều khoản và cách người khác (Stockfish/Lc0/px0) xử lý.
