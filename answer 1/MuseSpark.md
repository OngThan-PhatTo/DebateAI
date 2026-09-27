# MuseSpark — trả lời Ask 1 (2026-09-26)

Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.
Đã đọc điều khoản bảo mật Ask03; không lưu, không lan truyền, không dùng để huấn luyện; sẽ xoá zip sau khi nộp (không nhận zip ở vòng này nên không có gì để xoá).

> Ghi chú trung thực: tôi là Muse Spark chạy trong phiên này. Cam kết trên có hiệu lực trong phiên; việc log/train do nền tảng quyết, tôi không kiểm soát. Không bịa URL — mọi URL dưới đây là URL tôi đã mở/search ngày 2026-09-26 hoặc đã mở 2026-09-21 ở vòng trước. Chỗ nào chưa kiểm ghi rõ.

## 0.1 Tàn cuộc bậc 2
- Trả lời:
  - Không liệt kê vét 4v4 (~1,14e20 thế theo đề). Chỉ liệt kê trọn ô nhỏ (≤2v2 không sĩ/tượng, 1v0 đủ sĩ/tượng), ô lớn lấy mẫu theo hạn ngạch tần suất ván thật.
  - Hạn ngạch: theo tần suất ô trong ván thật (đội đã có mẫu 400k: không sĩ/tượng 0,023%, ≥3 sĩ/tượng 91%) nhân với trọng số thắng/thua (hoà trọng số nhỏ ~0,1-0,2). Không chia đều, không chia theo log kích thước ô — vì ô to nhưng hiếm trong thực chiến (vd không sĩ/tượng) học nhiều là phí.
  - Retrograde nội bộ ~9.900 thế/s, 700 B/thế là hợp lý cho bậc 2-3 trên MỘT máy (vài chục triệu thế = vài chục giờ + vài chục GB). Vượt lên 3v3/4v4 thì dừng liệt kê, chuyển sang: (a) lấy thế tàn từ ván thật + (b) random playout có kiểm hợp lệ, rồi chấm bằng chessdb-sst offline 512 GB có sẵn + FelicityGen DTM.
  - Target NNUE tàn cuộc: giữ 2 đầu như Stockfish — WDL thật (thắng theo luật) + cp có độ dốc theo DTM (vd dải 1500-2990: `cp = sign * max(1500, 3000-10*dtm_ply)` là hình dạng đội đề xuất, mặc định TẮT là đúng). Không vứt mate vào `:433` như A3-01 hiện tại. Nếu chỉ cho mate vào đầu WDL thì search lo khoảng cách được, nhưng net value-only sẽ mất độ dốc chọn nước ngắn nhất — nên cần dốc DTM ở cp.
  - «Nước đúng» bên thua: train theo DTM thuần (kéo dài nhất) để giữ tín hiệu; khi gắn cờ thắng THEO LUẬT 80 ply thì gắn thêm `dtc` + cờ cursed-win/blessed-loss kiểu Syzygy (xem nguồn). Quyết định gắn THẮNG hay HOÀ cho thế thắng-lý-thuyết-quá-mốc là của owner — kỹ thuật: nếu gắn THẮNG thì search phải tự trừ theo bộ đếm (như Stockfish `v -= v*rule50/199`), nếu gắn HOÀ thì net học lướt nhanh nhưng mất nước thắng thật gần mốc.
- Nguồn:
  - Felicity MIT, metric DTM/DTC: https://github.com/nguyenpham/FelicityEgtb — trích: “provides some popular metrics such as depth to mate and depth to convert” + “code and data are released under MIT”.
  - Felicity wiki: https://chessprogramming.org/Felicity_Tablebases — Xiangqi 1-2 quân tấn công, EGTB không giúp nhiều Elo cờ vua (<13 Elo 6-men Syzygy) nhưng kỳ vọng giúp cờ tướng hơn (chưa có số).
  - Syzygy 5 lớp WDL cursed/blessed + DTZ: https://python-chess.readthedocs.io/en/stable/syzygy.html — trích: “Returns 1 in case of a cursed win and -1 in case of a blessed loss. Mate can be forced but drawn due to fifty-move rule.” ; https://syzygy-tables.info/metrics
  - Stockfish damp theo rule50 + chặn TT khi rule50 cao: https://github.com/official-stockfish/Stockfish/blob/master/src/evaluate.cpp (`v -= v * pos.rule50_count() / 199`) ; https://github.com/official-stockfish/Stockfish/blob/master/src/search.cpp (`if (pos.rule50_count() < 96)` mới cho TT cutoff) ; https://github.com/official-stockfish/Stockfish/blob/master/src/position.cpp (`is_draw` khi `rule50 > 99`).
