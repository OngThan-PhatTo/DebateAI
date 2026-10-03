# ASK 01 — bộ hỏi cho OxAlpha — tệp 52/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 256 (LC1-05)

**Mục LC1-05 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A1-05: dùng start /AFFINITY 0xFFFFFFFFFF để chạy quá 64 luồng». Bằng chứng của đội: mặt nạ affinity chỉ áp trong MỘT processor group (≤ 64 LP) — không vượt được 64 (A1-64/A1-65 chốt 21/09; CLAUDE.md 23/09). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 257 (LC1-06)

**Mục LC1-06 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A1-06: «net nhỏ (35K tham số dense + FT thưa)»». Bằng chứng của đội: lặp số 35K đã bị bác và BigPickle đã NHẬN (BPCK-02); FT 10530×512 ≈ 5,4 M. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 258 (LC1-10)

**Mục LC1-10 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A1-10: 500–2000 mẫu/s ⇒ «~10–40 ngày/epoch»». Bằng chứng của đội: số học/đơn vị: 2×10⁹ mẫu (20M×100) ÷ 500–2000/s = 11,6–46 ngày cho CẢ 100 epoch, không phải mỗi epoch. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 259 (LC1-12)

**Mục LC1-12 — T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat (SAI):** AI T13-16 — Phần I bổ sung (lô B2, B4–B9): Longcat nói: «A1-12: «Intel Arc mới hỗ trợ ROCm»». Bằng chứng của đội: Intel Arc đi qua XPU (Intel Extension for PyTorch / oneAPI); ROCm là ngăn xếp của AMD. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 260 (T13-17)

### T13-17 — Phần I bổ sung (lô B2, B4–B9): Grok — SAI 10 · BỊA 0
Hỏi lại **đúng Grok**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Grok nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| GRK-ENG-E17 | **SAI** | TDDH setoption giữa phân tích ⇒ dừng search, trả bestmove sớm; GUI (TLN DatTuyChon) có thể hiểu nhầm | uci.cpp:795-797 `dung_search_neu_dang_chay("setoption")` + wait_for_search_finished; TLN UcciEngine.cs:1587-1591 gửi setoption không chờ rảnh | REVIEW_3009_ENGINE |
| GRK-KV-4B-MANUS | **SAI** | Manus Answer 01:99 «không có source Kỳ Viện trong repo để trích Program.cs hay ChessHub.cs» (tác giả Manus; Grok phán S… | Manus/Answer 01/Manus.md:99 đúng câu đó; ls ⇒ C:\AI\SourceCode\App_CoTuong_Online\Program.cs (sha256 7DDB054F…) và ChessHub.cs tồn tại, ~167 tệp .cs; Grok ĐÚNG… | REVIEW_0110_KYVIEN |
| GRK-KV-G22b | **SAI** | «Jeiqi» trong đường Root (:26) là viết sai | appsettings.json:26 «C:\ChineseChess\Jeiqi Engine\OngThan» đúng tên thư mục thật: «ls -d /c/ChineseChess/Jeiqi Engine/OngThan» ⇒ tồn tại; CLAUDE.md cũng gọi «J… | REVIEW_0110_KYVIEN |
| GRK-TB-B9 | **SAI** | Stop TryEnter 200 ms không lấy được khoá vẫn Dispose pipe ⇒ luồng đọc có thể ném ObjectDisposed (chú thích nói cố ý) | Engine.cs:1310 `TryEnter(lk, 200)`; :1364-1367 Dispose _pinStdin/_pinStdout bất kể gotLock | REVIEW_3009_TLN_BT |
| GRK-TB-T15 | **SAI** | Đường dẫn máy dev viết cứng (EngineSettings.exe, demo .obk) ⇒ máy khác mất tính năng/mở nhầm | HopChonNho.cs:382 nằm trong `#if DEV_MAY` (build_all.bat:320-325: đường máy dev chỉ ở DEV_MAY, gói khách 0 hit); KhungCongCu.cs:2406/2530 trong `if (!boQuaDev)… | REVIEW_3009_TLN_BT |
| GK-0.3 | **SAI** | 0.3: Không 1 txn 30 GB; bỏ REAL trùng; CAST REAL→INT «chỉ khi nguyên & trong int64»; SQL `typeof(vkey)`; loại 0/I64MIN/… | Số 567.206.917 / 92.495.146 / 64.795.168 / 7.830.204 / 494.581.545 khớp :9. `typeof(vkey)` đã có ở 9 script DataFen | ASK01_2609 |
| GK-A3-05/06 | **SAI** | A3-05/06: THUA && dtc>remain ⇒ trả cận hoà; Felicity MIT `.fexq`; Elo ≥50 = ước TalkChess, không phải số đo | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| GK-A3-07 | **SAI** | A3-07: Đổi 120→80 giữ nấc /8 từ 14 có thể va TT — phải đo; hằng 120 KHÔNG BIẾT | đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma… | ASK01_2609 |
| GK-A3-09 | **SAI** | A3-09: Khác lọc/luật chiếu-đuổi mãi; không khẳng định :182/:209/:275 | A01-9 đo 27/09: lệch là do không gian chỉ số Felicity (krk 4.806 tái lập tuyệt đối, `tan_cuoc_liet_ke.py:56-57`), không phải luật chiếu/đuổi mãi · chấm Fable 2… | ASK01_2609 |
| A1-06-GK | **SAI** | A1-06: nghĩa vụ giao nguồn GPL theo §6(a) | CHAM_ANSWER01_A1-01A1-18_2026-09-21.md §5 «Chưa chốt» 1: §6(a) chỉ cho vật mang vật lý; mô hình tải về của đội là §6(d). 02/10: `_LANE_NOTE/_tools/goi_khach/go… | ASK_A1_2109 |
