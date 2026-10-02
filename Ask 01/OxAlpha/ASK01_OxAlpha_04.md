# ASK 01 — bộ hỏi cho OxAlpha — tệp 4/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 16 (A1-16)

### A1-16 — EGTB làm nhãn tàn cuộc thuần CPU (SpaceBunny)
**SpaceBunny:** *«EGTB là nhãn miễn phí, thuần CPU … Felicity: "Generate all 5-men endgames for chess … Total time: 21 hours" trên Ryzen 7 1700 … Bạn đã có `positionendgame.fendb` bậc 1 = 11,9 triệu thế DTM+DTC»*.
**Bằng chứng đội:** probe FelicityEgtb thật đã cắm vào Nữ Oa/Bodetosu (`egtb.cpp:13-16`, `search.cpp:1394-1397`, build `-DUSE_EGTB`); Docco `--dtm-cp` ánh xạ mate trước sigmoid; chưa đo +Elo của nhãn EGTB trong train.
**Hỏi:** Cách trộn nhãn EGTB (DTM/DTC chính xác) với nhãn search (cp nhiễu) vào cùng một hàm mất sigmoid-MSE mà không làm net «đổ» về tàn cuộc: tỷ lệ mẫu, ánh xạ DTM→cp (tuyến tính hay mũ), và phép đo cho thấy net học đúng (Spearman ρ(giá trị net, −DTC) trên tập giữ nguyên?). Dẫn cách Stockfish/Pikafish xử lý nếu bạn biết.

---

## Câu 17 (T1)

**Ca thử T1 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T1 | Trainer Bodetosu SHIP: ROCm iGPU nhanh hơn CPU vài → 10× (Grok); 2×10⁴–1,5×10⁵ mẫu/s CPU | `--device cpu --luong-train {8,16,24}` vs `--device cuda`, corpus sanity 2 M mẫu, 1 epoch, in mẫu/s | A=A: 2 lượt cùng seed cùng device ⇒ loss cuối lệch < 1 %; dương: `--luong-train 8` chậm hơn 24 | ROCm ≥ 3× CPU-24 ⇒ Grok đúng; < 1,5× ⇒ CPU đủ | 30 phút máy owner |

---

## Câu 18 (T2)

**Ca thử T2 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T2 | Net HalfKAv2 (OngThan/Docco) train CPU «không thực tế» | sau OK-01/02: Docco trainer `--threads 24` vs ROCm, 1 M thế, mẫu/s | A=A | CPU < 1/10 ROCm ⇒ Grok đúng | 1 h |

---

## Câu 19 (T3)

**Ca thử T3 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T3 | 3 khác biệt loss vs upstream (SpaceBunny) có +Elo? | Docco: loss hiện tại vs 2 vế/offset 270/pow 2.5 trên cùng kho, cùng seed | A=A + dương (×4 nodes) theo luật 21/09 §4 | KTC95 ≥ +20 không chứa 0 | 1 đêm |

---

## Câu 20 (T4)

**Ca thử T4 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T4 | N job × ít luồng vs 1 job nhiều luồng | 4×`--luong-train 6` vs 1×24, tổng mẫu/s + A/B net | A=A | tổng mẫu/s ≥ 1,3× ⇒ SpaceBunny/BigPickle đúng; net không khác ⇒ Grok đúng | 2 h |
