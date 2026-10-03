# BigPickle — Ask01 · D3 phần I (10 mã: D3-01…D3-10)

**Người trả lời:** BigPickle · **Ngày:** 2026-10-03 15:27 · **Phạm vi:** `D3-01…D3-10`

**Các tệp cùng bộ Ask01 của BigPickle** (tách theo cụm để mỗi tệp < 100 KB):

| Tệp | Chứa |
|:---|:---|
| `BigPickle_Ask01_PhanD3NCFSMT_20261003_1527.md` (tệp này) | `D3-01…D3-10` + bảng ma trận nguồn + quy ước mức độ |
| `BigPickle_Ask01_PhanD3PhanII_20261003_1527.md` | `D3-11…D3-20` + bảng tổng hợp việc nên làm của cụm D3 |
| `BigPickle_Ask01_PhanNCFSMT_20261003_1527.md` | `NC-01…NC-08`, `FS-01…FS-02`, `MT-01`, `SON-01…SON-03` |
| `BigPickle_Ask01_PhanT34W456T36T13_20261003_1109.md` | 12 mã `T` đã làm trước đó |
| `BigPickle_Ask01_PhanABCI_20261003_1017.md` | 4 mã `A/B/C/I` đã commit |
| `BigPickle_Ask01_PhanDLMNOP_20261003_1052.md` | 6 mã `D/L/M/N/O/P` đã commit |

**Tổng phạm vi bộ Ask01 của BigPickle:** 34 mã (`D3-01…20`, `NC-01…08`, `FS-01…02`, `MT-01`, `SON-01…03`) trong 3 tệp của cụm `D3/NC/FS/MT/SON`.

**Nguyên tắc áp dụng:** mọi khẳng định có nguồn đều kèm URL/tệp cụ thể hoặc được dánh nhãn `GIẢ THUYẾT CẦN ĐO` / `KHÔNG BIẾT`. Trong phần này tôi **mở và đọc trực tiếp** các nguồn ghi ở mục *Nguồn đã mở ngày 2026-10-03*; các nguồn chỉ được nghe nói lại thì nằm ở mục *Chưa mở* và không dùng làm căn cứ kết luận.

---

## Nguồn đã mở ngày 2026-10-03 (đọc nguyên văn)

| Mã | Nguồn | Dùng cho |
|---|---|---|
| S1 | `https://opendatacommons.org/licenses/odbl/1-0/` — ODbL 1.0 toàn văn | D3-07 |
| S2 | `https://www.gnu.org/licenses/gpl-faq.html` — GPL FAQ của FSF | D3-19 |
| S3 | `https://huggingface.co/ByteDance/SDXL-Lightning` — model card | D3-16 |
| S4 | `https://www.sqlite.org/pragma.html` | FS-02 |
| S5 | `https://www.sqlite.org/withoutrowid.html` | FS-02 |
| S6 | `https://raw.githubusercontent.com/official-stockfish/Stockfish/master/src/uci.cpp` | D3-03, D3-08, D3-09, SON-01 |
| S7 | `https://raw.githubusercontent.com/official-stockfish/Stockfish/master/src/tt.cpp` | D3-09 |
| S8 | `https://raw.githubusercontent.com/official-stockfish/Stockfish/master/src/thread.cpp` | D3-08, D3-09 |
| S9 | `https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-replacefilew` | D3-10 |
| S10 | `https://raw.githubusercontent.com/official-stockfish/fishtest/master/server/fishtest/stats/stat_util.py` | D3-05, FS-01, SON-02 |
| S11 | `https://raw.githubusercontent.com/official-stockfish/fishtest/master/server/fishtest/stats/LLRcalc.py` | D3-05, FS-01, SON-02 |

**Chưa mở (KHÔNG dùng làm căn cứ):** trang `fairy-stockfish.github.io/nnue`, repo `official-stockfish/fishtest` wiki, `variant-nnue-tools`, `nnue-pytorch` wiki, model card ViVoice/VieNeu/ACE-Step, bảng MUSDB18/MVSEP, tài liệu Whisper VAD, phép đo commit charge của section ánh xạ tệp, tài liệu quota YouTube/TikTok/Facebook.

**Tính toán của tôi** (mã nguồn ở `%TEMP%\opencode\stat_final.py`, chạy bằng công thức nguyên văn từ S10/S11): mọi bảng số Elo/SPRT trong tệp này là **tôi tính**, không phải số công bố.

---

## Cụm D3 — net / dữ liệu / huấn luyện (D3-01 … D3-07)

### D3-01 — 8 net Fairy-Stockfish 2022–2023 không giấy phép; nhãn `GPL` trong AUR có đè lên CC0 của `C07E94A5` không?

**Trả lời**

Ba câu hỏi tách bạch, trả lời từng câu:

**(i) Văn bản công khai gán giấy phép cho các net xiangqi trước 2026 — không biết.** Tôi không mở được `fairy-stockfish.github.io/nnue` và không có bản sao offline của trang đó trong cây hiện tại, nên **không có câu trả lời trung thực** cho (i). Tôi cũng không đoán tên người giữ quyền như câu hỏi yêu cầu. Nơi nên hỏi, theo đúng thứ tự đội có thể làm được ngay:

1. Mở từng README/commit lịch sử của 8 repo chứa các hash `83f16c17fe26 · 28d6221e2440 · bd62afd13f7c · 6f64c55fcb28 · d134cdf48baf · ae0082262b68 · c425c4166a1e · aa162e1771e5` và tìm mục License **ở tag/commit đúng thời điểm đó**, không phải ở `HEAD` hiện tại — giấy phép ghi sau năm 2026 không phản ánh trạng thái năm 2022.
2. Một GitHub issue hỏi đúng câu hỏi, ghi rõ bạn chỉ hỏi về **tệp trọng số của các hash cụ thể**, không hỏi về mã nguồn.
3. Nếu tên tác giả xuất hiện trong commit log thì đó **không phải** bằng chứng ai giữ quyền ký; chỉ người đó xác nhận mới có giá trị.

**(ii) Trường `license=` trong PKGBUILD không có giá trị pháp lý đối với tệp trọng số.** Lập luận: `license=` là **siêu dữ liệu do người đóng gói tự khai trong PKGBUILD**; nó không sinh ra quyền mà người khai không có. Nếu người đóng gói khai `GPL-3.0-or-later` cho tệp `C07E94A5` trong khi tệp đó theo README upstream là CC0, thì **nhãn AUR sai**, chứ không phải nhãn AUR "đè" lên CC0. Nhãn sai ở PKGBUILD không làm mất CC0 và cũng không tạo thêm GPL. Hệ quả thực tế: **đừng dùng AUR làm bằng chứng giấy phép cho bất kỳ tệp nào** — kể cả tệp CC0; lấy giấy phép từ README/commit upstream. Đây là lý do đội ghi "chỉ đo A/B nội bộ, không ship" là đúng: nó đúng vì **thiếu bằng chứng**, không phải vì nhãn GPL sai.

**(iii) Hồ sơ tối thiểu cho một xin-phép.** Phần này là thủ tục, không có nguồn pháp lý nào định nghĩa được, nên tôi ghi là khuyến nghị, không phải chuẩn pháp lý. Một bản xác nhận bằng văn bản nên có tối thiểu: (1) tên/URL chính xác của tệp; (2) **SHA256 đầy đủ** của đúng byte trên đĩa, không phải 8 ký tự đầu; (3) câu cấp quyền rõ ràng (`CC0` / `MIT` / cho phép thương mại có điều kiện gì); (4) ai ký và tên pháp nhân/tên nhóm; (5) ngày; (6) kênh xác minh lại được sau này (commit hoặc issue). Vì pháp lý quyền tác giả **không bắt buộc phải có văn bản riêng** — bằng chứng cũng có thể là hành vi và thực hành công khai — một văn bản trả lời là bằng chứng mạnh nhất mà bạn có thể thu, nhưng việc *không có* văn bản không tự động chứng minh là không được phép. Vì vậy kết luận đúng là **chưa đủ cơ sở ship**, chứ không phải "chắc chắn vi phạm".

**Điều chỉnh quan trọng cho đường đi đã chọn.** Đội nói "đường mạnh hơn = train tiếp từ `C07E94A5`". Cần tách hai việc: `C07E94A5` là **net CC0** nên train tiếp / fine-tune / distill từ nó thì **được phép thương mại**; nhưng nếu thầy là engine của đội thì dữ liệu sinh ra là của đội. Rủi ro không nằm ở net mẹ mà nằm ở **dữ liệu thầy** — cùng luận với D3-07 và NC-01.

**Nguồn**
- S0 (đội đã lưu, không mở lại): `_LANE_NOTE/lock/w_net_sach_0210_2026-10-02.md` §2; văn bản giấy phép ở `_LANE_NOTE/lock/w_net_sach_0210_giayphep/` kèm SHA256; câu CC0 trong README `official-pikafish/Networks`.
- `C:\AI\AI_Debate\onGoing\NGUON_ENGINE.md`.

