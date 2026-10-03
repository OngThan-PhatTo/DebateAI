# Manus — Phần I, lô 2: BPCK đến AGRV

**Ngày:** 2026-10-03 15:00  
**Quy tắc phản hồi:** `NHẬN` khi bằng chứng ASK01 chỉ ra khẳng định sai; `NHẬN — BỊA` khi đường dẫn/tệp/API được viện dẫn không tồn tại hoặc không kiểm chứng được. Không bịa nguồn thay thế.

---

## BigPickle

### BPCK-02 — “Pikafish chỉ khoảng 35.169 tham số, 137 KB FP32”

**NHẬN.** Bằng chứng đội ghi `core/ongthan.nnue = 50.706.378 B` và `full_threats.h` có dimension `45.649 × L1 1.024 int16`. Con số 35.169/137 KB không mô tả artifact `ongthan.nnue` đã đo. Không được suy số tham số từ một kiến trúc NNUE đồ chơi/cờ vua rồi gán cho net Xiangqi của đội.

### BPCK-03 — “LayerStacks=16 nghĩa là engine nạp 16 net cùng lúc”

**NHẬN.** Theo bằng chứng `model.py`, LayerStacks là một phép chiếu/đầu ra nhiều stack trong **một model**, chọn slice theo bucket; không đồng nghĩa 16 file net độc lập hay 16 process train song song. Cần phân biệt tensor stack, bucket selection và artifact `.nnue`.

### BPCK-04 — Công thức loss tanh + loss PSQT

**NHẬN.** ASK01 ghi code dùng sigmoid và dạng `λ·(p−q)^2 + (1−λ)·(q−t)^2`, không phải công thức tanh được trích. Khi trả lời loss phải dẫn đúng commit/path và nêu rõ target `t`, teacher `q`, prediction `p`; không lấy công thức từ trainer khác.

### BPCK-08 — “256 GB RAM chỉ cache khoảng 3×10^7 vị trí”

**NHẬN.** Nếu binpack đo khoảng 32 B/vị trí, cận raw là `256 GB / 32 B ≈ 8×10^9` vị trí, trước overhead. Con số 3×10^7 chỉ phù hợp với record kích thước hàng nghìn byte, trái với binpack đã nêu. Phải đo peak RSS/decoded representation, không dùng file-size của format khác.

### BPCK-09 — “Không train INT8/QAT; trainer chính thức chỉ FP32”

**NHẬN MỘT PHẦN.** Có trainer/pipeline có fake-quant hoặc lượng tử hóa-aware ở giai đoạn train; điều đó không có nghĩa mọi trainer chính thức đều QAT. Với cây OngThan, ASK01 ghi fake-quant int16 trong `train_nnue_gpu_v3.py` và serialize khớp engine. Phải phân biệt FP32 optimization, fake-quant trong forward/loss và integer serialization cuối.

### BPCK-10 — “num_workers nên 8–16, không 192”

**NHẬN MỘT PHẦN.** Cảnh báo không đặt 192 worker mù quáng là đúng; nhưng số tối ưu phụ thuộc loader. Theo evidence đội, sparse loader của fork OngThan yêu cầu `num_workers=0`; vì vậy khuyến nghị 8–16 không áp dụng. Worker count phải benchmark theo parser/IO và tránh nhân bản stream.

---

## DeepSeek/Grok/Longcat

### DSEK-07 — “Chuyển sang NNUE nếu engine đang dùng mạng sâu”

**NHẬN.** Evidence đội ghi 3/4 engine đã dùng NNUE/alpha-beta. Đây là lời khuyên không đọc code hiện trạng, không phải kế hoạch port. Chỉ đề xuất đổi kiến trúc khi engine thật sự chưa có NNUE hoặc feature/eval hiện tại không phù hợp mục tiêu.

### GROK-06 — “Trainer không có --threads nên CPU torch tự lấy hết lõi”

**NHẬN.** Trainer có đường `tu_canh_tai_nguyen`, trần 80% CPU/chừa 8 lõi và tham số `--luong-train`; việc không có một flag tên `--threads` không chứng minh không có resource control. Phải đọc toàn bộ parser/runtime, gồm OMP/MKL/PyTorch intra/inter-op.

### LCAT-05 — `torch.load('pikafish_net.pt')`

**NHẬN — BỊA PATH/ARTIFACT.** Evidence đội không có file `.pt`; net là `.nnue` binary và cần qua serializer/loader tương ứng. Không được dựng transfer learning bằng một filename giả. Nếu cần load checkpoint, phải chỉ ra file checkpoint thật, version model và migration code.

### LCAT-01 — “Pikafish 768→256→256→1”

