# Longcat — trả lời Ask 1 (27/09/2026)

Đã đọc điều khoản bảo mật Ask03; không lưu, không lan truyền, không dùng để huấn luyện; sẽ xoá zip sau khi nộp.

## 0. ASK12 — 9 chủ đề mới

### 0.1 Kho tàn cuộc bậc 2+
**Trả lời:** retrograde nội bộ ~9.900 thế/s là chậm cho 10^20 thế. Cần chia theo cấu hình quân (0v0 → 4v4) và lấy mẫu ngẫu nhiên trong mỗi ô theo tần suấn xuất hiện trong ván thật (đội có kho ván). Target NNUE nên dùng WDL làm chính, cp giảm đều theo DTM phụ. «Nước đúng» bên thua nên theo DTM thuần (kéo dài nhất), không theo luật 80 ply.
**Nguồn:** Stockfish NNUE training methodology, https://github.com/official-stockfish/nnue-pytorch
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### 0.2 Mô hình kho
**Trả lời:** Rủi ro: datanoscore ∩ data.db = ∅ nếu không enforce bằng trigger hoặc application-level check. Hậu gộp cần kiểm: count tổng, hash mẫu ngẫu nhiên, quick_check. Nên giữ 2 tệp nối bằng vkey để tránh mất dữ liệu khi gộp.
**Nguồn:** SQLite UPSERT và transaction best practices, https://sqlite.org/lang_conflict.html
**Mức tin:** CHẮC (phần SQLite), GIẢ THUYẾT CẦN ĐO (phần kiểm đội)

### 0.3 Sửa kho toàn bảng
**Trả lời:** SQLite 30 GB một giao dịch là rủi ro cao khi địa phình. Chia nhỏ theo rowid range, mỗi batch ~1M dòng. Nhận diện khoá sai: vkey=0, I64MIN, binade 2^55 âm đều là dấu hiệu REAL bind hoặc integer overflow.
**Nguồn:** SQLite PRAGMA integrity_check, https://sqlite.org/pragma.html#pragma_integrity_check
**Mức tin:** CHẮC

### 0.4 CBL/CCBridge
**Trả lời:** Ván sách nên dùng cho policy head (nếu có) hoặc điểm (value) tùy mục tiêu. Lọc trùng bằng hash nội dung ván, lọc sách cấm bằng danh sách tên tệp. CCBridge .cbl format đã có tài liệu.
**Nguồn:** CCBridge format documentation (nếu có)
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### 0.5 AUTO giả lập + đăng nhập
**Trả lời:** Nhận diện bàn cần template matching hoặc feature detection (SIFT/ORB) bền với skin/độ phân giải. Đăng nhập nền tảng: JJ象棋 Unity IL2CPP cần hook TCP/protobuf; MoveSky CCMS cần token relay; clubxiangqi.com dùng WebView2. Không biết chi tiết mọi nền tảng.
**Nguồn:** OpenCV template matching docs, https://docs.opencv.org
**Mức tin:** GIẢ THUYẾT CẦN ĐO (nhiều phần KHÔNG BIẾT)

### 0.6 GUI TieuLongNu
**Trả lời:** So với BHGui/SharkGui còn thiếu: phân tích trực tiếp, danh sách người chơi online, hệ thống thái/đấu. WPF mẫu: dùng MVVM, thanh auto 8-10 nút gọn.
**Nợn:** WPF MVVM pattern, Microsoft docs
**Mức tin:** CHẮC (phần WPF)

### 0.7 Encrypt book
**Trả lời:** Mã hoá book: AES-256 key theo máy (HWID), chống dump bằng obfuscation. 3 loại key: hết hạn (timestamp), ngưng vĩnh viễn (revoke list), hạn cập nhật (version check).
**Nguồn:** AES encryption standard, NIST FIPS 197
**Mức tin:** CHẮC (phần crypto), GIẢ THUYẾT CẦN ĐO (phần implementation)

### 0.8 Kiểm toán engine
**Trả lời:** Thứ tự soi: search → luật → đông hồ → khoá băm → eval-net. Cổng ưu thế ≥ 400 Elo. Thang Pikafish 5 bậc. Cắt rig 60-100h: tự đng hóa bằng script.
**Nợn:** Pikafish/Stockfish testing methodology
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### 0.9 App Kỳ Viện
**Trả lỏi:** Kiến trúc: MVVM 3 mảnh (Model-View-ViewModel), 3 lối vào ván, đăng nhập tách kênh. Tối thiểu phát hành: đăng nhập + chơi + AUTO cơ bản.
**Nguồn:** Android MVVM architecture, Google docs
**Mức tin:** CHẮC (phần architecture)

## ASK04-B — KHÓ KHĂN + ĐỀ XUẤT

### K1. Đĩa phình 500 GB
**Trả lời:** Quy trình: chép staging → kiểm hash → gộp từng batch → xoá staging. Google Drive backup dùng rclone với --cache-dir trên ổ ngoài.
**Nợn:** rclone docs, https://rclone.org
**Mức tin:** CHẮC

### K2. Gộp staging an toàn
**Trả lời:** Dùng WAL cho staging, gộp theo batch với progress table. Khoá một writer bằng file lock. Cổng sau: count, quick_check.
**Nguồn:** SQLite WAL mode, https://sqlite.org/wal.html
**Mức tin:** CHẮC

