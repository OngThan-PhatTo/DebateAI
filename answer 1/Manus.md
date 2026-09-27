# Manus — trả lời Ask 1 (2026-09-26)

> Nguồn: owner gửi tệp .docx 27/09; đội chuyển sang markdown bằng python-docx (đoạn, tiêu đề, bảng), không sửa nội dung.

## Manus — trả lời Ask 1 (2026-09-26)
Phạm vi: tôi chỉ có nội dung công khai trong ask 1/ASK_2026-09-26.md, không có source.zip, cơ sở dữ liệu hay máy đo của đội. Vì vậy tôi không gán file:dòng cho mã chưa được đăng; các đề xuất cần đo được đánh dấu GIẢ THUYẾT CẦN ĐO.

### 0.1 Tàn cuộc bậc 2 trở lên
Trả lời: Không nên cố liệt kê toàn bộ không gian 3–4 quân. Chia theo cấu hình vật chất, đối xứng hợp lệ và lấy mẫu phân tầng. Hạn ngạch nên là:
giữ trọn các ô nhỏ;
với ô lớn, quota = Qmin + C * sqrt(N_real_cell) hoặc log1p(N_cell);
nhân thêm hệ số theo tần suất ô trong ván thật, nhưng chặn trần để một ô phổ biến không nuốt toàn bộ kho;
luôn có 10–20% mẫu đối chứng lấy đều trong ô và 10–20% mẫu trích từ ván thật.

Mẫu phản chiếu/đổi màu chỉ được gộp khi phép biến đổi bảo toàn luật, bên đi, quyền nhập thành và quy ước tọa độ; phải lưu canonical_key và transform_id, không chỉ xóa bản sao.

Với nhãn: lưu riêng wdl_rule80, dtm_ply, dtc_ply, wdl_theoretical, distance_unit. Không nén mọi mate thành một cp. Tôi chọn đầu WDL là mục tiêu chính, còn khoảng cách DTM là đầu phụ hoặc loss phụ có trọng số giảm dần. Công thức an toàn ban đầu:
y_dtm = sign * tanh((3000 - dtm_ply) / 600) với dtm_ply hữu hạn; thế thắng lý thuyết vượt mốc 80 ply phải có nhãn luật riêng, không tự gán thành thắng.

«Nước đúng» của chế độ engine đấu engine phải tối ưu theo luật 80 ply; kho nghiên cứu có thể lưu DTM lý thuyết nhưng không được dùng nó để tuyên bố thắng theo luật.
Nguồn: Câu hỏi đã chốt các số liệu và luật tại ask 1/ASK_2026-09-26.md:5-7, :14-17; tài liệu NNUE chính thức mô tả WDL-space, loss sigmoid và việc chuyển evaluation từ CP sang WDL: https://official-stockfish.github.io/docs/nnue-pytorch-wiki/docs/nnue.html (đã kiểm tra 2026-09-26).
Mức tin: CHẮC cho nguyên tắc tách WDL/DTM và không trộn ply với move; GIẢ THUYẾT CẦN ĐO cho các hệ số 10–20%, 600 và công thức quota.
Ca kiểm: tập thắng đã giải, tính Spearman giữa -dtm_ply và output net; báo riêng nhóm dtm_ply > 80, kiểm wdl_rule80 không bị dự đoán thắng quá ngưỡng. So sánh loss WDL-only với WDL+DTM trên seed cố định; không dùng book.

### 0.2 Mô hình kho
Trả lời: Mô hình hai hàng đợi là hợp lý nhưng bất biến phải được enforced bằng khóa duy nhất chung: data.db(vkey, vmove) và datanoscore.db(vkey, vmove) nên có cùng biểu diễn canonical, không dùng REAL. Đừng dựa chỉ vào hai file để bảo đảm giao nhau rỗng: khi crash hoặc copy dở, hai file có thể cùng chứa một khóa.

Cách an toàn hơn là một manifest/ledger nhỏ ghi (batch_id, source_hash, first_key, last_key, rows_read, rows_written, committed_at), và trong lúc chuyển lô dùng ATTACH, BEGIN IMMEDIATE, insert có ON CONFLICT, rồi ghi ledger trong cùng giao dịch. Nếu bắt buộc giữ hai file tách rời, job reconcile phải kiểm EXCEPT hai chiều theo khóa và chỉ đánh dấu lô hoàn tất sau khi cả hai phía đã commit.

Hậu gộp: PRAGMA integrity_check, quick_check, count theo batch, count distinct khóa, kiểm min/max/hash mẫu và kiểm đối chứng ngẫu nhiên bằng truy vấn độc lập. Giữ source_batch_id và vkey lâu dài; chỉ gộp vật lý sau khi đã có snapshot kiểm chứng.
Nguồn: Bối cảnh kho và bất biến được nêu tại ask 1/ASK_2026-09-26.md:8; SQLite xác nhận WAL chỉ có một writer và reader/writer đồng thời trên cùng host: https://www.sqlite.org/wal.html (đã kiểm tra 2026-09-26).
Mức tin: CHẮC cho canonical integer key, ledger, một writer và hậu kiểm; GIẢ THUYẾT CẦN ĐO cho lựa chọn tách file so với một DB.
Ca kiểm: cố ý kill process ở mỗi điểm commit của 100 lô; sau restart, chạy reconcile và yêu cầu: không giao nhau, số lô hoàn tất đúng, không duplicate (vkey,vmove), quick_check=ok.

### 0.3 Sửa kho toàn bảng
Trả lời: Không sửa tại chỗ bằng một giao dịch 30 GB duy nhất. Tạo bảng đích mới theo từng chunk có checkpoint; hoặc tạo bảng mapping rowid -> canonical_vkey, đổ từng lô vào bảng mới với UNIQUE(canonical_vkey, vmove), sau đó đổi tên trong một giao dịch ngắn. Không dùng CAST(REAL AS INTEGER) mù quáng: IEEE-754 đã có thể làm mất bit trước khi SQLite nhận giá trị.

