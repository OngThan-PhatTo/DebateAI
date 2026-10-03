# ASK 01 — bộ hỏi cho OxAlpha — tệp 25/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 121 (O-05)

### O-05 — Thang điểm nhãn: Fairy/Docco ↔ OngThan (Q5)
Dữ liệu 72 B/bản ghi hiện có điểm theo thang Fairy/Docco; OngThan dùng `to_cp` riêng (`uci.cpp:954`). Khi train net OngThan từ nhãn thầy Docco: quy đổi thang bằng gì (hệ số tuyến tính? khớp WDL?) và kiểm thế nào? Nguồn.

---

## Câu 122 (O-06)

### O-06 — Nối tàn cuộc vào đường train (Q6; luật owner 02/10 10:1x «engine mới phải có cách train + nối với endgame»)
Với trainer này, dữ liệu tàn cuộc (EGTB/positionendgame 11,95 M thế đã có) nên đưa vào như nguồn thường, hay cần nhãn WDL cứng/trọng số riêng? Có nguồn nào đo lợi ích «EGTB-labeled endgame data» cho NNUE không?

---

## Câu 123 (P-01)

### P-01 — Lõi nào cho engine NNUE train từ 0?
Giữa Fairy-Stockfish (GPL, nhiều biến thể, net CC0 có sẵn) và lõi riêng, cái nào dễ **đo được tiến bộ sớm** (net yếu vẫn chơi hợp lệ, không sập search)? Có ví dụ engine cờ tướng nào train từ random thành công công khai (px0, Pikafish lịch sử) với số ván/thế và mốc Elo theo thời gian? Dẫn nguồn.

---

## Câu 124 (P-02)

### P-02 — Vì sao net ngẫu nhiên làm Bodetosu chậm 27× nps?
Giả thuyết: eval nhiễu lớn ⇒ cắt tỉa (futility/razoring/LMR) hầu như không kích hoạt ⇒ cây nở; hoặc TT/aspiration fail nhiều. Bạn nghiêng giả thuyết nào, cách đo tách (đếm số nút cắt tỉa theo loại, |eval| phân phối)? Có cần kẹp/chuẩn hoá đầu ra net lúc khởi đầu không?

---

## Câu 125 (P-03)

### P-03 — Nối tàn cuộc đúng cách
Lúc đánh: probe EGTB/kho `positionendgame` ở nút gốc hay cả trong search (giới hạn depth)? Lúc train: dùng nhãn WDL từ EGTB cho thế ≤ N quân trộn với nhãn thầy — tỉ lệ bao nhiêu, nguồn nào đã đo? Ca đỏ nên là gì (gỡ EGTB ⇒ phải phát hiện được).
