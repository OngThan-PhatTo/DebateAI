# ASK 01 — bộ hỏi cho OxAlpha — tệp 34/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 166 (B8-LG-04)

**Mục B8-LG-04 — T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-06 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «test_cotuong.py kiểm liet_ke_nuoc_phao». Bằng chứng của đội: đo 02/10: `python test_cotuong.py` ⇒ ImportError «cannot import name 'parse_board'» (:3; Ling chỉ có parse_board_from_fen :112) — bộ test chưa từng chạy. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 167 (T13-07)

### T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron — SAI 2 · BỊA 10 (phần 1/2)
Hỏi lại **đúng Nemotron**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Nemotron nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| NM-A2-01 | **BỊA** | A2-01: bài Nemotron — 3 repo bịa | ô «✗ (3 repo bịa)» | ASK02_A2_2309 |
| NM-A2-03 | **BỊA** | A2-03: bài Nemotron — nguồn bịa | ô «◐ (nguồn bịa)» | ASK02_A2_2309 |
| NM-A2-06 | **BỊA** | A2-06: bài Nemotron — affinity=auto bịa | ô «◐ (affinity=auto bịa)» | ASK02_A2_2309 |
| NM-A2-14 | **BỊA** | A2-14: bài Nemotron — kênh bịa | ô «◐ (kênh bịa)» | ASK02_A2_2309 |
| NM-A2-17 | **BỊA** | A2-17: bài Nemotron — đường dẫn bịa | ô «◐ (đường dẫn bịa)» | ASK02_A2_2309 |
| NM-A2-32 | **BỊA** | A2-32: bài Nemotron — Ethereal bịa | ô «✗ (Ethereal bịa)» | ASK02_A2_2309 |
| NM-A2-34 | **BỊA** | A2-34: bài Nemotron — quy trình tìm bịa | ô «◐ (quy trình tìm bịa)» | ASK02_A2_2309 |
| NM-A2-35 | **BỊA** | A2-35: bài Nemotron — sharkhook.sys, Bilibili bịa | ô «✗ (sharkhook.sys, Bilibili bịa)» | ASK02_A2_2309 |
| NM-MA-PROFILE | **BỊA** | Mã kèm: `profiles/*.json` hồ sơ app cờ (play.sharkchess.com, window.__SHARK_GAME__, memory_offset 0x123456) | A1 §4: dữ liệu bịa (cả 野狐围棋 = cờ vây) | ASK02_A2_2309 |
| NM-MA-STUB | **BỊA** | Mã kèm: `ScreenshotBoardSource.cs:345-353` ExtractBoardRegion; `VectorNotationSource.cs:423-452` không thẻ FEN ⇒ trả th… | A1 §4: ExtractBoardRegion trả mảng toàn 0 (không đọc pixel) — stub báo «đã nhận»; nhận diện «thành công» giả = đúng bệnh §1 CLAUDE.md | ASK02_A2_2309 |
| NM-A2-13 | **SAI** | A2-13: bài Nemotron — contrast sai | ô «✗ (contrast sai)» | ASK02_A2_2309 |
| NM-A2-18-GUONG | **SAI** | A2-18: gương ngang «không hợp lệ» (L773–776) | A01-10 `cong_perft_guong.py` perft gương = gốc trên bộ thế (27/09) | ASK02_A2_2309 |

---

## Câu 168 (NM-A2-01)

**Mục NM-A2-01 — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (BỊA):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «A2-01: bài Nemotron — 3 repo bịa». Bằng chứng của đội: ô «✗ (3 repo bịa)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 169 (NM-A2-03)

**Mục NM-A2-03 — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (BỊA):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «A2-03: bài Nemotron — nguồn bịa». Bằng chứng của đội: ô «◐ (nguồn bịa)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 170 (NM-A2-06)

**Mục NM-A2-06 — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (BỊA):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «A2-06: bài Nemotron — affinity=auto bịa». Bằng chứng của đội: ô «◐ (affinity=auto bịa)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
