# ASK 01 — bộ hỏi cho OxAlpha — tệp 38/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 186 (SG-A312)

**Mục SG-A312 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (BỊA):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «A3-12: diff mẫu sửa `EngineState.cs` (enum AutoMode + singleton)». Bằng chứng của đội: 02/10 `ls`: `TieuLongNu.App/EngineState.cs` không tồn tại; diff có dòng «−» như sửa tệp có sẵn ⇒ dựng tệp ảo; trạng thái thật nằm ở `TuChoi.cs` (ChiXem/DangCha…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 187 (SG-K3-CS)

**Mục SG-K3-CS — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (BỊA):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «K3: đoạn C# «trích từ `Shared/DataFen/tan_cuoc_liet_ke.cs`»». Bằng chứng của đội: 02/10 `ls`: `SourceCode/Shared/DataFen/tan_cuoc_liet_ke.cs` KHÔNG tồn tại (chỉ có `.py`); đoạn mã băm TÊN tệp qua cp1252 không có ở đâu trong kho. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 188 (SG-A301)

**Mục SG-A301 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (SAI):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «A3-01: định nghĩa lại MATE_SENTINEL 30000; `mate_ply = 30000 − /cp/; target = sign·(30000 − mate_ply)`». Bằng chứng của đội: đoạn mã là phép đồng nhất (target = cp) ⇒ không đổi gì; trainer thật `train_nnue_gpu_v3.py:397` MATE_SENTINEL = 20000, `:737` loại. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 189 (SG-A302)

**Mục SG-A302 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (SAI):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «A3-02: thay `return e.pos.pieceAt(to) != Piece::None;` bằng `return e.pos.isLegalMove(move);` để giữ mọi nước». Bằng chứng của đội: lambda `do_filter` trả TRUE = BỎ mẫu (`training_data_loader.cpp:621-624`) ⇒ trả isLegalMove sẽ bỏ gần HẾT mẫu; chỗ cắt thật ở bộ sinh `training_data_generator.…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 190 (SG-K3)

**Mục SG-K3 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (SAI):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «K3: băm SHA-256 TÊN tệp (UTF-8) để chặn sách cấm; sửa mojibake CJK bằng cp1252→UTF-8». Bằng chứng của đội: đội chặn theo SHA256 NỘI DUNG tệp (`Shared/DataFen/nap_cbl_staging.py:15-28`) — băm tên sai đúng chỗ K3 hỏng (hai tên khác cùng nội dung); mojibake GBK/936 khô…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