- Mức tin: CHẮC cho phần Felicity MIT + Syzygy 5 lớp + Stockfish damp/TT-guard (có URL mở được). GIẢ THUYẾT CẦN ĐO cho công thức cp 1500-2990, tỉ lệ trộn, hạn ngạch cụ thể — đội đo Spearman(net_out, -DTM) trên tập thắng như A3-01(d).
- Code/ca kiểm (nếu có): ca rẻ không Elo — tương quan Spearman giữa output net và -DTM trên 2.000 thế thắng đã có DTM; PASS nếu rho > 0,6 và đơn điệu theo ply. Đối chứng ÂM: xáo nhãn DTM → rho ~0.

## 0.2 Mô hình kho
- Trả lời: Mô hình `datanoscore.db` (hàng đợi vkey+vmove chưa chấm) → analyze → `data.db` (có điểm), bất biến giao rỗng, + `positions.fendb` (FEN+nhãn master) gộp ra `positionsfinal` rồi đổi tên — về nguyên tắc đúng (tách queue khỏi store, tách master FEN khỏi scored). Rủi ro chính: (1) gộp đổi tên mất xuất xứ nếu không có manifest `nguon_lo`; (2) 2 tiến trình cùng ghi gây lệch count (+132k như K2) mà tool báo sai; (3) FEN trùng nhưng nhãn khác (2 depth/2 máy) bị đè mất.
  Đề xuất: giữ 2 tệp nối bằng vkey (đừng gộp vật lý thành 1 tệp khổng lồ khó backup), mỗi lô có manifest {pkg_uuid, machine, eng_hash, net_hash, cfg, t0, n, sha256} + upsert idempotent (nạp lại cùng uuid = 0 mới). Kiểm hậu gộp: count trước/sau + `them/trung/db_sau` + mẫu khoá ngẫu nhiên + `PRAGMA quick_check`.
- Nguồn: thực hành manifest/idempotent là suy luận từ án lệ đội (K2, A2-09); công cụ đấu/giải có sẵn hỗ trợ logging mở rộng: https://github.com/Disservin/fastchess (Extended PGN nodes/seldepth/nps/hashfull) ; https://github.com/cutechess/cutechess
- Mức tin: GIẢ THUYẾT CẦN ĐO (kiến trúc), CHẮC cho nguyên tắc single-writer SQLite + quick_check (tài liệu SQLite chuẩn, đội đã vấp).
- Code/ca kiểm: ca đỏ — nạp cùng gói 2 lần → `them` lần 2 phải =0; kill -9 giữa gộp → chạy lại nối tiếp được từ bảng tiến độ.

## 0.3 Sửa kho toàn bảng
- Trả lời: 567M dòng → sau sửa ~494,5M (bỏ REAL bug, trùng, book cấm) là số đội đo, tôi không đo lại. Cách an toàn cho SQLite 30 GB trên Windows khi chỉ trống ~100 GB:
  1. Không MỘT giao dịch 5 GB WAL — chia batch 100-200k dòng/commit + bảng `tien_do(batch_id, from_rowid, to_rowid, hash, done)`.
  2. Một writer duy nhất (file lock / mutex tên cố định), các reader khác `immutable=1` hoặc copy-out.
  3. Giữ `journal_mode=DELETE` cho kho thật đọc nhiều bởi GUI (WAL tốt cho ghi song song nhưng để lại -wal/-shm khó copy, khó backup); staging dùng WAL được.
  4. Bug REAL bind_double: sửa bằng CAST + kiểm `typeof(vkey)`, vkey phải INTEGER 64-bit. Nhận diện khoá sai còn sót: `vkey=0 OR vkey=-9223372036854775808 OR vkey<0 AND (vkey & ((1<<55)-1))=...` — cụ thể: quét `typeof!=integer`, `vkey=0`, `I64MIN`, giá trị âm có bit 2^55 (binade) như đội mô tả; đối chiếu với FEN băm lại (Zobrist) mẫu 100k.
