# ASK 01 — bộ hỏi cho OxAlpha — tệp 27/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 131 (W456-04)

### W456-04 — ADB trên LDPlayer / BlueStacks / MuMu cho cách kết nối AUTO «giả lập»
1. **Bối cảnh:** đo Ask 2 câu 5 trên qemu AVD: `PostMessage` vào HWND ⇒ 0 sự kiện `getevent`; `SendInput` ⇒ `ABS_MT` đúng ô (lệch 0,1–0,2 %) nhưng cần cửa sổ không bị che. `adb shell input tap` bơm thẳng vào máy khách nên sống khi cửa sổ bị che. TieuLongNu chọn giả lập qua chụp + `SendInput`; nhánh ADB đang hoãn (TLN giả lập v3 `27bd2421`). T-35 đang chọn có thêm thẻ «ADB» hay không.
2. **Câu hỏi:** Với LDPlayer 9, BlueStacks 5, MuMu Player 12: ADB có bật mặc định không, cổng/cách bật là gì, và độ trễ điển hình của `input tap` và `screencap` (theo tài liệu hoặc số đo công bố)?
3. **Trả lời tốt:** bảng 3 giả lập × (mặc định bật? · cổng · cách bật · URL tài liệu chính thức); ô nào không có nguồn ghi «không biết».

---

## Câu 132 (T36-01)

### T36-01 — BF16 autocast trên CPU cho train NNUE: an toàn với lượng tử int16 không?
1. Bối cảnh: torch 2.12 (ROCm build) CPU Ryzen AI MAX+ 395 (Zen 5, AVX512), 4 luồng, mô hình đại diện FT 11.340×520 (`F.embedding_bag`, fp32) + Linear 512→16→32→1 + Adam: `torch.autocast('cpu', bfloat16)` nhanh **2,59×** (15.085 → 39.135 mẫu/s), loss sau 30 bước lệch 0,021 %, grad mọi tham số lệch 0,1–0,5 %. FT vẫn fp32 dưới autocast; chỉ Linear ra bf16. Engine chạy net lượng tử int16/int8.
2. Câu hỏi: bật bf16 autocast cho bước gradient (FT giữ fp32, lớp Linear bf16) có làm hỏng độ chính xác sau lượng tử int16/int8 (QAT hoặc lượng tử sau) không — có dự án NNUE nào (nnue-pytorch, bullet, Stockfish/Pikafish) train lớp sau bằng bf16 rồi xuất int8 mà báo lệch Elo?
3. Trả lời tốt: nêu dự án + cấu hình cụ thể + số lệch (cp/Elo) hoặc lý do nguyên lý (bf16 mantissa 8 bit vs bước lượng tử 1/64…); «không biết dự án nào» cũng được.

---

## Câu 133 (T36-02)

### T36-02 — FT thưa trên CPU: embedding_bag không tăng tốc theo luồng, thay bằng gì?
1. Bối cảnh: cùng máy, B=4.096, 32 feature/thế, FT 11.340×520, forward+backward: `embedding_bag(mode='sum', per_sample_weights)` 10.252 mẫu/s ở 4 luồng ≈ 1 luồng (0,967×, máy đang tải 99 %); `index_add_` 0,33×, `torch.sparse.mm` (COO) 0,58× — cả hai CHẬM hơn. Nguồn: `pikafish-nnue-pytorch/feature_transformer_fallback.py:30-37` (Windows/ROCm không có cupy ⇒ luôn fallback).
2. Câu hỏi: backward của `embedding_bag` (grad dense, có per_sample_weights) trên CPU PyTorch có song song hoá không? Cách nhanh nhất đã được đo cho FT thưa NNUE trên CPU nhiều lõi (extension C++ tự viết, `sparse=True` + SparseAdam, bullet CPU backend…)?
3. Trả lời tốt: dẫn mã nguồn ATen (tên hàm/tệp) hoặc số đo mẫu/s có cấu hình; nói rõ khi nào đáng viết kernel riêng.

---

## Câu 134 (T36-03)