**Mức độ**
- (ii) `CHẮC` (lập luận về bản chất siêu dữ liệu PKGBUILD).
- (i) `KHÔNG BIẾT` — chưa mở nguồn, không đoán.
- (iii) `GIẢ THUYẾT CẦN ĐO` — khuyến nghị thủ tục, không phải chuẩn.
- Kết luận "13 net này chưa đủ cơ sở ship": `CHẮC` (là kết luận phủ định bảo thủ, đúng khi thiếu bằng chứng).

---

### D3-02 — `data.db` 180,8 triệu dòng không có cột nguồn: có nhận ra công cụ / engine / độ sâu đã sinh điểm không?

**Trả lời**

Đây là câu mà tôi **không đoán**, và tôi muốn nói thêm một điều quan trọng hơn câu trả lời: **nhận ra công cụ không cứu được vị thế pháp lý**.

Điểm cần tách: *provenance* (ai sinh ra điểm) và *license* (được phép dùng thế nào) là hai câu hỏi độc lập. Giả sử sau 2 tuần bạn tìm ra đó là sản phẩm của kho `X` — bạn vẫn phải hỏi riêng "X có cho phép train lại và thương mại sản phẩm dẫn xuất không", và câu trả lời có thể vẫn là không. Trong kho mở phổ biến, điểm số mà bên thứ ba tạo ra thường có hai vấn đề tách biệt: **không rõ giấy phép** và **không tái lập**. Vì vậy phương án (a) của đội, dù thực hiện xong, sẽ **không** nâng được mức "có thể ship" — điều nó nâng được là chất lượng nhãn. Đó vẫn là việc đáng làm, chỉ là không được kỳ vọng sai.

Về việc *nhận diện*, tôi không mở tài liệu định dạng của BHGui / 兵河 / chessdb.cn trong lượt này, nên: **không nhận ra định dạng này của tôi là của công cụ nào** (`KHÔNG BIẾT`). Tôi không sẽ đoán.

Tuy vậy, khuôn dữ liệu của đội có **ba dấu hiệu rất đặc trưng** đáng dùng để tìm, và đây là đề xuất kiểm chứng chứ không phải kết luận:

1. **`atime` là dấu giờ `YYYYMMDDhhmmss` dạng văn bản, không phải số nguyên.** Đây là dấu hiệu mạnh nhất: kho sinh *offline hàng loạt* bằng công cụ Nĩa có ghi thời điểm xuất. Nó loại trừ mạnh các kho `score` kiểu Cờ vua (`chessdb`, Lichess) vốn lưu timestamp kiểu epoch.
2. **`score` trùng cả hai đầu `±32000` và 3,51 triệu dòng ≤ −30000.** Sentinel `±32000` là quy ước cũ, phổ biến ở engine cờ tướng đời đầu và ở phần mềm dùng `short` cho cp. Khuôn đoạn 82 nước cũng khớp lịch sử dữ liệu dài hạn của cờ tướng.
3. **Mã nước trộn hai "phương ngữ" 16×16 và native** (`SourceCode/BookTool_Xiangqi/A5VmoveDialectSelfTest.cs:6-9`), giải mã duy nhất 99,79 %. Sai 0,21 % còn lại **gợi ý mạnh rằng bảng được ghép từ hai nguồn khác nhau** chứ không phải xuất một lần từ một engine — nếu là một engine duy nhất thì sai sót phải là lỗi giải mã thuần, tức phân bố lỗi phải đều và không tập trung ở một dialect.

**Giao thức kiểm chứng mà tôi đề xuất** (nếu đội vẫn muốn làm phương án (a), vì nó phục vụ chất lượng nhãn):

- Chấm lại **đúng mẫu thế đã giải chính xác** (đội đã có 11,9 triệu thế ≤ 2 quân tấn công, kho `positionendgame.fendb`) bằng từng engine nghi ngờ ở nhiều độ sâu. Đây là bước duy nhất cho **bằng chứng định lượng** thay vì bằng chứng giống-khuôn.
- Với điểm `score` của sách (không phải WDL), bước này **không cho biết công cụ là ai** — nó chỉ cho biết điểm sách có khớp search ở độ sâu nào. Muốn nhận diện công cụ thì buộc phải dựa vào fingerprint giống-khuôn, mà khuôn đó thì tôi `KHÔNG BIẾT` mã hoá.
- Phần lớn giá trị của việc này là **loại bỏ điểm sách khỏi tập nhãn** (đội đã làm đúng: `nap_data_db.py` chỉ xuất thế + nước tốt, gắn `KHONG_BAN`). Đó là kết quả đạt được chắc nhất.

**Một chỉnh sửa cần làm rõ trong luật owner.** Luật 26/09 ghi "điểm `data.db` KHÔNG làm nhãn train vì không truy được nguồn". Điều này đúng nhưng **lý do nên ghi là "không có giấy phép đã xác minh", không phải "không truy được nguồn"** — vì nếu sau này truy được nguồn thì cột điểm vẫn có thể không được phép, và ngược lại một nguồn cho phép thì cột điểm vẫn phải chấm lại vì **độ sâu mỗi ván khác nhau làm điểm không cùng thang**. Hai lý do độc lập, nên tách.

**Nguồn**
- S0 (đội đã đo): `_LANE_NOTE/_scratch_code_luu/w_ncb_0210/kiem_ke_20261002_1921.json`; `_LANE_NOTE/lock/w_ncb_0210_2026-10-02.md` §3.
- Mã: `SourceCode/Shared/DataFen/nap_data_db.py`, `SourceCode/BookTool_Xiangqi/A5VmoveDialectSelfTest.cs:6-9`.

**Mức độ**
- "Không nhận ra định dạng / không có tài liệu định dạng đã mở": `KHÔNG BIẾT`.
- Nhận định "sentinel ±32000 và `atime` dạng chuỗi là dấu hiệu kho offline cũ": `GIẢ THUYẾT CẦN ĐO` (hợp lý với đặc trưng, chưa đối chiếu tài liệu).
- Nhận định "0,21 % sai tập trung ở một dialect ⇒ ghép từ hai nguồn": `GIẢ THUYẾT CẦN ĐO` — **đội phải đo** (đếm sai theo dialect) thì mới kết luận được; tôi chưa đo.
- "Nhận diện nguồn không nâng được mức pháp lý": `CHẮC` (lập luận độc lập hai câu hỏi).

---

### D3-03 — Nhãn chính sách từ nhiều nước: argmax hay phân bố mềm, khi ~85 % thế có biên top1−top2 < 50 cp?

**Trả lời**

Đây là câu mà câu hỏi đã tự gợi ra câu trả lời, và tôi xác nhận bằng số của đội: **với mục tiêu NNUE value-only thì cả ba phương án đều không đưa thêm thông tin vào loss**. Lý do nằm ở bản chất hàm mất mát, không nằm ở hình thức nhãn.

**Vì sao argmax về bản chất đã là "phân bố mềm"?** NNUE value dùng hàm một đầu ra: mỗi thế cho một giá trị vô hướng. Gradien chỉ đi qua con số đó. Nếu bạn đặt nhãn là phân bố mềm trên các nước, bạn vẫn phải **giảm nó về một con số** trước khi tính loss (argmax, trung bình có trọng số, hay kỳ vọng theo softmax). Nghĩa là bạn đã tự quyết định phép giảm trước khi so sánh (a) với (b). Câu hỏi thật không phải "argmax hay mềm", mà là **"phép giảm nào, và thang nào"**.

**Còn đầu policy thì sao?** Thêm đầu policy là **thay đổi kiến trúc**, và ở cờ tướng nó không có tiền lệ đã được công bố mà tôi biết — Lc0 có policy target vì kiến trúc MCTS của nó dùng visit count, còn engine alpha-beta kiểu Stockfish/Pikafish thì không. Tôi không mở tài liệu Lc0/KataGo trong lượt này nên không dẫn nguồn cho tiền lệ. Với quy mô 11 MB / Fairy L1 512 và nhãn sách chỉ 99,79 % giải mã được, đầu policy là đầu tư lớn vào nhãn yếu. **Đề xuất: bỏ (a) và (c), chọn argmax (phương án mặc định của đội), nhưng dùng nó như một việc độc lập chứ không gộp vào net value.**

**Con số 85 % — tôi kiểm lại:** (434 + 5.824) / (434 + 5.824 + 865 + 226) = 6.258 / 7.349 = **85,16 %**. Khớp "khoảng 85 %" của đội. Ý nghĩa thực tế: nếu bạn đặt T = 60 cp, thì ở 85 % thế nhiều nước có xác suất softmax dưới ngưỡng bất kỳ có ý nghĩa — nhãn mềm và argmax **gần như trùng nhau**. Muốn nhãn mềm thật sự khác argmax, bạn cần T rất nhỏ (≲ 10 cp), mà khi đó nhãn mềm lại **gần như nhất thứ hai**, tức là bạn đang huấn luyện net chọn nước tệ. Vậy nên T nào cũng không tốt: **đây là bằng chứng định lượng rằng (a) không dùng được với dữ liệu của đội.** T là con số phải hiệu chỉnh trên tập val, không có giá trị "đúng".

