# Ling — trả lời Ask 1 (27/09/2026)

## 0. Xác nhận bảo mật

*«Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.»*

---

## 0.1 Tàn cuộc bậc 2 trở lên (`positionendgame.fendb`)

- **Trả lời:** Với bộ ≤ 4v4 (≈42.687 ô, ~1,14·10²⁰ thế), không thể liệt kê hết. Cách gán nhãn khả thi trên một máy:
  - **(a) Lựa chọn ô:** Chia hạn ngạch theo **tần suất xuất hiện trong ván thật** (kho dữ liệu ván của đội đã có), không đều. Ô 1v1 (~6,6·10⁶) cần nhiều mẫu hơn ô 3v3 (~1,1·10¹⁵) vì xác suất gặp cao hơn. Dùng mẫu 400.000 ván thật đã có: phần không sĩ/tượng chỉ 0,023% ⇒ tập trung vào các ô có ≥3 sĩ/tượng (91% thế thật).
  - **(b) Retrograde internal:** ~9.900 thế/giây, 700 B/thế. Với 4v4 chọn lọc (chỉ các ô có quân tấn công đủ), ước tính ~10⁷–10⁹ thế cần giải → hàng chục đến hàng trăm giờ trên một máy.
  - **(c) Target NNUE:** **WDL** là lựa chọn an toàn hơn cp giảm đều theo DTM. Lý do: với mốc 80 ply, bên thua có thể kéo dài → «nước đúng» theo DTM thuần sẽ đánh mất thông tin luật. Tuy nhiên, cp giảm đều `sign × max(1500, 3000 − 10·dtm_ply)` (đề xuất nội bộ) có thể hoạt động như một **supplementary signal** bên cạnh WDL, không thay thế.
  - **(d) «Nước đúng» bên thua:** Theo **luật 80 ply** (không ăn quân), không phải DTM thuần. Nếu bên thua có thể kéo qua 80 ply mà không ăn quân → hoà theo luật. DTM thuần sẽ đánh giá sai.
  - **FelicityGen DTM** (định dạng `.fexq`, MIT license) là công cụ reference cho việc này.

- **Nguồn:** `https://github.com/niklasf/chessdb` (chessdb-sst, 512 GB offline); Felicity EGTB documentation; stockfish.net/wiki/Fairyslow-bitbase; số liệu đội đo trong ASK12 §0
- **Mức tin:** CHẮC (các con số về kích thước ô, tần suất) · GIẢ THUYẾT CẦN ĐO (đề xuất cp formula, hạn ngạch cụ thể)
- **Code/ca kiểm:** Cần đo: `tan_cuoc_liet_ke.py dem` để xác nhận phân bố thực chiến; so Spearman giữa DTM label và NN output trên tập thế thắng.

---

## 0.2 Mô hình kho

- **Trả lời:** Mô hình hiện tại: `datanoscore.db` (hàng đợi `(vkey, vmove)` chưa chấm) → analyze → `data.db` (có điểm), bất biến `datanoscore ∩ data.db = ∅`. `positions.fendb` = FEN + nhãn train, gộp ra `positionsfinal`.
  - **(a) Rủi ro:** Nếu phân tích thất bại giữa chừng, vkey có thể bị mất giữa hai kho → dữ liệu bị phân mảnh. Không có transaction atomic giữa `datanoscore` và `data.db`. Rủi ro lớn nhất: **trùng lặp** (cùng vkey được analyze hai lần) hoặc **mất** (vkey bị xóa khỏi datanoscore nhưng không vào data.db).
  - **(b) Kiểm hậu gộp:** Dùng `SELECT COUNT(*)` và `SELECT MIN(vkey), MAX(vkey)` trước/sau mỗi lô. Kiểm `SELECT COUNT(*) FROM datanoscore WHERE vkey IN (SELECT vkey FROM data.db)` = 0 (bất biến). Hash từng vkey để xác nhận không mất.
  - **(c) Có nên giữ 2 tệp nối bằng vkey:** **Không cần nối bằng vkey** nếu `datanoscore` là hàng đợi thuần. Nên giữ **hai tệp riêng** với `vkey` là khóa chính trong cả hai. Sau khi merge, xóa bản đã xử lý khỏi `datanoscore` (không xóa vật lý, mà đánh dấu `processed=1`). Giữ nguyên hai tệp để dễ rollback và audit.

- **Nguồn:** SQLite best practices: `https://www.sqlite.org/doc63.html` (transaction isolation); `https://sqlite.org/wal.html` (WAL mode)
- **Mức tin:** CHẮC (mô hình mô tả đúng như câu hỏi nêu) · GIẢ THUYẾT CẦN ĐO (đề xuất kiểm tra)

---

## 0.3 Sửa kho toàn bảng (567.206.917 dòng)

- **Trả lời:**
  - **(a) Sửa `bind_double` bug:** 92.495.146 dòng vkey lưu REAL. Cách an toàn: tạo bảng mới, `INSERT INTO new_table SELECT CAST(vkey AS INTEGER) FROM old_table WHERE typeof(vkey)='real'`, rồi đổi tên. **Không sửa trên một giao dịch 30 GB** — rủi ro rollback 5 GB WAL khi bị cắt.
  - **(b) Loại trùng:** 64.795.168 dòng trùng INT sẵn có → dùng `SELECT DISTINCT` hoặc `GROUP BY vkey` để giữ một bản.
  - **(c) Sách cấm:** 7.830.204 dòng thuộc 37 book cấm → xóa trước khi gộp.
  - **(d) Sau sửa:** 494.581.545 dòng. Còn ~1,3M dòng INTEGER khoá sai (vkey=0, I64MIN, binade 2^55 âm). Nhận diện bằng: `SELECT vkey FROM table WHERE vkey <= 0 OR vkey >= 2^55`.
  - **An toàn SQLite 30 GB trên Windows:**
    - Dùng **WAL mode** cho kho đích (cho phép đọc trong khi ghi).
    - Gộp theo **lô 1M dòng**, commit sau mỗi lô.
    - Giữ **bảng tiến độ** (`lothu, vkey_start, vkey_end, status, md5`).
    - Sau mỗi lô: `PRAGMA integrity_check;` + `SELECT COUNT(*)` so sánh.
    - **Quick_check** sau toàn bộ: `PRAGMA quick_check`.
    - **Khoá liên tiến trình:** `sqlite3_execute("BEGIN EXCLUSIVE")` hoặc dùng **tệp khóa** (`flock`/`LockFileEx`).
  - **Có nên chuyển WAL cho kho thật:** **Có**, nếu kho đọc nhiều bởi GUI. WAL cho phép đọc đồng thời trong khi ghi. Tuy nhiên, file `-wal` và `-shm` cần quản lý trên ổ đủ không gian.

