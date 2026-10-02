# ASK 01 — bộ hỏi cho OxAlpha — tệp 8/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 36 (GT-B3-01)

**Ca thử GT-B3-01 (Phần D — đội tự chạy; góp ý cách đo/ngưỡng nếu thấy sai):**

| Mã | Giả thuyết | Đo | Ngưỡng đặt trước | Trạng thái |
|---|---|---|---|---|
| GT-B3-01 | Trung bình trọng số 2 shard ≈ tuyến tính (OxAlpha) | MLP đồ chơi, CPU, A=A + dương | TB khác init > 1,5× shard ⇒ SAI | **ĐÃ CHẠY 02/10:** khác init 1,37× (tệ hơn 1 shard), cùng init ≈ full ⇒ KHÔNG CHẮC; lặp trên trainer Bodetosu thật |

---

## Câu 37 (GT-B3-02)

**Ca thử GT-B3-02 (Phần D — đội tự chạy; góp ý cách đo/ngưỡng nếu thấy sai):**

| Mã | Giả thuyết | Đo | Ngưỡng đặt trước | Trạng thái |
|---|---|---|---|---|
| GT-B3-02 | DDP với loader ta đọc trùng stream | băm batch đầu mỗi rank, Linux 2 GPU | ≥ 1 cặp trùng / 10 batch ⇒ xác nhận | chờ máy thuê |

---

## Câu 38 (GT-B3-03)

**Ca thử GT-B3-03 (Phần D — đội tự chạy; góp ý cách đo/ngưỡng nếu thấy sai):**

| Mã | Giả thuyết | Đo | Ngưỡng đặt trước | Trạng thái |
|---|---|---|---|---|
| GT-B3-03 | Thang 410/361 của OngThan bão hoà với điểm cờ tướng | đếm tỉ lệ \|score\| > 1600 cp trong 1M mẫu kho | > 10 % ⇒ mở A/B 410/361 vs 1000/880 | chờ lane trainer |

---

## Câu 39 (BPPC-03)

**Mục BPPC-03 — BigPickle (SAI):** AI BigPickle nói: «Binpack tốn 8,35×10³ byte/vị trí; đồng thời 319 MB/s ở 20,9 triệu vị trí/s ⇒ 10¹⁰ thế = 84 TB, 256 GB RAM chỉ chứa 3×10⁷ thế». Bằng chứng của đội: tự mâu thuẫn số học: 319e6/20,9e6 = 15,3 B/vị trí; 8.350 B × 20,9e6/s = 174 GB/s ≠ 319 MB/s ⇒ bảng TB và kết luận RAM sai ~500×. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 40 (GLMPC-10)

**Mục GLMPC-10 — GLM 5.2 (SAI):** AI GLM 5.2 nói: «192 lõi vật lý sinh «vài trăm triệu tới ~2 tỷ thế/ngày»». Bằng chứng của đội: cận trên 2 tỷ/ngày = 23.148 thế/s ≈ 42× số đo 552,5/s máy 32 luồng (_SOI/B1_ASK05_FABLE_2026-10-01.md:60); tuyến tính 6× chỉ ~3.300/s ≈ 285 triệu/ngày (cận dưới thì khớp). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