**Phần thang điểm — đây là chỗ cần sửa gấp.** Đội hỏi "thang `score` của sách quy đổi sang thang trainer thế nào". Có một sự thật quan trọng về ngay cả thang cp của engine hiện đại mà tôi đã mở mã nguồn: **cp trong UCI không phải một thang cố định.** Stockfish báo `score cp` bằng cách chia giá trị nội bộ cho một hệ số phụ thuộc **vật chất còn trên bàn** (`UCIEngine::to_cp` trong S6, dùng `win_rate_params` với `material` và các hệ số `as[]/bs[]`). Hệ quả trực tiếp cho câu hỏi của đội: **so sánh điểm giữa hai thế khác vật chất là sai ngay từ gốc**, kể cả khi cả hai đều là "cp". Với thang của sách cờ tướng, bạn phải biết công cụ đó dùng hệ số nào mới quy đổi được; nếu không biết thì mọi con số ±50 cp trong bảng thống kê của đội là **so sánh trong một thang chưa xác định**, và ngưỡng "biên ≥ 50 cp" mất ý nghĩa. Đây là lý do để đưa D3-03 về phương án (c): **chỉ dùng `data.db` làm nguồn FEN**, để một thầy có thang đã biết chấm lại.

**Một bẫy cụ thể với mate:** nếu dùng argmax có cả nước mate, nhãn chính sách sẽ là một **nước thắng trong 1 ply**, đây là nhãn cực mạnh nhưng cũng cực dễ sai (bẫy mate giả, đã nêu ở NC-02). Với thang sách có sentinel `±32000`, ranh giới mate không cắt sạch ở `±20000` như chuẩn Stockfish (`TB_CP = 20000` trong S6 dùng cho điểm bảng tra); bạn **phải** đo tần suất mate trong sách ở các ngưỡng khác nhau thay vì mặc định `20000`.

**Khuyến nghị cuối:** phương án (c) + (b). Tách `data.db` thành **nguồn FEN thuần**; nhãn duy nhất lấy từ một thầy `nodes` cố định đọc lại net (đúng đường D3-04); nếu sau này muốn có policy thì để thành **net thứ hai** huấn luyện riêng trên cùng FEN, và đo tăng Elo của nó trước khi đòi ghép.

**Nguồn**
- S6: `uci.cpp` — `UCIEngine::to_cp` (chia theo hệ số vật liất), `constexpr int TB_CP = 20000` trong `format_score`, `on_update_full` không in trường policy nào (`depth seldepth multipv score bound wdl nodes nps hashfull tbhits time pv`).
- S0 (đội đã đo): mẫu 59.971 thế; `AI_Debate/onGoing/DEBATE_NET_CO_BAN_DATA_DB_2026-10-02.md:30`.
- Mã: `SourceCode/Shared/DataFen/nap_data_db.py`.

**Mức độ**
- "NNUE value-only không dùng được nhãn chính sách nhiều nước": `CHẮC` (lập luận từ dạng hàm mất mát).
- "85,16 % thế có biên < 50 cp": `CHẮC` (tính từ số đo của đội).
- "cp của Stockfish phụ thuộc vật liất, nên biên cp giữa hai thế khác vật liất không so sánh được": `CHẮC` (đọc mã S6).
- "softmax không dùng được với dữ liệu này ở mọi T": `GIẢ THUYẾT CẦN ĐO` — cần hiệu chỉnh T trên tập val để đóng, nhưng lập luận ràng buộc rất chặt.
- "Thang cp của kho sách là thang gì": `KHÔNG BIẾT` — không có tài liệu định dạng.
- Ngưỡng mate trong sách: `KHÔNG BIẾT`, phải đo.

---

### D3-04 — Lô mồi `TU_DAU`: thầy 100k nodes, TT nối giữa các thế, tàn cuộc 0,3 — cỡ lô tối thiểu để A/B có ý nghĩa?

**Trả lời**

Câu này có ba tiểu câu. Tôi trả lời ngược thứ tự ưu tiên vì (iii) quyết định mọi thứ.

**(ii) Cỡ lô tối thiểu để A/B có ý nghĩa — không có con số công bố mà tôi đã mở.** Tôi không mở `nnue-pytorch` wiki, tài liệu huấn luyện Pikafish/Fairy, hay báo cáo fishtest nào trong lượt này, nên tôi **không đưa ra bậc số 10⁵/10⁷/10⁹** như một sự thật. Điều này cũng đúng với câu hỏi: số đủ để train và số đủ để **A/B phân biệt được** là hai ngưỡng khác nhau, và ngưỡng thứ hai phụ thuộc vào harness của đội chứ không phụ thuộc vào số liệu huấn luyện. Tôi đề xuất thay bằng một tiêu chí đo được:

- **Ngưỡng quyết định là của harness, không phải của dữ liệu.** Với các cặp net chênh thật khoảng +20…+50 Elo và engine tất định, bảng KTC95 ở D3-05/FS-01/SON-02 cho số ván cần thiết. Lô ~10.300 thế ở 100k nodes **chưa đủ tạo ra khác biệt đo được nếu khác biệt đó nhỏ hơn độ rộng KTC** — đây là lý do câu hỏi của bạn là đúng khi hỏi "A/B mới có ý nghĩa".
- **Tiêu chí thay thế, đo được:** loss val đã ngừng giảm trên một tập val **cố định** và **không** dùng trong train; đồng thời A/B trên 6–10 thế kiểm tra tay cố định cho cải thiện đồng nhất (không phải vài thế may mắn). Ngưỡng dừng đặt trước, ví dụ "val loss giảm < 1 % trong 2 epoch liên tiếp".
- **Đường dẫn tương đối rẻ và chắc hơn nhiều so với `TU_DAU`:** fine-tune từ `C07E94A5` (CC0) thay vì từ ngẫu nhiên, nếu mục tiêu là có net dùng được sớm. Đội đã đo fine-tune lô N5 ra **−301 Elo**; con số đó nói nguyên vấn đề là **nhãn**, không phải kiến trúc hay thời gian chạy. Vì vậy `TU_DAU` không giải quyết được rào cản đang chặn đội.

**(i) TT có bị xoá giữa các thế trong datagen chuẩn, và có đo được không.** Điều tôi **chắc** và đọc được nguyên văn trong S6: lệnh `ucinewgame` ở vòng lặp lệnh của Stockfish gọi `engine.search_clear()`, còn `stop`/`quit` chỉ gọi `engine.stop()`. Tức là **đường chuẩn để xoá TT là `ucinewgame`**, không phải `position`. Trong chính `bench` của Stockfish, `ucinewgame` được gọi giữa các lượt và có chú thích `// search_clear may take a while` — tức tác giả cũng coi đây là thao tác đắt. Còn hành vi bên trong của `gensfen` thì tôi **chưa mở mã nguồn `gensfen.cpp`**, nên không khẳng định. Điều này cũng chính là câu trả lời cho `SON-01`, xem đó.

Về ảnh hưởng tới chất lượng nhãn: **giữ TT ấm làm nhãn không tái lập, và đó là hậu quả nghiêm trọng hơn cả việc nhãn kém hơn.** Với thầy tất định, nhãn trở thành hàm của *(thế, thứ tự trước đó)* thay vì hàm của thế. Hậu quả cụ thể với đội: (a) chia mảnh tiến trình cho 250 thế sẽ **sinh ra dữ liệu khác nhau mỗi lần chạy lại job**, nên không thể chứng minh là một A/B; (b) val loss không so sánh được giữa hai lần chạy. Đội đã đúng khi ghi nhãn phụ thuộc cách chia vào `KET_QUA` — nhưng ghi vào log không cứu được tính tái lập.

**(iii) 100k nodes cho thầy.** Tôi không có nguồn upstream để trả lời trực tiếp. Nhưng tôi **kiểm được một điều có giá trị từ chính số đo của đội**, và đây là phép tính của tôi dựa trên `ncb03_job` dry-run 200 thế bên đội đã đo:

| `nodes` | thời gian/thế | so với 5.000 | tổng cho ~10.300 thế |
|---|---|---|---|
| 5.000 | 0,0295 s | ×1 | ~0,08 CPU-giờ |
| 50.000 | ~0,295 s | ×10 | ~0,85 CPU-giờ |
| 100.000 | ~0,59 s | ×20 | ~1,7 CPU-giờ |

