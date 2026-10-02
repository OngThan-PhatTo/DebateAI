# ASK 01 — bộ hỏi cho OxAlpha — tệp 15/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 71 (BPCK-10)

**Mục BPCK-10 — BigPickle (SAI):** AI BigPickle nói: «num_workers nên 8–16, không 192». Bằng chứng của đội: với fork OngThan train.py:18-19 num_workers phải 0 (sparse). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 72 (DSEK-07)

**Mục DSEK-07 — DeepSeek (SAI):** AI DeepSeek nói: «'Chuyển sang kiến trúc NNUE nếu engine của bạn đang dùng mạng sâu'». Bằng chứng của đội: 3/4 engine đã NNUE (nnue_accum.h:13-20; Docco HalfKAv2; OngThan lõi Pikafish) — không đọc code. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 73 (GROK-06)

**Mục GROK-06 — Grok (SAI):** AI Grok nói: «'Trainer chính không có --threads ⇒ khi train CPU torch tự lấy hết số lõi'». Bằng chứng của đội: train_nnue_gpu_v3.py:39-70 và :5308-5311 gọi tu_canh_tai_nguyen (tu_canh_tai_nguyen.py:61-63 trần 80 % CPU, chừa 8 lõi; ghi đè --luong-train :287,:537). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 74 (LCAT-05)

**Mục LCAT-05 — Longcat (BIA):** AI Longcat nói: «Transfer learning: model.load_state_dict(torch.load('pikafish_net.pt'))». Bằng chứng của đội: không có tệp .pt; net là .nnue nhị phân, phải qua serialize.py. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 75 (LCAT-01)

**Mục LCAT-01 — Longcat (SAI):** AI Longcat nói: «Pikafish 'Input 768 → FC1 256 → FC2 256 → FC3 1'». Bằng chứng của đội: WebFetch nnue_architecture.h master: 1024→32→32→1 + FT FullThreats/HalfKAv2_hm. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
