# ASK 01 — bộ hỏi cho OxAlpha — tệp 19/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 91 (SBUN-09)

**Mục SBUN-09 — SpaceBunny (SAI):** AI SpaceBunny nói: «'10530 đúng bằng HKA của Lc0 — một tầng dày 10530×512'». Bằng chứng của đội: variant.py:42-43 HalfKAv2 NNUE; feature_transformer_fallback.py:17-19 embedding_bag thưa, không dense. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 92 (SBUN-11)

**Mục SBUN-11 — SpaceBunny (SAI):** AI SpaceBunny nói: «Cách 1: 'python tools/train_nnue_gpu_v3.py --device cpu --threads 64'». Bằng chứng của đội: SHIP không có --threads (grep 0 hit); luồng qua --luong-train và tự canh 80 % (tu_canh_tai_nguyen.py:61-63,:287). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 93 (U2909-05)

**Mục U2909-05 — Unknown_2909 (tác giả không rõ) (BIA):** AI Unknown_2909 (tác giả không rõ) nói: «torch.load('pikafish_net.pt') + freeze fc1». Bằng chứng của đội: không có tệp .pt; net .nnue nhị phân. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 94 (U2909-01)

**Mục U2909-01 — Unknown_2909 (tác giả không rõ) (SAI):** AI Unknown_2909 (tác giả không rõ) nói: «Pikafish 'Input 768 → 256 → 256 → 1'». Bằng chứng của đội: WebFetch master 1024→32→32→1. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 95 (U2909-02)

**Mục U2909-02 — Unknown_2909 (tác giả không rõ) (SAI):** AI Unknown_2909 (tác giả không rõ) nói: «DataLoader num_workers=192». Bằng chứng của đội: train.py:18-19. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
