# Qwen — trả lời Ask 1 (27/09/2026)

> Nguồn: owner gửi tệp `Qwen_python_20260927_odr7gk1cb.py` (Qwen xuất trả lời dưới dạng script Python ghi tệp; đội trích phần nội dung, không chạy script). Lưu nguyên văn phần trả lời.

Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.

# Trả lời của Qwen cho ASK04-B và ASK01

## ASK04-B — KHÓ KHĂN + ĐỀ XUẤT

### K1. Đĩa phình 500 GB trong 3 ngày khi nhập dữ liệu
- **Đề xuất quy trình**: Sử dụng lệnh SQLite `ATTACH DATABASE` để chuyển dữ liệu trực tiếp từ file staging sang kho chính, loại bỏ hoàn toàn bước chép file trung gian thứ 3 trên ổ C.
- **Kiểm tra không cần bản chép**: Tính hash (SHA-256) và đếm số dòng của file nguồn ngay khi đọc, sau đó so sánh với kết quả hash và số dòng sau khi `INSERT` vào staging.
- **Backup Google Drive**: Sử dụng công cụ như `rclone` với chế độ stream/upload trực tiếp từ file nguồn (không đồng bộ toàn bộ thư mục xuống ổ C), hoặc chia nhỏ file backup thành các phần 10-20 GB trước khi upload.