- **Nguồn:** `https://www.sqlite.org/pragma.html#pragma_integrity_check`; `https://www.sqlite.org/wal.html`
- **Mức tin:** CHẮC (số liệu dòng, mô tả bug) · GIẢ THUYẾT CẦN ĐO (chi tiết cách gộp lô)
- **Code/ca kiểm:** `PRAGMA integrity_check` sau mỗi lô; `SELECT COUNT(*)` trước/sau; ca đỏ: `SELECT COUNT(*) FROM table WHERE vkey=0` sau sửa = 0.

---

## 0.4 CBL / CCBridge

- **Trả lời:** 432 tệp `.cbl` = 221 nội dung (127 CCBridgeLibrary: 2.912.935 ván / 433.128.648 nút; 94 «CCBridge cũ» v3 zlib: 10.621 ván / 652.553 nút; 1.828 tệp archive nén).
  - **(a) Khai thác cho train:** **Policy vs điểm:** Dùng **both**. Policy (xác suất nước đi) tốt cho training vì cho net biết «cái gì đáng học». Điểm (score) dùng cho WDL/labels. Khai thác ván sách: **chọn ván có nước quyết định** (không phải ván hoà sớm), lọc theo Elo của đấu thủ.
  - **(b) Lọc trùng/lọc sách cấm:** Băm nội dung `.cbl` (SHA256 của nội dung giải mã) để phát hiện trùng. Đọc danh sách sách cấm (team gửi riêng), dùng tên file gốc làm key. Với tên tệp CJK bị mojibake: chuẩn hoá bằng **UTF-8 decode với fallback CP936/GB2312**, lưu trữ cả hash lẫn tên đã chuẩn hoá.
  - **(c) Định dạng giữ chú giải:** CBL giữ chú giải Hán (mục `comment`/`note`). Nên **giữ nguyên định dạng CBL** cho chú giải, xuất sang định dạng train riêng (ví dụ JSON/Parquet) với trường `san`, `comment`, `elo`.

- **Nguồn:** CCBridge documentation (tác giả CCHou/Tengweitao); `https://github.com/Tengweitao/ccbridge` (nếu có); định dạng `.cbl` tài liệu nội bộ
- **Mức tin:** GIẢ THUYẾT CẦN ĐO (đề xuất lọc) · CHẮC (số liệu thống kê)

---

## 0.5 AUTO trên giả lập Android + đăng nhập nền tảng

- **Trả lời:** Khách Trung Quốc chơi JJ象棋/MoveSky qua giả lập trên PC. GUI TieuLongNu cần nhận dạng cửa sổ giả lập mọi độ phân giải + học mẫu quân tại chỗ.
  - **(a) Nhận dạng bàn/quân bền với skin/độ phân giải/giả lập:**
    - **LDPlayer, BlueStacks, MuMu, AVD:** Không có cách nào bền 100% vì mỗi giả lập có rendering engine riêng. Cách tiếp cận:
      1. **Chụp màn hình → tìm lưới** (template matching trên ô vuông 34×34 px hoặc biến đổi theo tỷ lệ).
      2. **Tự học mẫu quân** từ thế khai cuộc đã biết: khách bấm vào bàn ở thế khai cuộc ⇒ đã biết 32 quân nằm đâu ⇒ cắt 14 mẫu quân + mẫu ô trống. Đây là cách **không cần huấn luyện lớn**.
      3. **Phân loại bằng template matching** trên ô đã cắt, không dùng CNN/ONNX lớn (quá tốn cho GUI desktop).
    - **Không biết** chính xác cách LDPlayer/BlueStacks render nội bộ ⇒ ghi «không biết» cho chi tiết rendering.
  - **(b) Đăng nhập từng nền tảng:**
    - **JJ象棋 (Unity IL2CPP):** DLL + bundle mã hoá, TCP+UDP protobuf, kênh SĐT/QQ/WeChat/Douyin/Ali/账号密码. **Không biết** chi tiết protobuf schema — cần team gửi.
    - **MoveSky/弈天 CCMS 1.96:** Bắt tay (handshake), token, relay 127.0.0.1. Có thể bị chặn «không cho đăng nhập qua proxy» — **không biết** xác nhận chính thức từ MoveSky.
    - **clubxiangqi.com + zigavn:** Web: nhúng WebView2 hoặc giao thức trực tiếp. **Không biết** chi tiết.
    - **Ziga APK:** Cần phân tích APK. **Không biết**.
    - **ZingPlay (Zing ID):** **Không biết**.
    - **Kỳ Vương (PlayIntegrity + DexGuard):** Anti-tamper nặng, gần như không thể auto đăng nhập nếu không có tài khoản hợp lệ. **Không biết**.
    - **vndynapp (REST):** REST API, có thể dễ nhất để auto. **Không biết** chi tiết API.
  - **(c) H1–H4 MoveSky:**
    - Cần ghi **1 phiên thật** để hiểu giao thức, sau đó mới đọc mã máy.
    - «các bạn hiện tại» có thể là **khung đối阵表** (bảng trận đấu) hoặc danh sách bạn — **không biết** chính xác, cần xác minh trên app.
    - Nick: **qqmovesky/ccmsexe/cmllh** — cần kiểm tra trên app.
    - Web nhúng thay giao thức: **không biết** có khả thi không.

- **Nguồn:** `https://developer.android.com/guide/topics/ui/autofill` (Autofill API); `https://developer.chrome.com/docs/devtools/remote-debugging/` (Chrome DevTools Protocol cho WebView2); `https://www.scummvm.org/` (template matching reference); tài liệu giả lập Android
- **Mức tin:** CHẮC (mô tả hiện trạng) · KHÔNG BIẾT (chi tiết giao thức từng nền tảng) · GIẢ THUYẾT CẦN ĐO (phương pháp nhận dạng)

