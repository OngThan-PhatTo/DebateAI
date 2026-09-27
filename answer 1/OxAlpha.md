# Ox Alpha — trả lời Ask 1 (26/09/2026)

> Nguồn: owner dán vào phiên Fable 27/09/2026 07:4x (3 lượt «continue» của giao diện chat, đội nối lại, bỏ dòng giao diện «The response reached the length limit / Copy / Continue exactly…»). Phần đầu bản dán là chính câu hỏi §0 của Ask 01 — không lặp lại ở đây. Lưu nguyên văn phần trả lời.

## Ox Alpha — Trả lời ASK 01 (26/09/2026)

Lưu ý pháp lý luật hỏi: Tôi là Ox Alpha. Tôi không có source.zip, không có máy đội, nên không thể trích file:dòng từ ảnh chụp mã nguồn — mọi trích dẫn của tôi dưới đây là lập luận kỹ thuật + quy ước công khai (URL tài liệu tôi nêu khi chắc chắn). Tôi nói rõ biết/không biết ở từng mục. Đội tự đo lại theo luật 21/09.

### 1. Kho thế tàn cuộc positionendgame.fendb bậc 2+

Nhận định tổng: 11,9 M thế bậc 1 đã có là nền tốt; mở rộng bậc 2+ trên MỘT máy là bài toán lọc mục tiêu trước, tính sau, không phải retrograde toàn 1,14·10²⁰ thế (điều đó bất khả thi trên một máy — con số đó là tổng lý thuyết không sĩ/tượng, còn mẫu ván thật cho thấy phân bố ngược lại).

(a) Chia hạn ngạch theo khối lượng ván thật — khuyến nghị:

- Đừng đi theo kích thước vật lý của tablebase. Đi theo tần suất trong 400.000 thế ván thật đã mẫu: bucket các thế thật theo (số sĩ+tượng, số quân tấn công, chênh chất liệu). Mỗi bucket = 1 đơn vị hạn ngạch.
- Retagrade nội bộ ~9.900 thế/s ⇒ 1 ngày ≈ 8,5·10⁸ thế. Trên 30 ngày ≈ 2,5·10⁸ thế. Đây là giới hạn thật — nghĩa là chỉ đủ phủ những bucket có xác suất xuất hiện cao nhất (ví dụ top 20 bucket chiếm ~90% ván thật). Bucket ngoài phạm vi ⇒ để NNUE extrapolate, không sinh tablebase.
- Chiến lược 2 tầng: (i) DTM chính xác cho bucket nóng, (ii) với bucket ấm chỉ chạy tới khi DTM hội tụ một chiều sâu cố định (giống bịt depth-cut của Shirogane/Qianyi — tham khảo cách SDT thân thiện không sĩ/tượng, và cách các dự án cắt như "DTM ≥ N thì đánh dấu unknown").

(b) Target NNUE: WDL hay cp giảm đều theo DTM?

- WDL là trục chính, thêm kênh DTM bị chuẩn hoá. Lý do: trong thế cờ tàn cuộc xiangqi, "thắng/kéo/thua" phân biệt được dễ hơn giá trị số; DTM tuyệt đối (đơn vị ply 1.500–2.990) nếu đưa thẳng vào eval sẽ nén động không tuyến tính. Khuyến nghị: WDL 3 lớp + đầu phụ học ũ = clamp(DTM/500, 0..1), trộn loss (WDL CE + MSE DTM, trọng số DTM thấp). Trong search thì blend về cp bằng hàm "khẩn cấp theo ply còn lại" (kiểu win-rate model của Lc0/Koivisto: cp = f(wdl, plyToDraw), tham khảo phương pháp Koivisto "Improved One-Ply Vision" — hơi khác thể loại nhưng nguyên lý f(wdl→cp) áp được).
- Dải 1.500–2.990 ply: dùng DTM chuẩn hoá theo thế loạn (DTM − 1500) để tránh thang quá lớn.

(c) Nước đúng bên thua: DTM thuần hay luật 80 ply?

