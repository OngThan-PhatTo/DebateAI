# ASK 01 — bộ hỏi cho OxAlpha — tệp 67/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 331 (D3-20)

### D3-20 — Meeting AI tự lái web: khi phiên web của CHÍNH BẠN hết hạn, trang hiện gì, và gửi lại câu hỏi thế nào để không trùng?
1. **Bối cảnh:** Local AI «Meeting AI» lái trang chat của các AI ngoài trong WebView2 (owner tự đăng nhập; app KHÔNG BAO GIỜ gõ vào form đăng nhập). Trước vá, trang bị đăng xuất rơi vào «Lỗi» vì không thấy ô nhập (`SourceCode/LocalAI/Meeting/WebTroWebView2.cs:225-226` «KHONG_THAY_O_NHAP»). Sổ `_LANE_NOTE/lock/w_localai_t19b2_0210_2026-10-02.md` mục 3–6.
2. **Đội đã chọn:** nhận diện đăng xuất MẠNH = ô `input[type=password]` đang hiện hoặc URL đăng nhập (/login, /signin, accounts.google.com…); YẾU = chữ «log in to continue / session expired…» + mẫu `dang_nhap.mau` của hồ sơ (không tính chữ nằm trong chính câu vừa gửi); trạng thái «chờ owner đăng nhập», hạn 30 phút ⇒ «quá hạn» (không phải Lỗi); đăng xuất SAU khi đã gửi mà không còn khối trả lời ⇒ đăng nhập lại xong GỬI LẠI phần đó ĐÚNG 1 lần.
3. **Phương án khác:** không tự gửi lại, chờ owner bấm; ưu tiên API khi có.
4. **Câu hỏi** (mỗi AI trả lời cho SẢN PHẨM CỦA CHÍNH MÌNH, giống K7): khi phiên web hết hạn, (i) trang chuyển tới URL nào, phần tử nào xuất hiện (selector / aria-label nếu biết); (ii) câu trả lời đang viết dở có còn sau khi đăng nhập lại không; (iii) gửi lại cùng câu hỏi có bị chặn / đánh dấu spam không?
5. **Trả lời tốt:** mô tả kèm URL trang trợ giúp chính thức; ô nào không biết ghi «không biết» — KHÔNG bịa selector.

---

## Câu 332 (NC-01)

### NC-01 — «Một net chung» tàn cuộc có thể LỒNG vào net hiện tại của từng engine, hay làm net khởi điểm cho engine khác kiến trúc không?
1. **Bối cảnh:** đội có 3 họ net: (a) Fairy-Stockfish xiangqi (arch `0x7AF32F20/0x3C103E72`) — Docco, Bodetosu, Trương Vô Kỵ hiện nạp CÙNG một tệp CC0 `xiangqi-c07e94a5c7cb.nnue` (sha256_16 `C07E94A5C7CBEAE4`, 11.261.932 B); (b) Pikafish 2026 — OngThan avx2/bmi + ThanDieuDaiHiep (net official cấm thương mại, trainer đang port T-40); (c) net piece-square riêng — Nữ Oa/NhuLai; HongQuanLaoTo/La Hầu không NNUE. Đã đo: nạp net CC0 vào ThanDieuDaiHiep (khác arch-hash dù cùng version `0x7AF32F20`) ⇒ `ERROR`/`NETHONG`, bench 0 nút (`AI_Debate/onGoing/NGUON_ENGINE.md:99`). Thử fine-tune từ net CC0 (lô N5, 20/09) ra net **yếu hơn −301 Elo** do dữ liệu (`NGUON_ENGINE.md:103-105`). Kho nhãn tàn cuộc `positionendgame.fendb`: 11.950.107 thế (11.900.907 nhãn CHÍNH XÁC bộ giải lùi ≤ 2 quân tấn công · 47.269 chessdb · 1.931 chưa), 132.327.311 nước có xếp hạng, 62.617.776 cờ `nuoc_dung` (`SourceCode/Shared/DataFen/kho_tan_cuoc.py:102-111`, đo 03/10 06:0x).
2. **Câu hỏi:** với 3 họ kiến trúc trên, «lồng một net chung vào net hiện tại» có cách nào ngoài (i) chia sẻ TẬP NHÃN rồi fine-tune từng họ, (ii) distill từ một «thầy tàn cuộc» chung? Trong cùng họ Fairy, gộp trọng số (weight averaging / model soup) của bản fine-tune tàn cuộc với net gốc C07E94A5 có an toàn không, điều kiện gì (cùng init, cùng quantization int8 1/64)? Có kiến trúc «hai net chọn theo số quân» (kiểu Stockfish small/big net theo vật liệu) đã dùng cho cờ tướng chưa, lợi/hại so với một net?
3. **Trả lời tốt trông thế nào:** nói rõ cái KHÔNG làm được (chuyển trọng số khác arch) và cái làm được (nhãn chung, distill, averaging cùng arch) kèm nguồn (nnue-pytorch, Stockfish NNUE docs, bài model soup); công thức/điều kiện gộp trọng số; đề xuất một lịch cụ thể cho họ Fairy (net chung cho Docco/Bodetosu/TVK) và cách kiểm (A/B nodes cố định, ρ(net, −DTC) trên ô đã giải chính xác).

