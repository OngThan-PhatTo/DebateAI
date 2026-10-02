# ASK 01 — bộ hỏi cho OxAlpha — tệp 5/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 21 (T5)

**Ca thử T5 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T5 | Windows > 64 luồng: torch chỉ dùng 1 processor group | trên PC 192 lõi: `torch.get_num_threads()`, CPU % theo PID 2 group (luật 23/09) | — | < 50 % tổng ⇒ cần CPU Sets | 10 phút khi có máy |

---

## Câu 22 (T6)

**Ca thử T6 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T6 | Datagen scale tuyến tính 32 → 192 lõi | `chay_datagen_nobook.py` 20 phút trên PC 192 | so 549,4/s máy 32 luồng | ≥ 4× ⇒ 168 ngày của SpaceBunny chia ≥ 4 | 20 phút |

---

## Câu 23 (T7)

**Ca thử T7 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T7 | BF16 autocast CPU nhanh ≥ 1,3× (Zen 5 AVX512-BF16) | trainer Bodetosu CPU, autocast on/off, cùng seed | A=A loss lệch < 2 % | ≥ 1,3× mới bật mặc định | 20 phút |

---

## Câu 24 (T8)

**Ca thử T8 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T8 | `data.db` ~181 M thế chưa khai thác | `SELECT COUNT(*)` read-only + kiểm `vkeyfen.idx.db` | — | số thật ± 10 % | 5 phút |

---

## Câu 25 (T9)

**Ca thử T9 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T9 | `pytorch-lightning==1.9.5` import được với torch 2.12 ROCm (venv nháp LaneScratch, không đụng `.venv`) | `pip install` vào bản chép venv + `train.py --help` | — | rc 0 ⇒ OK-01 có thể chỉ cần venv riêng | 15 phút |