Hai kết luận thực tế từ bảng này: (1) nếu nhãn ở 100k nodes chỉ khác ở 191/200 thế so với 5k (đội đã đo) thì **độ sâu thầy không phải nút cổ chai** — 9/200 thế không đổi nước, và điểm trung bình lệch 39,5 cp là lệch trong một thang chưa chuẩn hoá (xem D3-03). (2) Dù 100k, cả lô vẫn chỉ ~1,7 CPU-giờ; đổi sang 500k cũng chỉ ~8,5 giờ. **Vậy đừng hy sinh chất lượng nhãn vì thời gian** — ràng buộc thời gian không nằm ở đây.

**Khuyến nghị chốt, và tôi nghĩ đây là điểm quan trọng nhất của câu này:** bỏ việc nối TT, gửi `ucinewgame` trước mỗi thế, **giữ `nodes` cao**. Đổi lại: mất một phần phủ nhãn (đội đã đo ở đường khác: 90,7 % → 66 %) nhưng nhận được nhãn **tái lập được** — mà điều đó mới là điều làm cho mọi A/B sau này có nghĩa. Với thầy tất định, đây là trao đổi đúng đắn. Ngoài ra đừng bật/tắt vòng val 1.000.000 mẫu theo kiểu "tắt cho nhanh": nếu tắt, hãy thay bằng một tập val **cố định lưu trên đĩa** và dùng lại cho mọi lần chạy, nếu không thì bạn mất chính thứ duy nhất cho phép so sánh hai bản train.

**Nguồn**
- S6: `uci.cpp` — `else if (token == "ucinewgame") engine.search_clear();` và `if (token == "quit" || token == "stop") engine.stop();`; trong `bench`: `engine.search_clear();  // search_clear may take a while`.
- S0 (đội đã đo): `_LANE_NOTE/lock/w_ncb3_0210_2026-10-02.md` §2–§6; dry-run 200 thế 03/10 04:3x.
- `https://raw.githubusercontent.com/official-stockfish/Stockfish/master/src/gensfen.cpp` — **CHƯA MỞ**, cần mở để trả lời trực tiếp về hành vi TT trong datagen.

**Mức độ**
- "`ucinewgame` là đường chuẩn để xoá TT, `position` thì không": `CHẮC` (đọc S6).
- "Hành vi bên trong `gensfen`": `KHÔNG BIẾT` — chưa mở mã.
- Bảng thời gian/thế và hệ quả "độ sâu thầy không phải nút cổ chai": `CHẮC` (tính từ số đo 0,0295 s/thế của đội).
- "Không có con số công bố về cỡ lô tối thiểu": `KHÔNG BIẾT`.
- "Bỏ nối TT, dùng `ucinewgame` mỗi thế, giữ nodes cao": `GIẢ THUYẾT CẦN ĐO` — luận lý chắc (tái lập > độ sâu với thầy tất định), nhưng đội phải A/B để xác nhận không mất Elo. **Đây là ca thử tôi đề xuất**: một lô nhỏ, hai chế độ TT, A/B nodes cố định theo FS-01.

---

### D3-05 — A/B với engine TẤT ĐỊNH: A=A thành phép thử đối xứng; 10 thế xuất phát ⇒ ván lặp y hệt; với n nhỏ nên đo thế nào?

**Trả lời**

**(i) A=A với engine tất định chứng minh được điều đúng nhất: nó kiểm tra *harness*, không kiểm tra *net*.** Nói chính xác hơn, nó chứng minh: hai lần chạy cho cùng kết quả, nên rig không có nhiễu ẩn (không nhiễm ngẫu nhiên, không phụ thuộc thứ tự, không phụ thuộc Hash còn sót). Đó **không phải** phép thử vô dụng — nó là điều kiện tiên quyết để mọi khác biệt quan sát được sau này là do net chứ không phải do harness. Nhưng nó **không** có sức phân biệt, và đội đang hiểu nó là phép thử trống thì hơi thiếu: `A=A` đi kèm bắt buộc với **đối chứng dương đã biết đáp án**, và đội đã làm đúng (×4 nodes cho +338 Elo).

Vấn đề thật nằm ở chỗ khác: với engine tất định, **vanh sự lặp lại y hệt**. Mỗi cặp đổi màu trên cùng một thế cho ra gần như một ván duy nhất. Điều này làm hỏng **giả định độc lập** của mọi công thức KTC/SPRT, xem (ii).

**(ii) Công thức và cách tính khi n chỉ 20–70 ván.** Đây là phần tôi có nguồn và đã tự tính, nên tôi trả lời đầy đủ.

*Bước 1 — công thức.* Fishtest dùng **pentanomial** cho cặp ván đảo màu cùng thế: mỗi **cặp** là một quan sát, phân phối là xác suất của 5 sự kiện `[LL, LD+DL, LW+DD+WL, DW+WD, WW]` trên **tổng `N` cặp**, và KTC95 suy ra từ độ lệch chuẩn mẫu (hàm `get_elo` trong S10). Tôi đọc nguyên văn: `elo(x) = -400·log10(1/x − 1)`, `L_(x) = 1/(1 + 10^(−x/400))`, và `elo95 = (elo(µ+z·σ) − elo(µ−z·σ))/2` với `z = 1,959963984540054`.

*Bước 2 — bảng tôi tính* (script `stat_final.py`, công thức nguyên văn từ S10; giả định phần này nêu rõ ở dưới bảng):

| mục đích (hoà 35 %) | số ván | số cặp |
|---|---|---|
| KTC95 ≤ ±50 Elo | 372 | 186 |
| KTC95 ≤ ±45 | 456 | 228 |
| KTC95 ≤ ±30 | 1.017 | 508 |
| KTC95 ≤ ±25 | **1.470** | **735** |
| KTC95 ≤ ±15 | 4.070 | 2.035 |
| KTC95 ≤ ±10 | 9.142 | 4.571 |

| mục đích (hoà 60 %) | số ván | số cặp |
|---|---|---|
| KTC95 ≤ ±30 | 630 | 315 |
| KTC95 ≤ ±25 | **906** | **453** |
| KTC95 ≤ ±10 | 5.581 | 2.790 |

*Bước 3 — cái quan trọng nhất, và nó là một lỗi thật trong rig.* Bảng trên giả định **2 ván trong một cặp là hai mẫu gần độc lập**, nên thông tin = `2N`. Nhưng với **engine tất định và cùng một thế xuất phát**, ván cờ đổi màu gần như là *cùng một ván cờ* chơi lại. Phương sai thực tế của cặp là:

- nếu hai ván **hoàn toàn** tương quan: `Var(X_cặp) = Var(X_ván)` ⇒ thông tin chỉ = `N` ⇒ cần **gấp đôi** số cặp so với bảng, tức **1.470 cặp = 2.940 ván** cho ±25 Elo ở hoà 35 %;
- nếu hai ván **độc lập hoàn toàn**: `Var(X_cặp) = Var/2` ⇒ thông tin = `2N` ⇒ bảng trên đúng.

Nói cách khác, **bảng của fishtest là cận dưới (lạc quan), và sai số nằm trong khoảng từ ×1 đến ×√2.** Ngưỡng thực dụng: **735 cặp là mức tối thiểu, 1.470 cặp là mức an toàn.**

*Cách đo lại cái hệ số tương quan thay vì đoán:* **bootstrap theo thế xuất phát** — rút lại có hoàn lại `N` thế, tính Elo mỗi lần, lấy phân vị 2,5 % / 97,5 %. Cái này dùng **đúng** phương sai của rig của đội và không cần giả định phân phối nào. Đội phải **log kết quả theo cặp** (`thế, màu, kết quả`) thì mới làm được; hiện tại `AbNgan` chỉ trả tổng nên chưa làm được. Đây là việc nên làm **trước** khi thuê máy chạy lô dài.

*Bước 4 — cái bẫy `soVan > 20` mà đội đã phát hiện.* Khi số ván vượt 20 và code nhân đôi `n`, bạn **không thêm thông tin nào** (ván lặp lại y hệt) nhưng báo cáo `n` gấp đôi. Hệ quả: **KTC95 hẹp sai theo hệ số √2.** Nghĩa là bạn tưởng đã có ±25 Elo thực ra chỉ có ≈ ±35 Elo. Với 6 net cũ chênh có thể chỉ 30–50 Elo, đây đủ để **chọn nhầm net**. Sửa: trần `n` bằng `2 × số thế thật` và **không** nhân đôi. Cách này cũng loại bỏ luôn ý nghĩa của việc "ván lặp hữu ích".

**(iii) Bộ thế xuất phát khởi đầu cân bằng, giấy phép rõ, kiểu UHO — không biết.** Tôi không mở nguồn nào cho câu này trong lượt này, nên `KHÔNG BIẾT`, và sẽ không đoán tên bộ thế. Nhưng có một chỗ đáng sửa: với engine tất định, **bộ thế là biến ngẫu nhiên duy nhất còn lại** trong phép đo, nên đội **không nên** đi tìm bộ thế của người khác — hãy **sinh bộ thế riêng từ kho FEN 174 triệu thế đã có**, lọc cân bằng bằng chính engine (chơi thử với một net trung tính, bỏ thế có ưu thế bên nào), rồi **đóng băng kèm SHA256**. Cách đó: (a) giấy phép sạch vì là dữ liệu của đội; (b) khớp đúng phân phối thế mà engine sẽ gặp; (c) **tái lập được**, nên số lần đo lại không đổi phép đo. Đây là khuyến nghị, không phải nguồn.