---

## Câu 333 (NC-02)

### NC-02 — Gom nhiều engine tính depth cao cho thế 4v4 CHƯA có nhãn: hợp nhất nhãn thế nào, depth bao nhiêu, kiểm bằng gì?
1. **Bối cảnh:** engine chấm được: 2 bản quyền (Cyclone ≤ 2 con, BugChess ≤ 2 con — mở rồi giữ, nghỉ ≥ 60 s giữa lần mở, `SourceCode/BookTool_Xiangqi/ThayBanQuyen.cs:217,247`; XQMS không giới hạn), engine nhà Docco/Bodetosu/TVK/OngThan. Số đo chi phí hiện có: thầy Docco 0,0295 s/thế @ 5.000 nút (`ncb03_job` dry200), probe chessdb 12.141 thế/7,3 h (`lo_nap` 25); chưa đo thầy bản quyền ở depth cao (câu Q5 phiếu TRAIN 25/09 còn trống). Kho có **bộ hiệu chuẩn miễn phí**: 11,9 triệu thế ≤ 2 quân tấn công đã giải CHÍNH XÁC (wdl + dtm + dtc + nước đúng) để đo sai số nhãn engine theo depth. Nhãn chessdb `W|D|L-M-n` không phải DTM thuần khi bên thua còn quân tấn công (16/63 nước ngắn hơn 1–2 ply, `tan_cuoc_liet_ke.py:1749-1752`). Mục lục tàn cuộc 42.687 ô, chỉ 24 ô XONG; 3v1…3v3 ước 10¹⁴–10²⁰ thế ⇒ phải lấy MẪU (cột `cach=MAU` 41.844 ô).
2. **Câu hỏi:** (a) giao thức hiệu chuẩn: chạy từng engine ở depth D trên mẫu của các ô đã giải chính xác ⇒ đo tỉ lệ sai WDL và sai nước đúng theo D ⇒ chọn D tối thiểu đạt sai số < x %; x nên là bao nhiêu để nhãn còn có ích cho train? (b) khi 2–4 engine không đồng ý trên một thế: hợp nhất bằng bỏ phiếu WDL, lấy engine depth sâu nhất, hay giữ cả phân phối (nhãn mềm) + ghi độ bất đồng làm trọng số loss? (c) «nhiều phương án tính» = lấy MultiPV k nước tốt nhất làm nhãn chính sách — k bao nhiêu, cắt theo chênh cp nào? (d) ước thời gian: với 2 Cyclone 12 lõi + 2 BugChess 8 lõi trên PC 44 lõi (gói `sinhgame.zip`), depth ~22–26, bao nhiêu thế/giờ là thực tế và một mẫu bao lớn cho 3v2/3v3/4v4 là đủ để train (so với 11,9 triệu thế chính xác bậc ≤ 2)?
3. **Trả lời tốt trông thế nào:** giao thức đo có ngưỡng ghi TRƯỚC, công thức hợp nhất nhãn có nguồn (ensemble distillation, soft labels), ước lượng thế/giờ theo depth có cơ sở, cảnh báo bẫy (engine báo mate giả ở cờ thế — A3-11; nhãn cp ở thế hoà theo luật không ăn quân 60/80 ply).

---

## Câu 334 (NC-03)