Quy trình:
đọc từng batch theo rowid, lấy raw type bằng typeof(vkey);
loại/đưa quarantine mọi NULL, vkey=0, I64MIN, giá trị ngoài miền canonical và REAL không biểu diễn chính xác;
với REAL, chỉ chấp nhận nếu isfinite, floor(x)==x, abs(x)<=2^53 và chuyển qua decimal/integer kiểm soát;
insert vào bảng mới, ghi old_rowid, old_type, reason, batch_id vào audit;
mỗi batch commit, fsync/backup theo chính sách, có resume key;
cuối cùng so sánh tổng: input = output + duplicate + book_cấm + quarantine.

«~1,3 M khóa INTEGER sai» không thể nhận diện đầy đủ chỉ bằng vkey=0; phải có validator theo miền mã hóa, phân bố bit, đối chiếu FEN/position nếu có và mẫu giải mã ngược. Không xóa bản gốc trước khi snapshot và audit được xác nhận.
Nguồn: Số liệu lỗi nằm tại ask 1/ASK_2026-09-26.md:9; SQLite mô tả checkpoint và giới hạn chỉ một writer tại https://www.sqlite.org/wal.html (đã kiểm tra 2026-09-26).
Mức tin: CHẮC cho chiến lược rebuild + audit; KHÔNG BIẾT miền mã hóa vkey cụ thể nên không thể cho predicate bit chính xác.
Ca kiểm: tạo DB nhỏ chứa INT, REAL nguyên, REAL mất chính xác, 0, I64MIN, số âm biên và duplicate; chạy importer, đặt trước expected counts từng nhóm và kiểm DB mới bằng integrity_check.

### 0.4 CBL/CCBridge
Trả lời: Tách ba mục tiêu: policy (nước đã chọn), value (kết quả/chất lượng), và metadata/chú giải. Parse vào schema trung gian bất kể .cbl là v3 zlib hay CCBridgeLibrary; lưu game_id, ply, position_key, move, result, comment, variation, source_hash, license và format_version. Không train trực tiếp từ số lần xuất hiện trong sách.

Lọc trùng theo hash chuẩn hóa ván (bỏ tên người chơi/thời gian nếu mục tiêu là nhận diện bản sao), nhưng vẫn giữ source_hash và bản ghi nguồn. Sách cấm phải được lọc trước parse bằng hash tệp; sau parse phải có cổng forbidden_rows == 0. Policy có thể lấy nước thực tế với trọng số giảm theo độ sâu; value dùng kết quả/teacher sau khi kiểm legality; chú giải và biến phụ lưu riêng, không làm hỏng phần train.
Nguồn: Số liệu tệp do câu hỏi cung cấp tại ask 1/ASK_2026-09-26.md:10. Tôi không biết một đặc tả công khai đủ tin cậy cho cả hai biến thể CCBridge được nêu; không nên viện dẫn các trang mô tả .cbl chung chung như đặc tả.
Mức tin: CHẮC cho schema trung gian, hash, lọc hai tầng; KHÔNG BIẾT chi tiết byte layout nếu không có mẫu tệp/reader của đội.
Ca kiểm: bộ fixture 1 tệp mỗi định dạng, 1 ván có chú giải/variation, 1 duplicate và 1 sách cấm; expected: parse không mất chú giải, duplicate đúng số, forbidden output bằng 0.

### 0.5 AUTO giả lập + đăng nhập nền tảng
Trả lời: Tách adapter theo nền tảng và ưu tiên API/UI được nền tảng cho phép; không suy đoán giao thức, token, DLL, protobuf hay cơ chế chống proxy khi chưa có tài liệu/phiên được ủy quyền. Với nhận bàn: dùng pipeline window capture -> tìm vùng bàn -> chuẩn hóa phối cảnh -> phân loại ô -> kiểm trạng thái -> chỉ gửi nước khi confidence đủ. Template matching phù hợp làm baseline khi khách bấm học mẫu; OpenCV mô tả matchTemplate là trượt template trên ảnh và có ngưỡng cho nhiều vật thể. Nhưng template đơn độc dễ vỡ trước scale, highlight, animation và anti-aliasing; thêm đặc trưng màu/chữ hoặc CNN nhỏ chỉ sau khi baseline đo không đạt.

Hai điểm xe chỉ đủ nếu giả định bàn là hình bình hành affine và hai điểm đã biết chính xác các góc; không đủ để ước lượng phối cảnh tổng quát. Nên yêu cầu điểm thứ ba hoặc bốn góc, rồi dùng homography. Không suy ra lượt đi chỉ từ một frame nếu không có chỉ báo đáng tin; giữ trạng thái cũ, chờ frame ổn định và xác nhận nước hợp lệ/đổi đồng hồ.

Với Android, AccessibilityService là cơ chế hệ thống yêu cầu người dùng bật rõ ràng và tài liệu Android nói nó dành cho hỗ trợ người khuyết tật; không coi đó là đường mặc định để điều khiển app chơi cờ. Với app web, dùng API/DOM hoặc integration được chủ trang cho phép; với desktop, capture/UIA chỉ là fallback. Tôi không biết cách đăng nhập hợp pháp, schema giao thức hay H1–H4 của JJ/MoveSky từ repo này; không được bịa câu trả lời.
Nguồn: Câu hỏi và phạm vi nền tảng tại ask 1/ASK_2026-09-26.md:11; OpenCV template matching: https://docs.opencv.org/4.13.0/d4/dc6/tutorial_py_template_matching.html (đã kiểm tra 2026-09-26); Android AccessibilityService nêu rõ user bật dịch vụ và mục đích trợ năng: https://developer.android.com/reference/android/accessibilityservice/AccessibilityService (đã kiểm tra 2026-09-26); Windows Graphics Capture cung cấp capture cửa sổ/màn hình qua system picker: https://learn.microsoft.com/en-us/windows/apps/develop/media-authoring-processing/screen-capture (đã kiểm tra 2026-09-26).
Mức tin: CHẮC cho pipeline, homography, fail-closed và giới hạn nguồn; KHÔNG BIẾT chi tiết từng nền tảng.
Ca kiểm: 200 screenshot gồm scale 75/100/125/150%, 4 skin, highlight, animation, cửa sổ đổi cỡ; ngưỡng trước: không gửi nước nếu board confidence hoặc legality confidence dưới ngưỡng; báo confusion matrix và false-move rate, không chỉ accuracy quân.

