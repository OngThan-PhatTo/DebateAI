# ASK 01 — bộ hỏi cho OxAlpha — tệp 49/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 241 (AG1-12)

**Mục AG1-12 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-12: ROCm 6.x Linux nhanh 2–3× DirectML «đánh giá trên RTX 3090»». Bằng chứng của đội: ROCm không chạy trên RTX 3090 (GPU NVIDIA) ⇒ phép so dẫn ra không tồn tại; không nguồn. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 242 (AG1-15)

**Mục AG1-15 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-15: «Pikafish bản CC0 cho weights (nnue) — cho phép thương mại»». Bằng chứng của đội: net official pikafish.nnue: «No commercial use without permission» (Networks README, CHAM A1-04 21/09); CC0 chỉ là net Fairy C07E94A5 (luật owner 20/09 15:1x). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 243 (AG1-16)

**Mục AG1-16 — T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity (SAI):** AI T13-14 — Phần I bổ sung (lô B2, B4–B9): Antigravity nói: «A1-16: DTM→cp tuyến tính cp = DTM×0,5 «giống Stockfish» (cite search.cpp:1394-1397)». Bằng chứng của đội: trainer nnue-pytorch dùng remap MŨ base 4000 / decay 0,85 (model/nnue.py:19-36, config.py:106-110 @9f729465 — đội xác minh 27/09; SpaceBunny cùng vòng trích đú…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 244 (T13-15)

### T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký — SAI 2 · BỊA 1
Hỏi lại **đúng Không ký**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Không ký nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| UNK-A2-02 | **BỊA** | A2-02: «Tổng/Đỉnh ở hàng 1/10», mã `if (p3 == null)` cho Point, hệ số 0,707 | §8.6 `DEBATE_AUTOPLAY…:173`: Point là struct không null được; 0,707 không có cơ sở | ASK02_A2_2309 |
| C18-UN-01 | **SAI** | Danh sách ~200 tên model/LoRA 18+ «để dùng trong workflow» | không đạt yêu cầu Ask mục 3 (ASK_COMFYUI_MODEL_18PLUS_2026-09-29.md @ee701c81): 0 mục có tên tệp/byte/link tải; 50 hyperlink trong docx, 21 là trang tìm kiếm/e… | COMFY18_2909 |
| C18-UN-02 | **SAI** | Đưa «Petite adult body LoRA» vào danh sách (kèm «chỉ dùng khung người lớn») | trái yêu cầu 6 của chính Ask (loại tuổi mơ hồ) + luật đội chặn trẻ em; kho đội không có mục này: quét 9 tệp SourceCode/ComfyUI_Pro/ComfyUIPro/Assets/kho/model_… | COMFY18_2909 |

---

## Câu 245 (UNK-A2-02)

**Mục UNK-A2-02 — T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký (BỊA):** AI T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký nói: «A2-02: «Tổng/Đỉnh ở hàng 1/10», mã `if (p3 == null)` cho Point, hệ số 0,707». Bằng chứng của đội: §8.6 `DEBATE_AUTOPLAY…:173`: Point là struct không null được; 0,707 không có cơ sở. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