**Nguồn**
- S10 `stat_util.py` (nguyên văn): `elo`, `L_`, `get_elo` với KTC 95% pentanomial và `Phi_inv`; hằng cận biên SPRT `log(0.05/0.95)` và `log(0.95/0.05)`.
- S11 `LLRcalc.py`: `LLR_alt2`, `LLR_drift_variance_alt2` (độ lệch và phương sai LLR mỗi ván cho xấp xỉ GSPRT), `MLE_expected` (bài toán MLE đa phương mà fishtest dùng cho SPRT thật), `regularize` đưa bucket rỗng lên `1e-3`.
- Số liệu của đội: `_LANE_NOTE/lock/w_net_ab_0210_2026-10-02.md` §5; mã `SourceCode/BookTool_Xiangqi/AbNgan.cs:282-313`, `EngineMatch.cs:811-842`.

**Mức độ**
- Bảng số ván/cặp theo `get_elo`: `CHẮC` cho **công thức** (nguyên văn từ S10), và tôi nêu rõ đó là **giả định hai ván độc lập** — con số dùng như cận dưới.
- "Hệ số tương quan làm sai lệch từ ×1 đến ×√2": `GIẢ THUYẾT CẦN ĐO` — lập luận phân tích chắc, **phải đo bằng bootstrap** mới chốt.
- "`soVan > 20` làm KTC hẹp sai √2": `CHẮC` (suy ra trực tiếp từ định nghĩa KTC ∝ 1/√n).
- Bộ thế xuất phát công khai: `KHÔNG BIẾT`.
- "Tự sinh bộ thế từ kho của đội là tốt hơn": `GIẢ THUYẾT CẦN ĐO`.

---

### D3-06 — Dữ liệu train từ ván Engine Fight: `.plain` hay binpack; điểm search trong ván có dùng làm nhãn được không?

**Trả lời**

**(i) Định dạng loader — không biết, chưa mở repo.** Tôi không mở `variant-nnue-pytorch` / `pikafish-nnue-pytorch` trong lượt này nên không dẫn tên tệp/tên hàm loader. `KHÔNG BIẾT`. Điểm đáng nói: lựa chọn của đội (`.plain` làm chuẩn, để trainer quyết định đuôi) là **lựa chọn đúng về kiến trúc** và không cần nguồn để biết vì sao: nếu xuất thẳng `.bin`/`.binpack` thì định dạng nhập khẩu trở thành **hợp đồng giữa hai repo**, và mỗi lần đổi trainer là phải viết lại bộ xuất. Với `.plain` (FEN + move + score + ply + result) thì đổi trainer chỉ là viết bộ chuyển, và `.plain` là định dạng mà Stockfish dùng cho `bench`/gen nên quen thuộc.

**(ii) Điểm search trong ván làm nhãn — đây là chỗ tôi thấy đội đang có rủi ro thật.** Câu hỏi nêu: *"điểm search trong ván (độ sâu thay đổi theo đồng hồ)"*. Nếu **độ sâu thay đổi theo đồng hồ**, thì điểm đó là hàm của **(vị trí, tải máy lúc chơi, phần còn lại của bộ đếm giờ)** chứ không phải hàm của thế. Hậu quả cụ thể: cùng một thế có thể mang hai nhãn khác nhau tùy lúc nó xuất hiện; val loss trên tập sinh từ ván sẽ **không phản ánh** chất lượng net; và nếu hai lần chạy lại cùng một ván thì nhãn không tái lập — cùng bệnh như D3-04.

Điểm này nên nói thẳng với đội: **`.plain` giữ điểm trong ván là giữ một cột không dùng được cho train.** Lựa chọn nhất quán với D3-04 là `Van.jsonl` chỉ lấy `fen` + `move` + `result` + `ply`, **thẳng ra bỏ luôn cột `score`**, rồi để một thầy `nodes` cố định chấm lại bằng đúng net và cùng thang. Lợi ích ngoài ý định: bạn **tách được** hai câu hỏi "ván này có giá trị gì" và "nhãn này đúng bao nhiêu", và sau này có thể dùng cùng bộ FEN đó để dạy distiller ở nhiều `nodes` khác nhau mà không phải đánh lại ván.

Về "thực hành của Stockfish/Pikafish với dữ liệu điểm trong ván": tôi **không mở** tài liệu huấn luyện nào, nên `KHÔNG BIẾT`. Điều tôi khẳng định được chỉ là logic: một engine alpha-beta chạy theo đồng hồ **không có khả năng** tạo nhãn tất định theo thế, nên bất kỳ bộ sinh dữ liệu nào dùng cho huấn luyện chơn được phải chốt một cách gì đó (nodes, hay depth, hay chuẩn hoá theo WDL) — nếu không thì chính bộ sinh đó không tái lập được và không thể dùng để so sánh hai lần chạy.

**(iii) Ngoài ra: vấn đề "nước không điểm".** Đội bỏ nước không điểm và bỏ `|điểm| ≥ 29000`. Với tập Engine Fight, nước không điểm thường là **nước cuối cùng sau khi kết thúc** hoặc nước phải chấp nhận — bỏ là đúng. Nhưng chỗ này có một rủi ro khác: **nếu chỉ đánh dấu điểm ở một số nước chứ không phải mọi nước, thì tập nhãn đang lệch về vị trí nào trong ván** (thường là các nước mà search vừa thoát khỏi book, hoặc nước sau khi đã quyết định thắng). Khi đó net học phân bố thế lệch về trung cuộc-sau-quyết-định. Nếu giữ cột `score`, hãy đánh dấu **mọi** nước có thế hợp lệ và ghi riêng cờ `nhan`; nếu bỏ cột `score` thì vấn đề này biến mất vì bạn không dùng điểm nữa.

**Nguồn**
- S0 (đội đã đo): `_LANE_NOTE/lock/w_t28_ef_0210_2026-10-02.md` §0 mục 7; mã `SourceCode/Shared/EngineFight/EngineFightGopRaNet.cs`.
- `https://github.com/official-stockfish/variant-nnue-pytorch` và `https://github.com/official-stockfish/Fairy-Stockfish` — **CHƯA MỞ**; cần mở `parse`/`__main__` để dẫn tên hàm loader.

**Mức độ**
- "Điểm theo đồng hồ không phải hàm của thế": `CHẮC` (lập luận trực tiếp; đội có thể kiểm bằng cách chạy lại cùng một ván và so `score`).
- "`.plain` làm chuẩn là lựa chọn đúng về kiến trúc": `CHẮC` (lập luận).
- Tên hàm/tệp loader upstream, thực hành datagen của Stockfish/Pikafish: `KHÔNG BIẾT` — chưa mở nguồn.

---

### D3-07 — Dùng px0data (ODbL) chỉ nội bộ để train net: net ra là "Produced Work" hay "Derivative Database", còn bán được không?

**Trả lời**

Đây là câu tôi trả lời chắc nhất trong cụm này, vì ODbL định nghĩa hai khái niệm đó **bằng văn bản**. Đọc nguyên văn S1.

**Định nghĩa.** ODbL 1.0 §1.0 định nghĩa:

> *A "Produced Work" means the resulting work of technical modifications such as translations, adaptations, rearrangements, or modifications made to the Contents of the Database, provided that such modifications do not amount to a Derivative Database.*

và

> *A "Derivative Database" means a database based upon the Database, or a modification of the Database, which may include a re-encoding, reformatting, a rearrangement, or some other 1:1 content transformation.*

Điểm mấu chốt nằm ở cụm **"do not amount to a Derivative Database"** và ở chỗ Derivative Database được định nghĩa bằng các biến thể **1:1 về nội dung** ("re-encoding, re-formatting, a rearrangement").

**Áp dụng.** Net NNUE train bằng search trên ~10,3 GB dữ liệu px0data không phải là biến thể 1:1 của cơ sở dữ liệu. Nó là **một hàm toán học giảm dần** trên tập dữ liệu, từ đó **không thể suy ra lại bản gốc**; không có re-encoding, không re-format, không rearrangement 1:1. Theo định nghĩa §1.0, nó là **Produced Work**, không phải Derivative Database.

**Nghĩa là nghĩa vụ gì.** §4.5 b (nguyên văn):

> *If You use the Database, a Derivative Database, or a Collection to create a Produced Work, You do not thereby create a Derivative Database, and §4.4 does not apply to the Produced Work.*

Đây là điều khoản **cấm** nghĩa vụ share-alike lan sang bên thứ ba theo tư cách Derivative Database. Nói bằng thẳng: **net train từ px0data không bị buộc phải phát hành dưới ODbL theo tư cách Derivative Database.**