### 0.6 Giao diện TieuLongNu
Trả lời: Tối thiểu cho chế độ thực chiến: chọn hồ sơ, bắt bàn, start/stop, full/one, engine, đi ngay, đổi bên, book on/off, lưu/nạp. Các mục nghiên cứu, kỳ phổ, batch train, EGTB và chỉnh sâu ẩn trong mode Research, không xóa khỏi lõi. Danh sách nút phải là dữ liệu cấu hình; command, menu, shortcut, context menu và toolbar cùng lấy từ một registry để không có đường tắt lộ chức năng đã ẩn.

Kiểm UI: duyệt cây command khả dụng trong Fighting mode, kiểm cả keyboard gesture và context menu; mọi command ngoài allowlist phải unreachable và bị test đỏ khi gọi trực tiếp.
Nguồn: Bối cảnh chức năng và câu hỏi tại ask 1/ASK_2026-09-26.md:12; không có source WPF hoặc tài liệu công khai của BHGui/SharkGui trong repo để khẳng định danh sách của họ.
Mức tin: GIẢ THUYẾT CẦN ĐO cho số nút tối ưu; CHẮC cho nguyên tắc một command registry và test reachability.
Ca kiểm: snapshot command tree ở hai mode; Fighting chỉ chứa allowlist 9 mục; thử gọi từng command ẩn qua phím tắt, chuột phải, automation và API đều bị từ chối.

### 0.7 Encrypt book + update khách
Trả lời: Mã hóa book là bảo vệ bản quyền khi lưu/truyền, không thể bảo đảm chống dump sau khi plaintext đã vào RAM. Dùng AEAD chuẩn (AES-GCM hoặc ChaCha20-Poly1305), mỗi book có random nonce, version, key-id, hash nội dung và chữ ký Ed25519/RSA-PSS của manifest. Khóa máy nên là một yếu tố trong KDF/keystore, nhưng phải có quy trình đổi máy/thu hồi; không tự phát minh crypto.

Tách config/ khỏi payload update. Installer ghi vào staging, xác minh chữ ký/hash, migrate schema, rồi atomic rename; chỉ thay file thuộc allowlist. Trước và sau update tạo manifest SHA-256 của 5 tệp cấm đè; ca đỏ sửa cố ý một byte và đặt sai chữ ký phải fail trước khi activate. Chấp nhận rằng anti-dump mạnh cần OS keystore/TEE và vẫn không chống được người có quyền debug trên máy chạy.
Nguồn: Câu hỏi nêu V-41, ba loại key và năm file config tại ask 1/ASK_2026-09-26.md:13; tôi không có đặc tả key/license nên không thể xác nhận mô hình hiện tại.
Mức tin: CHẮC cho AEAD + signed manifest + atomic update; GIẢ THUYẾT CẦN ĐO cho khóa theo máy và mức chống dump.
Ca kiểm: update interrupted ở từng bước; yêu cầu rollback sạch, config hash không đổi, payload giả chữ ký/nonce/version sai đều không được activate.

### 0.8 Kiểm toán engine trước train
Trả lời: Thứ tự cổng nên là: (1) luật/movegen/perft và FEN round-trip; (2) đồng hồ/timeout và stop; (3) hash/repetition/TT với đối chứng tắt hash; (4) mate-distance và luật 80 ply; (5) EGTB probe với hit/miss/illegal; (6) eval/net load và determinism; (7) self-play A=A; (8) A/B dương. Không chạy train trước khi các cổng sai dữ liệu/nước đi đều xanh.

Cắt rig: chạy smoke deterministic trên mọi commit; test sâu phân tầng theo rủi ro trên 1–2 build; chỉ chạy 60–100 h sau khi smoke, perft và đối chứng qua. Mọi benchmark phải NoBook, hash seed/config ghi lại, có đối chứng A=A với KTC95 chứa 0 và một đối chứng dương. Không dùng con số ưu thế ≥400 hay bậc Pikafish làm bằng chứng correctness; đó là cổng strength riêng.
Nguồn: Quy tắc đo NoBook, A=A và cổng dương tại ask 1/ASK_2026-09-26.md:14-17; phần “sự thật đã chốt” về NoBook và 80 ply tại ask 1/ASK_2026-09-26.md:315-323.
Mức tin: CHẮC cho thứ tự kiểm và kỷ luật đối chứng; KHÔNG BIẾT ô KIEM_TOAN cụ thể vì bảng/mã không nằm trong repo.
Ca kiểm: test suite phải có ít nhất một ca đỏ thật cho mỗi cổng (sửa tạm một điều kiện rồi phải fail), ghi exit code, build hash, PASS/FAIL và log ngắn; cổng “skip” không tính PASS.

### 0.9 App điện thoại Kỳ Viện
Trả lời: Kiến trúc tối thiểu: MVVM + AuthService tách khỏi GameService; ba entry point Invite, RoomCode, Watch; server giữ authoritative state và luật, client chỉ render/input; SignalR/WebSocket cho sự kiện ván; reconnect bằng game_id + revision và idempotent command. Module实名/chống nghiện và policy phải tách khỏi engine/game domain.

MVP phát hành: đăng nhập, tạo/tham gia phòng, xem bàn, gửi nước hợp lệ, reconnect, resign/draw theo luật sản phẩm, báo lỗi, telemetry tối thiểu, privacy/terms và test server chống nước giả. AUTO/phân tích nên là module opt-in gọi engine service, không đặt engine quyền cao vào client và không để nó can thiệp server authority.

