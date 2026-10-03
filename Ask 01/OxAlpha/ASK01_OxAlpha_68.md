# ASK 01 — bộ hỏi cho OxAlpha — tệp 68/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 336 (FS-01)

### FS-01 — Cỡ mẫu A/B chọn net cho engine TẤT ĐỊNH (Threads 1, nodes cố định)
1. **Bối cảnh:** rig `_LANE_NOTE/_scratch_code_luu/w_net_ab_0210/ab_net.py` (trên `cong_c433a_ab_slot.chay_abngan` + BookTool `--abngan-cli`) lô mồi 02/10: Docco A=A +4 =4 −4 n=12 KTC95 [−108; +108] · đối chứng dương (×4 nodes) +12 =4 −0 n=16 Elo +338 [+176; +∞); Bodetosu A=A +2 =8 −2, dương +10 =6 −0 +255 [+96; +807]. Engine tất định ⇒ A=A luôn w=l (4/4, 4/4, 2/2) nên «KTC chứa 0» không thể trượt ngẫu nhiên; chỉ 10 thế BalancedOpenings trước khi vá `AbNganCli.DocHoSo` đọc `boThe`. 50 k nodes ≈ 1 phút/ván (`ab_net.py:78`). Ứng viên: 6 net Fairy xiangqi cũ (2022–2023, trang Fairy ghi Elo +541…+889 so +914 của C07E94A5) ⇒ chênh có thể < 50 Elo.
2. **Câu hỏi:** với engine tất định, nodes cố định, bộ thế xuất phát N thế (mỗi thế 2 ván đổi màu), cần N bao nhiêu và tính KTC thế nào (pentanomial theo cặp ván?) để xếp 6 net chênh ~30–50 Elo với cận dưới KTC95 ≥ +25 Elo; có nên thay «nodes cố định» bằng «nodes ± jitter nhỏ» hay chỉ cần đa dạng thế yên (lấy từ `positions.fendb`) để tránh tương quan giữa các ván cùng thế?
3. **Trả lời tốt trông thế nào:** công thức hoặc nguồn (Elo error bars, pentanomial/SPRT của fishtest hay cutechess), con số N cụ thể cho ±25 Elo, bẫy tương quan khi dùng chung bộ thế cho mọi cặp net, và cách ước giờ máy trước khi thuê (luật đội: lô mồi chứng minh harness phân biệt được rồi mới mở lô dài).

---

## Câu 337 (FS-02)

### FS-02 — Nạp ~174 triệu FEN duy nhất vào SQLite có `fen TEXT UNIQUE`: nạp từng tệp hay gom – sắp xếp – dựng chỉ mục sau?
1. **Bối cảnh:** bước 8b bộ nạp HF (`SourceCode/Shared/DataFen/nap_fendb.py --tai-cho`, wrapper `_LANE_NOTE/_scratch_code_luu/fable_nap_hf_0210/nap_hf_8b_only.ps1`) nạp TUẦN TỰ 246 tệp FEN text (tổng 9,66 GB; 8a đã khử trùng còn 174.417.976 FEN, đã lọc hợp lệ `--hop-le`) vào kho tạm `.fendb` lược đồ `fendb(id, fen TEXT NOT NULL UNIQUE, moves, score, source)` + `nguon_lo`. Đo 03/10 04:48–05:10: 17 tệp (220 MB) trong ~6 phút ≈ 0,6 MB/s; tệp 457 MB đang nạp > 15 phút, python 1 lõi 97 %, WS 2,2 GB, kho 1,27 GB; ước 5–13 giờ, và chèn ngẫu nhiên vào B-tree UNIQUE sẽ chậm dần khi bảng lớn. Máy 32 luồng / 47,6 GB RAM, commit limit 51,9 GB (đã sập 02/10 vì cạn commit), ổ C NTFS; owner có thể tắt máy giữa chừng.
2. **Câu hỏi:** cách nạp hàng loạt đúng cho SQLite ở quy mô 10⁸ dòng text UNIQUE — (a) bỏ chỉ mục UNIQUE rồi `CREATE UNIQUE INDEX` sau khi nạp hết (khử trùng ngoài bằng sort/hash trước) nhanh hơn bao nhiêu bậc so với chèn có chỉ mục? (b) sort ngoài rồi INSERT theo thứ tự khoá có giúp B-tree không? (c) PRAGMA nào (`journal_mode`, `synchronous`, `cache_size`, `temp_store`) tăng tốc mà KHÔNG mất dữ liệu đã commit khi máy tắt đột ngột; (d) có nên chia kho theo băm tiền tố FEN?
3. **Trả lời tốt trông thế nào:** dẫn SQLite docs (pragma, WITHOUT ROWID, bulk insert FAQ), ước lượng thứ tự độ lớn tốc độ (dòng/giây) cho từng cách trên máy tương tự, cảnh báo rõ cách nào mất dữ liệu khi tắt máy giữa chừng, và cách kiểm (COUNT + quick_check) sau nạp.

