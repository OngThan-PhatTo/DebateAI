# ASK 2 — PHẦN D: DEBATE GIỮA CÁC AI (bổ sung 27/09, sau khi đội đọc đủ 11 trả lời Ask 1)

Đọc kèm Ask 2 (10 câu). Phần này gom những chỗ **các AI trả lời trái nhau**, hoặc chỗ **đội đã đo xong và thấy một AI sai**. Với mỗi mục:
nói bạn đứng bên nào và vì sao; nếu bạn là AI bị nêu sai, xác nhận hoặc phản bác bằng số. Giữ luật cũ: CHẮC / GIẢ THUYẾT CẦN ĐO / KHÔNG BIẾT,
URL công khai khi khẳng định, không bịa. Trả lời ngắn cũng được.

## D-1. chessdb.cn: ghi chú `W-M-nnnn` bị chẵn ở nước tốt nhất

Đội gọi `queryall` 3 lần (27/09) cho thế `c8/9/3k5/9/9/3N5/R2p5/3K5/9/9 w` (Đỏ đi, Đỏ thắng):
- không tham số (chuỗi trả giống hệt `egtbmetric=dtm`): d2e2 score 29992 rank 2 «!» W-M-0008 · a3d3 29991 rank 1 W-M-0009 · d2d1 29989 W-M-0011.
- `egtbmetric=dtc`: a3d3 29991 rank 2 «!» W-00-009 · d2e2 29997 W-00-003 · d2d1 29993 W-00-007.

Bộ giải tiến của đội: DTM = 9 nửa nước qua a3d3 (a3d3 là nước XE ăn TỐT). Bên đi đang thắng thì DTM (nửa nước, tính từ thế hiện tại) phải LẺ ⇒
số 8 của d2e2 vi phạm chẵn lẻ. SpaceBunny nói đúng rằng `egtbmetric` mặc định là DTM; Longcat nói «M-nnnn đếm từ thế hiện tại» (không nguồn) —
với số 8 thì không thể. Hỏi: (a) vì sao d2e2 ra 8 (đếm sau nước đi? quy ước riêng của bảng gốc? chuyển bảng khi ăn quân?); (b) quy tắc chuyển
chuỗi `queryall` thành DTM nửa nước đáng tin; (c) nên luôn hỏi cả dtm lẫn dtc rồi đối chiếu không, và khi hai metric trái nhau thì tin cái nào.

## D-2. Số dòng sau khi sửa kho (để các AI tự hiệu chỉnh)

SpaceBunny tính 567.206.917 − 92.495.146 − 7.830.204 − 64.795.168 = 402.086.399 và nói số của đội (494.581.545) sai. Thực tế: 64.795.168 dòng
trùng là **tập con** của 92.495.146 dòng kiểu REAL (REAL trùng khoá INT thì xoá; 27.699.978 dòng REAL còn lại thì ép về INT64 và giữ); đội đã
chạy thật và một oracle độc lập đếm lại 494.581.545, lệch 0. Đề Ask 1 viết mơ hồ chỗ này — lỗi một phần của đội. Không cần trả lời lại trừ khi
bạn thấy lỗi trong lập luận trên.

## D-3. WAL hay DELETE cho kho SQLite 30 GB mà GUI đọc liên tục

Phe WAL: Qwen, Longcat, Ling, Grok (WAL + busy_timeout + một writer), big-pickle (hợp lý, cấm đặt trên ổ mạng), ChatGPT (nghiêng WAL nhưng phải đo
trước). Phe giữ DELETE: MuseSpark, SpaceBunny (WAL chỉ cho staging; tệp -wal/-shm khó sao lưu). Đội hiện: kho DELETE; sửa kho bằng cách chép →
sửa bản làm trong một giao dịch → đổi tên nguyên tử. Việc sắp tới: gộp 350 triệu dòng khi GUI vẫn đọc. Hỏi: phép đo nào quyết định (độ trễ đọc
p90 của GUI khi đang gộp, kích thước -wal đỉnh, checkpoint bị reader giữ lâu) và ngưỡng cụ thể bạn đề nghị ghi trước khi đo.

## D-4. Target khoảng cách chiếu bí cho net chỉ-value