Không nên bắt chước Unity IL2CPP/hot-update chỉ vì JJ dùng nó; chọn native/MAUI/WebView tùy yêu cầu offline, hiệu năng và store. Tôi không biết chính sách phát hành cụ thể của các nền tảng nêu trong câu hỏi nên không khẳng định phần compliance ngoài nguyên tắc trên.
Nguồn: Câu hỏi mô tả ba lối vào và tách module tại ask 1/ASK_2026-09-26.md:15; không có source Kỳ Viện trong repo để trích Program.cs hay ChessHub.cs.
Mức tin: CHẮC cho server-authoritative, idempotency, reconnect và tách domain; GIẢ THUYẾT CẦN ĐO cho lựa chọn MAUI/WebView/native.
Ca kiểm: mất mạng giữa command và ack, gửi lại cùng command_id, đổi revision và reconnect; expected: không đi trùng, server chỉ chấp nhận nước hợp lệ, client khôi phục đúng bàn.

### ASK04-B / ASK03 / ASK02
Trả lời: Repo công khai chỉ có phần tóm tắt câu hỏi; không có source.zip, code, diff, fixture, số đo hay URL đầy đủ cho các mã A3/A2. Do luật của debate cấm bịa tệp:dòng, lỗi cụ thể, HTTP status hoặc hành vi của BHGui/SharkGui, tôi không thể trả lời trung thực từng mã ASK03/ASK02. Đây là không đủ dữ liệu, không phải kết luận rằng các câu đó không có lời giải.
Nguồn: ask 1/ASK_2026-09-26.md:66-80, :87-117, :301-330 ghi rõ source.zip được gửi riêng và yêu cầu không bịa nguồn/số đo.
Mức tin: CHẮC về giới hạn dữ liệu của lượt này.

## Phần bổ sung: trả lời các mã ASK03/ASK02/ASK04-B
Các mã dưới đây được trả lời ở mức thiết kế và phản biện từ phần câu hỏi công khai. Những câu yêu cầu đọc mã nguồn, tệp nhị phân, license riêng hoặc số đo máy đội được ghi KHÔNG ĐỦ DỮ LIỆU; không suy ra lỗi cụ thể khi đoạn code đã bị lược.

### A3 — Training, engine, dữ liệu và AUTO
#### A3-01
Trả lời: Chuẩn hóa tất cả về dtm_ply ngay khi nhập; không dùng chung decoder cho UCI mate N và giá trị đã là ply. Giữ WDL luật làm đầu bắt buộc, dùng z_dtm = sign*tanh((dtm_ply-dtm0)/tau) làm đầu phụ hoặc trọng số, không dùng 30000-N lẫn 30000-ply. Cache phải chứa distance_unit.
Nguồn: ask 1/ASK_2026-09-26.md:151-156; NNUE docs có phần chuyển CP sang WDL và loss: https://official-stockfish.github.io/docs/nnue-pytorch-wiki/docs/nnue.html.
Mức tin: CHẮC về lỗi đơn vị; GIẢ THUYẾT CẦN ĐO về dtm0/tau. Không đủ source để chỉ thêm vị trí sai khác trong trainer.

#### A3-02
Trả lời: Không lọc nước ăn quân ở kho tàn cuộc; đó thường là nước chuyển hóa quan trọng. Nếu muốn loại thế không ổn định, phải có movegen và kiểm tra chiếu, capture, repetition/80-ply, không dùng heuristic “ăn quân là xấu”. Với sigmoid, đưa mate ra đầu WDL/DTM riêng; không đẩy 29.9xx qua in_scaling=1000 vì sẽ bão hòa gradient.
Nguồn: ask 1/ASK_2026-09-26.md:158-162; NNUE docs nói rõ sigmoid/WDL-space.
Mức tin: CHẮC về tách đầu và không lọc capture; KHÔNG ĐỦ DỮ LIỆU để chỉ lỗi dòng khác.

#### A3-03
Trả lời: Với value-only, lưu nhãn cho từng thế con và khi train dùng ranking giữa các nước hợp lệ: thắng chọn min(dtm) bên thắng, max(dtm) bên thua, đồng thời mask nước không hợp lệ. Lưu wdl_theoretical, wdl_rule80, dtm_ply, dtc_ply; thế “cursed win/blessed loss” phải giữ trạng thái thứ ba hoặc nhãn mềm, không âm thầm đổi thành thắng. Tôi không biết một trainer cờ tướng công khai chuẩn hóa dtc giống yêu cầu này.
Nguồn: ask 1/ASK_2026-09-26.md:163-165; NNUE docs: https://official-stockfish.github.io/docs/nnue-pytorch-wiki/docs/nnue.html.
Mức tin: CHẮC cho biểu diễn hai WDL; KHÔNG BIẾT về trainer cờ tướng cụ thể.

#### A3-04
Trả lời: Chọn quota lai: floor tối thiểu cho 527 ô, phần còn lại theo sqrt(N_real) và tần suất ván thật, giữ tập đều trong ô làm calibration. Không coi hòa là “trọng số nhỏ” mặc định vì sẽ làm net sai biên luật; cân bằng W/D/L bằng sampler riêng. Bắt đầu tàn cuộc ở 5–10% batch, tăng từng bậc chỉ khi test trung cuộc không giảm quá ngưỡng đã đặt; dừng sớm khi A=A lệch hoặc holdout trung cuộc tụt.
Nguồn: ask 1/ASK_2026-09-26.md:167-170.
Mức tin: GIẢ THUYẾT CẦN ĐO cho phần trăm; CHẮC về phân tầng và holdout.

