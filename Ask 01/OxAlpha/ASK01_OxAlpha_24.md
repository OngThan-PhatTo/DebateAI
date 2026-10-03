# ASK 01 — bộ hỏi cho OxAlpha — tệp 24/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 116 (K8)

### K8 — Khi bạn trả lời dài và bị cắt
Owner thấy nhiều AI cắt bài rồi hiện «continue», hoặc đề xuất «phân tích sâu A/B/C/D». Với bạn: dấu hiệu bài bị cắt là gì (để đội tự bấm tiếp), giới hạn độ dài thật mỗi lượt, cách bạn muốn đội yêu cầu «viết tiếp từ chỗ dừng» để không lặp.

---

## Câu 117 (O-01)

### O-01 — Thang float khi dựng lại trainer (Q1)
Thang FT 255 / fc ×128-64-128 / 600 / PSQT suy từ nguồn engine — bạn biết nguồn công khai nào mô tả lượng tử của kiến trúc Pikafish 2026 (hoặc Stockfish SFNNv8/v9 tương đương: L1 2560? L2 32? `ac_sqr`?) để đối chiếu? Dẫn URL + commit.

---

## Câu 118 (O-02)

### O-02 — Factorization HalfKAv2_hm (virtual features) (Q2)
Trainer upstream có dùng factorizer không? Bỏ factorizer ảnh hưởng tốc độ hội tụ bao nhiêu (số liệu nếu có)? Định dạng net có đổi không (đội tin là không).

---

## Câu 119 (O-03)

### O-03 — Chống tràn accumulator int16 (Q3)
Nên **kẹp trọng số FT** (±bao nhiêu) hay dùng phạt trong loss? Net slot đo được: bias −111..109, psqt −23047..24319, threatPsqt −4762..4299. Nguồn (SF/Pikafish trainer có `clip`/`weight clipping` ở đâu).

---

## Câu 120 (O-04)

### O-04 — DLL dùng `Position` của engine SHIP hay port Python? (Q4)
Để sinh đặc trưng đúng tuyệt đối, đội tính link DLL từ nguồn engine (kéo movegen/magics/tt). Bạn khuyên đường nào, vì sao; có dự án nào đã làm «feature DLL từ engine» cho trainer Python chưa?
