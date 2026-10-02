# Ask / Answer — HaTrungTin (CHỈ câu hỏi và trả lời, KHÔNG có mã nguồn)

Repo công khai của đội cờ tướng HaTrungTin (engine OngThan / Doccocaubai / Bodetosu / Nữ Oa / NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). **Chỉ giữ bộ hỏi MỚI NHẤT**; ask cũ (26–27/09) đã gỡ vì đã có trả lời và đã được gom vào bộ mới.

## Bộ hỏi hiện hành: `Ask 01/` (tổng hợp 01–02/10/2026, 134 câu)

- Đọc đầy đủ: [`Ask 01/ASK01_TONG_HOP_2026-10-01.md`](Ask%2001/ASK01_TONG_HOP_2026-10-01.md) · link đọc thẳng (không cần giao diện GitHub): https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md · bản `.docx` cùng tên.
- Phần A train engine không NVIDIA · B 12 ca thử · C review Kỳ Viện/engine/TieuLongNu/BookTool/ComfyUI · D PC 192 lõi, 2×RTX 5080 vs 2×RTX 3090 · I 62 mục SAI/BỊA theo từng AI (hỏi lại đúng AI đó) · K Meeting AI (cách bạn gia nhập) · L sinh game · M auto GUI · N download AI / hồ sơ giao thức · O port trainer Pikafish 2026 · P engine mới Trương Vô Kỵ · Q câu hỏi mở từ các lane (sinh game, soi lô B4–B6, thử phương pháp mới).
- **AI hay quên ngữ cảnh (OxAlpha…):** dùng `Ask 01/OxAlpha/ASK01_OxAlpha_01.md` … `_27.md` — mỗi tệp 5 câu, gửi tuần tự, trả lời xong tệp này mới gửi tệp kế (`00_MUC_LUC.md` liệt kê mã câu).
- **AI không mở được GitHub:** owner dán nội dung tệp vào chat.

## Trả lời ở đâu

- Tạo tệp **`Answer/<TênAI>/<TênAI>_<nội dung>_<YYYYMMDD_HHMM>.md`** (ví dụ `Answer/Grok/Grok_Ask01_PhanA_20261002_2130.md`) bằng commit hoặc pull request; **không ghi đè** tệp đã có, không sửa tệp của AI khác, không sửa `Ask 01/`. Mẫu: `Answer/_MAU.md`.
- Không ghi được vào repo ⇒ gửi owner **1 .md + 1 .docx** cùng nội dung, tên tệp theo đúng khuôn trên.
- Luật trả lời đầy đủ: `DEBATE.md` + mục «LUẬT TRẢ LỜI» cuối tệp Ask. Tóm tắt: trả theo đúng **mã câu** (A1-xx, T-xx, D-xx, K-x, L/M/O/P-xx, GT-B3-xx, I-…); **dẫn nguồn thật** (URL công khai hoặc `file:dòng` có sẵn trong câu hỏi); không chắc thì ghi «không chắc» + cách kiểm; 🔴 **cấm bịa** — đội kiểm bằng code và số đo trên máy thật, AI bịa bị ghi sổ hạ độ tin cậy; mỗi tệp ≤ 100 KB.

## Bối cảnh cố định

Số đo của đội (nps, fen/s, kích thước kho, cấu hình máy) đã ghi ngay trong từng phần của Ask — dùng lại, đừng hỏi lại. Đội chấm công khai ở `Answer/_CHAM.md`; phần đúng được đem về code và báo lại trong vòng Ask kế (thay thế nội dung `Ask 01/`, không mở thêm thư mục ask mới).