---

## Câu 338 (NC-05)

### NC-05 — Engine lúc đánh: ưu thì THÍ QUÂN để vào hình tàn cuộc đã biết THẮNG, lép thì ép về hình HOÀ hoặc thua chậm nhất — cách làm trong search NNUE là gì?
1. **Bối cảnh:** owner yêu cầu (03/10 07:3x): *«dù lép dù ưu cũng phải đưa về hình tàn lợi thế cho mình; ưu thì cần thí bao nhiêu quân để ra hình tàn có thể thắng dựa trên net của positionendgame đã học; lép thì ráng đưa về hình hoà hoặc bí lâu nhất»*. Đội có kho `positionendgame.fendb` 11.950.107 thế có nhãn wdl/dtm/dtc/nước đúng (ô ≤ 2 quân tấn công giải chính xác; 2v2/3v1 mẫu từ chessdb; `SourceCode/Shared/DataFen/kho_tan_cuoc.py:102-111`), engine nhà có EGTB probe lúc đánh (Nữ Oa cổng egtb 37/0, Bodetosu DTM), eval tàn cuộc hiện yếu (pháo đài −711 cp vs search −8; `AI_Debate/onGoing/CHOT_OWNER_2026-09-12_TOI.md:197`); luật hoà cờ tướng 60/80 ply không ăn quân, nên «thắng lý thuyết» có thể thành hoà theo luật. Phương án đang nghiêng: search lo «đúng số nước» (tách vai search–net), net học trộn có trọng số.
2. **Câu hỏi:** (a) để engine tự nguyện THÍ quân đi vào ô WDL=thắng, chỉ cần probe kho/EGTB ở nút ≤ N quân rồi ghi đè eval bằng điểm mate-theo-dtm là đủ (Stockfish Syzygy-style), hay cần thêm thưởng «đơn giản hoá về hình thắng» khi probe chưa chạm (nhiều quân hơn)? Cách thưởng nào không làm engine thí quân bừa ở trung cuộc? (b) khi lép: chọn nước maximize DTM/DTC (thua chậm nhất) hay maximize «cơ hội đối thủ sai» (swindle); với luật 60/80 ply không ăn quân, cách dùng DTC để ép hoà theo luật là gì? (c) cách KIỂM: bộ thế «ưu một quân, thí một quân ⇒ vào ô thắng đã giải» và «lép, một nước duy nhất vào ô hoà» — đo tỉ lệ chọn đúng theo depth; có bộ thế chuẩn nào của cộng đồng cờ tướng không?
3. **Trả lời tốt trông thế nào:** mô tả cụ thể chỗ cắm trong search (eval/qsearch/root), công thức điểm theo WDL/DTM/DTC và mốc luật, nguồn (Stockfish TB code, Pikafish/Fairy xiangqi EGTB), cảnh báo bẫy (TB cutoff ở nút lặp, điểm mate sai với luật không ăn quân, thí quân bừa), và cách đo A/B không làm yếu trung cuộc.

---

## Câu 339 (NC-06)

