# ASK 01 — bộ hỏi cho OxAlpha — tệp 55/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 271 (T13-18)

### T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny — SAI 9 · BỊA 0
Hỏi lại **đúng SpaceBunny**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | SpaceBunny nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| SB-0.1(a) | **SAI** | 0.1(a): bậc 2 ≈ 167 G chỉ số ≈ 84 GB; «≈ 19 ngày» ở 9.900 thế/s | XiangqiIndex.md (master) ✓ «167 G», «334/4 = 83.5 GB»; README Attempt 12 ✓. Nhưng 1,67·10¹¹/9.900 ≈ 1,69·10⁷ s ≈ **195 ngày** — SB gõ 1,67·10¹⁰ nên thiếu 10× | ASK01_2609 |
| SB-0.1(c) | **SAI** | 0.1(c): target = remap mate `base + decay^ply·scale` (4000/4000/0,85) rồi logistic chung; không WDL-only | `nnue.py:18-35` ✓ (SB 19-36), `config.py:67-72` ✓ (SB 106-111 LỆCH). Số SB sai cả 4: 0,85⁵·4000+4000 = **5775** (SB 5474) · ^10 → **4787** (SB 4265) · ^20 → **… | ASK01_2609 |
| SB-0.2 | **SAI** | 0.2: append-only + watermark; kiểm hậu gộp bằng quick_check + đếm theo nguồn + mẫu `vkey % 1000`; xxh3 thay CRC-32 làm… | đoạn Python «ca kiểm» đếm dòng CÓ trong data.db rồi gọi là MISSING, rc=3 khi n≠0 ⇒ luôn đỏ khi có dữ liệu — logic ngược | ASK01_2609 |
| SB-0.3(4) | **SAI** | 0.3(4): «số đội 494.581.545 sai, phải là 402.086.399» | 92,5 M REAL không bị xoá mà đổi kiểu; 64,8 M trùng là TẬP CON của 92,5 M. 567.206.917 − 64.795.168 − 7.830.204 = **494.581.545** (đội đúng); SB trừ chồng | ASK01_2609 |
| SB-A3-05(a) | **SAI** | A3-05(a): bảng THUA dtc>remain: trả cận hoà thay vì bỏ | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| C18-SB-04 | **SAI** | Pony V6 (LyliaEngine/Pony_Diffusion_V6_XL) giấy phép cdla-permissive-2.0 ⇒ an toàn thương mại, dễ hợp giấy phép hơn V7 | README repo đó (02/10): bản sao bên thứ ba `base_model: Bakanayatsu/…`, tag `template:sd-lora`; bản gốc Civitai #257749 API 02/10: allowCommercialUse [Image, R… | COMFY18_2909 |
| C18-SB-05 | **SAI** | stabilityai/sd-turbo = «SD1.5 turbo»; playground-v2.5 nền «SD2.5» | model card 02/10: sd-turbo «distilled version of Stable Diffusion 2.1»; playground-v2.5 «same architecture as Stable Diffusion XL» (không có SD2.5) | COMFY18_2909 |
| C18-SB-06 | **SAI** | SDXL dùng 2 text encoder 77 + 227 token | stabilityai/stable-diffusion-xl-base-1.0 tokenizer/tokenizer_config.json và tokenizer_2/tokenizer_config.json: model_max_length 77 cả hai | COMFY18_2909 |
| SB1-5308 | **SAI** | Bằng chứng câu hỏi sai: train_nnue_gpu_v3.py:5308-5311 là print [thang-cp]; set_num_threads thật ở tu_canh_tai_nguyen.p… | đo 02/10 bản 01/10 (git 98097248^): :5308-5311 = _tucanh.dat_luong_torch(torch, …) — chính hàm gọi tu_canh_tai_nguyen.py:340 set_num_threads; print [thang-cp]… | ASK01_TH_0110 |

---

## Câu 272 (SB-0.1(a))

**Mục SB-0.1(a) — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «0.1(a): bậc 2 ≈ 167 G chỉ số ≈ 84 GB; «≈ 19 ngày» ở 9.900 thế/s». Bằng chứng của đội: XiangqiIndex.md (master) ✓ «167 G», «334/4 = 83.5 GB»; README Attempt 12 ✓. Nhưng 1,67·10¹¹/9.900 ≈ 1,69·10⁷ s ≈ **195 ngày** — SB gõ 1,67·10¹⁰ nên thiếu 10×. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 273 (SB-0.1(c))

**Mục SB-0.1(c) — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «0.1(c): target = remap mate `base + decay^ply·scale` (4000/4000/0,85) rồi logistic chung; không WDL-only». Bằng chứng của đội: `nnue.py:18-35` ✓ (SB 19-36), `config.py:67-72` ✓ (SB 106-111 LỆCH). Số SB sai cả 4: 0,85⁵·4000+4000 = **5775** (SB 5474) · ^10 → **4787** (SB 4265) · ^20 → **…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 274 (SB-0.2)

**Mục SB-0.2 — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «0.2: append-only + watermark; kiểm hậu gộp bằng quick_check + đếm theo nguồn + mẫu `vkey % 1000`; xxh3 thay CRC-32 làm…». Bằng chứng của đội: đoạn Python «ca kiểm» đếm dòng CÓ trong data.db rồi gọi là MISSING, rc=3 khi n≠0 ⇒ luôn đỏ khi có dữ liệu — logic ngược. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 275 (SB-0.3(4))

**Mục SB-0.3(4) — T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny (SAI):** AI T13-18 — Phần I bổ sung (lô B2, B4–B9): SpaceBunny nói: «0.3(4): «số đội 494.581.545 sai, phải là 402.086.399»». Bằng chứng của đội: 92,5 M REAL không bị xoá mà đổi kiểu; 64,8 M trùng là TẬP CON của 92,5 M. 567.206.917 − 64.795.168 − 7.830.204 = **494.581.545** (đội đúng); SB trừ chồng. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
