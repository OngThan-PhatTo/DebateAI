# ASK 01 — bộ hỏi cho OxAlpha — tệp 63/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 311 (A1-06-GM)

**Mục A1-06-GM — T13-27 — Phần I bổ sung (lô B2, B4–B9): Gemini (SAI):** AI T13-27 — Phần I bổ sung (lô B2, B4–B9): Gemini nói: «A1-06: nghĩa vụ giao nguồn GPL theo §6(a)». Bằng chứng của đội: như A1-06-GK (CHAM_ANSWER01_A1-01A1-18_2026-09-21.md §5). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 312 (D3-01)

### D3-01 — 8 net Fairy-Stockfish xiangqi 2022–2023 không có một dòng giấy phép: xin ai, bằng cách nào; nhãn «GPL» của AUR có đè lên CC0 của `C07E94A5` không?
1. **Bối cảnh:** luật owner 02/10 18:1x: net bán được = tự train hoặc net trên mạng có giấy phép thương mại; cấm duy nhất net Pikafish official. Kiểm kê 9 nguồn (sổ `_LANE_NOTE/lock/w_net_sach_0210_2026-10-02.md` §2; văn bản giấy phép lưu ở `_LANE_NOTE/lock/w_net_sach_0210_giayphep/` kèm SHA256): net sạch DUY NHẤT nạp được vào Docco / Bodetosu / Trương Vô Kỵ là `xiangqi-c07e94a5c7cb.nnue` (11.261.932 B) — căn cứ CC0 là README `official-pikafish/Networks` (câu «the weights we train for the xiangqi variant of the Fairy-Stockfish are licensed under CC0»). 8 net cũ `83f16c17fe26 · 28d6221e2440 · bd62afd13f7c · 6f64c55fcb28 · d134cdf48baf · ae0082262b68 · c425c4166a1e · aa162e1771e5` (11,26 MB, header `0x7AF32F20/0x3C103E72` nạp được, HEAD 200 cả 8, Elo trên trang +541…+889) KHÔNG có giấy phép: câu CC0 của trang `fairy-stockfish.github.io/nnue` chỉ áp «networks with a date in 2026 or later»; 10 issue `license` của Fairy-Stockfish không cái nào nói về net. Thêm 2 net 11 MB `4507919dc833…` / `f1ad9ec65699…` và 3 net 22 MB `xiangqi-xy.nnue` (arch `0x3C102EF2`) không rõ nguồn. Gói AUR `fairy-stockfish-xiangqi-nnue` ghi `license=('GPL-3.0-or-later')` cho chính tệp `C07E94A5`.
2. **Đội đã chọn:** 8 + 5 net trên = CHƯA SẠCH ⇒ chỉ đo A/B nội bộ, không ship; coi nhãn GPL của AUR là nhãn của người đóng gói, không phải giấy phép của net; đường có net bán được mạnh hơn = train tiếp từ `C07E94A5` hoặc từ đầu (TU_DAU).
3. **Phương án khác:** (a) xin văn bản giấy phép từ từng tác giả (ianfab / Fabian Fichter, Belzedar, 竹影听溪, Vincentzyx) qua GitHub issue hoặc Discord Fairy-Stockfish — việc gửi tin là của owner; (b) lập luận «trọng số máy sinh không có bản quyền» — đội KHÔNG dựa vào nếu không có nguồn pháp lý.
4. **Câu hỏi:** (i) Có văn bản công khai nào (commit, issue, README cũ, tin Discord được trích lại) gán giấy phép cho các net xiangqi trước 2026 trên trang Fairy-Stockfish NNUE không? (ii) Trường `license=` trong PKGBUILD AUR có giá trị gì với tệp dữ liệu do người khác train? (iii) Nếu xin phép tác giả, văn bản tối thiểu cần có gì (CC0 / MIT cho tệp trọng số, tên tệp + SHA256) để làm hồ sơ nguồn gốc?
5. **Trả lời tốt:** URL cụ thể (commit / issue / thread) + một câu trích nguyên văn; hoặc «không tìm thấy» + nơi nên hỏi. Không đoán tên người giữ quyền.

---

## Câu 313 (D3-02)

