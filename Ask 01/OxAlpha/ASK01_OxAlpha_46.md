# ASK 01 — bộ hỏi cho OxAlpha — tệp 46/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 226 (BP-A2-07)

**Mục BP-A2-07 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A2-07: bài BigPickle — sai số học». Bằng chứng của đội: ô «◐ (**sai số học**)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 227 (BP-A2-21)

**Mục BP-A2-21 — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A2-21: bài BigPickle — 6.200 sai». Bằng chứng của đội: ô «◐ (**6.200 sai**)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 228 (BP-AUTO-VERIFY)

**Mục BP-AUTO-VERIFY — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A2-02: tự xác nhận bằng cách đọc lại 32 quân thế khai cuộc trên khung vừa học». Bằng chứng của đội: §8.6 `DEBATE_AUTOPLAY_NHAN_BAN_CO_2026-09-21.md:167`: đọc lại trên CHÍNH khung vừa học là tự-so-với-mình — đo được `up1` (cờ úp) vẫn PASS với nhãn cờ thường; đ…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 229 (A1-06-BP)

**Mục A1-06-BP — T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-12 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A1-06: giao nguồn theo §6(c) (offer kèm)». Bằng chứng của đội: CHAM_ANSWER01_A1-01A1-18_2026-09-21.md §5: §6(c) nguyên văn «only occasionally and noncommercially, and only if you received the object code with such an offer…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 230 (T13-13)

### T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle — SAI 3 · BỊA 0 (phần 2/2)
Hỏi lại **đúng BigPickle**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | BigPickle nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| C18-BP-03 | **SAI** | SD-Turbo thuộc kiến trúc SDXL | model card 02/10: «distilled version of Stable Diffusion 2.1» | COMFY18_2909 |
| C18-BP-04 | **SAI** | Pony, Illustrious, Anima là ba không gian latent khác nhau | cả ba là UNet họ SDXL dùng chung VAE SDXL (latent 4 kênh /8) — LoRA lệch hệ là do trọng số UNet/text encoder đã fine-tune, không phải latent khác; chính bài gh… | COMFY18_2909 |
| BP1-10 | **SAI** | A1-02(2)/A1-10: feature_transformer_fallback.py dựng ma trận DENSE batch×10530 ⇒ đổi sang embedding_bag giảm băng thông… | docstring feature_transformer_fallback.py (bản 01/10) ghi rõ dùng F.embedding_bag(mode='sum') THAY VÌ dense — BigPickle đọc ngược chú thích; SpaceBunny/MuseSpa… | ASK01_TH_0110 |