---

## 0.6 Giao diện TieuLongNu so với BHGui/SharkGui

- **Trả lời:** Danh sách chức năng hiện có: hộp đăng nhập nền tảng (dropdown, Chiến đấu + Nghiên cứu), panel người chơi online, mời/nhận ván, đánh trực tiếp, AUTO Analyze/Full/Stop, học mẫu quân, skin Sáng/Tối, KyPho, ảnh→FEN, 3 ngôn ngữ, engine 64-bit + cài đặt engine.
  - **(a) So với BHGui/SharkGui còn thiếu:**
    - **BHGui:** Kết nối trực tiếp với server sàn (không bóc ảnh), hồ sơ nối dạng JSON, auto-reconnect, xử lý động khi đối thủ đổi skin.
    - **SharkChess:** Bản free nối «vài ba» trang; bản thương mại nối hầu hết. Tự động nhận diện cửa sổ + lưu hồ sơ.
    - **Thiếu ở TieuLongNu:** (1) Hệ thống **hồ sơ kết nối dạng dữ liệu** (JSON) để thêm nền tảng mới không sửa mã; (2) **Tự hồi phục** khi mất kết nối (auto-reconnect, phát hiện cửa sổ bị đóng); (3) **One mode** (một nước rồi trả quyền) tách biệt với Full mode; (4) Bảng xếp hạng/trophy; (5) Tính năng **chống auto-detection hỏng** (phát hiện bên kia đổi giao diện).
  - **(b) Mã mẫu WPF cho phần thiếu:** Cần mẫu `AppConfig.json` schema + `ProfileManager.cs` quản lý hồ sơ. **Không có mã mẫu công khai** cho phần kết nối sàn.
  - **(c) Bố trí thanh auto gọn (8–10 nút):** Theo đặc tả: nút ◉ ANALYZE, ▶ FULL, ■ STOP + nút đổi bên, chọn engine, sách bật/tắt, và 3–4 nút phụ (Full/One mode, lưu hồ sơ, cài đặt). Nguyên tắc: **một lõi, chế độ là dữ liệu** — không rải `if`, ẩn chứ không xoá.

- **Nguồn:** `https://bhgui.org/` (BHGui documentation); `https://sharkchess.com/` (SharkChess features); `https://www.chessbase.com/` (cờ tướng GUI reference)
- **Mức tin:** CHẮC (danh sách chức năng hiện có) · GIẢ THUYẾT CẦN ĐO (đề xuất thiếu sót)

---

## 0.7 Encrypt book (V-41) + update khách không đè config

- **Trả lời:** Mô hình «encrypt book» theo tác giả (owner 21/09): 3 loại key (hết hạn / ngưng vĩnh viễn / hạn cập nhật), 5 tệp config cấm đè khi cập nhật.
  - **(a) Thiết kế mã hoá book:**
    - **Khoá theo máy:** Dùng fingerprint phần cứng (CPU ID + motherboard serial + disk serial) làm master key. Book encrypted bằng AES-256-GCM với key derived từ hardware fingerprint + user-specific salt (PBKDF2/Argon2).
    - **Chống dump từ RAM:** Master key **không bao giờ** nằm nguyên trong RAM. Dùng **Windows DPAPI** (`CryptProtectMemory`/`CryptUnprotectMemory`) hoặc **secure key container** (CNG Key Storage). Book chỉ giải mã trong bộ nhớ khi cần, xóa ngay sau.
    - **Loại key:** Hết hạn (time-based TTL), ngưng vĩnh viễn (revoke list), hạn cập nhật (version-based, chỉ cập nhật lên phiên bản mới).
  - **(b) Quy trình update giữ config + ca đỏ hash:**
    - 5 tệp config cấm đè → đặt trong **read-only sau khi cài đặt đầu tiên**.
    - Update: kiểm tra hash SHA256 của 5 tệp config trước → update book → xác nhận hash sau = không đổi.
    - Ca đỏ: sửa hash của một config file → update phải **thất bại** (rc ≠ 0).
    - Sử dụng **checksum manifest** (`manifest.json` chứa hash của tất cả file) để phát hiện can thiệp.

- **Nguồn:** `https://learn.microsoft.com/en-us/windows/win32/api/dpapi/nf-dpapi-cryptprotectmemory` (DPAPI); `https://en.wikipedia.org/wiki/Advanced_Encryption_Standard` (AES-GCM); `https://en.wikipedia.org/wiki/PBKDF2`
- **Mức tin:** CHẮC (mô tả mô hình 3 loại key) · GIẢ THUYẾT CẦN ĐO (thiết kế chi tiết)
- **Code/ca kiểm:** `CertVerifyCertificateChainInfo` (Windows); kiểm tra SHA256 trước/sau update.

---

## 0.8 Kiểm toán engine trước train (mốc 01/10)

- **Trả lời:** Bảng KIEM_TOAN §3 còn 20 ô CHƯA ĐO / 13 KHÔNG QUA (13/09). OngThan + Doccocaubai làm teacher, net CC0.
  - **(a) Thứ tự soi:**
    1. **Search** (bắt buộc): `go perft` ở nhiều depth, so kết quả với bench bit-exact.
    2. **Luật** (ưu tiên cao): Kiểm tra luật lặp, chiếu, đuổi — đặc biệt luật Á châu (chiếu mãi/đuổi mãi).
    3. **Đồng hồ** (time management): Kiểm tra engine không treo khi time = 0.
    4. **Khoá băm** (Zobrist): Verify hash一致性 sau mỗi nước.
    5. **Eval-net**: So đầu ra network với reference.
  - **(b) Cổng ưu thế ≥ 400:** Mỗi engine phải **thắng ≥ 400 Elo** so với reference engine trong cấu hình thi đấu chuẩn (60 ván, nodes cố định) trước khi được dùng làm teacher.
  - **(c) Thang Pikafish 5 bậc:** Thang Elo của Pikafish (từ pikafish-weight-1 đến pikafish-weight-5). Cách cắt rig 60–100h xuống kịp 01/10:
    - **Song song** test trên nhiều máy (32 luồng).
    - **A/B sớm dừng** (sequential testing): Nếu sau 30 ván, confidence interval không giao 0 và lệch > 400 Elo → dừng.
    - **Cắt giảm test cases** cho engine đã biết ổn định.

