# ASK 01 — bộ hỏi cho OxAlpha — tệp 41/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 201 (OX-0.3bc78)

**Mục OX-0.3bc78 — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.3 bước 7–8: VACUUM đổi rowid, cần 2× đĩa; sau đó UNIQUE index vkey». Bằng chứng của đội: VACUUM đúng theo docs; UNIQUE(vkey) sai lược đồ như trên. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 202 (OX-0.3giaodch)

**Mục OX-0.3giaodch — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.3 giao dịch: không một giao dịch 30 GB; batch 1–5 M dòng theo rowid; WAL + synchronous NORMAL; backup vật lý trước». Bằng chứng của đội: đội làm MỘT giao dịch nhưng trên **bản chép W**, kho K không bị ghi (`sua_kho_real_bookcam.py:11-19`) ⇒ rollback = giữ K, an toàn tương đương; đội có rc 5 thiế…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 203 (OX-0.4mu)

**Mục OX-0.4mu — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.4 mẫu: «chọn ngẫu nhiên 1–3 vị trí mỗi ván»». Bằng chứng của đội: owner 25/09: nạp FEN hết, chỉ check trùng; `nap_cbl_staging.py:16` nạp MỌI nút mọi biến. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 204 (OX-A2-16)

**Mục OX-A2-16 — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «A2-16: bài OxAlpha — header safetensors «big-endian» sai». Bằng chứng của đội: ô «◐ (header safetensors «big-endian» sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 205 (OX-A2-32)

**Mục OX-A2-32 — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (SAI):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «A2-32: bài OxAlpha — Seer URL sai; bỏ qua «cùng net»». Bằng chứng của đội: ô «◐ (Seer URL sai; bỏ qua «cùng net»)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
