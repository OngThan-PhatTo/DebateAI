# ASK 01 — bộ hỏi cho OxAlpha — tệp 50/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 246 (C18-UN-01)

**Mục C18-UN-01 — T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký (SAI):** AI T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký nói: «Danh sách ~200 tên model/LoRA 18+ «để dùng trong workflow»». Bằng chứng của đội: không đạt yêu cầu Ask mục 3 (ASK_COMFYUI_MODEL_18PLUS_2026-09-29.md @ee701c81): 0 mục có tên tệp/byte/link tải; 50 hyperlink trong docx, 21 là trang tìm kiếm/e…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 247 (C18-UN-02)

**Mục C18-UN-02 — T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký (SAI):** AI T13-15 — Phần I bổ sung (lô B2, B4–B9): Không ký nói: «Đưa «Petite adult body LoRA» vào danh sách (kèm «chỉ dùng khung người lớn»)». Bằng chứng của đội: trái yêu cầu 6 của chính Ask (loại tuổi mơ hồ) + luật đội chặn trẻ em; kho đội không có mục này: quét 9 tệp SourceCode/ComfyUI_Pro/ComfyUIPro/Assets/kho/model_…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 248 (T13-16)

### T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat — SAI 11 · BỊA 0
Hỏi lại **đúng Longcat**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Longcat nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| LC-0.2 | **SAI** | 0.2: enforce ∩ = ∅ bằng **trigger** hoặc app; hậu gộp count/hash mẫu/quick_check; giữ 2 tệp nối vkey | SQLite **không** cho trigger tham chiếu bảng ở tệp DB khác (chỉ TEMP trigger) ⇒ «trigger» giữa `datanoscore.db` và `data.db` không làm được; app-level thì đội… | ASK01_2609 |
| LC-0.6 | **SAI** | 0.6: TLN thiếu: «phân tích trực tiếp, **danh sách người chơi online**, hệ thống thái/đấu»; MVVM; 8–10 nút | Ask ASK12-6 liệt kê TLN **đã có** «panel người chơi online», «AUTO Analyze/Full/Stop» ⇒ không đọc đề; «hệ thống thái/đấu» vô nghĩa (lỗi gõ) | ASK01_2609 |
| LC-A3-01 | **SAI** | A3-01: (a) nén thang; (b) mate vào WDL, search lo DTM; (c) UCI mate = NƯỚC, chessdb = ply; **(d) `cp = max(0, 3000 − 5×… | (c) trùng SB-2 (+1 phiếu). (d) **thiếu dấu bên thua**, không kẹp dưới, mâu thuẫn chính (b) của nó; dtm > 600 ⇒ 0 = hoà. Nguồn Fairy-Stockfish repo không trỏ dò… | ASK01_2609 |
| LC-A3-05 | **SAI** | A3-05: cận hoà khi dtc > remain; cẩn thận graph-history; chặn chiếu/đuổi mãi | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| LC-A3-07 | **SAI** | A3-07: 120→80 «có thể» va chạm TT; tăng eval theo dtc | đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma… | ASK01_2609 |
| LC-A3-10 | **SAI** | A3-10: «M-nnnn đếm từ thế hiện tại»; hạn API không biết | đo D3 27/09 (3 lượt queryall): mặc định egtbmetric=dtm trả W-M-0008 (CHẴN) cho bên đang thắng ⇒ không thể là DTM tính từ thế hiện tại; dtc cho W-00-009 lẻ, khớ… | ASK01_2609 |
| LC-A3-11 | **SAI** | A3-11: thầy báo mate giả; trọng số theo đồng thuận; **(c) «UCCI mate = nửa nước»** | họ Stockfish/Pikafish in `mate` theo **NƯỚC** — `pikafish_cf_rebuild/src/uci.cpp:552-553` `(plies+1)/2` (SB kiểm), Fairy `uci.cpp:489`; engine TQ thương mại **… | ASK01_2609 |
| LC1-05 | **SAI** | A1-05: dùng start /AFFINITY 0xFFFFFFFFFF để chạy quá 64 luồng | mặt nạ affinity chỉ áp trong MỘT processor group (≤ 64 LP) — không vượt được 64 (A1-64/A1-65 chốt 21/09; CLAUDE.md 23/09) | ASK01_TH_0110 |
| LC1-06 | **SAI** | A1-06: «net nhỏ (35K tham số dense + FT thưa)» | lặp số 35K đã bị bác và BigPickle đã NHẬN (BPCK-02); FT 10530×512 ≈ 5,4 M | ASK01_TH_0110 |
| LC1-10 | **SAI** | A1-10: 500–2000 mẫu/s ⇒ «~10–40 ngày/epoch» | số học/đơn vị: 2×10⁹ mẫu (20M×100) ÷ 500–2000/s = 11,6–46 ngày cho CẢ 100 epoch, không phải mỗi epoch | ASK01_TH_0110 |
| LC1-12 | **SAI** | A1-12: «Intel Arc mới hỗ trợ ROCm» | Intel Arc đi qua XPU (Intel Extension for PyTorch / oneAPI); ROCm là ngăn xếp của AMD | ASK01_TH_0110 |

---

## Câu 249 (LC-0.2)

**Mục LC-0.2 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «0.2: enforce ∩ = ∅ bằng **trigger** hoặc app; hậu gộp count/hash mẫu/quick_check; giữ 2 tệp nối vkey». Bằng chứng của đội: SQLite **không** cho trigger tham chiếu bảng ở tệp DB khác (chỉ TEMP trigger) ⇒ «trigger» giữa `datanoscore.db` và `data.db` không làm được; app-level thì đội…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 250 (LC-0.6)

**Mục LC-0.6 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «0.6: TLN thiếu: «phân tích trực tiếp, **danh sách người chơi online**, hệ thống thái/đấu»; MVVM; 8–10 nút». Bằng chứng của đội: Ask ASK12-6 liệt kê TLN **đã có** «panel người chơi online», «AUTO Analyze/Full/Stop» ⇒ không đọc đề; «hệ thống thái/đấu» vô nghĩa (lỗi gõ). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