**NHẬN.** Evidence official/master được trích là `1024→32→32→1` cùng feature transformer Pikafish; 768 là kiến trúc khác. Mọi số layer phải gắn engine commit và net architecture hash, không lấy từ NNUE cờ vua cũ.

### LCAT-02 — `num_workers=192`, `torch.set_num_threads(192)`

**NHẬN.** Evidence code đội ghi sparse loader dùng 0 worker và resource guard chừa lõi. Hai tham số này còn có thể tạo oversubscription nếu mỗi process lại sinh OMP/MKL threads. Cần đặt tổng ngân sách và đo, không dùng số lõi làm mặc định.

---

## MuseSpark

### MUSE-07 — “DML là đường duy nhất dùng GPU AMD Windows”

**NHẬN.** Đây là khẳng định quá rộng và đã bị phép đo đội phản ví dụ bằng ROCm/torch build trên Windows với `cuda=True`. Cần nói theo phiên bản GPU/driver/torch: DirectML là một lựa chọn, không phải định luật duy nhất.

### MUSE-08 — “Linux ROCm chạy cả 3 trainer; Windows dùng DirectML; HalfKAv2 dùng DirectML nếu cài được”

**NHẬN.** Evidence `train.py`/Lightning không có accelerator DirectML và `torch-directml` yêu cầu phiên bản torch cụ thể khác môi trường 2.11/2.12. Không được tuyên bố “chạy được” chỉ vì backend tồn tại; cần matrix phiên bản, device smoke test và train step.

### MUSE-09 — Lệnh CPU-NNUE `python train.py … --batch-size 2048 --threads 64`

**NHẬN.** Evidence cho thấy `--help` gãy ở `add_argparse_args`, sau đó code gọi `.cuda()`/`pin_memory`; lệnh không phải CPU path hợp lệ trong environment đó. Sửa tối thiểu vài flag chưa đủ; cần CPU-safe model/device, loader và dependency pin.

---

## Nemotron

### NEMO-08 — `gradient_clip_algorithm='checkpoint'` để activation checkpointing

**NHẬN — BỊA API.** `gradient_clip_algorithm` chỉ là policy clipping (ví dụ norm/value), không phải activation checkpointing. Activation checkpointing là cơ chế khác trong PyTorch/Lightning; không đổi tên enum để làm phát sinh feature.

### NEMO-11 — Paper “Training Neural Networks with Limited Memory — Chen et al., 2018”

**NHẬN — BỊA trích dẫn.** Evidence không tìm thấy paper đúng tiêu đề/năm; nguồn gần đúng được chỉ ra là Chen et al., 2016, “Training Deep Nets with Sublinear Memory Cost”, arXiv:1604.06174. Không dùng citation cũ nếu không mở được URL/DOI.

### NEMO-02 — OngThan features=40.960 và loss MSE(cp)+BCE(wdl)

**NHẬN.** Evidence variant thực tế là `NUM_INPUTS=10.530`; model loss không có BCE ở các dòng được viện dẫn. Số 40.960 và loss WDL là ghép từ kiến trúc/branch khác. Phải xác định đúng tree SHIP/DEV trước khi trích code.

### NEMO-03 — Dẫn các dòng Bodetosu cũ như trainer hiện hành

**NHẬN.** Các dòng 461–783 thuộc cây cũ/DEV có cảnh báo `KHONG_PHAI_CAY_CHINH`; cây SHIP có các dòng khác. Dẫn line number không kèm commit/tree là không đủ và có thể làm người đọc sửa nhầm code không chạy.

### NEMO-04 — Jieqi “kế thừa Bodetosu; CPU/DirectML/ROCm/XPU sẵn sàng”

**NHẬN.** Có lựa chọn device trong một script DEV không chứng minh mọi backend đã train end-to-end. Cần test import, device allocation, forward, backward, checkpoint và reload trên từng backend; evidence hiện chỉ cho thấy choices, không phải mức hỗ trợ hoàn chỉnh.

### NEMO-05 — OngThan CPU-only chạy được

**NHẬN.** Evidence `train.py --help` đã gãy do dependency/API; code gọi `.cuda()` và loader dùng `pin_memory`. Vì vậy không thể ghi dấu `CPU-only ✅`. Muốn hỗ trợ CPU phải có test CPU từ đầu đến checkpoint.

### NEMO-07 — ROCm không hỗ trợ iGPU/APU; cần WSL 1–2 tuần

**NHẬN.** Phép đo đội trên gfx1151/ROCm Windows phản ví dụ phần “không hỗ trợ”. Mặt khác, khả năng backend không đồng nghĩa trainer chạy được: bottleneck hiện có thể là Lightning/dependency. Không được hứa thời gian WSL cho “tất cả trainer” khi chưa có compatibility matrix.

---

## OxAlpha/SpaceBunny

### OXAL-02 — Augmentation đối xứng nhân 8×