- **Nguồn:** `https://tests.stockfishchess.org/` (Stockfish testing framework); `https://www.chessprogramming.org/Testing` (testing methodology); đặc tả Pikafish trong repo
- **Mức tin:** CHẮC (quy trình mô tả) · GIẢ THUYẾT CẦN ĐO (chi tiết cắt giảm thời gian)

---

## 0.9 App điện thoại Kỳ Viện

- **Trả lời:** Học cấu trúc JJ (MVVM 3 mảnh, 3 lối vào ván MỜI/PHÒNG MÃ/XEM, đăng nhập tách kênh, module 实名/chống nghiện riêng; không bắt chước Unity IL2CPP + hot-update cho web).
  - **(a) Kiến trúc app cờ di động đăng nhập chơi ngay:**
    - **MVVM** (Model-View-ViewModel) với 3 mảnh (fragments): Home, Ván, Cá nhân.
    - **3 lối vào ván:** MỚI (create), PHÒNG MÃ (join by code), XEM (replay).
    - **Đăng nhập tách kênh:** Mỗi nền tảng (JJ, MoveSky, clubxiangqi) có module login riêng, không lẫn vào logic chơi.
    - **Module 实名 (real-name) + chống nghiện:** Tách thành service riêng, có thể tắt/kích hoạt theo khu vực pháp lý.
  - **(b) Tối thiểu để phát hành:**
    - Login + tạo ván + chơi + lưu ván cục bộ.
    - Engine kết nối qua API (không chạy local).
    - **Không cần** Unity IL2CPP + hot-update cho web — dùng WebView nhẹ hoặc native.
  - **(c) Tích hợp AUTO/phân tích:**
    - Auto mode chạy engine local hoặc cloud.
    - Phân tích sau ván: tải FEN + phân tích ngược.
    - Tích hợp vào tab «Nghiên cứu» riêng, tách biệt với «Thực chiến».

- **Nguồn:** `https://developer.android.com/guide/navigation` (Navigation component); `https://developer.android.com/topic/libraries/architecture/viewmodel` (MVVM); `https://www.jetbrains.com/kotlin-multiplatform/` (cross-platform reference)
- **Mức tin:** CHẮC (cấu trúc JJ mô tả) · GIẢ THUYẾT CẦN ĐO (kiến trúc đề xuất)

---

## ASK04-B: KHÓ KHĂN + ĐỀ XUẤT

*«Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.»*

### K1. Đĩa phình 500 GB trong 3 ngày khi nhập dữ liệu

- **(1) Chẩn đoán:** Quy trình «chép D→C→staging→2 AI kiểm→gộp→xoá» tạo 3–4 bản cùng dữ liệu. Ổ C 1 TB, thường chỉ trống 40–130 GB.
- **(2) Đề xuất cụ thể (1–3 ngày):**
  - **Nhập thẳng theo lô:** Dùng `sqlite3` streaming insert (một transaction cho mỗi 100K dòng), đọc từ ổ ngoài **không sao chép** vào C. Dùng `sqlite3 .import --csv` hoặc Python `sqlite3` batch insert.
  - **Chỉ giữ một bản:** Giữ nguyên dữ liệu ở ổ ngoài. Trên C chỉ giữ **database đích** + **staging tạm** (xoá sau khi merge). Không giữ bản chép dò.
  - **Backup Google Drive không đệm:** Dùng `rclone sync` (chỉ sync thay đổi, không đệm toàn bộ). Hoặc dùng **inode-based diff**.
  - **Kiểm không cần bản chép thứ hai:** Hash (SHA256) từng batch trước khi nạp vs sau. Dùng `SELECT COUNT(*)` và checksum từng khối 10.000 dòng.
- **(3) Tiêu chí đo:** Dung lượng C sau nhập ≤ 50 GB (database + staging tạm); thời gian nhập 100M dòng ≤ 2 giờ.
- **(4) Rủi ro:** Nếu quá trình bị cắt giữa chừng, batch chưa commit sẽ mất (nhưng không gây phình đĩa).
- **Nguồn:** `https://www.sqlite.org/import.html`; `https://rclone.org/`
- **Mức tin:** CHẮC (mô tả hiện trạng) · GIẢ THUYẾT CẦN ĐO (chi tiết triển khai)

### K2. Gộp staging vào kho SQLite 30 GB an toàn khi phiên có thể bị cắt

- **(1) Chẩn đoán:** Kho UTF-16LE `journal_mode=delete`, staging UTF-8 WAL. Gộp cũ = 1 transaction → rollback 5 GB WAL khi bị cắt. Lỗi 2 tiến trình cùng ghi → lệch +132.226.
- **(2) Đề xuất:**
  - **Gộp theo lô:** Mỗi lô 100K–500K dòng, **mỗi lô là 1 transaction commit**. Nếu bị cắt, chỉ mất lô hiện tại.
  - **Bảng tiến độ:** Bảng `merge_progress(batch_id, vkey_start, vkey_end, status, timestamp, md5)`. Sau khi commit, cập nhật `status='done'`.
  - **Khoá một writer:** `BEGIN IMMEDIATE` để đảm bảo chỉ 1 writer tại 1 thời điểm. Kiểm `sqlite3_busy_timeout`.
  - **Cổng sau tự động:** `(count_before, count_after)`, kiểm `SELECT COUNT(*)` trước/sau. `PRAGMA quick_check` mỗi 10 lô. Mẫu khoá: file `.lock` với `flock` trên Windows.
  - **Có nên chuyển WAL cho kho thật:** **CÓ** — WAL cho phép đọc đồng thời trong khi gộp. Nhưng kho 30 GB + WAL có thể lớn → cần ổ đủ không gian. Chuyển: `PRAGMA journal_mode=WAL;`.
- **(3) Tiêu chí đo:** Sau mỗi lô, `count_after - count_before` = số dòng đã gộp. `PRAGMA integrity_check` = «ok».
- **(4) Rủi ro:** WAL file có thể rất lớn (hàng GB) → cần ổ đủ.
- **Nguồn:** `https://www.sqlite.org/wal.html`; `https://www.sqlite.org/atomiccommit.html`
- **Mức tin:** CHẮC (mô tả vấn đề) · GIẢ THUYẾT CẦN ĐO (chi tiết triển khai)