- Chuẩn hoá theo luật thực tế 80 ply có kéo dài (repetition, cầu tướng lại). Lý do: tablebase DTM thuần (không đếm lượt ăn) cho nước đúng sai tương đối khi bên thua có thể "đuối hơn 80 ply nhưng vẫn theo luật". Chuẩn: label = dài nhất/giai tối ưu dưới ràng buộc 80 ply (như DTZ của chess — dùng cho luật 50 nước), tức DTZ-analog 80 ply cho xiangqi. Chi phí xây hơi cao hơn DTM thuần nhưng nước trả về đúng luật. Nếu chỉ xây được DTM thuần thì khi tra cứu phải cộng bù con trỏ "ply từ lần ăn/đổi gần nhất" — lưu kèm thế.

Các nguồn tham chiếu (không phải mã đội): chessdb.cn (https://www.chessdb.cn/), giấy về tablebase xiangqi của Ping-Che Yen (o_ngcf/chessdb repo GitHub: https://github.com/pgh26/chessdb nếu cần cơ chế format), FelicityGen (đã có sẵn ở đội).

### 2. Mô hình kho datanoscore.db → data.db / positions.fendb → positionsfinal

Phản biện & rủi ro chính:

Bất biến "datanoscore ∩ data.db = ∅" sẽ bị vi phạm trong race. Nếu process analyze đọc từ datanoscore, chấm, rồi ghi vào data.db mà xóa dòng datanoscore trước khi commit data.db — crash giữa 2 bước ⇒ mất vkey (không còn ở đâu). Ngược lại (xóa sau khi commit) crash ⇒ trùng 2 nơi, nhưng an toàn vì idempotent (chấm lại được). Khuyến nghị: xóa khỏi datanoscore SAU KHI commit data.db, trong cùng một transaction SQLite qua ATTACH (2 DB trong cùng connection SQLite được giao dịch nguyên tố — đây là tính năng chuẩn của SQLite, xem https://www.sqlite.org/lang_attach.html và https://www.sqlite.org/atomiccommit.html).

Vấn đề 2 tệp nối bằng vkey — nên giữ, với 3 điều kiện:

- vkey là INTEGER 64 bit chuẩn hoá kiểu (đừng lặp lại bug bind_double đã gặp ở mục 3 — bind 2^53+ bằng double làm mất chính xác; dùng sqlite3_bind_int64, xem https://www.sqlite.org/c3ref/bind_blob.html).
- Đặt UNIQUE index trên vkey ở data.db và datanoscore để SQLite tự từ chối trùng (foreign key không cần, chỉ cần index).
- Mục đích của 2 tệp phải là "hàng đợi" + "kho chấm", như mô tả — hợp lý vì có thể dùng SQLite làm queue bằng SELECT ... ORDER BY rowid LIMIT k + UPDATE ... WHERE rowid=? AND processed=0 (so sánh và so sánh (compare-and-set) để chống đòi 2 lần — xem https://www.sqlite.org/isolation.html).

Kiểm hậu gộp positionsfinal:

- Trước rename, chạy SELECT COUNT(*) FROM (SELECT vkey FROM positions EXCEPT SELECT vkey FROM positionsfinal) để đếm thiếu, và ngược lại để đếm thừa.
- Bắt buộc: PRAGMA integrity_check + foreign_key_check trước, và backup file .db trước khi rename — đổi tên file không phải giao dịch (nó nằm ngoài SQLite).
- Lưu master record: hash SHA-256 của tệp trước khi gộp và sau khi gộp để truy vết.

Một rủi ro tôi chưa thấy được từ mô tả: nếu master (positions.fendb) có nhãn đánh giá "ít ưa thích" hoặc dữ liệu có thời gian — gộp có thể ghi đè bản ghi mới hơn. Nên có trường updated_at và quy tắc last-writer-wins rõ ràng.

### 3. Sửa kho toàn bảng 567.206.917 dòng (SQLite 30 GB)

Nguyên tắc an toàn: không sửa trực tiếp 30 GB trong một giao dịch lớn — SQLite phải giữ toàn bộ rollback/WAL trong thời gian dài, tăng rủi ro tràn disk và mất điện. Khuyến nghị: chạy hàng loạt theo từng khối (batch) (mỗi lần ~1–5 M dòng), mỗi khối một BEGIN/COMMIT riêng.

Quy trình:

1. Backup vật lý trước (copy file .db khi không ai ghi). Đây là lớp bảo vệ quan trọng nhất, không phụ thuộc SQLite.
2. PRAGMA journal_mode=WAL; PRAGMA synchronous=NORMAL; — xem https://www.sqlite.org/wal.html và https://www.sqlite.org/pragma.html.
3. Sửa hàng loạt theo con trỏ (cursor) the rowid: mỗi khối BEGIN; UPDATE ... WHERE rowid BETWEEN a AND b AND typeof(vkey)='real'; COMMIT; với sqlite3_bind_int64 cho giá trị đích. typeof() là hàm chuẩn của SQLite phân biệt REAL/INT ngay cả khi schema là NUMERIC/ANY (xem https://www.sqlite.org/datatype3.html và https://www.sqlite.org/lang_corefunc.html#typeof).
4. Bản ghi 37 cấm (7.830.204 dòng): đưa vào một bảng tạm banned_vkeys, sau đó DELETE FROM main WHERE vkey IN (SELECT vkey FROM banned_vkeys). Đánh index tạm trên vkey nếu chưa có — chi phí index trên 500 M dòng trên máy một người là đáng kể, cần ước lượng disk/disk-io.
5. Số dư sau sửa theo phép tính của đội: 567.206.917 − 92.495.146 (số lượng REAL) − 7.830.204 (bị cấm) = 466.881.567, cộng 64.795.168 dòng INT mà REAL trùng (không phải xóa vì đã tồn tại) — nhưng đội tính ra 494.581.545. Chênh 27.700.086. Cần đội đối chiếu lại vì tôi không thấy số nguồn để xác minh — có thể có thêm phân loại tôi không biết. (Không biết — cần đội giải trình.)
6. Nhận diện khoá sai còn sót (1,3 M dòng INTEGER với vkey = 0 / I64MIN / binade 2^55 âm): truy vấn quét WHERE vkey=0 OR vkey=-9223372036854775808 OR (vkey < -288230376151711744 AND vkey > -576460752303423488). Lưu ý khoảng binade 2^55 âm là [−2^55, −2^54). Nếu có -2^55 thì thêm vkey=-36028797018963968.
7. VACUUM cuối cùng — nhưng lưu ý: VACUUM cần disk gấp đôi + reset rowid (không giữ rowid ổn định nếu có chỗ nào dùng rowid làm con trỏ — dùng riêng cột id INTEGER PRIMARY KEY thay vì rowid để ổn định, xem https://www.sqlite.org/lang_createtable.html#rowid).
8. Xây dựng UNIQUE index trên vkey (đã chuẩn INT) — để không bao giờ tái diễn việc trùng. 30 GB + index sẽ tăng dung lượng, cần không gian trống.

### 4. CBL/CCBridge — khai thác ván sách cho train

Dữ liệu: 221 nội dung duy nhất từ 432 tệp ⇒ khâu hash nội dung (SHA-256 hoặc GCS nền độc lập) là đúng, bỏ trùng đầu tiên. Việc 94 «CCBridge cũ» v3 zlib chỉ có 652.553 nút (chiếm 0,15%) so với 127 CCBridgeLibrary 433 M nút — nên lọc bỏ hoặc đánh nhãn weight thấp, đỡ làm noise trong train.

Khuyến nghị khai thác:

- Policy học từ nước đi của người chơi trong các ván, nhưng cần lọc theo chất lượng ván trước. Đánh trọng số policy per-move bằng điểm elo/tên người chơi nếu CCBridgeLibrary có metadata. Nếu không có metadata, dùng giới hạn ≥ 15 ply và loại các ván quá ngắn/là người từ bỏ giữa ván.
- Điểm (value) học từ kết quả ván: chỉ với các ván kết thúc rõ thắng/thua (không phải cầu hòa vô lý). Trộn với ván tự đánh của engine (kho data.db đã có) — tỷ lệ ban đầu tôi khuyến nghị sách người:engine = 1:4 (sách chỉ để cung cấp diversity, tự chơi mới là nguồn value chính). (Đây là phỏng đoán, chưa đo được trên máy đội — đề nghị thí nghiệm nhỏ trước khi scale lên.)
- Định dạng giữ chú giải: tôi không biết chi tiết format PGN dựa trên tệp .cbl nội bộ của CCBridgeLibrary / CCBridge cũ. Khuyến nghị chung: xuất ra PGN chuẩn cờ tướng (FEN + comment {...} ở mỗi nước) — PGN giữ được chú giải per-move theo chuẩn PGN (comment trong ngoặc nhọn, xem spec PGN của Saremba/Edwards tại https://www.saremba.de/chessx/misc/pgn.htm — chuẩn gốc Steven J. Edwards). Nếu cần tốc độ đọc lớn, chuyển đổi sang format nhị phân riêng (vkey + move + weight) sau khi lọc — PGN chỉ làm tầng lưu trữ/tra cứu, không làm tầng train trực tiếp.
- Lọc sách cấm: dùng bảng banned_vkeys/hash của 37 book cấm từ mục 3 — lọc cả tác giả/tệp nguồn, không chỉ nội dung, vì 221 nội dung từ 432 tệp nghĩa là cùng nội dung có thể đi kèm metadata khác nhau.
- Nút 433 M: khai thác theo ván, không theo nút — mỗi ván cho chuỗi nước có ngữ cảnh; nút chỉ là đơn vị lưu trữ. Chọn ngẫu nhiên 1–3 vị trí mỗi ván để train policy (giảm tương quan giữa các vị trí cùng ván, một kỹ thuật chuẩn trong pipeline người-máy).

### 5. AUTO giả lập Android + đăng nhập nền tảng

Nói rõ trước: tôi không biết hầu hết chi tiết giao thức riêng của các nền tảng này (chúng không có tài liệu công khai, và tôi không có mã máy đội). Tôi chỉ có thể lập luận từ mô hình chung.

(a) Nhận dạng bàn/quân bền với skin/độ phân giải/giả lập:

- Cách bền nhất: phát hiện lưới bàn theo hình học, không theo pixel mẫu cứng. Tìm 2–4 đặc trưng bất biến: khung bàn (biên nét dài thẳng), đường dọc/ngang, điểm giao (90 điểm). Chuẩn hoá phối cảnh (homography) về bàn chuẩn → mọi skin/độ phân giải quy về cùng toạ độ logic. OpenCV findHomography/cv::perspectiveTransform là chuẩn công khai (https://docs.opencv.org/).
- Quân: học mẫu tại chỗ (máy đã làm) đúng hướng — mở rộng bằng: phân lớp theo màu/hình khối (tròn đục/tròn viền, chữ Hán) thay vì template thô; tự hiệu chuẩn: nếu tỉ lệ tin cậy mẫu dưới ngưỡng → yêu cầu hiệu chuẩn lại.
- Giữa LDPlayer/BlueStacks/MuMu/AVD khác nhau chủ yếu ở: driver GPU (render chữ), DPI, có root/nested-VD, cách chụp (ADB screencap vs. khung cửa sổ Windows). Tôi không biết ma trận tương thích cụ thể từng giả lập — đội phải đo (đúng luật 21/09).

(b) Đăng nhập từng nền tảng — biết/không biết:

- JJ象棋 (Unity IL2CPP + protobuf TCP/UDP): tôi không biết giao thức thật (không công khai, không có mã). Lập luận: kênh SĐT/QQ/WeChat/Douyin đều là OAuth/token của bên thứ ba — viết client giả lập được đòi hỏi token hợp lệ từ app thật; con đường khả thi nhất thường là điều khiển app thật trong giả lập (chính là thiết kế AUTO hiện tại) thay vì dựng client riêng. DLL/bundle mã hoá của Unity IL2CPP phải dump khi chạy (il2cppdumper/frida) — đội tự làm và tự chịu rủi ro pháp lý; tôi không hướng dẫn bỏ qua bảo mật.
- MoveSky/弈天 CCMS 1.96: H1–H4 tôi không biết nội dung; để trả lời chính xác cần phiên thật có log (bắt gói + log token) — bắt phiên thật rẻ hơn và hợp pháp hơn so với dịch mã máy. Về relay 127.0.0.1 có bị chặn "không cho đăng nhập qua proxy": nguyên tắc chung — nếu server kiểm tra IP nguồn của phiên đăng nhập với IP của phòng chat, relay cục bộ (loopback) không đổi IP công cộng nên thường không bị chặn; chặn xảy ra khi relay đặt ở máy khác/VPN. Đây là lập luận, chưa có số đo.
- clubxiangqi.com + zigavn (web): nếu là web thuần → nhúng WebView2 là con đường ít rủi ro nhất (dùng cookie phiên thật, không dựng giao thức). Tôi không biết nó có WebSocket riêng hay không — cần đội mở DevTools một lần để xác nhận.
- ZingPlay (Zing ID), Kỳ Vương (PlayIntegrity + DexGuard), vndynapp (REST): Kỳ Vương có Play Integrity ⇒ mọi client tự dựng sẽ bị đào thải; chỉ còn đường điều khiển app thật. vndynapp REST thuần là dễ nhất (nếu đội đã có endpoint + token — tôi không biết chi tiết).

(c) nick qqmovesky/ccmsexe/cmllh, "các bạn hiện tại" là 对阵表 hay danh sách bạn: tôi không biết — các định danh này nằm trong dữ liệu riêng của đội, không có trong tài liệu công khai tôi truy cập được. Cần đội cung cấp 1 bản chụp/log phiên thật, tôi mới lập luận được tiếp.

### 6. TieuLongNu so với BHGui/SharkGui

Tôi không có tài liệu chức năng đầy đủ của BHGui/SharkGui trong tay, nên chỉ nêu các nhóm chức năng thường có trong GUI cờ tướng chuyên dụng mà danh sách 9 nhóm của đội chưa thấy:

- Đổi nước khi xem kỳ phổ (branch/variation tree, không chỉ đọc tuyến chính);
- Đồng hồ thi đấu + cờ lùi/làm lại có timer;
- Ký sự ván (move list) xuất PGN/đàn pháo đa tuyến + hash ván;
- Cửa sổ phân tích song song 2+ engine (đối chiếu đánh giá giữa engine chính/teacher);
- Quản lý thư viện ván (database view) — lọc, tìm kiếm theo FEN, gắn tag;
- Điều khiển engine từ xa (client–server) nếu AUTO đã tách process;
- Book editor (sửa/nhập/xuất book, nối mục 7).

Mã mẫu WPF tôi không viết đủ trong câu này nếu bạn cần — trả lời "mã mẫu" ở lượt sau; bố trí thanh auto gọn: 8–10 nút chia 2 hàng 5 nút, trái dock dọc, nhóm [Kết nối][AUTO][Stop][Phân tích][Kỳ phổ][Cài đặt] — hàng chính cho thao tác nóng, hàng phụ cho cài đặt, gọn nhất khi dùng ToolBar với OverflowMode=Never (chuẩn WPF, tài liệu tại https://learn.microsoft.com/dotnet/api/system.windows.controls.toolbar).

### 7. Encrypt book (V-41) + update khách không đè config

Thiết kế mã hoá book theo máy (không phải mật mã học tối ưu, chỉ mức "chống sao chép ngây thơ"):

- Khoá theo máy: fingerprint phần cứng (đĩa/UUID bo mạch — team tự chọn ổn định) → SHA-256 → AES-256-GCM cho phần book (chuẩn https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf và AES-GCM https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38D.pdf).
- 3 loại key (hết hạn / ngưng vĩnh viễn / hạn cập nhật): gói trong JWT tự ký HS256/RS256 với exp (hết hạn), revoked (ngưng — cần danh sách đen trên server khi mở máy), notAfter (hạn cập nhật) — tham chiếu https://datatracker.ietf.org/doc/html/rfc7519.
- Chống dump từ RAM: đối lập thực tế — nếu engine là máy 1 người tự dùng, chỉ có mức khoá book khi không dùng (giải mã vào RAM, dùng xong xóa SecureZeroMemory), không có cách chống dump 100%. Sẽ không bao giờ chống được người có quyền máy. Cần nắm rõ giới hạn này trước khi đầu tư thêm.
- Quy trình update không đè config + ca đỏ hash: gói update không chứa 5 tệp config, updater so sánh hash: nếu tệp config thiếu → viết mẫu mặc định; nếu có → bỏ qua tuyệt đối. Ca đỏ: trước khi ghi bất kỳ tệp nào, hash tệp hiện tại và lưu vào update.log — nếu hash khác với bản client kỳ vọng, dừng và yêu cầu xác nhận. Tài liệu tham chiếu quy trình ghi an toàn: https://learn.microsoft.com/windows/win32/fileio/file-transactioning (Windows transactional NTFS đã ngừng phát triển — thay bằng quy trình ghi tạm + rename nguyên tố).

### 8. Kiểm toán engine trước train (mốc 01/10) — cắt rig 60–100 h

Thứ tự ưu tiên soi, ngắn nhất để kịp 01/10:

1. Luật sinh nước (movegen) — soi trước, vì bug ở đây phá mọi test sau. Test bằng: với mỗi thế trong một tập mẫu, so số nước hợp lệ với một tham chiếu độc lập (Pikafish/Pawnocchi nếu đội có). Tôi không biết đội có bộ tham chiếu nào, nhưng đây là bài test rẻ (vài giờ).
2. Khoá băm + phân biệt nhánh (key/lock) — bug ở đây làm search tắt ngẫu nhiên, rất khó phát hiện nếu không soi. Cách rẻ: chơi 200 ván tự đấu và so kết quả thay đổi ngẫu nhiên khi đổi zobrist key — nếu không đổi chứng tỏ khoá trùng. (Lưu ý: tôi đoán — đội đo xác nhận.)
3. Đồng hồ + tìm kiếm nước (time management) — chỉ cần không timeout, không cạn nước; test 100 ván.
4. Eval-net — nhờ train mốc sau; chỉ cần không NaN/inf, cần giới hạn output.
5. Search nâng cao (TT, LMR, cắt…) — so với baseline Pikafish 5 bậc là rộng nhất, nên để cuối và cắt scope: thay vì rig 60–100 h, chạy SPRT cờ 2×2 ván × 60 thế → đủ dừng sớm (SPRT chuẩn, https://en.wikipedia.org/wiki/Sequential_probability_ratio_test); nếu đội đã có thang Pikafish 5 bậc, dùng đúng thang đó, không đặt thêm.

Cắt rig xuống kịp 01/10 — nguyên tắc: mỗi mục chỉ một test then chốt, không thử vạn ván. Ván ngắn + nhiều lặp nhỏ > ván dài ít lặp. Đội tự đo, đúng luật.

### 9. App điện thoại Kỳ Viện — kiến trúc tối thiểu

Học đúng điểm mạnh JJ: MVVM, 3 lối vào ván (MỜI / PHÒNG MÃ / XEM), đăng nhập tách kênh, module 实名/chống nghiện tách riêng. Kiến trúc tôi đề xuất cho app cờ di động phát hành tối thiểu:

- Tầng Android thuần (Kotlin, không Unity): 1 activity chính + fragment per màn; MVVM bằng ViewModel + StateFlow (tài liệu https://developer.android.com/topic/architecture), không cần thư viện phức tạp.
- 3 lối vào ván như JJ — đúng 3 luồng (mời/room-code/xem) là đủ; không thêm chế độ trước khi phát hành.
- Đăng nhập tách kênh: module auth riêng, mỗi kênh 1 adapter (SĐT / tài khoản nội bộ). Không nhúng OAuth WeChat/QQ/Douyin ngay đầu — cần tài khoản doanh nghiệp; để sau.
- Thực danh/chống nghiện: module riêng theo mô hình Trung Quốc (đã học từ JJ) — tách để dễ thay đổi theo từng thị trường.
- Tối thiểu để phát hành: 1 màn chính (bàn + đồng hồ), 1 màn chat + đề nghị hoà, 1 màn danh sách bạn (nếu có), 1 màn cài đặt, auth 1 kênh. Cắt mọi thứ khác.
- Tích hợp AUTO/phân tích: AUTO chạy trên server (engine là EXE Windows hiện tại — sẽ phải dịch hoặc chuyển sang Linux + gRPC); app chỉ gửi vị trí + nhận nước. Tôi không biết có cần thời gian bao lâu để dịch engine sang Android/Linux — đội phải đo bằng build thử.
- Không bắt chước Unity IL2CPP + hot-update — khẳng định của đội đúng: app web/di động dùng Unity chỉ để renders 2D là quá nặng; nếu cần hot-update config thì dùng Remote Config đơn giản (JSON trên CDN), không cần hot-update code.

### Tổng kết những điểm tôi không biết (đúng luật "không biết thì nói không biết")

| Mục | Không biết cụ thể |
|---|---|
| 4 | Format .cbl nội bộ chi tiết, chú giải trong tệp có giữ chú giải hay không; chất lượng Elo người chơi trong CCBridgeLibrary |
| 5 | Giao thức JJ象棋 thật (protobuf/endpoint), H1–H4 MoveSky, ý nghĩa các nick qqmovesky/ccmsexe/cmllh, 对阵表 hay danh sách bạn, ma trận tương thích LDPlayer/BlueStacks/MuMu/AVD |
| 6 | Danh sách chức năng chính xác của BHGui/SharkGui (không có tài liệu đầy đủ trong tay) |
| 7 | Mức chống dump thực tế trong môi trường Windows khách — chỉ nêu được giới hạn lý thuyết |
| 8 | Chi tiết 20 ô CHƯA ĐO / 13 KHÔNG QUA trong bảng KIEM_TOAN §3 (tôi không có bảng này) |
| 9 | Thời gian build/dịch engine sang Linux/Android của đội |
| 3 | Chênh 27.700.086 trong phép tính số dòng sau sửa — cần đội đối chiếu nguồn |

### Khuyến nghị xếp thứ tự ưu tiên cho buổi hỏi thứ Hai 29/09

1. Mục 3 (sửa kho) — mối rủi ro mất dữ liệu cao nhất, và có chênh số cần đối chiếu trước khi sửa. Duy nhất mục có thể mất dữ liệu vĩnh viễn nếu sai.
2. Mục 2 (mô hình kho) — gắn chặt với mục 3, quyết định schema trước khi sửa.
3. Mục 1 (tàn cuộc) — quyết định target NNUE (WDL+DTM chuẩn hoá, luật 80 ply) trước khi sinh tablebase, vì quyết định này rẻ thay đổi lúc này và đắt thay đổi sau.
4. Mục 8 (kiểm toán) — mốc cứng 01/10, chỉ cần scope cắt như trên.
5. Mục 5 + H1–H4 MoveSky — chờ 1 bản log phiên thật từ đội; mọi lập luận của tôi sẽ khác đi khi có log. Không nên đào giao thức thêm trước khi có dữ liệu thật.
6. Mục 4, 6, 7, 9 — các mục thiết kế/khai thác, có thể chờ vòng hỏi sau sau khi 1–5 xong.

(Hết ASK 01 — Ox Alpha. Đội tự đo mọi số liệu theo luật 21/09.)

---

*Ghi chú của đội (Fable, 27/09):* chênh 27.700.086 mà Ox Alpha hỏi ở mục 3 chính là **27.699.978 dòng REAL được ÉP về INT64** (không xoá, không trùng) trong phiếu sửa kho — 92.495.146 REAL = 64.795.168 trùng (xoá) + 27.699.978 ép (giữ); đội đã chạy ②③ ngày 27/09 ra đúng 494.581.545 (oracle độc lập thiếu/thừa/lệch 0). Chênh lệch 108 dòng còn lại (27.700.086 − 27.699.978) là do Ox Alpha tính từ số tròn trong Ask.
