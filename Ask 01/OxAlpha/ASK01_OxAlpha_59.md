# ASK 01 — bộ hỏi cho OxAlpha — tệp 59/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 291 (KK3-02)

**Mục KK3-02 — T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT (SAI):** AI T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT nói: «Trên Windows ưu tiên DXGI QueryVideoMemoryInfo (Budget/CurrentUsage); từ chối khi peak > min(80 % RAM vật lý, DXGI budg…». Bằng chứng của đội: đo 02/10 LaneScratch/w_t13_tonghop_0210/dxgi_budget_ketqua.txt (dxgi_budget.ps1, rc 0): Radeon 8060S LOCAL budget 46,72 GiB, CurrentUsage 0,00 GiB trong khi Gl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 292 (T13-21)

### T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 — SAI 4 · BỊA 0
Hỏi lại **đúng Grok47**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Grok47 nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| G47A1-L201 | **SAI** | L-2-01 NẶNG: `egtb.cpp:298-301` dtc>remain thì bỏ bảng — nên trả cận hoà | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| G47A2-B2 | **SAI** | Bốn chỗ #2: EGTB Bodetosu bỏ bảng khi dtc>remain — hướng sửa: giữ bảng, trả cận hoà | đo 27/09 23:0x A01-7 ĐÓNG — KHÔNG ỦNG HỘ (`_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): 13/20 thế dtc>remain vẫn THẮNG trong 80 ply (ăn quân đặt lại… | ASK02_2709 |
| G47A2-C4 | **SAI** | C4: `dwMemoryLoad` tính cả standby file cache ⇒ quyết «còn RAM» bằng `ullAvailPhys` | đo lane này 02/10 ~12:2x (ctypes GlobalMemoryStatusEx ×3 + Get-Counter): dwMemoryLoad = 57 đúng bằng 100·(1 − ullAvailPhys/ullTotalPhys) = 57,33 (Avail 20,33 /… | ASK02_2709 |
| G47A2-D7 | **SAI** | D-7: Legal = cộng hai bên, isLegal chỉ lọc tướng đối mặt; 4.806/3.294 «vẫn không tái lập» | A01-9 `de4e28df` 27/09 12:1x (TRƯỚC bài 28/09): quy ước «felicity» = không gian chỉ số ⇒ krk 4.806 · kpk 3.294 đúng tuyệt đối (`Shared/DataFen/tan_cuoc_liet_ke… | ASK02_2709 |

---

## Câu 293 (G47A1-L201)

**Mục G47A1-L201 — T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 (SAI):** AI T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 nói: «L-2-01 NẶNG: `egtb.cpp:298-301` dtc>remain thì bỏ bảng — nên trả cận hoà». Bằng chứng của đội: đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 294 (G47A2-B2)

**Mục G47A2-B2 — T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 (SAI):** AI T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 nói: «Bốn chỗ #2: EGTB Bodetosu bỏ bảng khi dtc>remain — hướng sửa: giữ bảng, trả cận hoà». Bằng chứng của đội: đo 27/09 23:0x A01-7 ĐÓNG — KHÔNG ỦNG HỘ (`_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): 13/20 thế dtc>remain vẫn THẮNG trong 80 ply (ăn quân đặt lại…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 295 (G47A2-C4)

**Mục G47A2-C4 — T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 (SAI):** AI T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 nói: «C4: `dwMemoryLoad` tính cả standby file cache ⇒ quyết «còn RAM» bằng `ullAvailPhys`». Bằng chứng của đội: đo lane này 02/10 ~12:2x (ctypes GlobalMemoryStatusEx ×3 + Get-Counter): dwMemoryLoad = 57 đúng bằng 100·(1 − ullAvailPhys/ullTotalPhys) = 57,33 (Avail 20,33 /…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