### K2. Gộp staging vào kho SQLite 30 GB an toàn
- **Khuôn giao dịch**: Sử dụng `BEGIN IMMEDIATE` để khóa ghi ngay từ đầu, tránh conflict. Chia nhỏ giao dịch thành các lô (ví dụ: 50.000–100.000 dòng/lô) để nếu bị cắt, chỉ cần rollback một lô nhỏ thay vì toàn bộ 5 GB WAL.
- **Bảng tiến độ**: Tạo một bảng `merge_progress` lưu `last_processed_id` hoặc `last_source_hash` để có thể resume (tiếp tục) khi bị ngắt.
- **Khóa liên tiến trình**: Sử dụng file lock (tạo file `.lock` với `FileShare.None` trong C#) hoặc đặt `PRAGMA locking_mode = EXCLUSIVE`.
- **WAL cho kho thật**: **Có**, nên chuyển sang `journal_mode = WAL`. WAL cải thiện đáng kể hiệu suất đọc đồng thời (cho phép GUI đọc trong khi background đang gộp dữ liệu). Cần kiểm soát checkpoint thủ công (`PRAGMA wal_checkpoint(TRUNCATE)`) để tránh file WAL bị phình to.

### K3. Lọc «sách cấm gộp» và nguồn tệp bị mojibake
- **Chuẩn hóa nguồn**: Khi nhập tệp, ngay lập tức tính hash (SHA-256) của tệp gốc và lưu vào bảng `source_files`, kèm theo tên tệp đã được ép buộc chuyển đổi sang UTF-8 (sử dụng thư viện như `chardet` để tự động phát hiện encoding gốc như GBK/Shift-JIS).
- **Lọc**: So khớp `file_hash` với danh sách hash của các tệp bị cấm, thay vì so khớp tên tệp (vốn dễ bị lỗi mojibake).
- **Ca kiểm**: Truy vấn `SELECT COUNT(*) FROM data WHERE source_hash IN (SELECT hash FROM banned_files)` phải luôn trả về 0.

### K4. Nhận diện bàn cờ trên web/app để auto («1 chạm»)
- **Kiến trúc nhận diện**: Sử dụng mô hình Object Detection nhẹ (ví dụ: YOLOv8n hoặc YOLOv11n) được fine-tune trên bộ dữ liệu ảnh chụp màn hình đa dạng (nhiều skin, độ phân giải, LDPlayer, BlueStacks). Tránh dùng template matching thuần túy vì dễ vỡ khi thay đổi giao diện.
- **Tiêu chí «đã bắt»**: Số lượng quân phát hiện được nằm trong khoảng hợp lý (28-32), có sự đối xứng cơ bản (tướng ở giữa, sĩ/tượng hai bên), và không có quân nào chồng lấn lên nhau quá mức.
- **Giao tiếp với client/APK**: Cách bền vững và hợp pháp nhất là nhận diện hình ảnh (Computer Vision) và mô phỏng đầu vào qua API của giả lập (ví dụ: `adb shell input tap` hoặc LDPlayer API). Việc bắt gói mạng (packet sniffing) hoặc can thiệp bộ nhớ có nguy cơ vi phạm điều khoản dịch vụ và bị khóa tài khoản.

### K5. Kho tàn cuộc để train từ gốc lên
- **Lấy mẫu**: Ưu tiên lấy mẫu dựa trên tần suất xuất hiện trong ván đấu thực tế (real-game frequency). Các thế tàn cuộc lý thuyết (thắng/hòa đặc biệt) nên được oversample để mạng không bỏ sót.
- **Định dạng nhãn + loss**: Sử dụng nhãn kết hợp WDL (Win/Draw/Loss) và DTM (Distance To Mate). Loss function nên là kết hợp giữa Cross-Entropy cho WDL và Huber Loss (Smooth L1) cho DTM, với trọng số điều chỉnh để mạng ưu tiên phân biệt thắng/thua/hòa trước, sau đó mới tối ưu số nước.
- **Cổng đo «đã học» rẻ**: Đo độ tương quan Spearman giữa giá trị DTM dự đoán của mạng và giá trị DTM thực tế trên một tập validation nhỏ (ví dụ: 10.000 thế đã giải chính xác). Nếu tương quan > 0.8, coi như đã học.

### K6. Đo lường & kiểm thử chập chờn trên máy đang tải
- **Quy ước ngưỡng**: Sử dụng phân vị thứ 90 (p90) của 3 lần chạy liên tiếp, nhân với hệ số tải (ví dụ: nếu CPU > 50%, ngưỡng thời gian cho phép tăng thêm 20%).
- **Tách lỗi thật khỏi máy bận**: Trong CI, chạy một tác vụ chuẩn (benchmark) song song để đo "baseline performance" của máy tại thời điểm đó. Nếu baseline cũng chậm, đánh dấu kiểm thử là "skipped due to high load" thay vì "failed".

### K7. Nhiều AI làm song song trên một kho git chung
- **Mô hình khóa/nhật ký**: Sử dụng mô hình "Task Queue" dựa trên file. Một file `tasks.json` chứa danh sách việc. Agent đọc, tìm task "pending", đổi thành "processing_<agent_id>" kèm theo file lock.
- **Nhật ký**: Mỗi agent ghi log vào file riêng `agent_<id>.log` để tránh xung đột ghi.
- **Nối lại**: Khi khởi động, agent quét các task có trạng thái "processing_<agent_id>" và tiếp tục xử lý từ đó.

### K8. Ngân sách token
- **Chuyển thành script**: Các tác vụ kiểm tra định dạng, đếm số dòng, tính hash, kiểm tra sự tồn tại của file, so khớp chuỗi đơn giản nên được viết bằng script (Python/Bash). AI chỉ đọc báo cáo tóm tắt (ví dụ: "Tìm thấy 5 file lỗi định dạng").
- **Bắt buộc AI đọc**: Phân tích logic, tìm nguyên nhân gốc rễ của lỗi phức tạp, đề xuất kiến trúc, viết code sửa lỗi.
- **Cách viết đề bài**: Cung cấp ngữ cảnh đầy đủ trong một prompt duy nhất, yêu cầu AI trả lời theo mẫu cố định (JSON hoặc Markdown có cấu trúc) để giảm thiểu token thừa và tránh phải hỏi lại.

---

## ASK01 — BỘ CÂU HỎI GOM (Tóm tắt trả lời)

1. **Kho thế tàn cuộc**: *Giả thuyết, cần đo*. Việc chia hạn ngạch nên dựa trên tần suất thực chiến. Target NNUE nên là WDL kết hợp DTM giảm dần để giữ độ dốc học tập.
2. **Mô hình kho**: Rủi ro là dữ liệu không nhất quán nếu `datanoscore.db` và `data.db` không được đồng bộ hóa bằng giao dịch phân tán hoặc logic bù trừ (compensating transaction). Nên giữ 2 tệp nhưng có bảng `sync_log` để theo dõi trạng thái.
3. **Sửa kho toàn bảng**: Cách an toàn là tạo bảng mới, thực hiện `INSERT INTO new_table SELECT ... FROM old_table WHERE ...` (lọc điều kiện), sau đó đổi tên bảng. Không nên `UPDATE` hoặc `DELETE` hàng loạt trên bảng 30 GB vì sẽ làm phình WAL. Nhận diện khóa sai bằng query: `WHERE typeof(vkey) != 'integer' OR vkey = 0 OR vkey < -9223372036854775808`.
4. **CBL/CCBridge**: Khai thác ván sách tốt nhất bằng cách trích xuất các vị trí trung cuộc từ ván đấu của kỳ thủ cao cấp, gán nhãn nước đi (policy) dựa trên nước đi thực tế, kết hợp điểm đánh giá của engine. Lọc trùng bằng hash của FEN (chuẩn hóa).
5. **AUTO trên giả lập + đăng nhập**: 
   - (a) Dùng AI object detection (YOLO) thay vì template matching để bền vững với skin/độ phân giải. 
   - (b) *Không biết* chi tiết giao thức nội bộ cụ thể của JJ象棋 hoặc MoveSky do chúng thường xuyên thay đổi và có biện pháp chống bot. Việc nhúng WebView2 khả thi cho nền tảng web, nhưng với APK native thì cần giả lập UI qua ADB. 
   - (c) *Không biết* chi tiết về "H1–H4 MoveSky" và các tài khoản cụ thể được nhắc đến.
6. **Giao diện TieuLongNu**: So với BHGui/SharkGui, có thể thiếu: tính năng phân tích đa biến thể (multi-variation analysis), xuất bản PGN/CBF nâng cao. Mã mẫu WPF: Sử dụng `ItemsControl` với `DataTemplate` cho danh sách nước đi, và `Grid` chia tỷ lệ cho thanh auto gọn gàng.
7. **Encrypt book (V-41)**: Thiết kế mã hóa AES-256-GCM, khóa được dẫn xuất từ thông số phần cứng máy (Hardware ID) kết hợp với salt ngẫu nhiên, lưu trữ an toàn qua DPAPI (Windows). Quy trình update: tải file mới, giải mã thử bằng khóa hiện tại, nếu thành công thì ghi đè, nếu không thì giữ nguyên file cũ và config.
8. **Kiểm toán engine**: Thứ tự soi ưu tiên: 1. Luật cờ (đi đúng nước, lặp lại), 2. Khóa băm (hash collision), 3. Eval-net (đầu ra ổn định), 4. Search (cắt tỉa đúng). Cổng ưu thế ≥ 400 cần được kiểm tra bằng các bộ test endgame đã biết kết quả chính xác.
9. **App điện thoại Kỳ Viện**: Kiến trúc tối thiểu: MVVM với 3 module chính: Auth (đăng nhập độc lập), Game (màn hình chơi cờ, nhận diện ảnh), Analysis (gửi FEN lên server để phân tích). Tập trung vào trải nghiệm "chơi ngay" (guest login) để tối thiểu hóa rào cản phát hành.

---

## ASK03 — RÀ MÃ NGUỒN
*Không biết / Không thể thực hiện*. Yêu cầu này cần truy cập trực tiếp vào tệp `Ask03_source_2026-09-25.zip` để rà soát mã nguồn cụ thể. Vì môi trường hiện tại không có quyền truy cập vào tệp zip này, tôi không thể đưa ra các mã lỗi `L-<nhóm>-<số>` hoặc `A3-xx` với trích dẫn `tệp:dòng` chính xác theo yêu cầu của quy tắc chấm điểm.