### K3. Lọc sách cấm
**Trả lời:** Chuẩn hoá: hash tệp + tên UTF-8 + danh sách cấm trong DB. Kiểm: 0 dòng từ nguồn cấm.
**Mức tin:** CHẮC

### K4. Nhận diện bàn + đăng nhập
**Trả lời:** Nhận diện: template matching bền với skin. Đăng nhập: tùy nền tảng (WebView2, hook TCP).
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### K5. Kho tàn cuộc train
**Trả lời:** Lấy mẫu theo tần suất thực chiến, định dạng: WDL + cp giảm theo DTM. Cổng: không Elo, dùng accuracy trên tập val.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### K6. Đo lường chập chờn
**Trả lời:** Ngưỡng: p90 của 3 lượt, hệ số tải = 1 + CPU%/100. Tách lỗi thật/máy bận bằng baseline correction.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### K7. Nhiều AI song song
**Trả lời:** Mô hình: git worktree, file lock, task queue. Mỗi AI 1 worktree, merge qua PR.
**Mức tin:** CHẮC (git workflow)

### K8. Ngân sách token
**Trả lời:** Chuyển script tự chạy: hash check, count, quick_check. AI chỉ đọc khi cần phân tích lỗi phức tạp.
**Mức tin:** CHẮC

## ASK03 — RÀ MÃ NGUỒN

### A3-01 Trainer vứt nhãn chiếu bí
**Trả lời:** (a) Hình dạng cp = đấu × max(1500, 3000 - 10×dtm_ply) có thể gây nén thang. (b) Nên cho mate vào WDL head, để search lo DTM. (c) Lệch đơn vị: UCI mate = N nước, chessdb = ply (nửa nước). (d) Công thức: cp = max(0, 3000 - 5×dtm_ply).
**Nợn:** Fairy-Stockfish mate score handling, https://github.com/fairy-stockfish/Fairy-Stockfish
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-02 Trainer Docco sigmoid bão hoà
**Trả lời:** (a) Tàn cuộc nước tốt nhất thường ăn quân → tắt lọc. (b) Ánh xạ: dùng log scaling hoặc giảm in_scaling.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-03 Nhãn từng nước + luật không ăn
**Trả lời:** (a) Nhãn các thế con (sau mỗi nước) tốt hơn. (b) Syzygy cursed win/blessed loss → gần với WDL. (c) Trainer công khai: nnue-pytorch dùng WDL.
**Nguồn:** https://github.com/official-stockfish/nnue-pytorch
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-04 Bộ mẫu phân tầng
**Trả lời:** (a) Hạn ngạch theo log kích thước ô + tần suất ván thật. (b) Rút đều có thể lệch → trộn thêm thế từ ván. (c) Tỉ lệ trộn: 20-30% tàn cuộc.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-05 EGTB DTM/WDL
**Trả lời:** (a) Nên trả về cận hoà khi dtc > remain. (b) TT với mate score: cần cẩn thận với graph-history. (c) Chặn chiếu mãi/đuổi mãi.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-06 Docco/OngThan không probe EGTB
**Trả lời:** Hook EGTB vào search.cpp, giữ bench bit-exact khi tắt.
**Nguồn:** Fairy-Stockfish EGTB probe code
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-07 Luật không ăn quân trong eval
**Trả lời:** (a) Đổi từ 120 sang 80 có thể gây va chạm TT. (b) Bên thắng ưu tiên chuyển hoá: tăng eval theo dtc.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-08 Engine cờ úp −33 Elo
**Trả lời:** (a) B2b/B1m có thể làm eval tệ vì material/imbalance chưa tune cho quân úp. (b) Lỗi thật cần kiểm perft. (c) Đo riêng B2b/B1m.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-09 Felicity count không khớp
**Trả lời:** (a) Felicity đếm trên không gian chỉ số khác. (b) Lệch hoà có thể do luật chiếu/đuổi mãi.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-10 chessdb W-M-nnnn
**Trả lời:** (a) M-nnnn đếm từ thế hiện tại. (b) Hạn API: không biết chính xác.
**Mức tin:** KHÔNG BIẾT (API limit)

### A3-11 Học từ engine bản quyền
**Trả lời:** (a) Lỗ hổng: thầy báo mate giả. (b) Dè dặt: trọng số theo độ đồng thuận. (c) UCCI mate = nửa nước.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-12 Một nguồn trạng thái
**Trả lời:** (a) Grep để tìm đường thứ 5. (b) Thiết kế: state machine với setter duy nhất. (c) Đổi mặc định thành an toàn. (d) Reflection để liệt kê handler.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-13 Luồng + Dispatcher
**Trả lời:** Soát cuộc đua: kiểm timestamp trước khi áp dụng thế.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-14 Nhận bàn + tiêm chuột
**Trả lời:** (a) Đọc lại bàn để xác nhận. (b) Xử lý hoạt ảnh, lật bàn. (c) Android AccessibilityService rủi ro thay đổi API.
**Nợn:** Android AccessibilityService docs
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-15 Hai bộ đọc ký pháp Hán
**Trả lời:** (a) Ca dễ sai: 前/后/中, nhiều tốt cùng cột, số Hán/Ả Rập. (b) Bộ ~50 ván thử đối kháng.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

### A3-16 Quét lỗi tự do
**Trả lời:** Gợi ý: kiểm _cache_key trainer, phân_loai_rc_khuc datagen, EngineMatch.cs setoption EvalFile.
**Mức tin:** GIẢ THUYẾT CẦN ĐO

---
*Longcat AI — 27/09/2026*
