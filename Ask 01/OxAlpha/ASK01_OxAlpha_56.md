# ASK 01 — bộ hỏi cho OxAlpha — tệp 56/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 276 (SB-A3-05(a))

**Mục SB-A3-05(a) — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «A3-05(a): bảng THUA dtc>remain: trả cận hoà thay vì bỏ». Bằng chứng của đội: đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 277 (C18-SB-04)

**Mục C18-SB-04 — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «Pony V6 (LyliaEngine/Pony_Diffusion_V6_XL) giấy phép cdla-permissive-2.0 ⇒ an toàn thương mại, dễ hợp giấy phép hơn V7». Bằng chứng của đội: README repo đó (02/10): bản sao bên thứ ba `base_model: Bakanayatsu/…`, tag `template:sd-lora`; bản gốc Civitai #257749 API 02/10: allowCommercialUse [Image, R…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 278 (C18-SB-05)

**Mục C18-SB-05 — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «stabilityai/sd-turbo = «SD1.5 turbo»; playground-v2.5 nền «SD2.5»». Bằng chứng của đội: model card 02/10: sd-turbo «distilled version of Stable Diffusion 2.1»; playground-v2.5 «same architecture as Stable Diffusion XL» (không có SD2.5). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 279 (C18-SB-06)

**Mục C18-SB-06 — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «SDXL dùng 2 text encoder 77 + 227 token». Bằng chứng của đội: stabilityai/stable-diffusion-xl-base-1.0 tokenizer/tokenizer_config.json và tokenizer_2/tokenizer_config.json: model_max_length 77 cả hai. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 280 (SB1-5308)

**Mục SB1-5308 — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «Bằng chứng câu hỏi sai: train_nnue_gpu_v3.py:5308-5311 là print [thang-cp]; set_num_threads thật ở tu_canh_tai_nguyen.p…». Bằng chứng của đội: đo 02/10 bản 01/10 (git 98097248^): :5308-5311 = _tucanh.dat_luong_torch(torch, …) — chính hàm gọi tu_canh_tai_nguyen.py:340 set_num_threads; print [thang-cp]…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