#### A3-05
Trả lời: Không được bỏ EGTB chỉ vì dtc > remain; phải chuyển nó thành kết quả theo luật (thường là draw/cursed win tùy bên và bộ đếm), giữ provenance tb_result và rule_result. TT phải encode/decode mate score đối xứng với ply, bound đúng, và key phải bao gồm toàn bộ trạng thái ảnh hưởng repetition/bộ đếm; chỉ có Zobrist bàn cờ là chưa đủ cho lịch sử lặp. Không đủ mã để kết luận search.cpp:1393-1426 sai cụ thể hay Felicity đã chặn perpetual chase.
Nguồn: ask 1/ASK_2026-09-26.md:172-178; SQLite/NNUE không thay thế được kiểm luật engine.
Mức tin: CHẮC về nguyên tắc; KHÔNG ĐỦ DỮ LIỆU về hunk cụ thể.

#### A3-06
Trả lời: Hook probe ở lớp position/search duy nhất, sau legality và trước move ordering/eval terminal; probe phải có hit/miss/illegal, không thay thế repetition checker. Tắt EGTB phải cho bench/PV y hệt binary baseline. Đừng tuyên bố Elo từ EGTB nếu chưa có A/B NoBook; lợi ích chắc chắn trước hết là đúng nước ở tàn cuộc, còn Elo phụ thuộc phân bố ván.
Nguồn: ask 1/ASK_2026-09-26.md:180-182.
Mức tin: CHẮC về thiết kế cổng; KHÔNG BIẾT API Felicity .fexq cụ thể và không có bằng chứng công khai đủ cho Elo.

#### A3-07
Trả lời: Nếu đổi 120 xuống 80, mọi công thức, bucket hash, FEN state và test fixture phải đổi nhất quán; /8 chỉ hợp lý nếu nó được chứng minh là lượng tử hóa bộ đếm đủ phân biệt, không phải vì giữ số cũ. Bộ đếm luật phải vào state key hoặc TT entry; nếu không, cùng bàn nhưng còn 1 ply và còn 79 ply có thể dùng nhầm kết quả. Không đủ source để liệt kê hằng số 120 còn sót.
Nguồn: ask 1/ASK_2026-09-26.md:184-186 và quy ước 80 ply tại :315-323.
Mức tin: CHẮC về invariant; KHÔNG ĐỦ DỮ LIỆU cho grep cụ thể.

#### A3-08
Trả lời: B2b/B1m có thể đúng luật nhưng làm yếu vì materialKey mới trỏ vào bảng chưa tune, PSQ bị cộng hai lần, hoặc TT key thay đổi làm giảm hit/ordering. Tách A/B thành 4 binary: baseline, B2b, B1m, cả hai; giữ nodes/seed/Threads 1, chạy perft và jqcheck trước rồi SPRT. Nếu B2b giảm còn B1m không giảm, xem material/PSQ; nếu chỉ cả hai giảm, xem interaction/TT.
Nguồn: số đo và diff được nêu tại ask 1/ASK_2026-09-26.md:188-190.
Mức tin: GIẢ THUYẾT CẦN ĐO; không thể khẳng định lỗi darkSquare khi diff không có trong repo.

#### A3-09
Trả lời: Hai bộ đếm có thể đang đếm hai sample space khác nhau: legal sau khi loại chiếu/đối mặt/bên đi so với index table đã canonical hóa. Không thể chọn Felicity hay bộ nội bộ chỉ từ tổng số. Cần viết enumerator cho từng filter, xuất danh sách canonical key rồi diff; với 1–1,5% hòa lệch, kiểm rule perpetual trước khi nghi index.
Nguồn: ask 1/ASK_2026-09-26.md:192-196.
Mức tin: CHẮC về phương pháp; KHÔNG BIẾT định nghĩa Felicity nếu chưa có mã nguồn/đặc tả.

#### A3-10
Trả lời: Tôi không biết tài liệu chính thức đủ tin cậy cho W-M-nnnn, rank và quota API chessdb.cn; không nên trả lời số chẵn hay hạn mức như sự thật. Để xác định, dùng fixture cùng FEN trước/sau capture, ghi request/response/hash và so M với DTM hiện tại, sau nước và bảng mới.
Nguồn: ask 1/ASK_2026-09-26.md:198-200.
Mức tin: KHÔNG BIẾT.

#### A3-11
Trả lời: Thứ tự trọng tài nên là legality → solver/EGTB có kết quả → engine thứ ba → thầy bản quyền; “thầy báo mate” không tự là ground truth. Hết ngân sách solver phải là UNKNOWN, không phải draw. Nhãn dè dặt: WDL chỉ khi đồng thuận; cp kẹp và giảm trọng số theo độ sâu/độ bất đồng; lưu engine/version/config. UCI/UCCI có thể khác quy ước mate N, nên parser phải có adapter và fixture riêng, không giả định N là ply.
Nguồn: ask 1/ASK_2026-09-26.md:202-204.
Mức tin: CHẮC về UNKNOWN và provenance; KHÔNG BIẾT quy ước từng engine thương mại.

#### A3-12
Trả lời: Dùng một reducer duy nhất SetAutoState(request, source); UI, menu, F9/F10, timer, callback và persistence chỉ gửi command, không ghi field trực tiếp. State machine có version, transition table, owner thread, token phiên và invariant: FULL => ANALYZE, STOP => no outbound move, stale callback bị bỏ qua theo token.
Nguồn: ask 1/ASK_2026-09-26.md:206-212; tên các đường N1–N4 chỉ xuất hiện trong câu hỏi, không có code đầy đủ.
Mức tin: CHẮC về thiết kế; KHÔNG ĐỦ DỮ LIỆU để đếm đường ghi lén còn lại.

#### A3-13
Trả lời: STOP phải hủy CancellationToken/generation, chặn producer và drain hoặc đánh dấu stale mọi item đã vào queue; khi chạy lại tạo generation mới. Worker kiểm token ngay trước side effect chuột và trước publish state. Test đỏ: enqueue N1, STOP, start N2; N1 tuyệt đối không được bấm ra ngoài.
Nguồn: mã queue không được đăng; câu hỏi thuộc phần A3 công khai.
Mức tin: CHẮC về invariant; KHÔNG ĐỦ DỮ LIỆU về lỗi hiện tại.

