# ASK 01 — bộ hỏi cho OxAlpha — tệp 32/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 156 (LG-A2-08)

**Mục LG-A2-08 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «A2-08: theo lô 1k–10k; **Lc0 cửa sổ ~100 ván**; λ 0,3 cho cờ tướng; thầy ×2–5; **CPU float32 phải giống hệt GPU**». Bằng chứng của đội: Lc0 cửa sổ dữ liệu cỡ **trăm nghìn ván**, không phải 100; CPU vs GPU float32 **không** bit-exact (thứ tự cộng/cuDNN) — đúng cái đội hỏi. `lc0.mosquitochess.org…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 157 (LG-K2)

**Mục LG-K2 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «K2: lô 100–500k + `merge_progress` + `BEGIN IMMEDIATE`; **`.lock` với `flock` trên Windows**; WAL cho kho thật». Bằng chứng của đội: `flock`/`fcntl` **không có trên Windows** (đội chạy Win11) — phải `LockFileEx`/`CreateFile FILE_SHARE_NONE`. Bảng tiến độ đội **đã có** (`_kho2_tien_do`) · 02/…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 158 (LG-K4)

**Mục LG-K4 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «K4: template matching + tiêu chí 32 quân; **bắt gói mạng + gửi lại là «hợp pháp và bền nhất»**». Bằng chứng của đội: «hợp pháp» không nguồn; Ask A2-03 dẫn chính tác giả BH: *giao thức đổi là tính năng chết* ⇒ «bền nhất» trái bằng chứng đội có. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 159 (LG-K7)

**Mục LG-K7 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «K7: `.lock` bằng `fcntl.flock`; **một `git.log` duy nhất, không file riêng per worker**; lock cũ > 30′ tự xoá». Bằng chứng của đội: `fcntl` không có trên Windows; «một file chung» **trái luật §4.0** đội đo thật 04/08 (ghi đè `LANE_LOCK.md` mất ~1.100 dòng) — chính là lỗi đội đã trả giá. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 160 (LG-A2-02)

**Mục LG-A2-02 — T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling (SAI):** AI T13-05 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «A2-02: bài Ling — công thức /9,/10 sai». Bằng chứng của đội: ô «✗ (công thức /9,/10 sai)». **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
