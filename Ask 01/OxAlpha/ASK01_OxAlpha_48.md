# ASK 01 — bộ hỏi cho OxAlpha — tệp 48/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 236 (AG1-01)

**Mục AG1-01 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-01: DirectML là đường DUY NHẤT trên Windows cho iGPU 8060S; iGPU không hỗ trợ ROCm 6.x». Bằng chứng của đội: đo 01/10 M1 (_SOI/B1_ASK05_FABLE_2026-10-01.md §0): .venv torch 2.12.0a0+rocm7.13, torch.cuda.is_available()=True trên Radeon 8060S Windows; Ask 01 A1-01 đã nê…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 237 (AG1-02b)

**Mục AG1-02b — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-02(1): port Lightning 2 = «thay add_argparse_args bằng pl.Trainer.add_argparse_args»». Bằng chứng của đội: tự mâu thuẫn (thay hàm bằng chính nó, hàm đã bị xoá ở Lightning 2); cách đúng = khai báo cờ tường minh (66180db8). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 238 (AG1-03)

**Mục AG1-03 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-03: chia 150 lõi cho train, 42 lõi cho datagen». Bằng chứng của đội: nút thắt là datagen (train iGPU 10.540/s ≫ datagen 549/s, CHAM_ANSWER01_A1-56A1-61:191,209 — số có trong câu hỏi) ⇒ phải dồn lõi cho datagen; đề xuất ngược chi…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 239 (AG1-09)

**Mục AG1-09 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-09: net Pikafish ≈ 35k tham số ≈ 137 KB «đối với một single net»». Bằng chứng của đội: ongthan.nnue = 50.706.378 B (M13); BigPickle đã NHẬN đúng lỗi này (BPCK-02). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 240 (AG1-10)

**Mục AG1-10 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-10: FT ≈ 3,4×10⁸ FLOP/batch; 24 lõi ≈ 1,2 TFLOP ⇒ ≈ 3.500 mẫu/s». Bằng chứng của đội: số học: 1,2e12 ÷ 3,4e8 ≈ 3.500 BATCH/s (×16.384 ≈ 5,7e7 mẫu/s), không phải 3.500 mẫu/s; tiền đề cũng sai: FT là gom thưa ~32 feature/bên (feature_transformer_f…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
