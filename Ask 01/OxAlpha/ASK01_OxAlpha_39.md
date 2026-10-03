# ASK 01 — bộ hỏi cho OxAlpha — tệp 39/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 191 (SG-K5)

**Mục SG-K5 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (SAI):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «K5: loss log1p chèn vào `train_nnue_gpu_v3.py`». Bằng chứng của đội: 02/10 grep `log1p` trong `train_nnue_gpu_v3.py` = 0 — đoạn được ghi như trích tệp thật nhưng không có; log-scale cp làm phẳng độ dốc DTM (ngược mục tiêu A3-01). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 192 (SG-K6)

**Mục SG-K6 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (SAI):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «K6: đo chập chờn bằng 60 mẫu `\Processor(_Total)` mỗi 500 ms + `Get-HighResolutionTimer`». Bằng chứng của đội: CLAUDE.md 21/09 04:1x §5: mẫu `Get-Counter` đơn lẻ là rác (44,8/31,0/97,4 %), phải cộng CPU-time theo PID cửa sổ ≥ 20 s; `Get-HighResolutionTimer` không phải c…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 193 (SG-K7)

**Mục SG-K7 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (SAI):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «K7: mỗi AI `git lfs lock` + `git push` nhật ký 30 s/lần; pull --rebase khi lỗi». Bằng chứng của đội: trái luật cứng: cấm push/remote ra ngoài (memory khong-day-code-len-ngoai) + mỗi lane một tệp `_LANE_NOTE/lock` + commit `-- <path>` (CLAUDE.md §4.0, §4.7). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 194 (T13-10)

### T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha — SAI 9 · BỊA 2
Hỏi lại **đúng OxAlpha**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | OxAlpha nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| OX-0.1ngun | **BỊA** | 0.1 nguồn: `github.com/pgh26/chessdb`, «giấy của Ping-Che Yen», «o_ngcf/chessdb» | WebFetch 27/09: **404**. Tác giả chessdb.cn là noobpwnftw (`github.com/noobpwnftw/chessdb`) | ASK01_2609 |
| OX-0.4PGN | **BỊA** | 0.4 PGN: comment `{…}` giữ chú giải; spec `saremba.de/chessx/misc/pgn.htm` | 404 (fetch 27/09); spec thật `saremba.de/chessgml/standards/pgn/pgn-complete.htm`. Ý PGN comment thì đúng | ASK01_2609 |
| OX-0.1(a)shc | **SAI** | 0.1(a) số học: 9.900 thế/s ⇒ 8,5·10⁸/ngày ✓; «30 ngày ≈ 2,5·10⁸ thế» | 8,55·10⁸ × 30 = **2,57·10¹⁰** (Ox lệch ×100). Kết luận «không vét được 1,14·10²⁰» vẫn đứng | ASK01_2609 |
| OX-0.2kho | **SAI** | 0.2 khoá: vkey INT64 + `sqlite3_bind_int64`; UNIQUE index trên **vkey** | PK thật là **(vkey, vmove)** (`Backend.cs:4323`, `sua_kho_real_bookcam.py:780`) — UNIQUE(vkey) một mình sẽ từ chối mọi vmove khác của cùng thế ⇒ sai lược đồ. b… | ASK01_2609 |
| OX-0.3bc4 | **SAI** | 0.3 bước 4: xoá book cấm bằng `DELETE … WHERE vkey IN banned_vkeys` | book cấm lọc theo cột **`source`** (`sua_kho_real_bookcam.py:477 cam(source)`), không theo vkey — xoá theo vkey cuốn cả dòng hợp lệ cùng thế từ book khác | ASK01_2609 |
| OX-0.3bc6 | **SAI** | 0.3 bước 6: «binade 2^55 âm là [−2^55, −2^54)»; query `vkey < −288230376151711744 AND > −576460752303423488`; thêm `−36… | binade 2^55 = / x / ∈ [2^55, 2^56); 288230376151711744 = **2^58**, 576460752303423488 = **2^59** ⇒ ba khoảng khác nhau trong một câu. Đội đã quét bằng `typeof`… | ASK01_2609 |
| OX-0.3bc78 | **SAI** | 0.3 bước 7–8: VACUUM đổi rowid, cần 2× đĩa; sau đó UNIQUE index vkey | VACUUM đúng theo docs; UNIQUE(vkey) sai lược đồ như trên | ASK01_2609 |
| OX-0.3giaodch | **SAI** | 0.3 giao dịch: không một giao dịch 30 GB; batch 1–5 M dòng theo rowid; WAL + synchronous NORMAL; backup vật lý trước | đội làm MỘT giao dịch nhưng trên **bản chép W**, kho K không bị ghi (`sua_kho_real_bookcam.py:11-19`) ⇒ rollback = giữ K, an toàn tương đương; đội có rc 5 thiế… | ASK01_2609 |
| OX-0.4mu | **SAI** | 0.4 mẫu: «chọn ngẫu nhiên 1–3 vị trí mỗi ván» | owner 25/09: nạp FEN hết, chỉ check trùng; `nap_cbl_staging.py:16` nạp MỌI nút mọi biến | ASK01_2609 |
| OX-A2-16 | **SAI** | A2-16: bài OxAlpha — header safetensors «big-endian» sai | ô «◐ (header safetensors «big-endian» sai)» | ASK02_A2_2309 |
| OX-A2-32 | **SAI** | A2-32: bài OxAlpha — Seer URL sai; bỏ qua «cùng net» | ô «◐ (Seer URL sai; bỏ qua «cùng net»)» | ASK02_A2_2309 |

---

## Câu 195 (OX-0.1ngun)

**Mục OX-0.1ngun — T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha (BỊA):** AI T13-10 — Phần I bổ sung (lô B2, B4–B9): OxAlpha nói: «0.1 nguồn: `github.com/pgh26/chessdb`, «giấy của Ping-Che Yen», «o_ngcf/chessdb»». Bằng chứng của đội: WebFetch 27/09: **404**. Tác giả chessdb.cn là noobpwnftw (`github.com/noobpwnftw/chessdb`). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
