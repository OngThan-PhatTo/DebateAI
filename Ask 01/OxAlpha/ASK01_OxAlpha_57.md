# ASK 01 — bộ hỏi cho OxAlpha — tệp 57/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 281 (T13-19)

### T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen — SAI 5 · BỊA 0
Hỏi lại **đúng Qwen**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Qwen nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| QW-A3 | **SAI** | A3: `INSERT INTO new SELECT … WHERE` + rename bảng; không UPDATE/DELETE hàng loạt; query `typeof(vkey)!='integer' OR vk… | `vkey < I64MIN` **không bao giờ đúng** (I64MIN là cận dưới) — bỏ sót đúng ca đội hỏi (binade 2^55 âm); rebuild bảng trong cùng tệp cũng cần 2× đĩa như bản chép… | ASK01_2609 |
| QW-K2WAL | **SAI** | K2 WAL: «**Có**, chuyển kho thật sang WAL» + `wal_checkpoint(TRUNCATE)` | (i) WAL **mất nguyên tử chéo tệp** khi ATTACH (`sqlite.org/lang_attach.html` fetch 27/09) — mâu thuẫn với chính K1 của Qwen; (ii) backup robocopy đội **loại `*… | ASK01_2609 |
| QW-K2kho | **SAI** | K2 khoá: file lock `FileShare.None` hoặc `PRAGMA locking_mode = EXCLUSIVE` | file lock: đội dùng cổng «0 tiến trình có kho trong CommandLine» (`sua_kho_real_bookcam.py` `doi`); `locking_mode=EXCLUSIVE` **chặn luôn GUI đọc** — trái yêu c… | ASK01_2609 |
| QW-K3 | **SAI** | K3: SHA-256 tệp vào `source_files`; `chardet` tự dò GBK/Shift-JIS; lọc theo hash; ca kiểm 0 dòng cấm | trùng BP-12 (+1 phiếu). `chardet` **lệch vấn đề**: mojibake do LPStr ANSI ghi sai lúc nạp (`Backend.cs:27`), không phải encoding tệp — cần ánh xạ ngược cp1252→… | ASK01_2609 |
| QW-K4 | **SAI** | K4: YOLOv8n/11n fine-tune đa skin thay template; «đã bắt» = **28–32 quân** + đối xứng; ADB tap / LDPlayer API; không bắ… | K4 đòi «không cần huấn luyện lớn» — fine-tune YOLO cần bộ ảnh đa skin đội chưa có; **28–32 sai**: giữa ván < 28 quân là bình thường, chỉ khai cuộc = 32; đội 2… | ASK01_2609 |

---

## Câu 282 (QW-A3)

**Mục QW-A3 — T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen (SAI):** AI T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen nói: «A3: `INSERT INTO new SELECT … WHERE` + rename bảng; không UPDATE/DELETE hàng loạt; query `typeof(vkey)!='integer' OR vk…». Bằng chứng của đội: `vkey < I64MIN` **không bao giờ đúng** (I64MIN là cận dưới) — bỏ sót đúng ca đội hỏi (binade 2^55 âm); rebuild bảng trong cùng tệp cũng cần 2× đĩa như bản chép…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 283 (QW-K2WAL)

**Mục QW-K2WAL — T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen (SAI):** AI T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen nói: «K2 WAL: «**Có**, chuyển kho thật sang WAL» + `wal_checkpoint(TRUNCATE)`». Bằng chứng của đội: (i) WAL **mất nguyên tử chéo tệp** khi ATTACH (`sqlite.org/lang_attach.html` fetch 27/09) — mâu thuẫn với chính K1 của Qwen; (ii) backup robocopy đội **loại `*…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 284 (QW-K2kho)

**Mục QW-K2kho — T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen (SAI):** AI T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen nói: «K2 khoá: file lock `FileShare.None` hoặc `PRAGMA locking_mode = EXCLUSIVE`». Bằng chứng của đội: file lock: đội dùng cổng «0 tiến trình có kho trong CommandLine» (`sua_kho_real_bookcam.py` `doi`); `locking_mode=EXCLUSIVE` **chặn luôn GUI đọc** — trái yêu c…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 285 (QW-K3)

**Mục QW-K3 — T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen (SAI):** AI T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen nói: «K3: SHA-256 tệp vào `source_files`; `chardet` tự dò GBK/Shift-JIS; lọc theo hash; ca kiểm 0 dòng cấm». Bằng chứng của đội: trùng BP-12 (+1 phiếu). `chardet` **lệch vấn đề**: mojibake do LPStr ANSI ghi sai lúc nạp (`Backend.cs:27`), không phải encoding tệp — cần ánh xạ ngược cp1252→…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
