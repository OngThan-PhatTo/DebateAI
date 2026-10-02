# ASK 01 — bộ hỏi cho OxAlpha — tệp 62/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 306 (T13-25)

### T13-25 — Phần I bổ sung (lô B2, B4–B9): Claude — SAI 1 · BỊA 0
Hỏi lại **đúng Claude**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Claude nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| CL1-08 | **SAI** | A1-08: STE truyền gradient qua clamp như không có; «Stockfish không clamp cứng khi train, chỉ khi serialize» | trainer nnue-pytorch (fork của đội) clamp cứng khi train: model.py:296-297 «clamp here is used as a clipped relu», :98, :102 (bản 66180db8^) — gradient torch.c… | ASK01_TH_0110 |

---

## Câu 307 (CL1-08)

**Mục CL1-08 — T13-25 — Phần I bổ sung (lô B2, B4–B9): Claude (SAI):** AI T13-25 — Phần I bổ sung (lô B2, B4–B9): Claude nói: «A1-08: STE truyền gradient qua clamp như không có; «Stockfish không clamp cứng khi train, chỉ khi serialize»». Bằng chứng của đội: trainer nnue-pytorch (fork của đội) clamp cứng khi train: model.py:296-297 «clamp here is used as a clipped relu», :98, :102 (bản 66180db8^) — gradient torch.c…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 308 (T13-26)

### T13-26 — Phần I bổ sung (lô B2, B4–B9): GLM — SAI 1 · BỊA 0
Hỏi lại **đúng GLM**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | GLM nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| GL-A2-14 | **SAI** | A2-14: bài GLM 5.3 — số sai | ô «◐ (số sai)» | ASK02_A2_2309 |

---

## Câu 309 (GL-A2-14)

**Mục GL-A2-14 — T13-26 — Phần I bổ sung (lô B2, B4–B9): GLM (SAI):** AI T13-26 — Phần I bổ sung (lô B2, B4–B9): GLM nói: «A2-14: bài GLM 5.3 — số sai». Bằng chứng của đội: ô «◐ (số sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 310 (T13-27)

### T13-27 — Phần I bổ sung (lô B2, B4–B9): Gemini — SAI 1 · BỊA 0
Hỏi lại **đúng Gemini**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Gemini nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| A1-06-GM | **SAI** | A1-06: nghĩa vụ giao nguồn GPL theo §6(a) | như A1-06-GK (CHAM_ANSWER01_A1-01A1-18_2026-09-21.md §5) | ASK_A1_2109 |