#### A3-14
Trả lời: Không coi “đã gọi mouse click” là “đã ăn nước”. Sau click phải chờ frame ổn định, nhận lại bàn, kiểm nước hợp lệ và state mới khác state cũ; timeout thì STOP/RETRY có giới hạn. Capture nên fail-closed khi board confidence thấp.
Nguồn: các yêu cầu nhận bàn/tiêm chuột tại ask 1/ASK_2026-09-26.md:342-360; OpenCV template matching: https://docs.opencv.org/4.13.0/d4/dc6/tutorial_py_template_matching.html.
Mức tin: CHẮC về xác nhận hậu hành động; GIẢ THUYẾT CẦN ĐO về timeout.

#### A3-15
Trả lời: Hai importer chỉ an toàn khi dùng cùng canonicalizer, source hash, license/filter policy và ledger. Audit phải kiểm count input/output, duplicate, forbidden, quarantine; nếu không có source code thì không thể kết luận “cùng điểm mù”.
Nguồn: yêu cầu trọng tài/import tại phần ASK03 của ask 1/ASK_2026-09-26.md.
Mức tin: KHÔNG ĐỦ DỮ LIỆU cho lỗi cụ thể; CHẮC về bộ kiểm.

### A2 — Auto, train, app và vận hành
#### A2-01
Trả lời: Chọn template + đặc trưng màu/chữ làm baseline trên ô đã normalize; CNN nhỏ chỉ khi baseline không đạt. Tự học mẫu phải chụp nhiều frame, loại highlight/mũi tên bằng median/temporal consistency, không tin một frame. Ngưỡng confidence phải đặt theo false-move cost và hiệu chuẩn trên tập skin giữ riêng. Tôi không biết repo mã mở nào có accuracy công bố đúng cho bàn cờ tướng screenshot; không bịa tên kho.
Nguồn: ask 1/ASK_2026-09-26.md:342-345; OpenCV docs đã dẫn ở trên.
Mức tin: CHẮC về baseline/fail-closed; KHÔNG BIẾT accuracy/GUI thương mại.

#### A2-02
Trả lời: Hai điểm chỉ xác định một đoạn và scale nếu giả định hình chữ nhật affine; không xác định phối cảnh. Dùng 4 góc hoặc 3 điểm + giả định hình học, nhận diện hướng bằng màu/label quân chứ không chỉ vị trí. Lượt đi không đáng tin từ frame nếu app không hiển thị clock/last-move; phải chờ state transition.
Nguồn: ask 1/ASK_2026-09-26.md:347-350.
Mức tin: CHẮC về hình học; KHÔNG BIẾT kỹ thuật các GUI thương mại.

#### A2-03 / A2-35
Trả lời: Full = engine thực hiện cả hai lượt; one = thực hiện một nước rồi trả quyền. Ba adapter là capture, UIA/Win32, và integration web được cho phép; hồ sơ dữ liệu nên có window matcher, vùng/transform, decoder, sender, turn detector, end detector, version, health checks. Tôi chỉ có thông tin đã được câu hỏi tóm tắt về BHGui/SharkChess, không có tài liệu gốc để phân loại từng GUI hay giải thích bản thương mại; ô còn lại là không biết.
Nguồn: ask 1/ASK_2026-09-26.md:352-371; Windows capture: https://learn.microsoft.com/en-us/windows/apps/develop/media-authoring-processing/screen-capture.
Mức tin: CHẮC về adapter/profile; KHÔNG BIẾT bảng GUI từng sản phẩm.

#### A2-04
Trả lời: Đo 4 đoạn bằng QPC và trace id cho từng ply; không dùng hằng 50 ms. Windows.Graphics.Capture phù hợp capture cửa sổ có picker, nhưng phải đo thêm GDI/DXGI trên target và DPI. Hash ROI là detector rẻ, sau đó xác nhận bằng recognizer; SendInput thường tạo input thật hơn PostMessage nhưng app đích có thể chặn cả hai. Tôi không có số công bố đáng tin về tổng latency GUI thương mại.
Nguồn: ask 1/ASK_2026-09-26.md:357-360; Microsoft capture docs: https://learn.microsoft.com/en-us/windows/apps/develop/media-authoring-processing/screen-capture.
Mức tin: CHẮC về cách đo; KHÔNG BIẾT ms thương mại.

#### A2-05
Trả lời: Fighting mode chỉ cần profile, capture, start/stop, full/one, engine, book và emergency stop; Research giữ phần còn lại. Registry command phải là nguồn duy nhất cho toolbar/menu/shortcut. Cổng tự động duyệt command tree và gọi trực tiếp mọi command bị ẩn.
Nguồn: ask 1/ASK_2026-09-26.md:362-366.
Mức tin: GIẢ THUYẾT CẦN ĐO cho số nút; CHẮC cho registry/reachability.

#### A2-06
Trả lời: Dùng scheduler bin-packing theo required_threads, reservation token theo tài khoản thầy, giữ session sống, không đếm theo process name mà theo executable hash + instance id. Trên >64 logical processors, ưu tiên OS/processor group mặc định; chỉ pin khi benchmark chứng minh jitter giảm, và pin đối xứng hai bên. Tắt ponder trong đo nodes/time chuẩn hóa; 6 vs 20 lõi không so Elo công bằng nếu time control khác, nên báo riêng resource và dùng fixed nodes khi mục tiêu search.
Nguồn: ask 1/ASK_2026-09-26.md:373-380.
Mức tin: CHẮC về reservation/session/NoBook; GIẢ THUYẾT CẦN ĐO về affinity.