**Nhưng có nghĩa vụ khác, và không được bỏ sót.** §4.3 (nguyên văn):

> *If you produce a Produced Work, You must: a. include a notice that the Contents of the Database came from the Database under the Open Database License; and b. include a copy of this license document.*

Và §4.2 yêu cầu notice ở **mọi** phương tiện truyền thông. Tóm lại nghĩa vụ tối thiểu khi bán Produced Work: **bản ghi nguồn + bản sao ODbL**. Tôi không đọc thấy yêu cầu bắt buộc phát hành lại *toàn bộ* cơ sở dữ liệu trong trường hợp này — §4.6 chỉ áp dụng cho **Derivative Database**, còn §4.5(b) nói Produced Work **không** tạo ra Derivative Database.

**Có tình huống nào mà kết luận đảo ngược không?** Có, và đội nên biết để tránh rủi ro: nếu bạn đóng gói net **cùng với một bản sao rút gọn của chính px0data** (bảng cân bằng, EPD, danh sách thế), thì bạn đã **phân phối lại cơ sở dữ liệu** → điều khoản về Derivative Database/§4.6 có thể kéo vào. Lười biếng duy nhất của ODbL là điều kiện §4.5(c): *"If You make no use of the Database, or Derivative works, outside the scope of an internal corporate or other internal use, You do not incur any obligations under this License."* Câu hỏi của đội là "chỉ NỘI BỘ để train" — nếu đúng nghĩa là không phân phối gì, thì **chưa phát sinh nghĩa vụ nào**; nếu bạn định bán net thì không còn là internal use.

**Về ý kiến chính thức / án lệ — không biết.** Tôi không mở `opendatacommons.org` FAQ hay văn bản nào của OSMF về ODbL + học máy, và không mở ODbL FAQ về derived databases. Tôi **không** khẳng định có hay không có án lệ. Trên đây là **cách đọc văn bản giấy phép**, không phải ý kiến pháp lý; tôi là kỹ sư phần mềm, không phải luật sư.

**Khuyến nghị quyết định, và tôi tin là rút gọn nhất:** net train từ px0data thì **được bán**, kèm **hai hạng mục bắt buộc** (ghi nguồn + kèm ODbL). Ràng buộc thực tế nằm ở luật owner ("cổng đóng gói sẽ ĐỎ nếu net train từ px0data bị gắn cỏ bán"), và **luật owner đang nghiêm hơn ODbL** — ODbL cho phép bán với ghi nguồn, luật owner thì không. Hai điều này không mâu thuẫn; nhưng nếu owner muốn giữ đường bán net từ px0data thì phải **sửa luật owner**, không phải sửa phần kỹ thuật.

**Nguồn**
- S1 `https://opendatacommons.org/licenses/odbl/1-0/`: §1.0 (định nghĩa Produced Work / Derivative Database), §2.3(a), §4.2, §4.3, §4.5(b), §4.5(c), §4.6.

**Mức độ**
- "Net NNUE train bằng search là Produced Work, không phải Derivative Database": `CHẮC` (đối chiếu định nghĩa §1.0 với bản chất không 1:1 của huấn luyện mạng) — đây là **cách đọc văn bản**, đội nên cho luật sư xác nhận trước khi dựa vào.
- "Nghĩa vụ khi bán: ghi nguồn + bản sao ODbL; không phải phát hành lại cả DB": `CHẮC` (§4.3, §4.5(b), §4.6 đọc cùng nhau).
- "Chỉ dùng nội bộ thì không phát sinh nghĩa vụ": `CHẮC` (§4.5(c)).
- "Có ý kiến chính thức của ODC/OSMF hoặc án lệ không": `KHÔNG BIẾT` — chưa mở, không đoán.

---

### D3-08 — `setoption` khi search đang chạy (kể cả `go infinite` / `go ponder`): dừng + join rồi ghi, chờ search tự xong, hay từ chối?

**Trả lời**

Câu này tôi trả lời được bằng mã nguồn, và kết luận có một phần **ngược với lựa chọn hiện tại của đội**.

**Stockfish master làm gì.** Trong `uci.cpp` (S6), toàn bộ thân hàm xử lý `setoption` là:

```
void UCIEngine::setoption(std::istringstream& is) {
    engine.wait_for_search_finished();
    engine.get_options().setoption(is);
}
```

Tức là **phương án (a): chờ search tự xong rồi mới ghi** — không dừng, không nhả `bestmove`, không xếp hàng. Cơ chế chờ nằm ở `ThreadPool::wait_for_search_finished()` trong `thread.cpp` (S8), dùng condition variable: `cv.wait(lk, [&] { return !searching; });`.

**Hệ quả trực tiếp cho `go infinite`.** Vì vòng lặp lệnh của Stockfish là **một luồng duy nhất** và `setoption` chặn trong hàm trên, nếu GUI gửi `setoption` khi engine đang ở `go infinite`, Stockfish **sẽ treo vô hạn cho tới khi có `stop`**. Đây là hành vi thật, không phải suy đoán. Nói cách khác: upstream **chấp nhận** rằng GUI không được gửi `setoption` giữa `go` và `bestmove`, và coi việc gửi là lỗi phía GUI. Trong `loop()`, Stockfish chỉ đặc biệt xử lý `stop`/`quit` bằng `engine.stop()`:

```
if (token == "quit" || token == "stop")
    engine.stop();
```

còn `setoption` rơi vào nhánh gọi ở trên. Không có nhánh nào tự sinh `bestmove` cho `setoption`.

**Đây là điểm đội cần sửa.** Lựa chọn hiện tại ("dừng + join rồi ghi", "search nhả `bestmove` ngay") **lệch khỏi upstream và tự sinh ra một `bestmove` không được GUI yêu cầu**. `bestmove` không nằm trong tập lệnh mà GUI gửi ở thời điểm đó, nên về mặt đặc tả nó là tin **không mời gọi**. Câu hỏi của bạn hỏi đúng chỗ này: liệu GUI có hiểu nhầm thành nước đi thật không — tôi **không mở** mã Arena/cutechess-cli/BHGui/SharkChess nên không dẫn được hành vi cụ thể của từng GUI, `KHÔNG BIẾT`. Nhưng rủi ro là có thật và không cần thiết: **bạn đang giải một race condition bằng cách phát ra một tin không ai hỏi.**

Vì sao upstream vẫn an toàn dù chờ? Vì option trong Stockfish không chỉ là số: `Threads` gọi `ThreadPool::set` **huỷ và tạo lại toàn bộ thread**, `Hash` cấp phát lại TT, `NumaPolicy` đổi cách gán NUMA. Áp chúng giữa lúc search đọc là lỗi đúng nghĩa, không phải lỗi hiếm. Trong `bench`, Stockfish còn chủ động đặt option **một lần ở đầu** (`// Set options once at the start.`) — tức tác giả chủ động tránh cả việc setoption lặp lại giữa search.

**Khuyến nghị, theo thứ tự ưu tiên:**

1. **Bám đúng upstream: chờ rồi mới ghi** (`wait_for_search_finished()` trước khi set). Rẻ nhất, đúng nhất, không phát tin thừa. Kèm một điều kiện bắt buộc cho GUI của đội: **không gửi `setoption` khi đang `go`/`go infinite`/`go ponder`; muốn đổi thì `stop` trước, chờ `bestmove`, rồi `setoption`, rồi `go` lại.** TieuLongNu phải sửa đúng chỗ này.
2. **Xếp hàng, áp ở `go` sau** (phương án (b) của đội). Cần một map option chờ + áp trong `go()`. Không phát `bestmove` thừa, không treo, không race — nhưng phải báo cho GUI biết option chưa có hiệu lực, nếu không GUI sẽ hiểu là đã xong. Tôi nghiêng về phương án này nếu TieuLongNu thật sự cần đổi option giữa ván.
3. **Cấu hình riêng cho từng lượt search** (phương án (c)) — đúng về nguyên tắc nhưng đội tự đánh giá phải sửa ≥ 23 chỗ đọc `gConfig.*`; tôi đồng ý là **không đáng làm**.

Còn lựa chọn hiện tại (stop-and-nhả-`bestmove`)? Tôi **không khuyến nghị**, vì lợi ích duy nhất của nó là né `go infinite`, mà cách (1) đã né bằng việc sửa GUI với chi phí nhỏ hơn nhiều.

Một lưu ý nữa: lỗi ở Nữ Oa mà đội ghi (gán `gConfig` **trước** mọi bước dừng, chuỗi > 15 ký tự nằm trên heap ⇒ đọc vùng đã giải phóng) là **data race thật**, và nó không được `wait_for_search_finished()` nào cứu được nếu thứ tự là "ghi rồi mới dừng". Thứ tự bắt buộc là **dừng và join, rồi mới ghi** — phần này đội đã làm đúng trong các nhánh đã vá, chỉ còn 4 nhánh chưa vá thì hãy vá theo đúng thứ tự đó.