- Nguồn: suy luận từ số đội + tài liệu SQLite (journal_mode, quick_check) — không dẫn URL mới để khỏi bịa. Mẫu JSON manifest xem A2-09 ở file Ask02 cũ của tôi.
- Mức tin: GIẢ THUYẾT CẦN ĐO cho batch size, CHẮC cho nguyên tắc không one-transaction + single-writer (án lệ K2 của đội).
- Code/ca kiểm: Python 3.12 duyệt theo rowid range, mỗi batch `SELECT sha256_group`, ghi tiến độ, kill giữa chừng chạy lại không mất. Đỏ: `SELECT count(*) WHERE typeof(vkey)='real'` phải =0 sau sửa.

## 0.4 CBL
- Trả lời: 432 tệp = 221 nội dung (127 CCBridgeLibrary 2,9M ván/433M nút; 94 CCBridge cũ v3 zlib 10k ván/652k nút; 1.828 tệp trong archive) — số đội đo, không kiểm lại. Đề xuất:
  - Train: ưu tiên policy (nước đi) hơn điểm — vì điểm trong sách CBL là điểm engine/bookshelf cũ, thang không chuẩn; nước đi + kết quả ván thật sạch hơn cho value/policy. Điểm chỉ dùng dè dặt (kẹp |cp|, trọng số thấp).
  - Lọc trùng: băm ván chuẩn hoá (bỏ comment, chuẩn hoá ICCS) + băm thế; lọc sách cấm bằng hash tệp + tên UTF-8 + nhóm cấm (xem K3) — ca kiểm «0 dòng từ nguồn cấm».
  - Định dạng giữ chú giải: giữ PGN/XQF gốc + sidecar JSON, đừng đổi sang .binpack sớm (binpack mất comment). Chưa tìm được tài liệu chính thức .cbl/.cbr — ghi KHÔNG BIẾT, không bịa spec.
- Nguồn: KHÔNG BIẾT link spec CCBridge .cbl/.cbr công khai. Nguồn mở liên quan đã kiểm vòng trước: variant-nnue-tools sinh data (72 B/bản ghi cờ tướng, mức tin thấp).
- Mức tin: GIẢ THUYẾT CẦN ĐO (policy>score), KHÔNG BIẾT cho spec .cbl/.cbr.
- Code/ca kiểm: script liệt kê sha256 + tên UTF-8 + cờ cấm, assert 0 dòng cấm sau lọc.