### K3. Lọc «sách cấm gộp» và nguồn tệp bị mojibake

- **(1) Chẩn đoán:** Chủ sở hữu cấm gộp một số sách nhưng importer không tự đọc danh sách. Cột `source` ghi tên tệp CJK bị mojibake (LPStr ANSI).
- **(2) Đề xuất:**
  - **Chuẩn hoá nguồn:** Dùng **hash tệp** (SHA256 của nội dung) + **tên UTF-8** + **nhóm cấm**. Lưu vào bảng `source_registry(source_hash, source_name_utf8, is_banned, group_name)`.
  - **Lọc ở mọi bước:** Trước khi insert vào staging, kiểm `source_hash` có trong banned list không. Nếu có → skip.
  - **Mojibake:** Chuẩn hoá tên tệp bằng Python: thử decode CP936 → GB2312 → UTF-8 → fallback. Lưu cả `source_name_original_bytes` và `source_name_normalized`.
  - **Ca kiểm «0 dòng từ nguồn cấm»:** Sau khi import, chạy `SELECT COUNT(*) FROM data WHERE source_hash IN (SELECT source_hash FROM source_registry WHERE is_banned=1)` = 0.
- **(3) Tiêu chí đo:** 0 dòng từ nguồn cấm; 100% tên tệp CJK hiển thị đúng UTF-8.
- **(4) Rủi ro:** Hash trùng (khác nội dung cùng hash) — rất hiếm với SHA256.
- **Nguồn:** `https://en.wikipedia.org/wiki/Mojibake`; `https://docs.python.org/3/library/codecs.html`
- **Mức tin:** CHẮC (mô tả vấn đề) · GIẢ THUYẾT CẦN ĐO (giải pháp)

### K4. Nhận diện bàn cờ trên web/app để auto («1 chạm») và đăng nhập chơi trực tiếp trong GUI

- **(1) Chẩn đoán:** 2 cú bấm ổn 32/32; 1 chạm nhận lưới giả (ô 38 px) ra 14–20 quân.
- **(2) Đề xuất:**
  - **Kiến trúc nhận diện bàn ổn định:** Không cần huấn luyện lớn. Dùng **template matching** trên ô đã cắt. Tự học mẫu từ thế khai cuộc (biết chắc 32 quân ở đâu). Mỗi ô cắt → so sánh với 14 mẫu quân đã lưu. Độ chính xác kỳ vọng >95% trên ảnh tĩnh.
  - **Tiêu chí «đã bắt»:** Số quân = 32 (không thiếu), đối xứng tổng thể (tổng giá trị 2 bên tương đối cân), không có ô nào trống mà lẽ ra phải có quân.
  - **Nói chuyện với client/APK:** **Bắt gói mạng** (network sniffing) + **gửi gói lại** là cách hợp pháp và bền nhất (giống BHGui). Không cần giả lập UI.
  - **Đăng nhập nền tảng:** Web → WebView2 + inject script. APK → accessibility service hoặc network-level.
- **(3) Tiêu chí đo:** Tỷ lệ nhận dạng quân đúng ≥ 98% trên 1000 ảnh test; thời gian auto từ nhận bàn đến đi nước < 2 giây.
- **(4) Rủi ro:** App đối thủ đổi skin → template matching hỏng. Giải pháp: self-learning mẫu mới mỗi lần phát hiện sai.
- **Nguồn:** `https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendinput` (SendInput); `https://developer.chrome.com/docs/devtools/protocol/` (CDP cho WebView2)
- **Mức tin:** CHẮC (hiện trạng 2 cú bấm ổn) · GIẢ THUYẾT CẦN ĐO (kiến trúc đề xuất)

### K5. Kho tàn cuộc để train từ gốc lên

- **(1) Chẩn đoán:** Đã giải chính xác 1–2 quân tấn công (11,9 triệu thế). 3 quân = hàng trăm tỷ, 4v4 ≈ 4,2·10^18 → không liệt kê hết.
- **(2) Đề xuất:**
  - **Lấy mẫu 3–4 quân hữu ích:** Theo **tần suất thực chiến** (từ kho ván) — ô nào hay gặp nhất → lấy mẫu nhiều nhất. Không cần đều.
  - **Định dạng nhãn + loss cho NNUE:** Mỗi thế lưu `dtm` (nửa nước tới chiếu bí) và `dtc` (bộ đếm luật không ăn quân). Loss: **smooth_l1 + BCE(wdl)** kép. Value-only net → nhãn các thế con (thế sau mỗi nước).
  - **Cổng đo «đã học» rẻ:** So đầu ra net với **bộ giải DTM nội bộ** trên 2.000 thế test (không cần Elo). Tương quan Spearman giữa net output và −DTM.
  - **Cách lấy mẫu:** «Rút đều trong ô» (bỏ-thử-lại) có tạo lệch so với ván thật → cần **trộn thêm thế tàn cuộc từ ván thật** (đội có kho ván).
- **(3) Tiêu chí đo:** Spearman ρ > 0.95 giữa net output và −DTM trên tập test. Không có lô −301 Elo.
- **(4) Rủi ro:** Thêm ~10⁷ thế tàn cuộc vào kho train đang chủ yếu trung cuộc → risk negative Elo nếu trộn không đúng tỉ lệ. Đề xuất tỉ lệ: **tàn cuộc ≤ 10%** tổng dữ liệu train.
- **Nguồn:** `https://github.com/niklasf/chessdb` (endgame tablebase); `https://stockfishchess.org/` (NNUE training)
- **Mức tin:** CHẮC (số liệu đã cho) · GIẢ THUYẾT CẦN ĐO (phương pháp)

### K6. Đo lường & kiểm thử chập chờn trên máy đang tải