### D3-02 — `data.db` 180,8 triệu dòng điểm KHÔNG có cột nguồn: có cách nào nhận ra công cụ / engine / độ sâu đã sinh điểm không?
1. **Bối cảnh:** schema `score(vkey INT64, vmove INT, score INT, atime INT64, vscore INT, vvalid INT, PRIMARY KEY(vkey,vmove)) WITHOUT ROWID` + `mirror(k1,k2,flag)` (kiểm kê chỉ đọc 02/10, `_LANE_NOTE/_scratch_code_luu/w_ncb_0210/kiem_ke_20261002_1921.json`; sổ `_LANE_NOTE/lock/w_ncb_0210_2026-10-02.md` §3). Số đo toàn bảng: 180.813.410 dòng / 107.121.412 thế (52,5 M thế 1 nước, tối đa 82 nước/thế); `score` từ −32000 tới 32000, 107,2 M dòng trong [−50; 50], 3,51 M dòng ≤ −30000, 3,33 M ≥ 30000; `vscore` NULL ở 180.811.977 dòng; `vvalid = 0` ở 27,4 M dòng; `atime = 0` ở 112,5 M dòng, còn lại là dấu giờ dạng `YYYYMMDDhhmmss` từ 20201115073501 tới 20221120222131; mã nước trộn 2 «phương ngữ» (16×16 và native, `SourceCode/BookTool_Xiangqi/A5VmoveDialectSelfTest.cs:6-9`), giải mã duy nhất 99,79 %. Luật owner 26/09: điểm data.db KHÔNG làm nhãn train vì không truy được nguồn (không lập được hồ sơ giấy phép).
2. **Đội đã chọn:** bộ nạp `SourceCode/Shared/DataFen/nap_data_db.py` chỉ xuất THẾ + nước tốt, gắn nhãn `KHONG_BAN`, không xuất cột điểm; điểm train do thầy CC0 chấm lại (D3-04).
3. **Phương án khác:** (a) «vân tay engine»: chấm lại một mẫu thế bằng các engine nghi ngờ ở nhiều độ sâu rồi so phân bố điểm; (b) nhận dạng theo khuôn: bộ cột `vscore/vvalid/atime` + sentinel ±32000 giống khuôn sách của GUI / kho nào (BHGui `.obk` có bảng `bhobk(vkey,vmove,vscore,vwin,vdraw,vlost,vvalid,vmemo,vindex)`, chessdb.cn, …).
4. **Câu hỏi:** (i) Khuôn `score(vkey, vmove, score, atime)` với dấu giờ `YYYYMMDDhhmmss` và mate ±32000 là định dạng xuất của công cụ / kho nào phổ biến trong cộng đồng cờ tướng (BHGui, 兵河, 象棋旋风, chessdb.cn dump…)? (ii) Phương án (a) có tiền lệ / tài liệu nào đủ tin để làm HỒ SƠ NGUỒN (không chỉ đoán), hay về nguyên tắc không chứng minh được?
5. **Trả lời tốt:** tên công cụ + tài liệu định dạng có URL; hoặc «không nhận ra» — đừng đoán tên engine.

---

## Câu 314 (D3-03)

### D3-03 — Nhãn CHÍNH SÁCH từ nhiều nước có điểm của cùng một thế: argmax hay phân bố mềm, khi ~85 % thế có biên top1−top2 < 50 cp?
1. **Bối cảnh:** mẫu 59.971 thế của data.db (cùng tệp kiểm kê D3-02): thế có ≥ 2 nước — biên top1−top2 = 0 ở 434 · 1–49 cp ở 5.824 · 50–199 ở 865 · ≥ 200 ở 226 thế. Phiếu `AI_Debate/onGoing/DEBATE_NET_CO_BAN_DATA_DB_2026-10-02.md:30` còn 3 câu chưa chắc: dùng nhiều nước có điểm làm nhãn chính sách thế nào; cỡ lô mồi tối thiểu (xem D3-04); chuẩn hoá thang `score` của sách (cp? mate?) sang thang trainer.
2. **Đội đã chọn** (`nap_data_db.py`, sổ `w_ncb_0210` §1): `nuoc_tot` = argmax `score` trên các nước giải mã DUY NHẤT và `vvalid != 0`; điểm mate (|score| ≥ 20000) VẪN tính khi chọn argmax; `vvalid` NULL = hợp lệ; hoà điểm ⇒ nước UCI nhỏ hơn (tất định). Nhãn này hiện CHƯA vào trainer nào (NNUE Fairy / Pikafish chỉ học value).
3. **Phương án khác:** (a) phân bố mềm softmax(score / T) trên mọi nước có điểm; (b) chỉ nhận thế có biên ≥ 50 cp; (c) bỏ hẳn nhãn chính sách, data.db chỉ là nguồn FEN.
4. **Câu hỏi:** với mục tiêu NNUE value (kiểu Fairy-Stockfish / Pikafish), có thể thêm đầu policy nhỏ để xếp nước — nhãn policy từ điểm nhiều nước có ích không; nếu có thì (a), (b) hay argmax, T bao nhiêu? Thang `score` của sách (cp kiểu Pikafish? mate = ±32000 − ply?) quy đổi sang thang trainer thế nào (liên quan O-05, T36-03)?
5. **Trả lời tốt:** tiền lệ có URL (Lc0 policy target, KataGo, «policy distillation» cho engine alpha-beta); con số T / ngưỡng kèm lý do; «không có ích cho NNUE value-only» cũng là câu trả lời.