### NC-03 — Lấy mẫu không gian 3v3/4v4 (10¹⁹–10²³ thế) để train: phân phối nào, bao nhiêu thế mỗi ô, kiểm tổng quát hoá ra sao?
1. **Bối cảnh:** owner chốt phạm vi 0v0…4v4 quân tấn công × sĩ/tượng 0→đủ, «vài chục triệu hình là đủ» (LANE_LOCK ĐP 933); đội đã có mục lục ô thấp→cao với `so_the_uoc` từng ô (`kho_tan_cuoc.py` bảng `o`), bộ giải lùi 2.099–2.832 thế/s/tiến trình nhưng 57 ô 2v1+3v0 ≈ 2,1·10⁹ thế ≈ 278 giờ-tiến trình, kho ≈ 1,1 TB ⇒ không liệt kê đủ trên máy này (`DEBATE_POSITIONENDGAME_LAM_XONG_2026-09-26.md:36-37`). Grok (Answer 01) đề nghị: không vét 4v4, bậc ≥ 2 chỉ gán nhãn mẫu, WDL chính + cp-DTM phụ. Chưa có số: bao nhiêu mẫu/ô và phân phối mẫu (ngẫu nhiên hợp lệ · từ ván thật · sát biên thắng/hoà) cho net tổng quát hoá.
2. **Câu hỏi:** cho một ô 4v4 cỡ 10¹⁵–10¹⁹ thế, phân phối mẫu nào cho NNUE học được quy luật (ngẫu nhiên đều, lấy từ ván engine, hay tập trung vùng biên WDL / dtm ngắn), cỡ mẫu tối thiểu mỗi ô theo kinh nghiệm EGTB-rescoring của Stockfish/Fairy (Syzygy ≤ 7 quân) là bao nhiêu, và kiểm tổng quát hoá bằng gì (giữ lại ô chưa thấy, đo ρ(net, WDL) + sai nước đúng trên ô giải chính xác)? Có nên ưu tiên lấy mẫu từ chính thế tàn cuộc xuất hiện trong `positions.fendb`/ván thật (cột `so_quan_tan_cong`, ĐP 943) thay vì không gian liệt kê?
3. **Trả lời tốt trông thế nào:** quy tắc lấy mẫu có nguồn (TB rescoring trong nnue-pytorch, bài về coverage của EGTB trong dữ liệu train), con số mẫu/ô theo bậc, cách kiểm tổng quát hoá, cảnh báo lệch phân phối với ván thật.

---

## Câu 335 (NC-04)

### NC-04 — Giả thuyết owner «tới 4v4 quân là cơ bản có net mạnh»: biết hết tàn cuộc ≤ 4v4 mang lại bao nhiêu Elo cho engine cờ tướng?
1. **Bối cảnh:** đội đo eval tàn cuộc yếu (pháo đài −711 vs search −8; r vùng tàn 0,57 — `AI_Debate/onGoing/CHOT_OWNER_2026-09-12_TOI.md:197`); engine nhà có EGTB probe lúc đánh (cổng `probe_uci_10fen.py`), còn net thì chưa học tàn cuộc. Owner muốn ưu tiên nguồn lực (giờ engine bản quyền, GPU) cho nhãn ≤ 4v4 trước trung cuộc (ASK11 §1 «train từ gốc lên»).
2. **Câu hỏi:** có số đo nào (Stockfish Syzygy, Pikafish/Fairy EGTB, cutechess) về Elo tăng khi engine «biết hết» tàn cuộc ≤ N quân — tách riêng (a) dùng bảng tra trong search và (b) net đã học nhãn tàn cuộc — để ước lượng phần Elo thực sự đến từ tàn cuộc ≤ 4v4 so với trung cuộc? Với cờ tướng (nhiều hoà, luật không ăn quân 60/80 ply), vùng nào của tàn cuộc đổi Elo nhiều nhất (chuyển hoá thắng, giữ hoà, pháo đài)?
3. **Trả lời tốt trông thế nào:** số Elo có nguồn (test fishtest/Pikafish, báo cáo EGTB), tách (a)/(b) rõ, kết luận thẳng giả thuyết «≤ 4v4 ⇒ net mạnh» đúng tới đâu, và gợi ý phép đo nội bộ rẻ (A/B có/không EGTB probe; A/B net tàn cuộc vs net gốc trên bộ thế tàn cuộc thật từ ván của đội).
