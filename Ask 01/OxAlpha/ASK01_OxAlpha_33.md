# ASK 01 — bộ hỏi cho OxAlpha — tệp 33/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 161 (LG-A2-10)

**Mục LG-A2-10 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «A2-10: bài Ling — trích OpenAI sai». Bằng chứng của đội: ô «◐ (trích OpenAI sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 162 (LG-A2-21)

**Mục LG-A2-21 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «A2-21: bài Ling — toán sai». Bằng chứng của đội: ô «✗ (toán sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 163 (T13-06)

### T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling — SAI 3 · BỊA 0 (phần 3/3)
Hỏi lại **đúng Ling**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Ling nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| B8-LG-02 | **SAI** | CLI perft in số như perft thật | _perft (:132-154) chỉ đếm quân Pháo cả hai bên (docstring «chưa triển khai tất cả quân»): đo 02/10 d1 48 · d2 2.172 · d3 95.496 ≠ 44/1.920/79.666, CLI không bá… | CODE_ENGINE_MAU |
| B8-LG-03 | **SAI** | verdict lặp nước | main() (:163-166) luôn in «HOA» + «Chưa triển khai» ⇒ chấm tự động sẽ đếm đúng mọi ca hoà (kết quả hằng) | CODE_ENGINE_MAU |
| B8-LG-04 | **SAI** | test_cotuong.py kiểm liet_ke_nuoc_phao | đo 02/10: `python test_cotuong.py` ⇒ ImportError «cannot import name 'parse_board'» (:3; Ling chỉ có parse_board_from_fen :112) — bộ test chưa từng chạy | CODE_ENGINE_MAU |

---

## Câu 164 (B8-LG-02)

**Mục B8-LG-02 — T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «CLI perft in số như perft thật». Bằng chứng của đội: _perft (:132-154) chỉ đếm quân Pháo cả hai bên (docstring «chưa triển khai tất cả quân»): đo 02/10 d1 48 · d2 2.172 · d3 95.496 ≠ 44/1.920/79.666, CLI không bá…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 165 (B8-LG-03)

**Mục B8-LG-03 — T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «verdict lặp nước». Bằng chứng của đội: main() (:163-166) luôn in «HOA» + «Chưa triển khai» ⇒ chấm tự động sẽ đếm đúng mọi ca hoà (kết quả hằng). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