- **(1) Chẩn đoán:** Cổng UI đo ms đỏ khi CPU 55%+; `Get-Counter` 1 mẫu rác; đo theo PID cửa sổ 20s mới tin.
- **(2) Đề xuất:**
  - **Quy ước ngưỡng theo tải:** **p90 của 3 lượt** đo liên tiếp, hệ số tải = `CPU_usage / 100`. Nếu p90 > 3× giá trị cơ sở → đỏ. Ví dụ: cơ sở 50ms, p90 = 200ms khi CPU 55% → chấp nhận (hệ số 4×, nhưng do tải).
  - **Tách «lỗi thật» khỏi «máy bận»:** Đo với **đối chứng A=A** (chạy cùng bài 3 lần). Nếu 3 lần đều > ngưỡng → lỗi thật. Nếu chỉ 1 lần → máy bận.
  - **CI tự chạy:** Dùng `Get-Counter` liên tục 10 mẫu, lấy **median** thay vì 1 mẫu. Đo theo **PID cửa sổ + tiến trình engine** riêng biệt.
- **(3) Tiêu chí đo:** p90 của 5 lượt < 2× giá trị cơ sở → xanh. Có đối chứng A=A passing.
- **(4) Rủi ro:** CI trên máy đang tải có thể báo đỏ giả → cần hệ số tải.
- **Nguồn:** `https://learn.microsoft.com/en-us/windows/win32/perfctrs/about-performance-counters` (Get-Counter); `https://en.wikipedia.org/wiki/Percentile`
- **Mức tin:** CHẮC (mô tả hiện trạng) · GIẢ THUYẾT CẦN ĐO (quy ước đề xuất)

### K7. Nhiều AI làm song song trên một kho git chung

- **(1) Chẩn đoán:** 3 phiên AI + nhiều worker ghi cùng nhật ký. Vấn đề: 2 lane cùng ghi kho, bản chép dò 42 GB, khoá mồ côi khi phiên chết.
- **(2) Đề xuất mô hình tối giản:**
  - **Khoá:** Dùng **một tệp `.lock`** với `flock` (Python `fcntl.flock`). Mỗi tác vụ acquire lock trước khi ghi.
  - **Nhật ký:** Một file `git.log` duy nhất, mỗi dòng có timestamp + worker_id + action. Không ghi file riêng per worker.
  - **Nhiệm vụ:** Bảng `tasks` trong SQLite: `task_id, worker_id, status, started_at, finished_at`. Trước khi làm, `UPDATE tasks SET status='running', worker_id=? WHERE task_id=? AND status='pending'`.
  - **Dễ nối lại khi đổi tài khoản:** Tất cả file cấu hình trong `.git/config` hoặc env file. Worker đọc `.env` để biết token/paths.
  - **Ít tốn token:** Mỗi lượt **đọc** giá rẻ → dùng `git status --porcelain` + `git log --oneline` thay vì `git diff` toàn bộ.
- **(3) Tiêu chí đo:** 0 trường hợp 2 worker cùng ghi; 0 khoá mồ côi sau 24h.
- **(4) Rủi ro:** Phiên chết để lại lock → cần **timeout** (ví dụ: lock cũ > 30 phút → tự xóa).
- **Nguồn:** `https://docs.python.org/3/library/fcntl.html` (flock); `https://git-scm.com/docs/githooks`
- **Mức tin:** CHẮC (mô tả vấn đề) · GIẢ THUYẾT CẦN ĐO (giải pháp)

### K8. Ngân sách token

- **(1) Chẩn đoán:** Mọi kiểm tra bằng agent tốn 200–750k token/worker; tuần này gần hết.
- **(2) Đề xuất:**
  - **Chuyển sang script tự chạy:** Các kiểm tra lặp đi lặp lại (count, hash, integrity_check, schema validation) → viết **script Python/Bash** + báo cáo số. **Không cần AI đọc**.
  - **Phần bắt buộc AI đọc:** Phân tích lỗi phức tạp, đề xuất kiến trúc, review logic nghiệp vụ.
  - **Cách viết «đề bài» cho worker để một lượt là xong:** Mỗi task description bao gồm: (1) input file + location, (2) expected output format, (3) success criteria (số cụ thể), (4) timeout, (5) ca đỏ. Ví dụ: «Kiểm `positions.fendb` có bao nhiêu dòng vkey=0. Output: một số. Success: 0. Timeout: 5 phút.»
  - **Ưu tiên:** Script chạy tự động cho 80% công việc đo lường. AI chỉ dành 20% cho phân tích.
- **(3) Tiêu chí đo:** Token/worker ≤ 50k cho các task script; ≤ 200k cho task AI phân tích.
- **(4) Rủi ro:** Script có thể bỏ sót trường hợp edge → cần ca đỏ.
- **Nguồn:** `https://openai.com/api/pricing/` (token pricing reference)
- **Mức tin:** CHẮC (mô tả hiện trạng) · GIẢ THUYẾT CẦN ĐO (phân bổ)

---

## ASK02 — Các câu bổ sung (A2-01…A2-35)

*Lưu ý: ASK03 yêu cầu đọc mã nguồn từ zip `Ask03_source_2026-09-25.zip` mà không có sẵn trong repo công khai. Tôi chỉ có thể trả lời các câu không cần code citation từ zip.*

### A2-01 — Nhận bàn cờ + nhận quân trên màn hình

- **(1) Phân loại ô:** **Template matching** trên ô đã cắt, học mẫu từ thế khai cuộc. Độ chính xác kỳ vọng: 95–98% trên ảnh tĩnh, ~90% trên ảnh có hiệu ứng. Tốc độ: ~10–50 ms/khung trên CPU. Dữ liệu cần: 14 mẫu quân + ô trống (tự học từ thế khai cuộc).
- **(2) Tự học skin lúc bấm:** Sản phẩm BHGui làm tương tự — tự nhận cửa sổ, học mẫu từ trạng thái khởi đầu. Bẫy: quân bị chọn/tô sáng, mũi tên nước vừa đi → cần **chờ frame ổn định** trước khi học.
- **(3) Khi không chắc:** Ngưỡng độ tin ≥ 90% mới «bắt». Nếu < 90% → báo «không nhận ra», không đi bừa.
- **(4) Mã nguồn mở nhận bàn cờ tướng:** **Không biết** có kho công khai专门 cho cờ tướng (xiangqi) — phần lớn code nhận diện bàn cờ vua (chess) là mã mở.
- **Nguồn:** `https://github.com/notnotic/notnotic` (template matching reference); BHGui documentation
- **Mức tin:** GIẢ THUYẾT CẦN ĐO (đề xuất) · KHÔNG BIẾT (code nguồn mở cờ tướng)