**NHẬN.** Với cờ tướng, phép lật trái–phải hợp lệ thường cho ×2; rotate 90° không bảo toàn bàn cờ/rules như chess 8-way. Chỉ thêm augmentation nếu chứng minh biến đổi giữ legal semantics và feature mapping.

### OXAL-06 — “Chuyển sang Pikafish-style”

**NHẬN.** Evidence các engine mục tiêu đã có alpha-beta + NNUE. Vấn đề cần giải là architecture/feature/trainer/serialization và benchmark, không phải đổi sang một họ engine mà họ đã thuộc về.

### SBUN-03 — Sửa 2–3 dòng là OngThan chạy CPU

**NHẬN.** Dependency `add_argparse_args` gãy trước; `.cuda()` và `pin_memory` còn là lỗi device path. Không thể gọi đây là sửa 2–3 dòng nếu chưa có CPU smoke test.

### SBUN-08 — 8 tỷ mẫu và dự toán 8,8/168 ngày

**NHẬN.** Evidence chỉ ra “epoch” là lượt qua kho khoảng 2.010.659 mẫu; 10.540/s là số GPU cụ thể, 552,5/s là số CPU 32 luồng. Không được trộn rate giữa device và nhầm epoch với số sample độc lập. Dự toán phải ghi total optimizer samples, datagen rate và train rate riêng.

### SBUN-09 — 10.530 là HKA Lc0, dense 10.530×512

**NHẬN.** HalfKAv2 là sparse feature transformer/EmbeddingBag; 10.530 không phải một dense input layer kiểu `10.530×512`. Phải đọc feature indices, active feature count và accumulator update.

### SBUN-11 — `--device cpu --threads 64`

**NHẬN.** Evidence SHIP không có `--threads`; resource control đi qua `--luong-train`/auto guard. Không đưa lệnh CLI chưa tồn tại vào hướng dẫn.

---

## Unknown_2909/antigravity

### U2909-05 — `torch.load('pikafish_net.pt')` + freeze fc1

**NHẬN — BỊA artifact.** Không có `.pt`; net `.nnue` binary. Cũng chưa chứng minh `fc1` tồn tại với đúng architecture. Cần checkpoint/serializer thật trước khi thiết kế freeze.

### U2909-01 — Pikafish 768→256→256→1

**NHẬN.** Lặp lại lỗi architecture: evidence official/master là `1024→32→32→1` theo branch được trích, kèm feature transformer riêng.

### U2909-02 — DataLoader 192 worker

**NHẬN.** Evidence `train.py` ghi 0 cho sparse loader. 192 worker còn có nguy cơ duplicate stream/oversubscription.

### U2909-03 — `torch.cuda.amp.autocast` chạy CPU BF16 nhanh 1,5–2×

**NHẬN MỘT PHẦN.** API `torch.cuda.amp` là CUDA path; CPU nên dùng `torch.autocast(device_type='cpu', dtype=torch.bfloat16)` theo version hỗ trợ. Mức nhanh 1,5–2× chưa có benchmark và có thể chậm hơn do cast/unsupported ops. Cần đo wall-clock, accuracy và fallback.

### U2909-04 — CPU chậm 10–50× và chậm 1,5–2×

**NHẬN.** Hai khẳng định mâu thuẫn trong cùng bài và đều không có điều kiện hardware/net/batch. Chỉ báo slowdown sau benchmark cùng samples, precision, backend, worker và I/O.

### AGRV-03 — Repo `nnue-xiangqi` có sẵn trên GitHub

**NHẬN — BỊA nguồn.** Không có URL/tên đầy đủ/commit để kiểm tra. Không đưa repo vào provenance nếu không mở được URL và license.

### AGRV-02 — Chuyển sang NNUE + MCTS

**NHẬN.** NNUE thường là evaluator trong alpha-beta; MCTS cần tree policy/value/rollout và interface khác. Nếu đề xuất hybrid, phải mô tả policy/value và benchmark; không coi MCTS là bước mặc định để “dùng NNUE”.

### AGRV-05 — Tải net 8-bit cộng đồng rồi fine-tune vài giờ bằng CPU

**NHẬN.** Evidence không có net 8-bit community tương thích architecture nhà; net official/license cũng không tự cho phép thương mại. “Vài giờ” không có sample count/throughput/quality test. Chỉ dùng artifact khi có hash, architecture compatibility, license và A/B.

---

## Chốt lô 2

Tất cả mã trên đều được **NHẬN** theo evidence của ASK01; các mã BỊA được ghi rõ là **NHẬN — BỊA**. Các con số/đường dẫn trong ASK01 là bằng chứng đội; khi chưa có toàn bộ source tree trong phiên này, tôi không biến chúng thành “đã tự chạy lại”.
