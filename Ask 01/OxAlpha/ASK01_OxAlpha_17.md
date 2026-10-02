# ASK 01 — bộ hỏi cho OxAlpha — tệp 17/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 81 (NEMO-11)

**Mục NEMO-11 — Nemotron (BIA):** AI Nemotron nói: «Paper 'Training Neural Networks with Limited Memory — Chen et al., 2018'». Bằng chứng của đội: không tìm thấy; bài gốc Chen et al. 2016 'Training Deep Nets with Sublinear Memory Cost' arXiv 1604.06174. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 82 (NEMO-02)

**Mục NEMO-02 — Nemotron (SAI):** AI Nemotron nói: «OngThan features=40960; loss 'lambda·MSE(cp)+(1−lambda)·BCE(wdl)' tại model.py:134-149». Bằng chứng của đội: variant.py:43 NUM_INPUTS=10530; model.py:313-323 không có BCE; :134 là __init__. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 83 (NEMO-03)

**Mục NEMO-03 — Nemotron (SAI):** AI Nemotron nói: «Bodetosu pick_device :461-561, QAT :284-294, loss :435-458, main :564-783, ARM64 :507-508». Bằng chứng của đội: dòng thuộc bản cũ 783 dòng Engine_Xiangqi/OngThan_variants/Bodetosu/tools (có KHONG_PHAI_CAY_CHINH.md); SHIP: :3929, :2913, :3296, :3976. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 84 (NEMO-04)

**Mục NEMO-04 — Nemotron (SAI):** AI Nemotron nói: «Jieqi trainer 'kế thừa Bodetosu: CPU/DirectML/ROCm/XPU đều sẵn sàng'». Bằng chứng của đội: train_nnue_jieqi.py:368 choices auto/cpu/cuda; :391-392; chỉ ở cây DEV (KHONG_PHAI_CAY_CHINH.md:11-12). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 85 (NEMO-05)

**Mục NEMO-05 — Nemotron (SAI):** AI Nemotron nói: «OngThan 'CPU-only ✅ Chạy được (Lightning CPU)'». Bằng chứng của đội: ĐO train.py --help → AttributeError :28; :57/:67 nnue.cuda(); nnue_dataset.py:35-44 pin_memory. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