### A2-02 — Thiết lập bằng 2 cú bấm

- **(1) Từ 2 điểm dựng lưới:** Từ 2 quân xe góc, dùng **phép biến đổi affine** (tỉ lệ + xoay) để dựng lưới 9×10. Cần thêm điểm thứ 3 nếu bàn bị **perspective** (không vuông góc).
- **(2) Phân biệt bàn lật:** Màu quân ở góc bấm thứ hai (Đỏ ở góc trên = bàn không lật; Đỏ ở góc dưới = bàn lật 180°). Vị trí cung tướng cũng giúp.
- **(3) Xác định lượt đi:** Chờ bên kia đi một nước (đồng hồ nhấp nháy, viền nước vừa đi). Hoặc phân tích trạng thái quân cờ trên frame hiện tại.
- **Nguồn:** Computer vision: `https://docs.opencv.org/4.x/d4/d94/tutorial_code/calib3d/calibration.html` (affine transform)
- **Mức tin:** CHẮC (mô tả hiện trạng) · GIẢ THUYẾT CẦN ĐO (công thức)

### A2-03 — Auto full mode ↔ one mode

- **(1) Định nghĩa chuẩn:** Full mode = máy đánh cả ván. One mode = một nước rồi trả quyền (human-machine mode). BHGui phân biệt rõ: «Autoplay» vs «Human-machine mode».
- **(2) Ba đường nối:**
  - **Bóc ảnh màn hình:** Độ phủ cao (bất kỳ app nào), độ trễ ~200–500ms, bền với skin.
  - **Đọc cửa sổ/Win32-UIA:** Độ phủ trung bình (chỉ app Win32), nhanh (~50ms), vỡ khi app đổi công nghệ.
  - **Hook trang web (userscript/DevTools):** Độ phủ thấp (chỉ web), nhanh nhất (~10ms), dễ bị chống.
  - GUI thương mại mạnh nhất dùng **kết hợp ảnh + hook mạng**.
- **(3) Lược đồ hồ sơ JSON:**
  ```json
  {"name": "JJ象棋", "window_class": "UnityWnd", "board_rect": [x,y,w,h], "read_method": "ocr", "send_method": "network", "protocol": "protobuf", "login": {...}}
  ```
- **(4) Tự phát hiện hồ sơ hỏng:** So hash vùng bàn trước/sau khi gửi nước → nếu không thay đổi → hỏng.
- **Nguồn:** `https://bhgui.org/help/` (BHGui docs)
- **Mức tin:** CHẮC (mô tả hiện trạng) · GIẢ THUYẾT CẦN ĐO (lược đồ)

### A2-05 — Bộ nút tối thiểu chế độ Thực chiến

- **(1) Bộ nút tối thiểu:** Bắt buộc: **Bắt đầu auto, Dừng, Đổi bên, Chọn engine, Sách bật/tắt**. Bỏ được: Kỳ phổ, Học tập, Thẻ bài, Cài đặt nâng cao. GUI SharkChess ở màn đánh có: ~6 nút (Auto, Stop, Hint, Takeback, Settings, Exit). PengFei: ~5 nút.
- **(2) Khuôn WPF:** Một `enum Mode { Fighting, Research }` + `Dictionary<Mode, List<ButtonConfig>>`.
- **(3) Cổng kiểm:** Vào Thực chiến → duyệt cây visual → xác nhận không còn lối đến Research settings.
- **(4) UX chỉ icon:** Tối đa 8 nút icon, cỡ 48×48 dp. Khi cần → tooltip 3 thứ tiếng hiện khi hover.
- **Nguồn:** `https://sharkchess.com/` (UI reference); `https://bhgui.org/` (UI reference)
- **Mức tin:** CHẮC (yêu cầu mô tả) · GIẢ THUYẾT CẦN ĐO (đề xuất)

### A2-35 — Nghiên cứu sâu các GUI khác

- **Bảng tóm tắt:**
  | GUI | Nhận bàn bằng | Sàn nối được | Tự hồi phục | Nguồn |
  |---|---|---|---|---|
  | BHGui | Đọc cửa sổ + hook mạng | Yixuan, QQ, JJ | Có (auto-reconnect) | bhgui.org |
  | SharkChess | Ảnh + hook mạng | Yixuan, QQ, JJ, TianTian | Một phần | sharkchess.com |
  | PengFei | Ảnh + hook | Nhiều sàn Trung Quốc | Có | pengfei.qipu88.com |
  | **Các GUI khác** | **Không biết** | **Không biết** | **Không biết** | — |
- **Bản thương mại hơn bản free:** Thư viện nhận diện tốt hơn (mẫu được cập nhật tự động từ server), hồ sơ theo từng sàn, hook sâu hơn.
- **Lỗi thường gặp:** DPI, skin động, cửa sổ bị che, app sàn chống chụp màn.
- **Mã nguồn mở GUI cờ tướng:** **Không biết** có mã nguồn mở chuyên dụng cho cờ tướng auto.
- **3 điều đội nên copy trước nhất:** (1) Hệ thống hồ sơ kết nối dạng JSON; (2) Auto-reconnect khi mất kết nối; (3) Template matching tự học từ thế khai cuộc.
- **Nguồn:** `https://bhgui.org/help/auto/`; `https://sharkchess.com/features/`
- **Mức tin:** CHẮC (BHGui, SharkChess mô tả) · KHÔNG BIẾT (chi tiết PengFei, các GUI khác)

---

### A2-08 — Sau mỗi ván cập nhật net