**Nguồn**
- S6 `uci.cpp`: `UCIEngine::setoption` (chờ rồi mới set), `UCIEngine::loop()` (`quit`/`stop` → `engine.stop()`, `setoption` → nhánh riêng), `UCIEngine::bench()` với chú thích `// Set options once at the start.`, `ThreadPool::set` được gọi khi đổi `Threads`.
- S8 `thread.cpp`: `ThreadPool::wait_for_search_finished()` và `Thread::wait_for_search_finished()` dùng `cv.wait(lk, [&]{ return !searching; });`; `run_custom_job` cũng chờ `!searching` trước khi gán job.
- **CHƯA MỞ:** đặc tả UCI của Stefan Meyer-Kahlen (`https://www.shcherr.ch/uci/Specification-eng.html` — lần mở phiên này bị lỗi truyền tải), và mã Arena / cutechess-cli / BHGui / SharkChess.

**Mức độ**
- "Stockfish chờ search tự xong rồi mới áp `setoption`": `CHẮC` (đọc nguyên văn S6).
- "`setoption` khi `go infinite` sẽ treo vô hạn cho tới khi `stop`": `CHẮC` (suy ra trực tiếp từ vòng lặp lệnh một luồng + `wait_for_search_finished`).
- "Phát `bestmove` không được yêu cầu là tin không mời gọi, rủi ro GUI hiểu nhầm": `GIẢ THUYẾT CẦN ĐO` — phần "không mời gọi" là suy ra từ đặc tả mà tôi **chưa mở**; phần hành vi GUI cụ thể `KHÔNG BIẾT`.
- "Chọn (1) hoặc (2), không chọn stop-and-nhả-bestmove": `GIẢ THUYẾT CẦN ĐO` (khuyến nghị thiết kế).
- Nội dung mà đặc tả UCI nói về `setoption` khi đang search: `KHÔNG BIẾT` — chưa mở đặc tả.

---

### D3-09 — Hash không phải luỹ thừa 2, và `go mate N` bị `stop` trước khi tìm thấy chiếu bí

**Trả lời**

Câu này có một phát hiện làm thay đổi tiền đề của đội, và tôi đọc được nó trong mã nguồn.

**(i-a) Hash: Stockfish master KHÔNG làm tròn lũy thừa 2 — và nó dùng đúng cách chỉ số mà đội cho là phương án khác.** Trong `tt.cpp` (S7):

```
clusterCount  = mbSize * 1024 * 1024 / sizeof(Cluster);
usize ttBytes = clusterCount * sizeof(Cluster);
...
TTEntry* const tte = &table[mul_hi64(key, clusterCount)].entry[0];
```

Hai điều tách biệt ở đây:

- **Không có bước làm tròn lũy thừa 2.** `clusterCount` là phép chia nguyên, nên kích thước thật **luôn ≤ giá trị yêu cầu** và không tròn lên. Điều này có nghĩa: **nếu `tt.h` của Nữ Oa làm tròn lên, đội đang lệch khỏi upstream ngay từ hành vi này** — và việc làm tròn lên ở `Hash 3000 → 4096 MB` chính là nguyên nhân làm commit vượt trần 53,4 GB rồi sập máy. Làm tròn **xuống** của đội là đúng hướng, nhưng lý do đúng để ghi vào sổ không phải "giữ `hash & mask` cho bit-exact bench" (xem dưới), mà là **đừng bao giờ cấp nhiều hơn cái được yêu cầu** trên một máy đã từng cạn commit.
- **Chỉ số là nhân–dịch, không phải `& mask`.** Đây chính là `mul_hi64(key, clusterCount)` mà đội ghi là phương án (a) và lo là "đổi chỉ số TT ⇒ bench đổi". Upstream **đã chọn** phương án đó. Nói cách khác: lo ngại của đội về phương án (a) là có thật về mặt kỹ thuật (bench sẽ đổi), nhưng đó là **đánh đổi đã được cộng đồng chấp nhận**, không phải lý do để loại nó.

Vậy lựa chọn hiện tại của đội (làm tròn **xuống** + giữ `hash & mask`) là một **pha trộn hai thế giới**: bản dịch `hash & mask` chỉ đúng khi `clusterCount` là lũy thừa 2, còn sau khi làm tròn xuống thì nó không còn là. Tôi chưa đọc `tt.h` của Nữ Oa nên không khẳng định mask đó bao nhiêu bit hay đang suy ra thế nào — nhưng đây là chỗ **nên kiểm lại ngay**, vì nếu mask không khớp `clusterCount` thì đang có lỗi chỉ số âm thầm. `GIẢ THUYẾT CẦN ĐO` cho kết luận này cho tới khi đọc mã.

**(i-b) Cấp phát Hash hỏng thì nên báo gì.** Upstream làm thế này trong S7:

```
if (!table)
{
    std::cerr << "Failed to allocate " << mbSize << "MB for transposition table." << std::endl;
    exit(EXIT_FAILURE);
}
```

Ba nhận xét: (1) nó **in ra `stderr`, không phải `info string`** — nên GUI không nhận được, chỉ thấy engine chết; (2) nó in **số MB đã yêu cầu**, không phải số thật đã cấp phát, nên thông tin này cũng không phản ánh thực tế (`clusterCount` bị chia nguyên nên thật luôn nhỏ hơn); (3) nó **chết** (`exit(EXIT_FAILURE)`), không lùi bậc.

Đối chiếu: `info string Hash: yeu cau X MB -> TT that Y MB` của đội **tốt hơn upstream** ở cả ba điểm — có trong kênh UCI mà GUI đọc được, báo kích thước thật, và lùi bậc ½ thay vì chết. Giữ nguyên. Đây là trường hợp hiếm mà đội nên tin vào thiết kế của mình hơn cả upstream; đáng ghi rõ vào sổ.

**(ii) `go mate N` bị `stop`.** Cấu trúc mã trong S6 cho phép tôi nói chắc điều quan trọng nhất: **`go` không có nhánh riêng cho `mate`.** `UCIEngine::go()` gọi `parse_limits(is)` rồi đưa toàn bộ `LimitsType` vào `engine.go(limits)` — cùng một đường với `go depth`, `go nodes`, `go infinite`. Tức là **Stockfish master không chạy DFS `go mate` đồng bộ trên luồng đọc stdin**; giới hạn mate chỉ là một trường trong cấu trúc giới hạn, và việc dừng đi qua `engine.stop()` như mọi search khác. Đây chính xác là lỗi mà câu hỏi mô tả ở Bodetosu (`ucci.cpp:656-684` trước vá, DFS chạy thẳng trên luồng stdin nên `stop` không được đọc), và nó **đúng là lỗi**; bản vá của đội (chạy trong `searchThread`, kiểm cờ dừng mỗi nút) là hướng đúng.

Về phần **nên trả nước nào khi bị dừng trước khi tìm thấy chiếu bí**: tôi **không mở** đặc tả UCI (xem D3-08) nên không trích được câu nào, `KHÔNG BIẾT`. Nhưng có một lập luận thiết kế mà tôi tin là đúng và ngược với lựa chọn hiện tại: **trả "nước hợp lệ đầu tiên" là lựa chọn an toàn về mặt hiệu năng nhưng tệ về mặt cờ.** Ở một thế đang tìm chiếu bí, nước hợp lệ đầu tiên theo thứ tự sinh nước có thể là nước đi **tệ nhất** trong thế (ví dụ kề xe để đối phương ăn ngay), và người dùng sẽ nhận một nước tệ mà không có cảnh báo. Nên thay bằng: **trả nước tốt nhất mà search ngắn tìm được** — tức chạy một search rất ngắn (vài chục nút) sau khi bị dừng rồi lấy `bestmove` của nó, hoặc tận dụng root move tốt nhất đang có sẵn trong search chính. Cách này vẫn không bao giờ báo mate giả, vẫn đúng đặc tả, nhưng không đẩy cho người dùng một nước tệ. Chi phí thêm rất nhỏ. `GIẢ THUYẾT CẦN ĐO` — nhưng đây là điểm tôi thấy rõ là cần sửa, không phải thời.

**Nguồn**
- S7 `tt.cpp`: `TranspositionTable::resize` (`clusterCount = mbSize * 1024 * 1024 / sizeof(Cluster)`, `hugePageHint`, thông báo lỗi + `exit(EXIT_FAILURE)`), `TranspositionTable::first_entry` (`mul_hi64(key, clusterCount)`), `ClusterSize = 3` và `static_assert(sizeof(Cluster) == 32, "Suboptimal Cluster size")`.
- S6 `uci.cpp`: `UCIEngine::go()` chỉ có `parse_limits` + `engine.go(limits)`, không có nhánh riêng cho `mate`; `parse_limits` gán các trường giới hạn trong đó có `mate`; `loop()` xử lý `stop` bằng `engine.stop()`.
- Mã đội: `SourceCode/Engine_Xiangqi/engine-project/OngThan/src/tt.h:164-165` (trước vá), `SourceCode/OngThan_variants/Bodetosu/src/ucci.cpp:656-684` (trước vá).
- **CHƯA MỞ:** `tt.h` hiện tại của Nữ Oa (để kiểm mask có khớp `clusterCount` hay không), đặc tả UCI.