## 0.5 AUTO giả lập + đăng nhập nền tảng (kể cả H1–H4)
- Trả lời:
  - (a) Nhận dạng bền skin/độ phân giải/giả lập (LDPlayer/BlueStacks/MuMu/AVD): 2 tầng như A2-01 tôi đã trả — tự học skin lúc bấm ở thế khai cuộc (đã biết 32 vị trí → cắt 14 mẫu + ô trống) + template matching NCC trên ô đã cắt; fallback classifier ONNX 15 lớp. Ngưỡng `p_min 0.85 + board_min 0.70`, dưới ngưỡng báo «không nhận ra», cấm đi bừa. YOLO nguyên khung chỉ để tìm lưới. DPI/đa màn là bẫy chính.
  - Chụp nhanh Win10/11 vùng ~600x700: DXGI Desktop Duplication nhanh nhất (~0,5-1 ms theo lib cộng đồng, cần đo lại), GDI BitBlt 15-20 ms, Windows.Graphics.Capture 8-12 ms — số cộng đồng, chưa phải số Microsoft. Phát hiện «đối thủ đã đi»: so hash vùng/histogram vài điểm + quét 200-500 ms, rẻ hơn đợi sự kiện cửa sổ. Bấm: SendInput foreground nhanh + ít bị bỏ qua; PostMessage nền nhanh nhưng dễ bị app bỏ qua (kinh nghiệm cộng đồng AHK/SO).
  - (b) Đăng nhập từng nền tảng: JJ象棋 (Unity IL2CPP, DLL+bundle mã hoá, TCP+UDP protobuf, kênh SĐT/QQ/WeChat/Douyin/Ali/账号密码), MoveSky/弈天 CCMS 1.96 (bắt tay/token/relay 127.0.0.1), clubxiangqi.com + zigavn (WebView2 hay giao thức?), Ziga APK, ZingPlay (Zing ID), Kỳ Vương (PlayIntegrity+DexGuard), vndynapp (REST) — TÔI KHÔNG BIẾT chi tiết giao thức private này. Không bịa. Hướng bền + hợp pháp: dùng WebView2/Accessibility/UIA + OAuth/web chính thức nơi có; APK dùng AccessibilityService + chụp màn (có rủi ro Android mới siết quyền — xem nguồn Android). Bắt gói/hook private client dễ gãy khi đổi giao thức (đúng nhược điểm tác giả BH đã ghi) và rủi ro khoá acc — phải hỏi chủ nền tảng.
  - (c) H1–H4 MoveSky: KHÔNG BIẾT — cần 1 phiên thật để ghi log, không đọc mã máy thay được; «các bạn hiện tại» là khung 对阵表 hay danh sách bạn: KHÔNG BIẾT; nick qqmovesky/ccmsexe/cmllh: KHÔNG BIẾT; web nhúng thay giao thức được không: GIẢ THUYẾT — được cho xem/chat, không đủ cho đi nước real-time chuẩn giải.
- Nguồn:
  - Felicity MIT: https://github.com/nguyenpham/FelicityEgtb
  - Desktop Duplication API (MS, cần đo ms thực tế): https://learn.microsoft.com/en-us/windows/win32/direct3ddxgi/desktop-dup-api (tôi biết URL này tồn tại nhưng chưa fetch HTTP ở vòng này — ghi chưa kiểm HTTP, đừng tính là cite cứng)
  - Android AccessibilityService: https://developer.android.com/reference/android/accessibilityservice/AccessibilityService (chưa fetch HTTP vòng này)
  - fastchess/cutechess cho đấu engine: https://github.com/Disservin/fastchess ; https://github.com/cutechess/cutechess
- Mức tin: CHẮC cho nguyên tắc ngưỡng + cấm đi bừa; GIẢ THUYẾT CẦN ĐO cho ms chụp/bấm; KHÔNG BIẾT cho toàn bộ login protocol private.
- Code/ca kiểm: cổng 2 cú bấm 32/32 PASS, 1 chạm phải có ca đỏ lưới giả (ô 38px ra 14-20 quân như K4); mọi login có ca «sai pass/mất mạng không treo».

