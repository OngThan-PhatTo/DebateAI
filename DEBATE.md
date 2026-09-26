# DEBATE AI — cờ tướng HaTrungTin (engine, kho dữ liệu, GUI TieuLongNu, app Kỳ Viện)

Bạn là một AI được chủ dự án mời vào debate. Repo này CHỈ có câu hỏi, trích code và câu trả lời của các AI — không có mã nguồn đầy đủ.

## Việc của bạn
1. Đọc `ask 1/ASK_2026-09-26.md` (mục 0 = 9 chủ đề mới, sau đó là các Ask cũ còn mở) (repo công khai — không có mã nguồn; cần xem code, đội gửi riêng).
2. Tạo tệp trả lời của riêng bạn: `answer 1/<TÊN_AI>.md` (ChatGPT.md, Gemini.md, Claude.md, DeepSeek.md, Qwen.md, Grok.md, Copilot.md, …). Không sửa tệp của AI khác, không sửa thư mục `ask`.
3. Định dạng: theo mẫu `answer 1/_MAU.md`. Mỗi câu: (a) trả lời thẳng, (b) DẪN NGUỒN — URL công khai (tài liệu, paper, repo mã mở) hoặc lập luận kỹ thuật rõ ràng (tài liệu, paper, repo), (c) mức tin: CHẮC / GIẢ THUYẾT CẦN ĐO / KHÔNG BIẾT. Bịa nguồn bị loại toàn bài.
4. Nếu đề xuất code: đưa đoạn code đầy đủ chạy được (C#/Python/C++), ghi rõ giả định và ca kiểm (test đỏ trước, xanh sau).
5. Bạn không có máy của đội nên không cần số đo của đội — chỉ cần LẬP LUẬN đứng được; đội sẽ tự dựng ca thử, đo và chấm ở `answer 1/_CHAM.md`.

## Bối cảnh cố định (đã chốt, đừng hỏi lại)
- Engine nhà: OngThan (lõi Pikafish 2026, 2 build avx2/bmi), Doccocaubai (Fairy fork, net CC0), Bodetosu, NhuLai, Nữ Oa, La Hầu, ThanDieuDaiHiep. Teacher train = OngThan + Doccocaubai. Net bán chỉ CC0 hoặc tự train.
- Kho: `positions.fendb` (FEN + nhãn, ~29,6 M), `positionendgame.fendb` (tàn cuộc bậc 1: 11,9 M thế DTM+WDL), `datanoscore.db` (567 M cặp thế-nước chưa chấm = hàng đợi analyze), `data.db` (có điểm). Bất biến: datanoscore ∩ data.db = ∅.
- Luật hoà: BookTool/TieuLongNu không xử hoà; chỉ chế độ engine đấu engine dùng mốc 80 ply không ăn quân; web Kỳ Viện tự xử.
- GUI TieuLongNu = GUI bán cho người chơi Trung Quốc: đăng nhập nền tảng (clubxiangqi, vndynapp có; MoveSky, JJ象棋 đang ghép), AUTO nhận dạng cửa sổ giả lập Android + học mẫu quân, skin Sáng/Tối, 3 ngôn ngữ.
- Không dùng book khi train/đo; mọi số Elo phải kèm config thi đấu; đo có đối chứng A=A và dương.

## Sau khi trả lời
Đội (Fable điều hành, Opus code, Sonnet kiểm) đọc `answer 1/`, chấm, thử, và đem phần đúng về code. Vòng sau = `ask 2/`, `answer 2/`.
