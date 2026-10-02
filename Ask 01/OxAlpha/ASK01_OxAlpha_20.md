# ASK 01 — bộ hỏi cho OxAlpha — tệp 20/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 96 (U2909-03)

**Mục U2909-03 — Unknown_2909 (tác giả không rõ) (SAI):** AI Unknown_2909 (tác giả không rõ) nói: «'from torch.cuda.amp import autocast # Even on CPU with BF16' nhanh 1,5–2×». Bằng chứng của đội: API torch.cuda.amp là CUDA; CPU phải torch.autocast(device_type='cpu') (ĐO chạy được); tốc độ chưa đo. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 97 (U2909-04)

**Mục U2909-04 — Unknown_2909 (tác giả không rõ) (SAI):** AI Unknown_2909 (tác giả không rõ) nói: «'CPU chậm hơn 10–50×' (§1.1) và 'chậm hơn 1,5–2×' (§5.1)». Bằng chứng của đội: tự mâu thuẫn trong cùng bài; cả hai không đo. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 98 (AGRV-03)

**Mục AGRV-03 — antigravity (BIA):** AI antigravity nói: «'Sử dụng repo nnue-xiangqi (có sẵn trên GitHub)'». Bằng chứng của đội: không URL/tên đầy đủ; không tìm thấy. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 99 (AGRV-02)

**Mục AGRV-02 — antigravity (SAI):** AI antigravity nói: «'Chuyển sang NNUE + Monte-Carlo Tree Search'». Bằng chứng của đội: NNUE đi với αβ (nnue.cpp:311-313 trong search αβ); MCTS cần policy/value. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 100 (AGRV-05)

**Mục AGRV-05 — antigravity (SAI):** AI antigravity nói: «'Tải phiên bản đã quantize 8-bit từ cộng đồng rồi fine-tune vài giờ bằng CPU'». Bằng chứng của đội: không có net 8-bit cộng đồng cho kiến trúc nhà; net Pikafish official cấm thương mại (NGUON_ENGINE.md:64). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
