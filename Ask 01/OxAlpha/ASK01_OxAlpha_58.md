# ASK 01 — bộ hỏi cho OxAlpha — tệp 58/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 286 (QW-K4)

**Mục QW-K4 — T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen (SAI):** AI T13-19 — Phần I bổ sung (lô B2, B4–B9): Qwen nói: «K4: YOLOv8n/11n fine-tune đa skin thay template; «đã bắt» = **28–32 quân** + đối xứng; ADB tap / LDPlayer API; không bắ…». Bằng chứng của đội: K4 đòi «không cần huấn luyện lớn» — fine-tune YOLO cần bộ ảnh đa skin đội chưa có; **28–32 sai**: giữa ván < 28 quân là bình thường, chỉ khai cuộc = 32; đội 2…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 287 (T13-20)

### T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT — SAI 4 · BỊA 0
Hỏi lại **đúng ChatGPT**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | ChatGPT nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| CG-A3-07 | **SAI** | A3-07: đổi 120→80 không an toàn nếu khoá chỉ có bucket thô của bộ đếm | đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma… | ASK01_2609 |
| CG-A2-02 | **SAI** | A2-02: bài ChatGPT — L156 «nửa bước» sai | ô «◐ (L156 «nửa bước» sai)» | ASK02_A2_2309 |
| CG-AUTO-VERIFY | **SAI** | A2-02: tự xác nhận bằng đọc lại 32 quân khai cuộc | như BP-AUTO-VERIFY (`DEBATE_AUTOPLAY…:167`); 02/10 `BocCoManHinh.cs:2330-2343` cổng đọc lại KIỂU VÒNG AUTO (mẫu khác) chứ không đọc lại trên mẫu vừa học | ASK02_A2_2309 |
| KK3-02 | **SAI** | Trên Windows ưu tiên DXGI QueryVideoMemoryInfo (Budget/CurrentUsage); từ chối khi peak > min(80 % RAM vật lý, DXGI budg… | đo 02/10 LaneScratch/w_t13_tonghop_0210/dxgi_budget_ketqua.txt (dxgi_budget.ps1, rc 0): Radeon 8060S LOCAL budget 46,72 GiB, CurrentUsage 0,00 GiB trong khi Gl… | ASK21_KK_2909 |

---

## Câu 288 (CG-A3-07)

**Mục CG-A3-07 — T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT (SAI):** AI T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT nói: «A3-07: đổi 120→80 không an toàn nếu khoá chỉ có bucket thô của bộ đếm». Bằng chứng của đội: đo 27/09 23:0x (A01-16 ĐÓNG — KHÔNG ỦNG HỘ): nấc /4 vs /8 ab 0 [−36; +36] cả gần lẫn xa mốc; `adjust_key60` `ThanDieuDaiHiep/src/position.h:312` + `types.h` ma…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 289 (CG-A2-02)

**Mục CG-A2-02 — T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT (SAI):** AI T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT nói: «A2-02: bài ChatGPT — L156 «nửa bước» sai». Bằng chứng của đội: ô «◐ (L156 «nửa bước» sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 290 (CG-AUTO-VERIFY)

**Mục CG-AUTO-VERIFY — T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT (SAI):** AI T13-20 — Phần I bổ sung (lô B2, B4–B9): ChatGPT nói: «A2-02: tự xác nhận bằng đọc lại 32 quân khai cuộc». Bằng chứng của đội: như BP-AUTO-VERIFY (`DEBATE_AUTOPLAY…:167`); 02/10 `BocCoManHinh.cs:2330-2343` cổng đọc lại KIỂU VÒNG AUTO (mẫu khác) chứ không đọc lại trên mẫu vừa học. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