---

## Câu 315 (D3-04)

### D3-04 — Lô mồi «net cơ bản» TU_DAU cho Trương Vô Kỵ: thầy Docco 100k nodes, TT nối giữa các thế, tàn cuộc 0,3 — cỡ lô tối thiểu bao nhiêu thì A/B mới có nghĩa?
1. **Bối cảnh:** job `SourceCode/Shared/DataFen/ncb03_job.py` (sổ `_LANE_NOTE/lock/w_ncb3_0210_2026-10-02.md` §2–§6): thế từ data.db → thầy Docco (net CC0 `C07E94A5`, Threads 1, Hash 64, NoBook, readback NNUE) chấm → trộn tàn cuộc `--bac 1` → train từ net khởi tạo ngẫu nhiên trên CPU (khuôn trainer Docco) → readback net ra trong engine TVK. Dry-run 200 thế (03/10 04:3x): thầy 5.000 nodes = 0,0295 s/thế; nodes ×4 ⇒ điểm đổi ở 191/200 thế, nước đổi 95/200, lệch TB 39,5 cp; 30 bước × 256 mẫu: loss đo độc lập train 0,014917 → 0,014551, val 0,016166 → 0,016151; net ra readback đúng SHA, NNUE. Lô thật ~10.300 thế ở 100k nodes (ngoại suy ~0,26 s/thế ≈ 0,74 CPU-giờ) đang chạy.
2. **Đội đã chọn:** thầy Docco **100.000 nodes**; bỏ nhãn |cp| > 10000 (mate) khỏi lô train; tàn cuộc tỉ lệ 0,3; tắt vòng val 1.000.000 mẫu của trainer (đo loss độc lập thay thế); chia mảnh 250 thế/tiến trình và **TT của engine NỐI giữa các thế trong một mảnh** (nhãn phụ thuộc cách chia — ghi `the_moi_manh` vào KET_QUA).
3. **Phương án khác:** (a) `ucinewgame` / xoá TT trước MỖI thế (nhãn độc lập cách chia, chậm hơn); (b) thầy theo depth cố định thay vì nodes; (c) giữ nhãn mate dưới dạng điểm kẹp thay vì bỏ.
4. **Câu hỏi:** (i) Trong datagen chuẩn (Stockfish `gensfen` / tools, Pikafish, `variant-nnue-tools` của Fairy) TT có được xoá giữa các thế không, có ảnh hưởng đo được tới chất lượng nhãn? (ii) Net khuôn Fairy xiangqi ~11 MB train TỪ ĐẦU cần tối thiểu bao nhiêu thế có nhãn (bậc 10⁵? 10⁷? 10⁹?) trước khi A/B với net khởi tạo cho khác biệt có nghĩa — có số công bố? (iii) 100k nodes cho thầy có hợp lý so với thực hành gensfen (depth hay nodes bao nhiêu)?
5. **Trả lời tốt:** số có nguồn (fishtest / nnue-pytorch wiki / tài liệu Pikafish, Fairy) + cấu hình; hoặc thiết kế ca thử (A=A + dương, ngưỡng đặt trước).
