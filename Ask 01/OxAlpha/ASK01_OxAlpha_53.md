# ASK 01 — bộ hỏi cho OxAlpha — tệp 53/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 261 (GRK-ENG-E17)

**Mục GRK-ENG-E17 — T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok (SAI):** AI T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok nói: «TDDH setoption giữa phân tích ⇒ dừng search, trả bestmove sớm; GUI (TLN DatTuyChon) có thể hiểu nhầm». Bằng chứng của đội: uci.cpp:795-797 `dung_search_neu_dang_chay("setoption")` + wait_for_search_finished; TLN UcciEngine.cs:1587-1591 gửi setoption không chờ rảnh. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 262 (GRK-KV-4B-MANUS)

**Mục GRK-KV-4B-MANUS — T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok (SAI):** AI T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok nói: «Manus Answer 01:99 «không có source Kỳ Viện trong repo để trích Program.cs hay ChessHub.cs» (tác giả Manus; Grok phán S…». Bằng chứng của đội: Manus/Answer 01/Manus.md:99 đúng câu đó; ls ⇒ C:\AI\SourceCode\App_CoTuong_Online\Program.cs (sha256 7DDB054F…) và ChessHub.cs tồn tại, ~167 tệp .cs; Grok ĐÚNG…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 263 (GRK-KV-G22b)

**Mục GRK-KV-G22b — T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok (SAI):** AI T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok nói: ««Jeiqi» trong đường Root (:26) là viết sai». Bằng chứng của đội: appsettings.json:26 «C:\ChineseChess\Jeiqi Engine\OngThan» đúng tên thư mục thật: «ls -d /c/ChineseChess/Jeiqi Engine/OngThan» ⇒ tồn tại; CLAUDE.md cũng gọi «J…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 264 (GRK-TB-B9)

**Mục GRK-TB-B9 — T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok (SAI):** AI T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok nói: «Stop TryEnter 200 ms không lấy được khoá vẫn Dispose pipe ⇒ luồng đọc có thể ném ObjectDisposed (chú thích nói cố ý)». Bằng chứng của đội: Engine.cs:1310 `TryEnter(lk, 200)`; :1364-1367 Dispose _pinStdin/_pinStdout bất kể gotLock. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 265 (GRK-TB-T15)

**Mục GRK-TB-T15 — T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok (SAI):** AI T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok nói: «Đường dẫn máy dev viết cứng (EngineSettings.exe, demo .obk) ⇒ máy khác mất tính năng/mở nhầm». Bằng chứng của đội: HopChonNho.cs:382 nằm trong `#if DEV_MAY` (build_all.bat:320-325: đường máy dev chỉ ở DEV_MAY, gói khách 0 hit); KhungCongCu.cs:2406/2530 trong `if (!boQuaDev)…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