### T36-03 — Chọn `in_scaling`/`out_scaling` cho cờ tướng theo cách nào?
1. Bối cảnh: kho `docco_train.bin` 1.255.954 mẫu (điểm kẹp ±2000, đơn vị nội bộ): |score| > 1600 (σ(s/410) > 0,98) = 6,0 %; > 900 = 11,9 %; p95 = 1766; 3,8 % nằm đúng ở trần 2000. OngThan trainer dùng 410/361 (`pikafish-nnue-pytorch/model.py:314-319`, thang cờ vua), Docco đã nới 1000/880 (`trainer_xiangqi/model.py:323-327`). Ngưỡng đặt trước của đội («> 10 % bão hoà mới A/B») KHÔNG đạt ⇒ chưa A/B.
2. Câu hỏi: thang sigmoid nên chọn bằng phép khớp nào (vd fit điểm ↔ tỉ lệ thắng/hoà/thua thực tế như `perf_sigmoid_fitter.py`, hay theo Elo-per-cp của engine thầy)? Tỉ lệ mẫu bão hoà có phải tiêu chí đúng không?
3. Trả lời tốt: công thức/thủ tục cụ thể + nguồn (Stockfish/Pikafish/nnue-pytorch docs, commit); nếu dựa vào WDL thì nói cần bao nhiêu ván có kết quả.

---

## Câu 135 (T13-01)

### T13-01 — Chặn job ComfyUI (PyTorch ROCm) trước khi chạy trên iGPU UMA: đọc số bộ nhớ nào?
**Bối cảnh (đo 02/10, máy ROG Flow Z13, Ryzen AI MAX+ 395, Radeon 8060S gfx1151, Windows 11, torch 2.10.0+rocm7.13 HIP 7.13.26174):** lắp 8 × 8 GiB = 64 GiB; `GlobalMemoryStatusEx` tổng 47,65 GiB, trống **21,76 GiB** (load 54 %). DXGI `IDXGIAdapter3::QueryVideoMemoryInfo` (adapter Radeon): LOCAL Budget **46,72 GiB**, CurrentUsage **0,00 GiB**, AvailableForReservation 23,49 GiB; DXGI_ADAPTER_DESC1: DedicatedVideo 15,83 GiB, SharedSystem 31,65 GiB (`LaneScratch/w_t13_tonghop_0210/dxgi_budget_ketqua.txt`). ComfyUI tự báo VRAM 40.690 MB + RAM 48.792 MB và lập kế hoạch như hai bể riêng ⇒ chạy hết 20/20 bước rồi chết ở giải mã VAE; phải ép `--reserve-vram 26` + `VAEDecodeTiled`. Đội đang chặn theo RAM trống `GlobalMemoryStatusEx` (`SourceCode/ComfyUI_Pro/ComfyUIPro/Core/Eta.cs:6`: ≥ 98 % chặn, ≥ 85 % cảnh báo). ChatGPT (Ask21 KK3) đề xuất trần `min(80 % RAM vật lý, DXGI Budget − dự trữ)` — với số trên trần đó = 38,1 GiB, lớn hơn RAM đang trống 21,76 GiB.
**Hỏi:** Trên Windows + ROCm/HIP với APU UMA, cấp phát của HIP có đi qua WDDM (được tính vào DXGI CurrentUsage/Budget) không? Số nào phản ánh đúng phần còn cấp được cho một job mới: `hipMemGetInfo`/`torch.cuda.mem_get_info()`, DXGI Budget − CurrentUsage, hay RAM trống của hệ điều hành?
**Trả lời tốt:** dẫn tài liệu AMD/Microsoft có URL (ROCm for Windows, WDDM GPU memory model); hoặc một ca thử: cấp phát N GiB bằng `torch.empty(..., device='cuda')`, đọc cả ba số trước/sau, đối chứng A=A (không cấp phát) và đối chứng dương (cấp phát gần hết) — ngưỡng quyết ghi trước. «Không chắc» kèm cách kiểm cũng được.
