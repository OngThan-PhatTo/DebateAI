# ASK 01 — bộ hỏi cho OxAlpha — tệp 47/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 231 (C18-BP-03)

**Mục C18-BP-03 — T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «SD-Turbo thuộc kiến trúc SDXL». Bằng chứng của đội: model card 02/10: «distilled version of Stable Diffusion 2.1». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 232 (C18-BP-04)

**Mục C18-BP-04 — T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «Pony, Illustrious, Anima là ba không gian latent khác nhau». Bằng chứng của đội: cả ba là UNet họ SDXL dùng chung VAE SDXL (latent 4 kênh /8) — LoRA lệch hệ là do trọng số UNet/text encoder đã fine-tune, không phải latent khác; chính bài gh…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 233 (BP1-10)

**Mục BP1-10 — T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle (SAI):** AI T13-13 — Phần I bổ sung (lô B2, B4–B9): BigPickle nói: «A1-02(2)/A1-10: feature_transformer_fallback.py dựng ma trận DENSE batch×10530 ⇒ đổi sang embedding_bag giảm băng thông…». Bằng chứng của đội: docstring feature_transformer_fallback.py (bản 01/10) ghi rõ dùng F.embedding_bag(mode='sum') THAY VÌ dense — BigPickle đọc ngược chú thích; SpaceBunny/MuseSpa…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 234 (T13-14)

### T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity — SAI 8 · BỊA 1
Hỏi lại **đúng Antigravity**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Antigravity nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| AG1-02 | **BỊA** | A1-02: «Thử nghiệm đã cho thấy torch 2.12 + Lightning 1.9.5 chạy được CPU nếu không gọi .cuda()» | không có phép thử nào như vậy: T9 (Phần B) chưa chạy; đội đã chọn port Lightning 2 (commit 66180db8). AI không có máy đội ⇒ khai «thử nghiệm đã cho thấy» là bị… | ASK01_TH_0110 |
| AG1-01 | **SAI** | A1-01: DirectML là đường DUY NHẤT trên Windows cho iGPU 8060S; iGPU không hỗ trợ ROCm 6.x | đo 01/10 M1 (_SOI/B1_ASK05_FABLE_2026-10-01.md §0): .venv torch 2.12.0a0+rocm7.13, torch.cuda.is_available()=True trên Radeon 8060S Windows; Ask 01 A1-01 đã nê… | ASK01_TH_0110 |
| AG1-02b | **SAI** | A1-02(1): port Lightning 2 = «thay add_argparse_args bằng pl.Trainer.add_argparse_args» | tự mâu thuẫn (thay hàm bằng chính nó, hàm đã bị xoá ở Lightning 2); cách đúng = khai báo cờ tường minh (66180db8) | ASK01_TH_0110 |
| AG1-03 | **SAI** | A1-03: chia 150 lõi cho train, 42 lõi cho datagen | nút thắt là datagen (train iGPU 10.540/s ≫ datagen 549/s, CHAM_ANSWER01_A1-56A1-61:191,209 — số có trong câu hỏi) ⇒ phải dồn lõi cho datagen; đề xuất ngược chi… | ASK01_TH_0110 |
| AG1-09 | **SAI** | A1-09: net Pikafish ≈ 35k tham số ≈ 137 KB «đối với một single net» | ongthan.nnue = 50.706.378 B (M13); BigPickle đã NHẬN đúng lỗi này (BPCK-02) | ASK01_TH_0110 |
| AG1-10 | **SAI** | A1-10: FT ≈ 3,4×10⁸ FLOP/batch; 24 lõi ≈ 1,2 TFLOP ⇒ ≈ 3.500 mẫu/s | số học: 1,2e12 ÷ 3,4e8 ≈ 3.500 BATCH/s (×16.384 ≈ 5,7e7 mẫu/s), không phải 3.500 mẫu/s; tiền đề cũng sai: FT là gom thưa ~32 feature/bên (feature_transformer_f… | ASK01_TH_0110 |
| AG1-12 | **SAI** | A1-12: ROCm 6.x Linux nhanh 2–3× DirectML «đánh giá trên RTX 3090» | ROCm không chạy trên RTX 3090 (GPU NVIDIA) ⇒ phép so dẫn ra không tồn tại; không nguồn | ASK01_TH_0110 |
| AG1-15 | **SAI** | A1-15: «Pikafish bản CC0 cho weights (nnue) — cho phép thương mại» | net official pikafish.nnue: «No commercial use without permission» (Networks README, CHAM A1-04 21/09); CC0 chỉ là net Fairy C07E94A5 (luật owner 20/09 15:1x) | ASK01_TH_0110 |
| AG1-16 | **SAI** | A1-16: DTM→cp tuyến tính cp = DTM×0,5 «giống Stockfish» (cite search.cpp:1394-1397) | trainer nnue-pytorch dùng remap MŨ base 4000 / decay 0,85 (model/nnue.py:19-36, config.py:106-110 @9f729465 — đội xác minh 27/09; SpaceBunny cùng vòng trích đú… | ASK01_TH_0110 |

---

## Câu 235 (AG1-02)

**Mục AG1-02 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (BỊA):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-02: «Thử nghiệm đã cho thấy torch 2.12 + Lightning 1.9.5 chạy được CPU nếu không gọi .cuda()»». Bằng chứng của đội: không có phép thử nào như vậy: T9 (Phần B) chưa chạy; đội đã chọn port Lightning 2 (commit 66180db8). AI không có máy đội ⇒ khai «thử nghiệm đã cho thấy» là bị…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
