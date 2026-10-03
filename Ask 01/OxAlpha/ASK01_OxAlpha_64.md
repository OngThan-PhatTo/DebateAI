# ASK 01 — bộ hỏi cho OxAlpha — tệp 64/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 316 (D3-05)

### D3-05 — A/B net với engine TẤT ĐỊNH (Threads 1 + nodes cố định): A=A thành phép thử đối xứng, 10 thế xuất phát ⇒ ván lặp lại y hệt; với n nhỏ nên đo thế nào?
1. **Bối cảnh:** rig `_LANE_NOTE/_scratch_code_luu/w_net_ab_0210/ab_net.py` gọi BookTool `AbNgan` (`SourceCode/BookTool_Xiangqi/AbNgan.cs:282-313`: nodes cố định, Ponder tắt, NoBook, luật 80 ply, A=A + đối chứng DƯƠNG nodes ×4). Lô mồi 02/10 (nodes 50.000, Threads 1, Hash 64): Docco A=A +4 =4 −4 / 12 ⇒ Elo 0 [−108; +108], dương +12 =4 −0 / 16 ⇒ +338 [+176; +∞]; Bodetosu A=A 0 [−108; +108], dương +255 [+96; +807]; TVK y hệt Docco. Phát hiện (sổ `_LANE_NOTE/lock/w_net_ab_0210_2026-10-02.md` §5): Threads 1 + nodes cố định là TẤT ĐỊNH ⇒ hai ván cùng thế đảo màu đánh lại đúng chuỗi nước (độ dài 90/90, 117/117…); `AbNgan` lấy thế `(g/2) % N` từ `EngineMatch.BalancedOpenings()` = **10 thế** (`SourceCode/BookTool_Xiangqi/EngineMatch.cs:811-842`) và bản CLI không đọc bộ thế ⇒ `soVan > 20` nhân đôi n giả, KTC95 hẹp giả.
2. **Đội đã chọn:** nodes 50.000 (NPS đo 241k–690k; 100k ⇒ ~2 phút/ván, quá dài cho «test ngắn» luật 18/09); trần số ván = 2 × số thế; ván trùng hệt ⇒ rc 6; vá BookTool đọc bộ thế `SourceCode/Shared/LuatCo/bo_the_khaicuoc_binhthuong.txt` (35 thế ⇒ tới ~70 ván không lặp); KTC95 + SPRT hai phía dừng sớm trên lô 20 ván; chọn net khi cận dưới KTC95 > 0.
3. **Phương án khác:** (a) phá tất định: nodes ± ngẫu nhiên nhỏ mỗi nước, hoặc Threads 2; (b) bộ thế lớn (hàng nghìn thế cân bằng, kiểu UHO của cờ vua) — phải có nguồn KHÔNG từ book (luật NoBook); (c) thống kê pentanomial theo cặp ván.
4. **Câu hỏi:** (i) Với engine tất định, A=A còn chứng minh được gì, nên thay bằng đối chứng nào? (ii) Công thức thống kê đúng cho cặp ván cùng thế đảo màu (pentanomial — công thức KTC / SPRT theo fishtest) khi n chỉ 20–70 ván? (iii) Có bộ thế khởi đầu cân bằng cho cờ tướng công khai, giấy phép rõ, như UHO / 8moves của cờ vua không?
5. **Trả lời tốt:** công thức + URL (fishtest wiki, cutechess `-repeat`, bài pentanomial SPRT); tên bộ thế + giấy phép + cách sinh.

---

## Câu 317 (D3-06)

