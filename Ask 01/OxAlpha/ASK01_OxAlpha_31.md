# ASK 01 — bộ hỏi cho OxAlpha — tệp 31/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 151 (T13-05)

### T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling — SAI 10 · BỊA 2 (phần 2/3)
Hỏi lại **đúng Ling**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Ling nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| LG-A2-22 | **BỊA** | A2-22: bài Ling — UCI option bịa | ô «✗ (UCI option bịa)» | ASK02_A2_2309 |
| LG-A2-30 | **BỊA** | A2-30: bài Ling — TradingView API bịa | ô «◐ (TradingView API bịa)» | ASK02_A2_2309 |
| LG-0.2 | **SAI** | 0.2: không có giao dịch atomic giữa 2 tệp; kiểm `COUNT`/`MIN,MAX`/giao = 0; **giữ dòng đã xử lý trong datanoscore, đánh… | (c) trái bất biến owner 26/09 `datanoscore ∩ data.db = ∅` (Ask ASK12-2 ghi rõ) — giữ dòng đã chấm trong hàng đợi là phá bất biến. (a) SQLite **có** giao dịch a… | ASK01_2609 |
| LG-0.3 | **SAI** | 0.3: bảng mới + `CAST`, không 1 giao dịch 30 GB; lô 1 M; bảng tiến độ; khoá sai = `vkey <= 0 OR vkey >= 2^55` | vkey là Zobrist **int64 toàn dải**: `gop_kho2_vao_kho.py:154,225` quét `I64MIN..I64MAX`, `loc_staging_lon_kho.py:707` sinh khoá âm/dương hợp lệ ⇒ bộ lọc của Li… | ASK01_2609 |
| LG-A2-08 | **SAI** | A2-08: theo lô 1k–10k; **Lc0 cửa sổ ~100 ván**; λ 0,3 cho cờ tướng; thầy ×2–5; **CPU float32 phải giống hệt GPU** | Lc0 cửa sổ dữ liệu cỡ **trăm nghìn ván**, không phải 100; CPU vs GPU float32 **không** bit-exact (thứ tự cộng/cuDNN) — đúng cái đội hỏi. `lc0.mosquitochess.org… | ASK01_2609 |
| LG-K2 | **SAI** | K2: lô 100–500k + `merge_progress` + `BEGIN IMMEDIATE`; **`.lock` với `flock` trên Windows**; WAL cho kho thật | `flock`/`fcntl` **không có trên Windows** (đội chạy Win11) — phải `LockFileEx`/`CreateFile FILE_SHARE_NONE`. Bảng tiến độ đội **đã có** (`_kho2_tien_do`) · 02/… | ASK01_2609 |
| LG-K4 | **SAI** | K4: template matching + tiêu chí 32 quân; **bắt gói mạng + gửi lại là «hợp pháp và bền nhất»** | «hợp pháp» không nguồn; Ask A2-03 dẫn chính tác giả BH: *giao thức đổi là tính năng chết* ⇒ «bền nhất» trái bằng chứng đội có | ASK01_2609 |
| LG-K7 | **SAI** | K7: `.lock` bằng `fcntl.flock`; **một `git.log` duy nhất, không file riêng per worker**; lock cũ > 30′ tự xoá | `fcntl` không có trên Windows; «một file chung» **trái luật §4.0** đội đo thật 04/08 (ghi đè `LANE_LOCK.md` mất ~1.100 dòng) — chính là lỗi đội đã trả giá | ASK01_2609 |
| LG-A2-02 | **SAI** | A2-02: bài Ling — công thức /9,/10 sai | ô «✗ (công thức /9,/10 sai)» | ASK02_A2_2309 |
| LG-A2-10 | **SAI** | A2-10: bài Ling — trích OpenAI sai | ô «◐ (trích OpenAI sai)» | ASK02_A2_2309 |
| LG-A2-21 | **SAI** | A2-21: bài Ling — toán sai | ô «✗ (toán sai)» | ASK02_A2_2309 |
| LG-A2-34 | **SAI** | A2-34: bài Ling — ngày sai | ô «◐ (ngày sai)» | ASK02_A2_2309 |

---

## Câu 152 (LG-A2-22)

**Mục LG-A2-22 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (BỊA):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «A2-22: bài Ling — UCI option bịa». Bằng chứng của đội: ô «✗ (UCI option bịa)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 153 (LG-A2-30)

**Mục LG-A2-30 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (BỊA):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «A2-30: bài Ling — TradingView API bịa». Bằng chứng của đội: ô «◐ (TradingView API bịa)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 154 (LG-0.2)

**Mục LG-0.2 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «0.2: không có giao dịch atomic giữa 2 tệp; kiểm `COUNT`/`MIN,MAX`/giao = 0; **giữ dòng đã xử lý trong datanoscore, đánh…». Bằng chứng của đội: (c) trái bất biến owner 26/09 `datanoscore ∩ data.db = ∅` (Ask ASK12-2 ghi rõ) — giữ dòng đã chấm trong hàng đợi là phá bất biến. (a) SQLite **có** giao dịch a…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 155 (LG-0.3)

**Mục LG-0.3 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «0.3: bảng mới + `CAST`, không 1 giao dịch 30 GB; lô 1 M; bảng tiến độ; khoá sai = `vkey <= 0 OR vkey >= 2^55`». Bằng chứng của đội: vkey là Zobrist **int64 toàn dải**: `gop_kho2_vao_kho.py:154,225` quét `I64MIN..I64MAX`, `loc_staging_lon_kho.py:707` sinh khoá âm/dương hợp lệ ⇒ bộ lọc của Li…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
