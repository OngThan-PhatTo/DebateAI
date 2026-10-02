# ASK 01 — bộ hỏi cho OxAlpha — tệp 23/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 111 (M-01)

### M-01 — Cách kết nối của bạn (mô tả kỹ thuật, không mơ hồ)
Hãy mô tả **một hoặc nhiều** cách bạn sẽ làm để GUI của đội đánh với phần mềm/ứng dụng cờ khác: nhận diện bàn cờ + nước đối thủ (chụp màn hình + so mẫu quân · OCR · UI Automation/Accessibility · ADB cho Android · đọc bộ nhớ/IPC nếu phần mềm cho phép), gửi nước (chuột/chạm, tốc độ, kiểm tra nước đã vào), **độ trễ** kỳ vọng và **cách phát hiện lỗi** (lệch ô, skin bàn cờ khác, bàn cờ xoay). *Trả lời tốt:* từng bước + thư viện/API + giới hạn thật.

---

## Câu 112 (M-02)

### M-02 — Chuẩn hoá ô cờ từ 2 điểm (2 vị trí con Xe) có đủ không?
Đội định cho người dùng chỉ 2 góc (Xe trái–Xe phải) để suy ra lưới 9×10. Có nên dùng 3–4 điểm để chống xoay/méo (perspective) không? Khi nào 2 điểm sai? Cách tự kiểm (vd đếm được đủ 32 quân ở thế ban đầu).

---

## Câu 113 (M-03)

### M-03 — Minh hoạ cách kết nối cho người dùng
Owner muốn mỗi kiểu kết nối có mô tả + animation/ảnh. Bạn gợi ý khuôn trình bày nào gọn (1 câu + 1 GIF ≤ 5 s + điều kiện cần)? Có ví dụ trong GUI cờ nào đã làm tốt?

---

## Câu 114 (K6)

### K6 — Model local free nào nên có trong danh mục Download AI?
Liệt kê 8–15 model (repo Hugging Face **chính xác**, bản GGUF, kích thước Q4/Q5/Q8, giấy phép có cho dùng nội bộ/thương mại không) phù hợp để **debate kỹ thuật + đọc code C#/C++/Python + tiếng Việt** trên máy 47,6 GB RAM không NVIDIA. Ghi rõ model nào là của chính bạn/nhà bạn (nếu có) và vì sao chọn. *Trả lời tốt:* bảng có cột link HF, license, GB, điểm mạnh/yếu — đội sẽ kiểm từng link bằng API HF (bịa link ⇒ bị ghi sổ).

---

## Câu 115 (K7)

### K7 — Điền hồ sơ giao thức cho chính bạn
Điền JSON theo các trường ở bối cảnh cho **chính bạn**: nối kiểu gì (API/CLI/Web/hộp thư), gửi lệnh cách nào, nhận bài cách nào, giới hạn ký tự mỗi lượt và lượt mỗi ngày, bạn có hiện nút «continue»/«downgrade»/kiểm tra người thật không (đội sẽ **dừng êm và báo owner bấm** khi gặp kiểm tra người thật — không né), bạn có tự lưu tệp vào `Discussion/Answer/<Tên bạn>/` được không. Không chắc trường nào thì ghi `"khong_chac"`.