### D3-06 — Dữ liệu train xuất từ ván Engine Fight: `.plain` hay binpack; điểm search ngay trong ván có dùng làm nhãn được không?
1. **Bối cảnh:** T-28 `SourceCode/Shared/EngineFight/EngineFightGopRaNet.cs` (sổ `_LANE_NOTE/lock/w_t28_ef_0210_2026-10-02.md` §0 mục 7): `van.jsonl` ⇒ `.plain` (fen / move / score / ply / result; điểm theo bên đi), CHỈ ván NoBook, bỏ ván hỏng / 0 nước, bỏ nước không điểm và |điểm| ≥ 29000, khử trùng theo FEN chuẩn hoá, ván có «Thầy» xếp trước; 0 thế ⇒ rc 2. Trainer của đội: Docco (fork `variant-nnue-pytorch`), OngThan (`pikafish-nnue-pytorch`), Bodetosu (`train_nnue_gpu_v3.py` tự viết), TVK theo khuôn Docco. Lane ghi «chưa chắc (Q-a)»: trainer từng engine đọc đuôi nào.
2. **Đội đã chọn:** xuất `.plain` chuẩn, chuyển định dạng là việc của trainer; điểm lấy NGUYÊN từ search trong ván (theo đồng hồ / nodes của giải).
3. **Phương án khác:** (a) xuất thẳng `.bin` / `.binpack` (khuôn nào cho bàn 90 ô?); (b) chỉ lấy FEN từ ván rồi cho thầy chấm lại ở nodes cố định (như D3-04) để đồng nhất thang điểm.
4. **Câu hỏi:** (i) `variant-nnue-pytorch` và `pikafish-nnue-pytorch` đọc trực tiếp định dạng nào cho xiangqi (plain / bin / binpack), có công cụ chuyển / rescore chính thức nào? (ii) Điểm search trong ván đấu (độ sâu thay đổi theo đồng hồ) làm nhãn có hại không — thực hành của Stockfish / Pikafish với dữ liệu «điểm trong ván» thế nào? (liên quan T34-01)
5. **Trả lời tốt:** tên tệp / hàm loader trong repo upstream + URL; tên tool chuyển định dạng nếu có.

---

## Câu 318 (D3-07)

### D3-07 — Dùng px0data (ODbL) chỉ NỘI BỘ để train net: net ra là «Produced Work» hay «Derivative Database», còn bán được không?
1. **Bối cảnh:** px0data (dữ liệu Pika Xiangqi Zero, ~10,3 GB đã có trên máy) được README / Kaggle khai theo **ODbL**; đội đã sửa ví dụ sai `--giay-phep CC0` ở `SourceCode/Shared/LuatCo/xoa_sau_nap.py:45` (sổ `_LANE_NOTE/lock/w_t13_vua_0210_2026-10-02.md` §1e). Luật owner 20/09 15:1x mục 3: không trộn dump Pika Zero (ODbL) vào dữ liệu cho net bán. Nửa sau việc CT-B3-04 (bộ đổi chunk px0data → FEN cho dùng nội bộ) đang chờ owner quyết.
2. **Đội đã chọn:** CHƯA làm bộ đổi px0data → FEN; cổng đóng gói sẽ ĐỎ nếu net train từ px0data bị gắn cờ bán.
3. **Phương án khác:** (a) px0data chỉ để đo / đánh giá, không train; (b) train net NỘI BỘ (không ship) từ px0data; (c) chỉ dùng kết quả ván (thắng/hoà/thua), không dùng nhãn của px0.
4. **Câu hỏi:** theo văn bản ODbL 1.0 (các điều về «Produced Work», «Derivative Database», share-alike và ghi chú nguồn), trọng số mạng nơ-ron train từ cơ sở dữ liệu ODbL thuộc loại nào? Có ý kiến chính thức của Open Data Commons / OSMF (OpenStreetMap + máy học) hay án lệ nào không? Nếu là Produced Work thì ghi chú tối thiểu khi phân phối là gì?
5. **Trả lời tốt:** trích điều khoản ODbL kèm số mục + URL opendatacommons.org; ý kiến chính thức nếu có; «không có án lệ» + mức rủi ro ước lượng.

---

## Câu 319 (D3-08)

