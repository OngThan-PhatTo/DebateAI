# ASK 01 — bộ hỏi cho OxAlpha — tệp 40/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 196 (OX-0.4PGN)

**Mục OX-0.4PGN — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (BỊA):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.4 PGN: comment `{…}` giữ chú giải; spec `saremba.de/chessx/misc/pgn.htm`». Bằng chứng của đội: 404 (fetch 27/09); spec thật `saremba.de/chessgml/standards/pgn/pgn-complete.htm`. Ý PGN comment thì đúng. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 197 (OX-0.1(a)shc)

**Mục OX-0.1(a)shc — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.1(a) số học: 9.900 thế/s ⇒ 8,5·10⁸/ngày ✓; «30 ngày ≈ 2,5·10⁸ thế»». Bằng chứng của đội: 8,55·10⁸ × 30 = **2,57·10¹⁰** (Ox lệch ×100). Kết luận «không vét được 1,14·10²⁰» vẫn đứng. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 198 (OX-0.2kho)

**Mục OX-0.2kho — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.2 khoá: vkey INT64 + `sqlite3_bind_int64`; UNIQUE index trên **vkey**». Bằng chứng của đội: PK thật là **(vkey, vmove)** (`Backend.cs:4323`, `sua_kho_real_bookcam.py:780`) — UNIQUE(vkey) một mình sẽ từ chối mọi vmove khác của cùng thế ⇒ sai lược đồ. b…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 199 (OX-0.3bc4)

**Mục OX-0.3bc4 — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.3 bước 4: xoá book cấm bằng `DELETE … WHERE vkey IN banned_vkeys`». Bằng chứng của đội: book cấm lọc theo cột **`source`** (`sua_kho_real_bookcam.py:477 cam(source)`), không theo vkey — xoá theo vkey cuốn cả dòng hợp lệ cùng thế từ book khác. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 200 (OX-0.3bc6)

**Mục OX-0.3bc6 — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.3 bước 6: «binade 2^55 âm là [−2^55, −2^54)»; query `vkey < −288230376151711744 AND > −576460752303423488`; thêm `−36…». Bằng chứng của đội: binade 2^55 = / x / ∈ [2^55, 2^56); 288230376151711744 = **2^58**, 576460752303423488 = **2^59** ⇒ ba khoảng khác nhau trong một câu. Đội đã quét bằng `typeof`…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