### NC-06 — Trợ lý AI trong app sinh ảnh/video (ComfyUI_Pro): «nói gì cũng biết làm» — hiểu yêu cầu tự nhiên, đề xuất model, giải thích, rồi tự dựng workflow chạy luôn — kiến trúc nào chạy được OFFLINE trên máy owner?
1. **Bối cảnh:** owner (03/10 07:3x): *«Trong ComfyUI có sẵn 1 AI, tạo triệu tình huống, tôi yêu cầu gì nó phải biết làm, ví dụ làm ảnh nhảy TikTok nó biết, đề xuất model, giải thích và thậm chí làm luôn»*. Đã có: trợ lý theo kho tình huống JSON (`ComfyUIPro/Views/AiView.xaml.cs:56` nạp `TroLyChung.KhoTinhHuong`, gói `scenarios*.json`, mục tiêu ≥ 1.500 tình huống, cổng `--troly-tinhhuong-selftest`), kho model có hash + category (C-408), kho model ngoài `thu_muc_model_ngoai.json` (model ở ổ F), LLM local qua llama-server (Local AI, trần commit 75 %), máy AMD ROCm 47,6 GB RAM, không gọi API trả tiền. ComfyUI chạy workflow JSON qua API nội bộ.
2. **Câu hỏi:** kiến trúc hợp lý cho trợ lý này là gì: RAG trên kho tình huống + catalog model (embedding local) → LLM local sinh «kế hoạch» (model, node, tham số) theo lược đồ JSON có kiểm → dựng workflow từ mẫu đã kiểm → chạy → đọc lỗi ComfyUI và sửa vòng lặp? Nên dùng LLM cỡ nào chạy được trên máy này mà vẫn chọn đúng model/category (so với bảng tra tay)? Làm sao để «triệu tình huống» = tổng quát hoá thật (kiểm bằng tập giữ lại) chứ không phải liệt kê; cách đo độ đúng «đề xuất model» và «làm luôn» (ca theo tên, ca đỏ) ra sao?
3. **Trả lời tốt trông thế nào:** sơ đồ thành phần + định dạng kế hoạch JSON có schema, gợi ý model LLM local cụ thể cho máy ROCm, cách RAG trên JSON tình huống + catalog, vòng lặp sửa lỗi có trần, cách kiểm (bộ 100 yêu cầu tự nhiên giữ lại → đo chọn đúng category/model, thời gian), và bẫy (ảo giác tên model, tham số VRAM, nội dung chặn trẻ em).

---

## Câu 340 (MT-01)

### MT-01 — Bạn (AI đang trả lời) có thể THAM GIA «AI Meeting» của chúng tôi bằng cách của bạn không?
1. **Bối cảnh:** đội có app Local AI với tính năng Meeting AI (nhiều AI cùng bàn một câu hỏi, có vai Operator cite code, đăng nhập web do owner tự làm trong WebView2, không lưu mật khẩu — `SourceCode/LocalAI`, cổng `--meeting-selftest` 30/30, `--webtro-selftest` 7/7). Hiện AI ngoài tham gia bằng cách owner copy câu hỏi/đưa file rồi dán trả lời về. Owner (03/10 07:5x): *«yêu cầu AI tham gia AI Meeting bằng cách của họ, không thì mình làm web login»*.
2. **Câu hỏi:** bạn có kênh nào để tham gia trực tiếp một phiên meeting do chúng tôi điều phối — API có khoá (model/endpoint, giới hạn lượt, giá), webhook/polling một repo GitHub công khai (như repo Ask/Answer hiện nay), MCP/connector, hay chỉ qua giao diện web (khi đó chúng tôi sẽ làm web login cho bạn vào)? Hãy nêu đúng cách bạn dùng được, định dạng tin nhắn bạn muốn nhận/trả (markdown, JSON), và ràng buộc (độ dài, tần suất, đăng nhập).
3. **Trả lời tốt trông thế nào:** liệt kê rõ CÓ/KHÔNG cho từng kênh, ví dụ URL/endpoint/định dạng, điều kiện (tài khoản, khoá API, chi phí), và đề xuất quy trình tối thiểu để chúng tôi nối bạn vào trong tuần này.