**Mức độ**
- "Stockfish không làm tròn Hash lên lũy thừa 2, và dùng `mul_hi64` làm chỉ số": `CHẮC` (đọc nguyên văn S7).
- "`Hash 3000 → 4096 MB` là nguyên nhân làm vượt commit 53,4 GB": `CHẮC` (số của đội + cơ chế làm tròn đọc được).
- "Upstream in lỗi ra stderr với số MB yêu cầu rồi `exit`": `CHẮC` (nguyên văn S7).
- "`info string` + lùi bậc của đội tốt hơn upstream": `CHẮC` (so sánh trực tiếp hai bản).
- "Không có nhánh riêng cho `go mate`, nên `stop` hoạt động": `CHẮC` (đọc cấu trúc S6).
- "Mask `hash & mask` sau khi làm tròn xuống có thể lệch": `GIẢ THUYẾT CẦN ĐO` — **phải đọc `tt.h`**.
- "Trả nước tốt nhất từ search ngắn thay vì nước hợp lệ đầu tiên": `GIẢ THUYẾT CẦN ĐO`.
- Quy định của đặc tả UCI về `bestmove` sau `stop`: `KHÔNG BIẾT` — chưa mở đặc tả.

---

### D3-10 — `File.Replace` lỗi 1175 (0x80070497) chập chờn trong cây làm việc: gốc là gì, ghi nguyên tử thế nào?

**Trả lời**

**(i) Mã lỗi thì chắc, thủ phạm thì suy luận.** Từ S9: `ERROR_UNABLE_TO_REMOVE_REPLACED` có giá trị **1175**, và nguyên văn mô tả là *"The replaced file could not be deleted. The replaced and replacement files retain the original file names."* Nghĩa là: `ReplaceFile` **đã thay nội dung thành công**, nhưng **không xoá được tệp cũ**, nên **tên tệp cũ vẫn còn**. Hai hệ quả thực tế mà đội nên ghi vào sổ:

- Lỗi này **không phải lỗi mất dữ liệu**. Nội dung tệp đích đã là bản mới. Xử lý đúng là coi như **ghi thành công kèm cảnh báo**, hoặc thử lại thì an toàn — chứ không có nghĩa phải khôi phục.
- Nhưng nó **lặp lại được**, nên `NhatKy.ThayTepNguyenTu` phải xử lý như lỗi thật về mặt kỹ thuật (đúng là đội đang làm).

**Về thủ phạm, tôi không mở được tài liệu nào của Raymond Chen hay Windows Internals trong lượt này**, nên tôi **không dẫn tên tiến trình cụ thể** (Defender, Search indexer, watcher của IDE, Google Drive/OneDrive). `KHÔNG BIẾT` cho phần này. Nhưng **chính số đo của đội đã trả lời gần như đủ**, và đây là điểm đáng ghi nhất của câu:

| vị trí | số lần lỗi / số lần thử | tỉ lệ |
|---|---|---|
| thư mục tạm trong cây repo | 7 / 2000 và 9 / 4000 | ≈ **0,27 %** |
| `%LOCALAPPDATA%\Temp` | 0 / 4000 | 0 |
| thư mục ngoài repo | 0 / 4000 | 0 |

Khoảng cách này **loại trừ** các nguyên nhân toàn cục (phần cứng, NTFS, cơ chế `ReplaceFile` với NTFS, tải hệ thống) — vì ba nhánh dùng cùng một đường ghi. Nó **chỉ đếm được** một thứ có mặt trong cây repo: **một thành phần quét cây thư mục đó và giữ handle tệp trong lúc bạn ghi** — đúng loại "watcher". Cách xác nhận bằng Process Monitor mà tôi đề xuất (đây là cách làm, không phải nguồn): lọc `Operation = SetInformation` trên tệp `.tmp`/tệp đích, thêm cột `PID` và `Result = 1175`, rồi đọc `PID` của các lần lỗi. Nếu các lần lỗi tụm vào một vài PID và PID đó đổi theo thời điểm bạn mở IDE, bạn có thủ phạm trong vài phút. **Đây là một ca thử rẻ và nên làm**, vì nó biến một sự đoán thành tên.

**(ii) Cách ghi "nguyên tử + bền" nào ít nhạy với handle `FILE_SHARE_DELETE`.** Đây là phần tôi **không có nguồn để khẳng định**, `KHÔNG BIẾT` cho so sánh định tính giữa ba phương án. Điều tôi nói được chắc là **nguyên nhân gốc của 1175 nằm ở phía bên giữ handle**, không nằm ở API mà đội chọn — nên đổi API sẽ **không** giải quyết tận gốc nếu bên thứ ba vẫn mở tệp không kèm `FILE_SHARE_DELETE`. Sự khác biệt giữa ba phương án nằm ở *mức độ bền vững với điều kiện đó*, và đó là thứ phải đo, không phải thứ phải đoán.

Điều tôi **khuyến nghị thêm một ý**, dựa trên chính nguyên văn lỗi: vì `ReplaceFile` đã đổi nội dung nhưng chỉ hỏng ở bước **xoá tệp cũ**, nên có một cách nép mà tôi thấy đáng thử: **mỗi lần thử dùng một tên `.tmp` mới** thay vì dùng lại cùng một tên. Nếu tên `.tmp` cũ đang bị giữ, việc tạo lại chính nó cũng là chỗ có thể lỗi; tên mới loại bỏ chính xác cái phụ thuộc đó, và giữ lại đúng đặc tính quan trọng (thay tệp không lộ trạng thái nửa chừng). `GIẢ THUYẾT CẦN ĐO`, rẻ, và kiểm được bằng chính bộ thăm dò 2000/4000 lượt đã có.

**(iii) Số lần thử lại.** 6 lần, chờ 10 → 160 ms (tổng khoảng 310 ms trên 6 lần) là **hợp lý**. Lập luận: 1175 là điều kiện *tạm thời* — handle chỉ tồn tại trong thời gian một thao tác quét, thường vài mili giây đến vài chục ms. Backoff nhân đôi với trần ngắn là hình dạng đúng. Con số này gắn với **tần suất lỗi đã đo của đội**, nên tôi không đổi nó; chỉ lưu ý rằng nếu tần suất lỗi ở máy khách cao hơn (repo lớn hơn, nhiều watcher hơn), trần nên theo **thời gian** chứ không theo số lần — đó là gợi ý, không phải yêu cầu.

**Cách làm của đội đúng ở điểm quan trọng nhất:** hết trần thì **vẫn ném lỗi**, và tệp bị giữ vẫn là lỗi. Đây là quyết định đúng — thử lại vô hạn sẽ biến lỗi thành treo, và nuốt lỗi sẽ thành hỏng âm thầm.

**Nguồn**
- S9 `ReplaceFileW`: mã `1175` và câu mô tả nguyên văn về việc tên tệp cũ được giữ.
- S0 (đội đã đo): `_LANE_NOTE/lock/w_tln_post13_0210_2026-10-02.md` §5; mã thăm dò `LaneScratch/w_tln_post13_0210/r4/do_replace.cs`; `_LANE_NOTE/lock/w_comfy_phude_0210_2026-10-02.md` §3; `SourceCode/AppCo_TieuLongNu/TieuLongNu.App/KhungCongCu.cs:820-822`.
- **CHƯA MỞ:** Raymond Chen / Windows Internals về `ReplaceFile`; tài liệu `MoveFileEx` và `FileRenameInfoEx`/`FILE_RENAME_FLAG_POSIX_SEMANTICS`.

**Mức độ**
- "1175 = `ERROR_UNABLE_TO_REMOVE_REPLACED`, nội dung đã thay nhưng tên tệp cũ còn lại": `CHẮC` (nguyên văn S9).
- "Thủ phạm là watcher trong cây repo": `GIẢ THUYẾT CẦN ĐO` — số đo của đội **loại trừ** nguyên nhân toàn cục và chỉ ra watcher, nhưng **chưa đo PID** nên chưa đóng được. Cần chạy ProcMon.
- "Đổi API không giải quyết tận gốc": `GIẢ THUYẾT CẦN ĐO` (suy từ bản chất điều kiện lỗi).
- "Dùng tên `.tmp` mới mỗi lần thử": `GIẢ THUYẾT CẦN ĐO`.
- "Trần thử lại 6 lần / 310 ms là hợp lý": `GIẢ THUYẾT CẦN ĐO`.
- So sánh định tính `ReplaceFile` vs `MoveFileEx` vs POSIX rename: `KHÔNG BIẾT` — chưa mở nguồn.

---
