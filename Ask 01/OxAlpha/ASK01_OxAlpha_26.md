# ASK 01 — bộ hỏi cho OxAlpha — tệp 26/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 126 (T34-01)

### T34-01 — Thế lấy dọc PV của mọi depth: dùng làm NHÃN train hay chỉ làm NGUỒN FEN (đi analyze lại)?
1. **Bối cảnh:** SinhGame nay ghi thêm thế dọc PV của MỌI dòng `info depth d … pv` (mọi MultiPV 4) — `SourceCode/SinhGame_WPF/Loi.cs` `BoSinh.GomThePv`; nhãn = điểm gốc của dòng đó đảo dấu theo bên đi (mate cộng 1 ply/nửa nước), ghi `pv_depth` + `pv_ply`. Đo 02/10 (Docco, Threads 1, `go depth 20 movetime 5000`, 4 slot, 10′): **70.192 thế PV + 451 thế nước đã chơi** (bản cũ cùng lúc: 482); trọng tài LuatCo: 70.643/70.643 hợp lệ. Độ sâu còn lại `pv_depth − pv_ply`: **0–5: 56.502 (80 %) · 6–11: 13.201 · ≥12: 489**.
2. **Câu hỏi:** với net NNUE kiểu Pikafish/Fairy, thế ở cuối PV có độ sâu còn lại 0–5 nên (a) bỏ hẳn, (b) giữ làm FEN đưa vào hàng đợi analyze (`datanoscore`) để engine chấm lại sâu, hay (c) dùng thẳng nhãn nhưng giảm trọng số theo độ sâu còn lại? Ngưỡng độ sâu còn lại tối thiểu nào là phổ biến?
3. **Trả lời tốt:** dẫn thực hành của Stockfish/Leela/Pikafish data-gen (có link), hoặc lập luận + phép thử A/B đề xuất (đối chứng A=A, ngưỡng đặt trước).

---

## Câu 127 (T34-02)

### T34-02 — Sinh dữ liệu nhiều tiến trình 1 luồng trên máy BẬT SMT: chia theo lõi vật lý hay luồng logic?
1. **Bối cảnh:** luật owner 21/09 07:2x buộc tính theo **lõi vật lý** (PC 44 lõi tắt SMT ⇒ không đổi gì). Trên máy nhà 16 lõi / 32 luồng, trần tiến trình engine của SinhGame tụt **25 → 12** (`CauHinhMay.LoiNganSach`, `TaiNguyen.LuongDuocDung`). Đo cũ 21/09: 24 tiến trình ⇒ 17,4 lõi-giây/giây, 30 tiến trình ⇒ 15,4 (thêm tiến trình làm tổng công engine GIẢM — nghẽn băng thông bộ nhớ, `Loi.cs` chú thích `BoDieuToc`).
2. **Câu hỏi:** với engine alpha-beta NNUE 1 luồng (nps bị giới hạn bởi truy cập TT + accumulator), SMT thường cho thêm bao nhiêu % thông lượng thế/giây khi chạy N = số luồng logic so với N = số lõi vật lý? Có số đo công khai (Stockfish fishtest, cutechess concurrency) không?
3. **Trả lời tốt:** số đo có nguồn, hoặc phép đo đề xuất (cùng cấu hình, N = 12/16/24/32, đếm thế/giây + lõi-giây theo PID, cửa sổ ≥ 20 s).

---

## Câu 128 (W456-01)

