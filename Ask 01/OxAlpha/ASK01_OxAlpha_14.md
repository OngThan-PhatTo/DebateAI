# ASK 01 — bộ hỏi cho OxAlpha — tệp 14/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 66 (BPCK-02)

**Mục BPCK-02 — BigPickle (SAI):** AI BigPickle nói: «'Pikafish chỉ ≈35.169 tham số ≈137 KB FP32'». Bằng chứng của đội: ĐO ls C:/ChineseChess/Engine/OngThan/core/ongthan.nnue = 50.706.378 B; full_threats.h:42 Dimensions 45649 × L1 1024 int16. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 67 (BPCK-03)

**Mục BPCK-03 — BigPickle (SAI):** AI BigPickle nói: «'LayerStacks=16 ⇒ engine nạp 16 net cùng lúc; train 16 net song song mỗi net 1 PSQT bucket'». Bằng chứng của đội: model.py:38-50 LayerStacks = nn.Linear(2*L1, L2*count) trong MỘT net, chọn theo bucket quân, train chung. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 68 (BPCK-04)

**Mục BPCK-04 — BigPickle (SAI):** AI BigPickle nói: «Loss NNUE 'L=(tanh(FC2)−y)² + loss_psqt' đọc từ trainer». Bằng chứng của đội: model.py:313-323: sigmoid, λ·(p−q)²+(1−λ)·(q−t)², không tanh. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 69 (BPCK-08)

**Mục BPCK-08 — BigPickle (SAI):** AI BigPickle nói: «'256 GB RAM cache được khoảng 3×10^7 vị trí'». Bằng chứng của đội: binpack ~32 B/thế ⇒ ~8×10^9; lệch 2 bậc. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 70 (BPCK-09)

**Mục BPCK-09 — BigPickle (SAI):** AI BigPickle nói: «'Đừng huấn luyện INT8/QAT; trainer chính thức huấn luyện FP32'». Bằng chứng của đội: ngữ cảnh engine nhà: fake-quant int16 train_nnue_gpu_v3.py:2913,:3080 khớp nnue.h:135-137; chỉ đúng với Pikafish FP32→serialize. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