### D3-08 — `setoption` khi search đang chạy (kể cả `go infinite` / `go ponder`): dừng + join rồi ghi, chờ search tự xong, hay từ chối?
1. **Bối cảnh:** Nữ Oa `SourceCode/Engine_Xiangqi/engine-project/OngThan/src/ucci.cpp:203` (bản trước vá, `ef6d4951`) ghi `gConfig` TRƯỚC mọi bước dừng trong khi searchThread đọc các ô đó ở mỗi nút (`board.cpp:36`, `search.cpp:251,432,434,533,536,621,631`, `search_smp.h:234-236`); chuỗi > 15 ký tự (MSVC SSO) nằm trên heap ⇒ gán lại giữa lúc đọc = đọc vùng đã giải phóng; `gBook.close()` (`sqlite3_close`) chạy khi search đang `probe()`. Bodetosu đã `stopAndJoin()` trước `gConfig.setOption` từ 13/09 (`SourceCode/OngThan_variants/Bodetosu/src/ucci.cpp:348`) nhưng còn 4 nhánh đứng trước dòng đó (`ProfileNodes`, `NganDem`, `Rule`/`EvalBlend` giá trị rác, lệnh `conthist`) ghi trạng thái mà search đọc. GUI của đội (TieuLongNu) có gửi setoption giữa `go infinite`.
2. **Đội đã chọn** (sổ `_LANE_NOTE/lock/w_nuoa_e12_0210_2026-10-02.md` §2 mục 5–7, `_LANE_NOTE/lock/w_bodetosu_e12_0210_2026-10-02.md` §2): MỌI setoption (kể cả option lạ): search còn sống ⇒ in `info string setoption <tên>: search con song -> dung …`, `stopAndJoin()` (search nhả `bestmove` ngay) rồi mới ghi; search đã tự xong ⇒ chỉ join, không in. GUI muốn phân tích tiếp phải `go` lại.
3. **Phương án khác:** (a) CHỜ search tự xong rồi ghi (với `go infinite` sẽ treo tới khi có `stop`); (b) xếp hàng setoption, áp ở lần `go` sau; (c) chụp cấu hình riêng cho từng lượt search (phải sửa ≥ 23 chỗ đọc `gConfig.*`).
4. **Câu hỏi:** đặc tả UCI (bản Stefan Meyer-Kahlen) và UCCI nói gì về `setoption` khi engine đang tính — «chỉ gửi khi engine đang chờ» là nghĩa vụ của GUI hay engine phải tự xử lý? Stockfish / Pikafish hiện xử lý thế nào (tên hàm trong `uci.cpp` / `engine.cpp`)? Dừng-và-nhả-`bestmove` có làm GUI phổ biến (Arena, cutechess-cli, BHGui, SharkChess) hiểu nhầm thành nước đi thật không?
5. **Trả lời tốt:** trích đặc tả + tên hàm / dòng upstream có URL commit; hành vi GUI có nguồn.

---

## Câu 320 (D3-09)

### D3-09 — Hai hành vi biên của engine: Hash không phải luỹ thừa 2, và `go mate N` bị `stop` trước khi tìm thấy chiếu bí
1. **Bối cảnh:** Nữ Oa `tt.h:164-165` (trước vá) làm tròn LÊN luỹ thừa 2 ⇒ `Hash 3000` thành 4096 MB (đúng đại lượng commit làm máy đội sập 02/10 14:15), `new TTBucket[n]()` không `nothrow` ⇒ thiếu bộ nhớ là chết engine. Bodetosu `go mate N` chạy DFS đồng bộ ngay trên luồng đọc stdin (`SourceCode/OngThan_variants/Bodetosu/src/ucci.cpp:656-684` trước vá) ⇒ thế `3akab2/9/4b4/9/9/9/9/2N1C4/4R4/2BK5 w - - 0 1` với `go mate 30` chưa xong sau 60 s và `stop` không được đọc (sổ `_LANE_NOTE/lock/w_engine_cao_0210_2026-10-02.md` §1, §4.2).
2. **Đội đã chọn** (đã build + deploy): Hash làm tròn XUỐNG luỹ thừa 2 ≤ yêu cầu (giữ `hash & mask` ⇒ bench bit-exact), cấp phát `nothrow` lùi từng nấc ½, readback `info string Hash: yeu cau X MB -> TT that Y MB`; `go mate` chạy trong searchThread, kiểm cờ dừng mỗi nút — bị dừng ⇒ `info string go mate: dung truoc khi tim thay chieu bi` + `bestmove` = nước hợp lệ ĐẦU TIÊN (không bao giờ báo mate giả); không có mate ⇒ `info string no forced mate` + search thường. Đo sau vá: `stop` → `bestmove` 1 ms.
3. **Phương án khác:** Hash đúng X MB bằng chỉ số nhân-dịch (`mul_hi64(key, số cụm)`) — đổi chỉ số TT ⇒ bench đổi; `go mate` bị dừng ⇒ trả nước tốt nhất của một search ngắn thay vì nước hợp lệ đầu tiên.
4. **Câu hỏi:** (i) Stockfish / Pikafish / Fairy xử lý Hash không phải luỹ thừa 2 thế nào, có báo lại kích thước thật không; cấp phát Hash hỏng thì nên báo gì cho GUI? (ii) Đặc tả UCI / UCCI quy định gì về `bestmove` sau `stop` của `go mate N` khi chưa có kết quả — trả nước nào là chuẩn ngành?
5. **Trả lời tốt:** trích mã upstream (tệp / hàm + URL commit) và đặc tả.