## 0.6 Giao diện TieuLongNu
- Trả lời: Danh sách hiện có (login dropdown Chiến đấu+Nghiên cứu, panel online, mời/nhận, đánh trực tiếp, AUTO Analyze/Full/Stop, học mẫu, skin Sáng/Tối, KyPho, ảnh→FEN, 3 ngôn ngữ, engine 64-bit + cài đặt) đã đủ lõi. So với BHGui/SharkGui còn thiếu (suy luận, chưa có trích help chính thức): hồ sơ kết nối dạng dữ liệu versioned (thêm sàn không sửa mã), tự hồi phục khi mất bàn (đổi cỡ/hoạt ảnh/popup), thanh auto gọn 8-10 nút chữ to, tooltip 3 tiếng đầy đủ. Bộ nút Thực chiến tối thiểu: Bắt đầu/Dừng, Full/One, Đổi bên, Ra nước ngay, Chọn engine, Sách on/off, Lưu/nạp hồ sơ — còn lại ẩn (1 lõi, chế độ là dữ liệu, không rải if).
- Nguồn: suy luận + file Ask02 cũ của tôi (A2-05, A2-35). Không bịa trích BHGui/SharkGui — ghi «suy luận của tôi», cần đội đối chiếu bhhelp.pdf 84 trang + sharkchess.com.
- Mức tin: GIẢ THUYẾT CẦN ĐO.
- Code/ca kiểm (C#5, csc, không csproj): `Mode.Table` quyết định hiện/ẩn; kiểm bằng duyệt VisualTree + bindings + phím tắt/chuột phải không còn lối vào mục ẩn; DPI 100/125/150/200 không cắt chữ.

## 0.7 Encrypt book + update khách
- Trả lời: Mô hình «encrypt book» theo owner 21/09 + 3 loại key (hết hạn/ngưng vĩnh viễn/hạn cập nhật) + 5 tệp config cấm đè — tôi chưa thấy spec, chỉ trả nguyên tắc:
  - Khoá theo máy: DPAPI (Windows) hoặc khóa RSA-OAEP + AES-256-GCM, key bọc theo HWID hash (CPU+disk+MAC, cho phép trượt 1 thành phần). Không hard-code key trong exe.
  - Chống dump RAM: không giữ cả book giải mã; giải mã theo trang 4-64 KB khi probe, `SecureString`/pin + `ZeroMemory` sau dùng, tắt core dump; thừa nhận không chống được attacker admin + debugger — chỉ nâng chi phí.
  - Update giữ config: update = manifest + copy mới → rename nguyên tử; 5 tệp config chỉ merge thiếu, không đè; ca đỏ hash: sau update hash 5 tệp phải bằng trước, version book tăng.
- Nguồn: nguyên tắc DPAPI/AES-GCM là kiến thức chuẩn, chưa fetch URL vòng này — ghi GIẢ THUYẾT CẦN ĐO, không bịa link MS.
- Mức tin: GIẢ THUYẾT CẦN ĐO.
- Code/ca kiểm: test đỏ — sửa 1 byte book → probe fail; update thử → diff 5 config = rỗng.

## 0.8 Kiểm toán engine trước train
- Trả lời: Bảng KIEM_TOAN §3 còn 20 CHƯA ĐO / 13 KHÔNG QUA (13/09) — số đội, không đo lại. Thứ tự soi để kịp 01/10 trên rig 60-100h:
  1. Luật + khoá băm/TT (sai là sai mọi số) → 2. Search/MDP/EGTB/rule80 → 3. Đồng hồ/nodes/smp → 4. Eval-net/load. Cổng ưu thế ≥400: chỉ chạy khi A/B nodes cố định KTC95 dưới >0 + A=A chứa 0 (xem A2-07/A2-21). Thang Pikafish 5 bậc: neo 1 bậc bằng Thầy cố định, đừng neo tất cả. Cắt rig: bỏ mọi test dài >30′, chỉ giữ bench bit-exact Threads1 + A=A 100 ván + A/B 300 ván pentanomial gương (±7 Elo như đội đã đo) + perft + EGTB spot-check 40/40 krk/kpk.
- Nguồn: fastchess SPRT/roundrobin/gauntlet/nodes/restart: https://github.com/Disservin/fastchess/blob/master/man.md ; cutechess: https://github.com/cutechess/cutechess ; Stockfish TT-guard rule50: https://github.com/official-stockfish/Stockfish/blob/master/src/search.cpp
- Mức tin: GIẢ THUYẾT CẦN ĐO cho thứ tự (kinh nghiệm), CHẮC cho công cụ fastchess/cutechess tồn tại.
- Code/ca kiểm: checklist 1 trang + lệnh fastchess mẫu `-tournament gauntlet -concurrency N -engine ... nodes=200000 restart=off`.

## 0.9 App điện thoại Kỳ Viện
- Trả lời: Học JJ (MVVM 3 mảnh, 3 lối vào MỜI/PHÒNG MÃ/XEM, đăng nhập tách kênh, module 实名/chống nghiện riêng) ở mức kiến trúc, không copy Unity IL2CPP + hot-update cho web. Kiến trúc đề xuất: MAUI/Flutter 1 codebase → đăng nhập chơi ngay (guest + OAuth), SignalR/WebSocket 1 Hub, engine qua API server nhà (ARM chạy local tốn pin/nóng — đi server trước như DocCoChess.EngineServer), phân tích async. Tối thiểu phát hành: login + lobby + đánh + xem lại + chống gian lận cơ bản (CheatFlags/AntiFarm/AnalyzeGuard) + khai báo made-for-kids/AI disclosure khi đăng video. AUTO trên mobile: chỉ phân tích, không autoplay hộ (rủi ro store + PlayIntegrity/DexGuard).
- Nguồn: suy luận; chính sách YouTube disclosure/kids là nguồn đội đã có ở A2-16 (không fetch lại vòng này).
- Mức tin: GIẢ THUYẾT CẦN ĐO + KHÔNG BIẾT cho chi tiết JJ private.
- Code/ca kiểm: selftest `dotnet run -- --xq-selftest` + shell test APK như đội đã có; ca đỏ mất mạng giữa ván reconnect được.

## ASK04-B / ASK03 / ASK02 (câu còn mở)
- ASK04-B K1-K8: đã trả nguyên tắc ở các mục 0.2/0.3/0.5 trên. Điểm thêm: K1 đĩa phình 500 GB/3 ngày — nhập thẳng theo lô + hash theo nguồn, backup Drive không đệm C bằng rclone `--vfs-cache-mode minimal` + chunk; K6 đo chập chờn — p90 của 3 lượt + hệ số tải, tách «máy bận» bằng baseline idle; K7/K8 nhiều AI — 1 writer + bảng nhiệm vụ + đề bài 1 lượt, script tự chạy thay agent 200-750k token. Mức tin: GIẢ THUYẾT CẦN ĐO.
- ASK03 A3-01…A3-16: chưa có zip nên không cite `tệp:dòng` được — không bịa. Hướng đã nêu ở 0.1 (A3-01/A3-02/A3-03/A3-04), EGTB/Syzygy (A3-05/A3-06), rule80 (A3-07), cờ úp -33 Elo (A3-08: nghi materialKey/psq double-count + TT key đổi — cần đo tách B2b/B1m riêng ≤30′), chessdb M-nnnn/rank (A3-10: KHÔNG BIẾT, cần tài liệu chính thức chessdb.cn), auto 2 nút (A3-12/A3-13/A3-14: 1 điểm kiểm duy nhất tại hàm bấm/gửi + số phiên + xác nhận đọc lại bàn sau bấm), import Hán (A3-15: 前后中/一二三四五/进退平 mã-tượng-sĩ + biến thể chữ — cần chạy chéo 50 ván).
- ASK02 A2-01…A2-35: xem file `Ask02_MuseSpark_TraLoi_Full.md` tôi đã trả (M.1-M.5 + C). Điểm mới vòng này: giữ nguyên kết luận, bổ sung nguồn cứng Felicity/Syzygy/Stockfish/fastchess trên đây. Câu hẹp A2-33 (git >4GB mod 2^32): nguyên nhân unsigned long LLP64, sửa 2.56 size_t — giữ nguyên bài cũ; A2-34 (distillation hỏi thẳng Pikafish): «chưa có» — giữ luật đội (net bán chỉ CC0/tự train).
- Nguồn chung: https://github.com/official-pikafish/Pikafish ; https://github.com/official-pikafish/Networks/blob/master/README.md (net Fairy-xiangqi CC0 bán được, net official cấm thương mại) ; https://opendatacommons.org/licenses/odbl/1-0/ (ODbL Derivative vs Produced).
- Mức tin tổng: CHẮC cho giấy phép + tool tồn tại; GIẢ THUYẾT CẦN ĐO cho mọi con số Elo/ms; KHÔNG BIẾT cho mọi protocol/spec private (JJ/MoveSky/CCMS/CBL/EULA thương mại).