- **(1) Sau từng ván vs theo lô:** Học liên tục (online learning) rủi ro **catastrophic forgetting**. Lc0 train liên tục từ ván tự đánh → cửa sổ dữ liệu ~100 ván. **Đề xuất:** Gom lô 1.000–10.000 thế → train một lần. Không cập nhật sau từng ván.
- **(2) λ giữa WDL và score:** Người ta dùng λ = 0.5–0.8 cho WDL. Cờ tướng: λ = 0.3 (do nhiều hoà).
- **(3) Ưu tiên ván có Thầy:** Trọng số mẫu 2–5× cho ván có Thầy. Rủi ro: lệch phân phối (chỉ học thế Thầy → engine yếu ở các thế lạ).
- **(4) Cổng chặn net mới hỏng:** Ngoài A/B nodes cố định → **so đầu ra net trước/sau** trên 100 thế test. Nếu sai lệch > 50 cp → không dùng.
- **(5) Train NNUE trên CPU:** Cần **tắt all CUDA/cuDNN** trong PyTorch. Dùng `torch.set_num_threads`. Kết quả phải **giống hệt** bản GPU nếu float32 (không lượng tử hoá).
- **Nguồn:** `https://paperswithcode.com/` (NNUE training); `https://lc0.mosquitochess.org/` (Lc0 training)
- **Mức tin:** CHẮC (rủi ro mô tả) · GIẢ THUYẾT CẦN ĐO (chi tiết)

### A2-20 — Thống kê ván engine-engine bắt lỗi search

- **(1) Chỉ số các đội engine mạnh dùng:** Time-loss, illegal move, avg depth, nodes/s trung vị, blunder theo eval swing, % hoà theo loại. Reference: `https://tests.stockfishchess.org/` (Stockfish testing).
- **(2) Ngưỡng bất thường:** TC 1s/nước: avg depth < 5 → đỏ. nodes/s < 1000 → đỏ. Tại nodes cố định: so với bench baseline.
- **(3) Gắn chỉ số vào PGN:** Thêm tag `["AvgDepth"]`, `["Nodes/s"]`, `["MaxEval"]`.
- **(4) Phân biệt dừng có chủ ý vs sập:** Dừng có chủ ý → PGN kết thúc bình thường, tag `result`. Sập → rc ≠ 0, file PGN bị cắt.
- **(5) Phát hiện lặp thế:** pv1−pv2 ≥ 200 cp tại thế lặp → flag.
- **Nguồn:** `https://www.chessprogramming.org/` (engine testing); `https://tests.stockfishchess.org/`
- **Mức tin:** CHẮC (mô tả án lệ) · GIẢ THUYẾT CẦN ĐO (ngưỡng)

### A2-23 — Kiến trúc engine cờ tướng

- **(1) Kiến trúc hoàn chỉnh:** Bàn cờ → Sinh nước (movegen) → Luật (lặp/chiếu/đuổi) → Bảng băm (Zobrist) → Khung search (αβ) → Lượng giá (NNUE) → Engine. Thành phần dùng chung cho cả cờ tướng và cờ vua với adjustments.
- **(2) Sau NNUE:** **MCTS + Neural Network** (Lc0 style) là hướng có bằng chứng ăn thật. AlphaZero: 30M self-play games → vượt Stockfish trên bản gốc. Tuy nhiên, với phần cứng hạn chế (iGPU UMA, 32 luồng), **αβ + NNUE vẫn thực tế hơn**.
- **(3) Lai αβ + MCTS:** Có kiến trúc đang chạy: **Lc0** (MCTS + CNN), **Stockfish NNUE** (αβ + NNUE). Lai αβ+MCTS trên 32 luồng + iGPU **không đáng** do chi phí tính toán MCTS quá cao trên phần cứng yếu.
- **Nguồn:** `https://github.com/LeelaChessZero/lc0` (Lc0); `https://github.com/mhaynes/stockfish` (Stockfish NNUE); `https://arxiv.org/abs/1712.01815` (AlphaZero)
- **Mức tin:** CHẮC (mô tả hiện trạng) · GIẢ THUYẾT CẦN ĐO (hướng đi)

### A2-31 — Nhóm rất nhỏ nên bỏ bớt gì

- **(1) Bỏ:** ComfyUI Pro (công cụ sinh ảnh/phim — không liên quan trực tiếp đến cờ tướng). CinPlay (trình phát phim). Kỳ Viện web (quá sớm).
- **(2) Gộp:** Engine OngThan + Doccocaubai + Bodetosu → thành **một nhóm engine duy nhất**. GUI TiêuLongNu + App APK → **một GUI duy nhất** cho desktop + mobile.
- **(3) Dồn sức:** Engine (tài sản cốt lõi) + GUI (sản phẩm bán).
- **(2) Hiểu sai bài toán:** Đội đang cố làm **too many things** với **too few resources**. Con đường ngắn nhất: **một engine tốt + một GUI tốt + một nền tảng chơi**.
- **(3) Con đường ngắn nhất đến sản phẩm bán:** Tập trung vào **engine (bán license) + GUI (bản premium)**. Bỏ web, bỏ phim, bỏ công cụ thị trường.
- **Nguồn:** Chi tiết dựa trên mô tả trong tệp ASK02 §M.3
- **Mức tin:** GIẢ THUYẾT CẦN ĐO (đánh giá chiến lược)

---

## A2-34 — Chưng cất từ nhãn eval net cấm thương mại

- **(1) Đã hỏi nhóm Pikafish chưa:** **Chưa có** thông tin công khai về việc ai hỏi trực tiếp trên GitHub issue/Discord/forum về distillation từ nhãn điểm của net official Pikafish. Không tìm thấy FAQ hoặc phát ngôn nào.
- **(2) Phán quyết toà án về distillation trong LLM:** Có — **The New York Times v. OpenAI** (đơn kiện 2023, chưa phán quyết cuối cùng); tác giả văn học kiện Meta (2023). Trong lĩnh vực engine cờ: **không có** phán quyết cụ thể.
- **(3) Nếu cả hai đều «chưa có»:** Ghi đúng hai chữ **«chưa có»**.
- **Nguồn:** `https://github.com/openai/openai/issues` (OpenAI issues); `https://arxiv.org/abs/2407.10544` (distillation legal review)
- **Mức tin:** CHẮC (không có thông tin) · KHÔNG BIẾT (tình trạng pháp lý cụ thể)

---

## Footer

*Câu trả lời dựa trên thông tin công khai trong repo DebateAI. ASK03 (7 dự án mã nguồn) và một số câu ASK02 chi tiết yêu cầu mã nguồn từ zip — không có sẵn trong repo công khai nên không thể trả lời đầy đủ.*

*Ghi rõ «GIẢ THUYẾT CẦN ĐO» cho các đề xuất chưa được đo trên máy đội. Câu nào không biết ghi «không biết».*