#### A2-07
Trả lời: Với bảng thiếu cân bằng, dùng logistic/BayesElo và báo posterior/KTC; neo Thầy chỉ đổi gốc, không làm dữ liệu bớt bất định nếu giữ uncertainty của neo. Không trộn book/no-book trong một rating không có cột config. Không thể cho một số ván tối thiểu duy nhất: phải mô phỏng theo draw-rate, color balance và variance; 30 Elo thường cần nhiều hơn 300 ván nếu chỉ gặp cùng một Thầy.
Nguồn: ask 1/ASK_2026-09-26.md:382-385.
Mức tin: CHẮC về tách config; KHÔNG ĐỦ DỮ LIỆU để cam kết số ván.

#### A2-08 / A2-09
Trả lời: Không update net sau từng ván nếu mục tiêu đo strength; đó là online learning gây drift và leakage. Gom immutable shard theo batch, train theo manifest, giữ validation cố định; ưu tiên ván có Thầy chỉ bằng sampling weight có giới hạn, không bỏ phần còn lại. Multi-machine phải dedup theo position_key + label_schema + source_hash, lưu provenance và quarantine nhãn mâu thuẫn, không tự “bình chọn” im lặng.
Nguồn: ask 1/ASK_2026-09-26.md:387-... và mô hình kho tại :8; không có đoạn đầy đủ nên không cite code.
Mức tin: CHẮC về chống leakage/provenance; GIẢ THUYẾT CẦN ĐO về weight.

#### A2-10 / A2-11
Trả lời: Nhãn từ engine đóng nguồn chỉ được dùng thương mại khi license cho phép và có quyền dẫn xuất; không suy ra quyền bán net từ quyền chạy engine. Nếu không rõ license: không dùng cho sản phẩm bán. Tách sinh thế rẻ khỏi chấm đắt: sampler tạo canonical positions theo strata; chấm WDL bằng solver/EGTB/teacher được phép, UNKNOWN giữ riêng và không biến thành draw.
Nguồn: quy định net CC0/tự train tại ask 1/ASK_2026-09-26.md:13-17, và A2-10/A2-11 trong cùng tệp.
Mức tin: CHẮC về license/provenance; KHÔNG BIẾT license cụ thể engine đóng.

#### A2-12
Trả lời: Hash chỉ giúp khi TT hit hữu ích; RAM dư không tự biến thành thế/s. Đo hit rate, nodes/s, RSS, page fault và hashfull theo 4 mức hash, có đối chứng lặp. Chọn mức tốt nhất trên holdout; không dùng vượt RAM an toàn hoặc làm swap.
Nguồn: câu hỏi A2-12 trong ask 1/ASK_2026-09-26.md.
Mức tin: CHẮC về phương pháp; KHÔNG BIẾT mức tối ưu máy đội.

#### A2-13 / A2-17 / A2-27 / A2-28
Trả lời: Theme và DPI dùng resource dictionary/semantic style, không hard-code màu/kích thước; đổi theme bằng replace resource trên UI thread. WPF blank-screen cần capture exception ở InitializeComponent, build hash, generated files và test 6 lần liên tiếp; không chữa bằng retry mù. Dialog phải Grid/ScrollViewer, đo AutomationPeer ở 100/150/200% và kiểm không cắt text. App portable dùng base directory, không path tuyệt đối; lưu config/user data tách khỏi install và kiểm quyền ghi.
Nguồn: các mã A2 trong ask 1/ASK_2026-09-26.md; không có source UI để cite dòng.
Mức tin: CHẮC về nguyên tắc; KHÔNG ĐỦ DỮ LIỆU về lỗi cụ thể.

#### A2-14 / A2-15 / A2-16
Trả lời: Video nhiều cảnh cần character identity embedding + shot-level reference, nhưng phải có human/automatic rejection khi mặt sai; ngân sách nên là pipeline có cache và render proxy trước. Chia theo thời gian cần overlap một frame và hash/PTS để nối không giật; không chia đúng 10 phút nếu GOP/transition cắt giữa shot. Tách “ngạch” bằng workspace manifest, model allowlist, output root, license tag và process env; mọi input/output phải kiểm path containment và provenance.
Nguồn: các mã A2 tại ask 1/ASK_2026-09-26.md.
Mức tin: GIẢ THUYẾT CẦN ĐO cho model/ETA; CHẮC về isolation/provenance.

#### A2-18 / A2-19 / A2-20
Trả lời: Import phải normalize → validate → canonicalize → dedup → quarantine → commit theo batch. HKA lớn chỉ đáng chọn nếu inference cost/strength đo được; distillation phải giữ teacher holdout và test calibration. Engine game logs cần kiểm terminal, legal move, ply/result consistency; ván 0 nước là lỗi dữ liệu, không tính rating.
Nguồn: mô hình kho tại ask 1/ASK_2026-09-26.md:8-9 và các mã A2 tương ứng.
Mức tin: CHẮC về invariant; KHÔNG BIẾT kiến trúc tối ưu/strength cụ thể.

#### A2-21 / A2-22 / A2-23 / A2-24
Trả lời: Ponder bật làm kết quả không tất định hơn; benchmark phải cố định seed/time/nodes và báo ponder như config riêng. Hiệu ứng cụm âm có thể do interaction, tune chưa đủ, selection bias hoặc A/B sai; tháo từng thay đổi bằng factorial/ablation. Alpha-beta vẫn là đường chính cho engine deterministic; MCTS lai chỉ đáng làm prototype có test, không nên giữ sáu nhánh khi thiếu người. Gộp nhánh theo shared rules/protocol/test, chỉ tách eval/search khi có bằng chứng sản phẩm.
Nguồn: các mã A2 tại ask 1/ASK_2026-09-26.md; quy tắc A=A và đối chứng dương tại :330.
Mức tin: GIẢ THUYẾT CẦN ĐO cho Elo; CHẮC về ablation/config discipline.

