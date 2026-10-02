# ASK 01 — bộ hỏi cho OxAlpha — tệp 36/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 176 (NM-MA-PROFILE)

**Mục NM-MA-PROFILE — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (BỊA):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «Mã kèm: `profiles/*.json` hồ sơ app cờ (play.sharkchess.com, window.__SHARK_GAME__, memory_offset 0x123456)». Bằng chứng của đội: A1 §4: dữ liệu bịa (cả 野狐围棋 = cờ vây). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 177 (NM-MA-STUB)

**Mục NM-MA-STUB — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (BỊA):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «Mã kèm: `ScreenshotBoardSource.cs:345-353` ExtractBoardRegion; `VectorNotationSource.cs:423-452` không thẻ FEN ⇒ trả th…». Bằng chứng của đội: A1 §4: ExtractBoardRegion trả mảng toàn 0 (không đọc pixel) — stub báo «đã nhận»; nhận diện «thành công» giả = đúng bệnh §1 CLAUDE.md. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 178 (NM-A2-13)

**Mục NM-A2-13 — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (SAI):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «A2-13: bài Nemotron — contrast sai». Bằng chứng của đội: ô «✗ (contrast sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 179 (NM-A2-18-GUONG)

**Mục NM-A2-18-GUONG — T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron (SAI):** AI T13-07 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «A2-18: gương ngang «không hợp lệ» (L773–776)». Bằng chứng của đội: A01-10 `cong_perft_guong.py` perft gương = gốc trên bộ thế (27/09). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 180 (T13-08)

### T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron — SAI 3 · BỊA 0 (phần 2/2)
Hỏi lại **đúng Nemotron**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Nemotron nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| NM-MA-C5 | **SAI** | Mã kèm tự xưng C# 5 | A1 §4: dùng `$""`, `?.`, `out var`, tuple, `=>`, `catch…when` — không biên dịch được bằng csc C# 5 của đội | ASK02_A2_2309 |
| NM-MA-PASSGIA | **SAI** | Mã kèm: `integration_test.py:36-80` + `build.proj:55-57` cổng kiểm | A1 §4: accuracy = conf > 0,9 không nhãn (PASS giả); Verify grep `obj\**` sau csc ⇒ luôn xanh — cổng chưa từng đỏ | ASK02_A2_2309 |
| NM-MA-UPDATER | **SAI** | Mã kèm: `Src/App.xaml.cs:69-96` AutoUpdater tải bản phát hành GitHub rồi chạy quyền admin lúc khởi động | A1 §4: placeholder `yourorg/xiangqi-autoplay`, không hash/chữ ký, `catch {}` nuốt — trái luật không đưa mã/tải mạng; KHÔNG chạy | ASK02_A2_2309 |
