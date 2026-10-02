# ASK 01 — bộ hỏi cho OxAlpha — tệp 22/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 106 (L-01)

### L-01 — Tụt fen/s là do lịch depth hay do rò?
Số đo trên cho thấy tốc độ giảm ~100× khi depth 20 → 40 với cùng movetime 60 s, tức là chủ yếu do **cấu hình depth**, không phải rò bộ nhớ. Bạn có đồng ý không? Nếu bạn nghĩ còn nguyên nhân khác (TT đầy, trùng lặp thế tăng theo thời gian, đồng bộ ghi tệp `AutoFlush` mỗi dòng `Loi.cs:236`), hãy nói **cách đo tách bạch** từng nguyên nhân (biến đổi gì, kỳ vọng số gì). *Trả lời tốt:* bảng nguyên nhân → phép đo → ngưỡng.

---

## Câu 107 (L-02)

### L-02 — Sinh thế có nhãn ở quy mô lớn: giới hạn theo NODES hay theo DEPTH, bao nhiêu là chuẩn ngành?
Pipeline Stockfish/Pikafish/Fairy dùng `nodes` cố định cho mỗi thế khi sinh nhãn (ví dụ 5k–20k nodes) chứ không dùng depth lớn. Với 44 lõi vật lý, engine NNUE ~1–2 Mnps/luồng, bạn ước **thế có nhãn/giây** bao nhiêu ở 5k / 10k / 20k nodes? Dẫn nguồn thật (README/sổ tay của pipeline đã công bố, kèm số).

---

## Câu 108 (L-03)

### L-03 — «Đặt depth 60, lấy PV ở MỌI depth» — nhãn có hợp lệ không?
Owner muốn một lần search sâu (depth N) rồi lấy tất cả thế trên PV làm FEN. Nhãn cho thế ở ply thứ i của PV chỉ được search sâu N−i (đội đã ghi KHÔNG CHẮC, `_LANE_NOTE/lock/w_sinhgame_t34_0210_2026-10-02.md:14`) và các thế đó **tương quan mạnh** (cùng một dòng chơi). Hỏi: (a) có pipeline công khai nào dùng PV-harvest làm dữ liệu train không, kết quả thế nào; (b) nếu dùng thì nên ghi `pv_depth`/`pv_ply` để lọc ở ngưỡng nào; (c) có nên **re-label** các thế PV bằng search ngắn độc lập (nodes cố định) thay vì tin điểm của search gốc? *Trả lời tốt:* nguồn + đề xuất cụ thể, không «tuỳ».

---

## Câu 109 (L-04)

### L-04 — Dùng RAM lớn để sinh nhanh hơn: có đáng không?
Máy đích 128 GB nhưng engine dùng Hash 16–256 MB. Với datagen nhiều thế **độc lập** (mỗi thế search ngắn), Hash lớn có tăng thế/giây không, hay chỉ có ích khi đánh ván dài? Bạn có số đo/nguồn nào về Hash ↔ nps/thế-giây cho datagen không? Đề xuất công thức chia RAM cho N engine song song (đội đang dùng trần 80 % RAM, tính lúc chạy).

---

## Câu 110 (L-05)

### L-05 — Đa dạng khai cuộc không dùng book (luật owner 20/09: train không chạy book)
Đội dùng nước ngẫu nhiên hợp lệ 4–8 ply. Bạn có đề xuất khác (bộ thế xuất phát cân bằng, chọn theo |eval| < ngưỡng, random ply có trọng số theo tần suất thực tế)? Nguồn.