#### A2-25 / A2-26 / A2-27
Trả lời: Web cộng đồng nhỏ nên ưu tiên authoritative game state, xem giải live, reconnect, anti-cheat cơ bản, moderation, replay/export và observability trước tính năng phụ. Lõi luật dùng chung được nếu có một spec test vectors và adapter C#5/.NET8; không chia sẻ UI/serialization ngầm. DPI test phải chạy tự động qua accessibility tree.
Nguồn: bối cảnh Kỳ Viện và app tại ask 1/ASK_2026-09-26.md:15; không có code web đầy đủ.
Mức tin: CHẮC về kiến trúc; GIẢ THUYẾT CẦN ĐO về ưu tiên sản phẩm.

#### A2-29 / A2-30 / A2-31
Trả lời: Trước khi chạy phải tính ngân sách RAM: model + optimizer + batch + decode + cache + OS headroom; nếu vượt ngưỡng thì reject, không đợi OOM. ETA dùng throughput đo trên sample đại diện và khoảng tin, model do người dùng thêm phải hash/license/shape validate. Công cụ thị trường phải lưu timestamp, source, revision, survivorship/look-ahead controls và backtest out-of-sample; LLM chỉ hỗ trợ trích xuất/giải thích, không là nguồn giá. Nhóm nhỏ nên bỏ các nhánh sản phẩm chưa có test/khách, và giải nhầm bài toán khi tối ưu chức năng trước pipeline đo và dữ liệu sạch.
Nguồn: các mã A2 tương ứng trong ask 1/ASK_2026-09-26.md.
Mức tin: CHẮC về guardrails; GIẢ THUYẾT CẦN ĐO về roadmap.

#### A2-32 / A2-33 / A2-34
Trả lời: Engine kém −317,8 Elo: ưu tiên legality/search correctness, time management, TT/move ordering, eval scale, rồi mới tune tính năng; mỗi thay đổi phải có A=A và positive control. Với Git blob/index, tôi không đủ dữ liệu để khẳng định cơ chế mod 2^32 nếu chưa có object bytes và code parser; cần hash raw blob, header blob <size>\0, zlib output length và mọi arithmetic width. Với net cấm thương mại, phải hỏi nhóm phát hành/đọc license bằng văn bản; chưa có xác nhận thì không đưa vào sản phẩm bán.
Nguồn: các mã A2-32..34 và quy định license tại ask 1/ASK_2026-09-26.md:315-323.
Mức tin: CHẮC về thứ tự kiểm/license; KHÔNG BIẾT lỗi Git cụ thể và trạng thái trao đổi với nhóm phát hành.

### ASK04-B — K1 đến K8
#### K1–K3: nhập kho, giao dịch và nguồn
Trả lời: Dùng streaming import theo batch từ ổ nguồn, không tạo bản sao đầy đủ; mỗi batch có source hash/range/row count và ledger. Backup SQLite bằng backup API hoặc snapshot nhất quán, không copy riêng .db khi WAL còn hoạt động. Gộp staging bằng một writer, batch commit, resume cursor và lock file có PID/heartbeat/owner; busy_timeout không thay thế lock. Chuẩn hóa source_hash, tên UTF-8, alias và forbidden-group trước khi parse; cổng cuối là COUNT(*) WHERE forbidden=1 = 0 cùng audit.
Nguồn: ask 1/ASK_2026-09-26.md:35-43; SQLite WAL: https://www.sqlite.org/wal.html (đã kiểm tra 2026-09-26).
Mức tin: CHẮC.

#### K4: nhận bàn và giao tiếp client
Trả lời: Ưu tiên API/DOM/UIA được phép; capture là fallback. Có board detector, homography, recognizer, temporal stability, legality checker và fail-closed. Không reverse engineer/bắt token/giao thức của client bên thứ ba khi không có quyền; với Android Accessibility, Android yêu cầu user bật rõ ràng và giới hạn mục đích trợ năng.
Nguồn: ask 1/ASK_2026-09-26.md:44-45; Android docs: https://developer.android.com/reference/android/accessibilityservice/AccessibilityService.
Mức tin: CHẮC.

#### K5–K6: tàn cuộc và đo lường
Trả lời: Lấy mẫu tàn cuộc theo strata vật chất + tần suất thực chiến, giữ calibration set đều; lưu theoretical WDL và rule-80 WDL riêng. Cổng rẻ gồm calibration, monotonicity DTM, legality và fixed-position regression trước Elo. Đo timing bằng nhiều lượt, p50/p90/p99, CPU/RAM/PID/window correlation; nếu tải cao thì gắn trạng thái BUSY/INCONCLUSIVE chứ không PASS/FAIL giả.
Nguồn: ask 1/ASK_2026-09-26.md:47-51; NNUE docs ở trên.
Mức tin: CHẮC về phương pháp; GIẢ THUYẾT CẦN ĐO cho ngưỡng.

#### K7–K8: nhiều agent và token
Trả lời: Chỉ một writer cho kho/log quan trọng; agent nhận lease có TTL, task idempotency key, append-only event log và checkpoint. Worker đọc manifest nhỏ, chỉ gửi diff/summary; script tự động làm count/hash/quick_check/perft/benchmark, AI chỉ đọc các ca bất thường hoặc quyết định thiết kế. Mỗi task phải có input path, invariant, output schema, timeout và tiêu chí PASS/FAIL trong một prompt.
Nguồn: ask 1/ASK_2026-09-26.md:53-57; SQLite WAL xác nhận chỉ một writer.
Mức tin: CHẮC.

### Kết luận phạm vi
Tôi đã trả lời tất cả mã được liệt kê công khai trong Ask ở mức có thể kiểm chứng từ repo. Tôi chưa thể làm các yêu cầu “tìm lỗi thật tại tệp:dòng”, xác nhận behavior/giấy phép của GUI thương mại, hoặc đưa số đo Elo/ms cụ thể vì repo công khai cố ý lược source và ghi rằng source riêng được gửi cho AI được chọn. Những phần đó cần bạn cung cấp source/fixture trong cùng phiên; nếu không, câu trả lời đúng theo luật debate là không đủ dữ liệu, không phải đoán.