Đồng thuận: WDL là đầu chính, khoảng cách là phụ hoặc trọng số. Các công thức đề xuất trái nhau: tuyến tính max(1500, 3000 − 10·dtm) (đề xuất cũ
của đội — độ dốc bằng 0 khi dtm ≥ 150) · tanh((3000 − dtm)/600) (Manus) · 1500·(1 − e^(−dtm/30)) (DeepSeek) · remap mũ base + decay^ply·scale
với 4000/4000/0,85 (SpaceBunny, dẫn nnue-pytorch; trainer của đội hiện không có remap này) · max(0, 3000 − 5·dtm) (Longcat). Ngưỡng tương quan
Spearman(net, −DTM) được đề xuất: 0,5 / 0,6 / 0,8 / 0,9 / 0,95. Hỏi: chọn MỘT công thức và MỘT ngưỡng, nói vì sao ngưỡng đó phân biệt được với
nhiễu khi đo trên 2.000–20.000 thế thắng.

## D-5. Nấc băm theo bộ đếm không-ăn-quân khi đổi mốc 120 → 80 nửa nước

big-pickle: «lỗi im lặng, CHẮC». SpaceBunny: «vẫn hợp lệ, chỉ tăng va chạm». DeepSeek: «bộ đếm 6 và 14 cùng khoá» — đội kiểm mã: không đúng (từ
14 trở đi khoá được XOR thêm một giá trị khác 0). Đội đã có trong mã: không cắt TT khi bộ đếm ≥ mốc − 4; điểm mate đọc từ TT bị hạ theo số nửa
nước còn tới mốc; mọi hằng 120 đã thay bằng tham số mốc (có ca đỏ). Hỏi: còn cơ chế nào khiến kết quả HOÀ bị sai (không chỉ trộn eval trong cùng
nấc 8 nửa nước)? Đề xuất phép đo nhỏ, có đối chứng.

## D-6. PrintWindow trên cửa sổ vẽ bằng GL/ANGLE

SpaceBunny khẳng định «PrintWindow không bắt được nội dung GPU/DirectX». Đội đo trên cửa sổ qemu của trình giả lập Android: một lần khớp ảnh
`adb screencap`, một lần ra đen 11 % (xem Ask 2 câu 6). Hỏi thêm: cờ PW_RENDERFULLCONTENT thay đổi gì trong trường hợp này (dẫn tài liệu), và khi
nào kết quả phụ thuộc cửa sổ có đang bị che / thu nhỏ hay không.

## D-7. FelicityEgtb «Legal positions»

SpaceBunny dẫn mã Felicity: Legal = tổng hai bên đi trên không gian chỉ số đã gộp gương (45 cặp tướng × 90 ô cho krk), và luật hợp lệ chỉ là «hai
tướng không đối mặt» (không loại thế mà bên không tới lượt đang bị chiếu). Đội đối chiếu mã: đúng cả hai điểm. Nhưng 16 cách lọc của bộ đếm đội vẫn
không ra 4.806 (krk) và 3.294 (kpk); bộ đếm đội ra krk 8.748 (Đỏ đi 3.834 + Đen đi 4.914), gộp gương 4.401. Hỏi: ánh xạ 45 cặp tướng của Felicity
xử lý thế nằm trên trục giữa và thế có quân chồng ô thế nào, để tái lập đúng hai con số.

## D-8. Sửa sai đã bắt (AI tương ứng tự hiệu chỉnh, không cần tranh luận)

- UCCI «mate = nửa nước» (Longcat): sai với mã Fairy-Stockfish — UCI và UCCI đều in số NƯỚC, chỉ USI in nửa nước.
- «vkey < −9223372036854775808» (Qwen): điều kiện luôn sai (đó là số nhỏ nhất của INT64).
- «8,5·10⁸ thế/ngày ⇒ 30 ngày ≈ 2,5·10⁸» (Ox Alpha): đúng là ≈ 2,5·10¹⁰.
- «sqlite3_execute» (Ling): hàm không tồn tại (sqlite3_exec); fcntl của Python không có trên Windows; CertVerifyCertificateChainInfo không dùng để
  so hash tệp.
- URL không tồn tại / 404 lúc đội kiểm 27/09: Ling 13/46 (gồm bhgui.org, lc0.mosquitochess.org, github.com/Tengweitao/ccbridge,
  github.com/openai/openai/issues) · SpaceBunny 5/57 dù đầu bài ghi «mọi URL HTTP 200» · Ox Alpha 4/20 (github.com/pgh26/chessdb).
- «Bộ đếm 6 và 14 cùng khoá băm» (DeepSeek): sai (xem D-5).

*Luật cite: URL công khai khi khẳng định; số đo của đội ở trên được dùng lại; không cần mã nguồn.*
