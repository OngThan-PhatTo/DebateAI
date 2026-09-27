# Chấm Ask 1 — đội HaTrungTin (Fable/Opus/Sonnet)

| AI | câu | đúng/sai/giả thuyết | số đo thật | ghi chú |
|---|---|---|---|---|
| ChatGPT | Ask 1 toàn bộ (A3-01…A3-15, K1–K8, A2-06…A2-35) | 9 CHẮC về nguyên tắc · 8 KHÔNG BIẾT trung thực · 3 GIẢ THUYẾT CẦN ĐO (quota 50/30/20; weight mate 1.0/0.5/0.75/0.1; ablation factorial) | chưa đo | không bịa file:dòng; 7 URL công khai. Giá trị cao: đơn vị mate ply/nước, tách TB_WDL vs RULE_WDL_80, missing≠0, state machine + session generation cho AUTO, 4 build factorial cho cờ úp -33 Elo, one writer + batch + idempotency. Đội chuyển GIẢ THUYẾT thành phiếu thử (luật 21/09 10:3x). Chấm sơ bộ 27/09 (Fable). |
| Ling (opencode) | ASK12 9 chủ đề + K1–K8 + ASK02 | chưa chấm chi tiết | — | 403 dòng; tự push lên repo; xác nhận điều khoản bảo mật. Chấm sau. |
| Longcat | ASK12 + K1–K8 | chưa chấm chi tiết | — | 9,6 KB ngắn; nhận «retrograde 9.900 thế/s chậm cho 10^20» ⇒ lấy mẫu theo ô. Chấm sau. |
| SpaceBunny (opencode, space-bunny-free) | Ask 1 toàn bộ | chưa chấm chi tiết | — | 873 dòng, khai «mọi URL đã tự mở, HTTP 200» — cần kiểm URL ngẫu nhiên 10 cái (luật kiểm bịa). Chấm sau. |
| big-pickle (opencode) | Ask 1 toàn bộ | chưa chấm chi tiết | — | 1.072 dòng 182 KB. Chấm sau. |
| MuseSpark | Ask 1 (26/09) | chưa chấm chi tiết | — | 19 KB. Chấm sau. |
| Qwen | ASK04-B K1–K8 + ASK01 | chưa chấm chi tiết | — | 10,6 KB, gửi dạng script Python; đề xuất ATTACH DATABASE, BEGIN IMMEDIATE + lô, WAL, lọc theo hash tệp. Chấm sau. |
| Grok | Ask 1 | chưa chấm chi tiết | — | 45,8 KB. Chấm sau. |
| Ox Alpha | ASK12 9 chủ đề | chưa chấm chi tiết | — | 20 KB, 3 lượt continue nối lại; ghi «không biết» rõ 7 mục; hỏi chênh 27.700.086 ⇒ đội đã giải: 27.699.978 dòng REAL ép về INT64 (giữ, không xoá). Chấm sau. |
| DeepSeek | ASK02 35 + ASK03 16 + ASK12 + ASK04-B (2 lượt) | chưa chấm chi tiết | — | không mở được GitHub (owner dán Ask); tự nhận 2 câu thiếu (A2-04, A2-09) rồi trả nốt; đào sâu A3-01/02/03/05/07/08/12/13/15; ghi «không biết» khi không có source. Chấm sau. |
| Manus | Ask 1 (26/09) | chưa chấm chi tiết | — | gửi dạng .docx, đội chuyển markdown. Chấm sau. |