### W456-01 — Suy nhãn «thắng/hoà theo luật 80 ply không ăn quân» từ bảng DTM/DTC
1. **Bối cảnh:** search Bodetosu bỏ bảng khi `dtc > remain` (`OngThan_variants/Bodetosu/src/egtb.cpp:298-301`, gọi ở `search.cpp:1413-1416`; `remain = max_no_capture_ply − halfMove`, mốc 80). Bảy AI đề nghị «trả cận hoà». Đội đo 27/09 (20 thế `dtc > remain` từ bảng Felicity ≤ 4 quân): **13/20 vẫn THẮNG** trong luật 80 ply vì ăn quân đặt lại bộ đếm; bản trả hoà A/B −26 Elo [−71; +18] ⇒ bỏ ý đó.
2. **Câu hỏi:** Từ bảng DTM/DTC (DTC = số ply tới lần ăn quân kế), có cách nào đúng để gán nhãn WDL theo luật (kiểu cursed win / blessed loss của Syzygy DTZ) mà không sinh lại bảng theo trạng thái (thế, bộ đếm)? `dtc ≤ remain ⇒ thắng theo luật` có đúng một chiều không, và chiều ngược sai ở đâu?
3. **Trả lời tốt:** định nghĩa DTC của Felicity (đếm tới ăn quân của bên nào), chứng minh hoặc phản ví dụ cho cả hai chiều; nếu phải retrograde theo (thế, bên đi, bộ đếm) thì ước kích thước cho ≤ 4 quân + nguồn. «Không chắc» kèm cách kiểm cũng được.

---

## Câu 129 (W456-02)

### W456-02 — Đại lượng rẻ nào báo «học tàn cuộc mà quên trung cuộc» trước khi chơi ván
1. **Bối cảnh:** trainer Bodetosu đã có holdout CỐ ĐỊNH theo nhóm + dừng sớm (`OngThan_variants/Bodetosu/tools/train_nnue_gpu_v3.py:406-407,553-569`) nhưng chỉ một tập. Án lệ đội: 6 net khớp nhãn tốt (valid thấp) vẫn thua A/B −338…−800 Elo; lô N5 −301 Elo do dữ liệu. Ba AI (Grok47, Astra 6, Manus) đề nghị đo hai tập giữ riêng mỗi 2–10 % lô, ngưỡng tự đặt, không số nguồn.
2. **Câu hỏi:** Có số công bố (fishtest, nnue-pytorch, Lc0, Pikafish) cho thấy đại lượng nào ngoài valid loss tương quan với Elo khi trộn kho tàn cuộc vào lô train — và tỉ lệ trộn tàn cuộc/trung cuộc thường dùng?
3. **Trả lời tốt:** tên đại lượng + bằng chứng tương quan có URL; hoặc «không có» + thiết kế ca thử (đối chứng A=A + dương, ngưỡng ghi trước).

---

## Câu 130 (W456-03)

### W456-03 — Bộ tự canh RAM: chỉ số nào khi một tiến trình ghi ~30 GB SQLite
1. **Bối cảnh:** đo 26–27/09: sau ghi ~30 GB SQLite (journal DELETE), `dwMemoryLoad` 95 % trong khi tổng working set mọi tiến trình 25,8 / 47,6 GB; cấp phát 10 GB rồi thả ⇒ 47 %. Đo 02/10: `dwMemoryLoad` = 57 đúng bằng `100·(1 − ullAvailPhys/ullTotalPhys)` = 57,33; `\Memory\Available Bytes` 20,36 GB ≈ Standby 18,69 + Free&Zero 1,61 GB ⇒ standby ĐÃ được tính là rảnh. Vậy «95 %» là bộ nhớ đang dùng ngoài working set tiến trình — ba AI (Grok47, Astra 6, Manus) đều chưa giải thích.
2. **Câu hỏi:** Trên Windows 11, phần nào chiếm (Modified Page List, working set của cache manager / `System Cache Resident Bytes`, view ánh xạ), và bộ canh nên đọc counter nào (cùng commit headroom) để không dừng oan khi IO lớn mà vẫn chặn được treo máy thật?
3. **Trả lời tốt:** tên counter `\Memory\…` chính xác, cơ chế (lazy writer, ngưỡng dirty page) có nguồn Microsoft/Windows Internals, và một cách đo xác nhận trong một lần chạy.
