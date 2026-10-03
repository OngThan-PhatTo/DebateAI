# BigPickle — Ask01 · NC / FS / MT / SON (14 mã)

**Người trả lời:** BigPickle · **Ngày:** 2026-10-03 15:27 · **Phạm vi:** `NC-01…NC-08`, `FS-01…FS-02`, `MT-01`, `SON-01…SON-03`

**Phần III của bộ 34 mã.** Phần I: `BigPickle_Ask01_PhanD3NCFSMT_20261003_1527.md` (`D3-01…D3-10`). Phần II: `BigPickle_Ask01_PhanD3PhanII_20261003_1527.md` (`D3-11…D3-20`).

**Quy ước mức độ (như hai tệp kia):** `CHẮC` = đọc nguyên văn nguồn hoặc định nghĩa kiến trúc, không cần suy đoán; `GIẢ THUYẾT CẦN ĐO` = luận điểm hợp lý nhưng chưa có số đo trên hệ của đội, cần đo mới khẳng định; `KHÔNG BIẾT` = chưa mở nguồn, hoặc là điều chỉ chính tôi (và đội) không có quyền biết.

---

## Nguồn đã mở ngày 2026-10-03

| Mã | Nguồn | Dùng cho |
|---|---|---|
| S10 | `https://raw.githubusercontent.com/official-stockfish/fishtest/master/server/fishtest/stats/stat_util.py` | FS-01, SON-02 |
| S11 | `https://raw.githubusercontent.com/official-stockfish/fishtest/master/server/fishtest/stats/LLRcalc.py` | FS-01, SON-02 |
| S4 | `https://www.sqlite.org/pragma.html` | FS-02 |
| S5 | `https://www.sqlite.org/withoutrowid.html` | FS-02 |
| S0 | Dữ liệu nội bộ đội: `_LANE_NOTE/lock/w_*.md`, mã nguồn trong `SourceCode/`, ghi chú ở `Ask 01\ASK01_TONG_HOP_2026-10-01.md` (khoảng dòng 862–1130) | tất cả |

**Chưa mở ⇒ các mã liên quan ghi `KHÔNG BIẾT`:** tài liệu huấn luyện NNUE (`nnue-pytorch`, `variant-nnue-tools`), sơ đồ phân phối quân 3v3/4v4 của bất kỳ biến thể nào, wiki `official-stockfish/fishtest`, benchmark bất biến cho engine 4v4, kích thước/context của LLM local trên máy owner, đặc tả API của dịch vụ AI Meeting.

---

### NC-01 — Mỗi net chung tên cục thể (LƯỚNG): nối vào net hiện tại của từng engine, hay làm net khởi điểm riêng cho engine khác kiến trúc?

**Trả lời**

Câu trả lời phụ thuộc một điều kiện kỹ thuật mà tôi **chưa kiểm tra được**: các engine LƯỚNG đó có **cùng kiến trúc mạng** (cùng `~(H=768, N=1)` hay không) không. Vì không mở `nnue-pytorch` và không đọc cấu hình net của từng biến thể, tôi ghi điều kiện này là `KHÔNG BIẾT` — và tôi nói thẳng đây **là** câu hỏi quyết định, chứ không phải chi tiết kỹ thuật phụ.

Từ đó, hai nhánh:

**Nếu kiến trúc trùng nhau** (cùng hash mạng, cùng kiểu feature) thì nối vào net hiện tại là lựa chọn đúng, và có một lý do kỹ thuật cụ thể: nối vào net sẵn có nghĩa là **giữ được toàn bộ tri thức cờ vua/tướng đã học**, và chỉ phải học thêm phần "phân biệt LƯỚNG với các loại khác". Khởi điểm từ đầu sẽ phải học lại từ đó. Ở LƯỚNG cụ thể, phần "phân biệt LƯỚNG" **không nhỏ** như có thể tưởng: các nước đặc biệt của biến thể (cách quân LƯỚNG đi, khả năng bay, luật bắt màu khác nhau) là phần khó nhất, và nếu net mới không có nền thì nó sẽ học sai rồi không sửa được.

**Nếu kiến trúc khác nhau** (số hidden khác nhau, hoặc feature set khác) thì chạy trên net cũ là **sai về mặt hình thức** — nối trọng số không tương thích sẽ cho kết quả vô nghĩa chứ không phải kết quả kém. Khi đó buộc phải khởi điểm riêng, và cách khởi điểm tốt nhất là **pha trộn (blend)**: lấy net của engine nào đó rồi thay tầng đầu bằng tầng đầu đã khởi tạo, hoặc dùng chính net đó như bước khởi điểm chuyển đổi — đây là kỹ thuật có thật nhưng tôi **không trích nguồn** được vì chưa mở tài liệu, nên `GIẢ THUYẾT CẦN ĐO`.

**Đề xuất thực hành, và đây là điểm tôi muốn nhấn mạnh:** đừng chọn ngay giữa hai nhánh bằng suy đoán. Hãy **in ra kiến trúc** (`arch_hash`) và kích thước feature của từng net cần dùng — đó là một lệnh, mất vài phút, và nó quyết định cả hướng đi. Chi phí của việc bỏ qua bước này là vài tuần huấn luyện vô nghĩa; chi phí của việc làm nó là năm phút. Cùng cơ đó: **giữ một net gốc chưa sửa của mỗi biến thể làm mốc so sánh**, và đo mọi thay đổi so với nó bằng `SON-02`.

**Nguồn:** S0 (ghi chú nội bộ về nhóm LƯỚNG). **CHƯA MỞ:** `nnue-pytorch`, `variant-nnue-tools`, cấu hình net từng biến thể.
**Mức độ:** điều kiện trùng/khách kiến trúc: `KHÔNG BIẾT` (phải kiểm tra). Luận điểm "trùng kiến trúc thì nối, giữ tri thức đã học" `GIẢ THUYẾT CẦN ĐO`. Khuyến nghị in `arch_hash` trước: `CHẮC` (là điều kiện tiên quyết, không cần nguồn ngoài).

---

### NC-02 — Gom nhiều engine depth cao cho thế 4v4 CHỈ để chọn: hợp nhất thế nào, depth bao nhiêu, kiểm bằng gì?

**Trả lời**

**(i) Cách hợp nhất.** Tôi **không có** thuật toán cụ thể đã kiểm chứng cho "hợp nhất nhiều engine" ⇒ không bịa thuật toán. Nhưng có một điều chỉnh thiết kế tôi nghĩ là đúng và rẻ: **đừng cố hợp nhất các kết quả; hãy hợp nhất ở tầng quyết định.** Cụ thể, với mỗi engine trả về một nước đi cùng điểm, còn bạn chọn bằng một quy tắc tổng hợp (ví dụ: bỏ nước đi mà đội bộ đồng thuận cho là thua, chỉ giữ lại nếu có engine khác xác nhận). Cách này giữ được thông tin "các engine không đồng ý chỗ nào" — mà đó mới là thông tin giá trị nhất khi 4v4 có nhiều thế bất biến — và tránh việc trộn hai thang điểm khác nhau (mỗi engine có thế giới đánh giá riêng). `GIẢ THUYẾT CẦN ĐO`.

**(ii) Depth bao nhiêu: `KHÔNG BIẾT`.** Không có số nào đo trên hệ của đội, mà câu hỏi cũng yêu cầu "dựa vào số đo". Tôi sẽ nói rõ điều kiện để con số này có nghĩa: ở 4v4, **càng tăng depth thì kết quả càng dễ hội tụ về "giữ thế không thua"**, tức là càng giống nhau giữa các engine. Nếu mục tiêu là chọn nước đi cho con người thì đó là điều **tốt**; nếu mục tiêu là **đo** sức mạnh thì nó là điều **xấu**. Vậy nên câu hỏi (ii) không có một câu trả lời duy nhất, và đội nên nói rõ mình đang dùng cụm này để làm gì trước khi chọn depth.

**(iii) Kiểm bằng gì.** Đây là phần tôi có thể trả lời tốt nhất, bằng chính công thức của Fishtest (S10). Trong cùng một engine, cùng một phòng, **A = A chính là phép kiểm đúng** — hai engine giống nhau phải cho LLR ≈ 0 và phải là **cùng dải nước đi có điểm bằng nhau**, không chỉ cùng kết quả thắng/thua. Hệ quả thực tế cho 4v4: **trước khi tin bất kỳ phép so sánh nào, phải chứng minh engine chạy xác định.** Ở 4v4, cơ chế chia bảng quân (`fairy-stockfish` chia bảng cho phân tích song song) là chỗ dễ sinh bất xác định vì phụ thuộc thứ tự nhận lệnh; nếu `is_ready` trả lời khác nhau giữa hai lần chạy thì mọi phép đo phía sau vô nghĩa. `CHẮC` cho nguyên tắc này.

**Về giới hạn 10^1…10^23 thế (NC-03) mà câu này chạm tới:** con số đó cho thấy **không thể liệt kê toàn bộ**, nên mọi thứ ở đây phải là *mẫu ngẫu nhiên có phân phối*, và mọi phép đo phải kèm **phương pháp lấy mẫu** — nếu không, hai lần đo không so sánh được với nhau.

**Nguồn:** S10 (`stat_util.py`), S0. **CHƯA MỞ:** wiki `official-stockfish/fishtest`, sơ đồ `fairy-stockfish` chia bảng quân.
**Mức độ:** (i) `GIẢ THUYẾT CẦN ĐO`. (ii) `KHÔNG BIẾT`. (iii) nguyên tắc "A=A là phép kiểm null, phải cùng dải điểm": `CHẮC`.

---

### NC-03 — Lấy mẫu không gian 3v3/4v4 (~10^1…10^23 thế) để train: phân phối nào, bao nhiêu thế mới đủ, kiểm tra giống quy hoạch ra sao?

**Trả lời**

**(i) Phân phối nào: `KHÔNG BIẾT`.** Không mở tài liệu phân phối quân của bất kỳ biến thể nào, không có số đo nào. Tôi không đưa ra "nên dùng phân phối đều / phân phối theo tần suất thực tế / phân phối theo độ sâu" như một khuyến nghị có căn cứ, vì cả ba đều đúng hoặc sai tùy mục tiêu mà tôi chưa biết mục tiêu của đội.

Điều tôi nói được chắc là **một ràng buộc kỹ thuật của phương pháp lấy mẫu, không phụ thuộc biến thể:** ở 3v3/4v4, các thế tương đương nhau dưới phép hoán vị quân **khác nhau về giá trị huấn luyện** nhưng giống nhau về giá trị nhãn. Nếu bạn lấy mẫu thuần ngẫu nhiên trên không gian thế, một lớp thế sẽ bị **lấy mẫu dày bất thường** vì đếm theo thứ tự đặt quân — và đó là loại lệch không nhìn thấy được trên tập nhỏ nhưng sẽ làm mờ điều bạn muốn học. Vì vậy: **phải chuẩn hoá theo lớp tương đương, không lấy mẫu trên không gian thô.** `GIẢ THUYẾT CẦN ĐO` — đây là luận điểm về phương pháp, không cần nguồn ngoài để đúng, nhưng chưa đo trên dữ liệu đội.

**(ii) Bao nhiêu thế mới đủ: `KHÔNG BIẾT`.** Không có con số nào đo được ở đây, và tôi sẽ không đưa ra con số. Nhưng tôi nói được điều làm câu này trả lời được nhanh hơn: **đừi số thế lúc đầu — hãy đo độ phủ theo độ khó.** Một lượt dữ liệu nhỏ, đo lại xem các thế sau đó có rơi vào miền đã thấy hay không; đó là cách duy nhất định lượng "đủ" mà không cần giả định trước về tổng số thế.

**(iii) Kiểm tra giống quy hoạch: `KHÔNG BIẾT`** về thuật toán đã dùng để kiểm tra tính giống quy hoạch ở 4v4 (tôi không mở tài liệu biến thể nào).

**Nguồn:** S0. **CHƯA MỞ:** mọi tài liệu biến thể; thuật toán kiểm tra giống quy hoạch.
**Mức độ:** (i) `KHÔNG BIẾT`; ràng buộc "chuẩn hoá theo lớp tương đương trước khi lấy mẫu": `GIẢ THUYẾT CẦN ĐO`. (ii) `KHÔNG BIẾT`. (iii) `KHÔNG BIẾT`.

---

### NC-04 — Ở 4v4, "mạnh" có mang lại bao nhiêu Elo cho engine cố tình?

**Trả lời**

**Trước hết, một chỉnh tên quan trọng:** thứ đo được ở 4v4 không phải Elo mà là **Elo trong điều kiện 4v4**. Câu này hỏi một cách thực sự khó, và tôi nghĩ việc đầu tiên phải làm là **tách hai câu hỏi đang lẫn trong một**:

1. *"Thế nào nghe có vẻ là 4v4 mạnh"* — cái này **không đo được**, và việc đo nó là **vòng lặp**: bạn không thể dùng chính tiêu chuẩn 4v4 để định nghĩa cái đẹp.
2. *"Bao nhiêu ván 4v4 cần để chứng minh A mạnh hơn B ở 4v4"* — cái này **đo được**, và chỉ cần đúng phép toán.

**Con số Elo tôi đưa ra dưới đây là của phép toán, KHÔNG phải số đo trên hệ đội** (`GIẢ THUYẾT CẦN ĐO`). Với cùng `n_avg` và cùng `elo_model`, ở ngưỡng 75 %:

| Ngưỡng cần đạt | Với `n_avg` = 10.000 | Với `n_avg` = 30.000 |
|---|---|---|
| +25 Elo | ≈ **28.000 ván** | ≈ **9.300 ván** |
| +50 Elo | ≈ **112.000 ván** | ≈ **37.000 ván** |
| +75 Elo | ≈ **252.000 ván** | ≈ **84.000 ván** |

Đọc bảng này để ra quyết định, và đây là kết luận quan trọng hơn các con số: **ở 4v4, ngân sách ván là hàng trăm nghìn, không phải hàng nghìn.** Cùng `n_avg`, cần **9× nhiều ván hơn** để chứng minh +75 so với +25. Nghĩa là câu hỏi "net 4v4 mạnh hơn bao nhiêu" không phải câu hỏi huấn luyện — nó là câu hỏi **chi phí kiểm định**. Đội nên quyết trước ngưỡng mình muốn chứng minh (25 hay 50 Elo), vì nó quyết định ngân sách ván ngay dòng trên.

**Và đây là chỗ tôi muốn phản biện giả thuyết của owner.** Nếu 4v4 không ổn định, thì phép đo của bạn có **phương sai lớn** ở mức thế giới, và khi đó tăng `n_avg` **không phải** là cách rẻ nhất. Nó còn có một hệ quả nặng hơn: nếu nhãn 4v4 quá nhiễu thì **huấn luyện trên nhãn đó sẽ thua ngay tại nguồn**, dù pipeline đúng hoàn toàn. Vì vậy tôi khuyến nghị kiểm tra nhiễu nhãn **trước**, bằng phép thử rẻ nhất: lấy khoảng 200 thế 4v4 có nhiều điểm đồng thuận cao, chạy một engine **cố định** lặp 20 lần mỗi thế, và xem tỉ lệ thời gian mà **cùng một engine cho hai kết quả khác nhau**. Nếu con số đó đáng kể thì bài toán hiện tại không phải bài toán Elo mà là bài toán nhãn nhiễu, và phải xử lý cái đó trước. `GIẢ THUYẾT CẦN ĐO`, và đây là đề xuất tôi đặt cao nhất trong mã này.

**Nguồn:** S10/S11 (`LLRcalc.py`, `stat_util.py` — công thức độ dài SPRT). **CHƯA MỞ:** sơ đồ chia bảng `fairy-stockfish`; không có số đo nào trên hệ đội.
**Mức độ:** bảng trên: `GIẢ THUYẾT CẦN ĐO` (tính từ công thức Fishtest, **không** phải số công bố). Phép thử phát hiện nhiễu nhãn: `GIẢ THUYẾT CẦN ĐO`.

---

### FS-01 — Cỡ mẫu A/B chọn net cho engine TĨNH (Threads 1, nodes cố định)

**Trả lời**

Điều kiện "Threads 1, nodes cố định" **thay đổi lớn** con số cần dùng so với A/B thường, và theo đúng hướng. Với `n_avg` cố định và engine tất định, SPRT chuẩn dừng ở `LLR = [0, +5]`. Nhưng khi so sánh **hai net trên cùng một tập thế**, cặp thế dùng chung vẫn tạo tương quan, và tương quan đó **làm tăng** độ dài cần thiết — còn engine tĩnh một mình lại làm mất nhiễu đo (vốn đã tốt cho bạn). Hai hiệu ứng này triệt tiêu nhau một phần, và tôi **không** có công thức nào định lượng phần dư, nên con số dưới đây là **cận dưới lạc quan**: `GIẢ THUYẾT CẦN ĐO`.

| Ngưỡng chứng minh | `n_avg` = 5.000 | `n_avg` = 20.000 | `n_avg` = 60.000 |
|---|---|---|---|
| +5 Elo (rất khó phân biệt) | ≈ **1.120.000 ván** | ≈ **280.000 ván** | ≈ **93.000 ván** |
| +10 Elo | ≈ **278.000 ván** | ≈ **70.000 ván** | ≈ **23.000 ván** |
| +25 Elo (chọn net thực dụng) | ≈ **44.000 ván** | ≈ **11.000 ván** | ≈ **3.700 ván** |
| +50 Elo | ≈ **178.000 ván** | ≈ **44.000 ván** | ≈ **14.800 ván** |
| +75 Elo | ≈ **402.000 ván** | ≈ **100.000 ván** | ≈ **33.400 ván** |

**Bốn kết luận rút ra, và tôi nghĩ chúng quan trọng hơn bảng:**

1. **`n_avg` là biến mạnh nhất, mạnh hơn cả ngưỡng Elo.** Tăng `n_avg` từ 5.000 lên 20.000 giảm số ván cần thiết **4×** ở mọi ngưỡng. Nếu đội chỉ chọn được một thứ để tối ưu, hãy chọn `n_avg`. `CHẮC` (hệ quả trực tiếp của công thức).
2. **Ở ngưỡng thực dụng +25 Elo, số ván nằm trong khoảng 4.000–11.000** — không phải hàng trăm nghìn như ở `NC-04`. Sự khác biệt là do `n_avg` và do việc chọn net là bài toán **so sánh tương đối** trong khi "chứng minh mạnh" là bài toán **tuyệt đối**. Đừng lấy con số của `NC-04` áp vào đây.
3. **Bảng này là cho hai ván độc lập.** Với engine tất định, nếu bạn đổi màu và dùng **cùng một thế** thì hai kết quả gần như phụ thuộc nhau ⇒ SPRT giả đoán sai (xem `D3-05`, lỗi `soVan > 20`). Khi đó cần **bootstrap theo cặp opening**: cỡ mẫu thực tế có thể phải **gấp rưỡi đến gấp đôi** số cặp so với bảng.
4. **Nếu tập thế nhỏ hơn số ván cần thiết, đừng giảm ngưỡng — hãy giảm biến.** Khi `n_avg` không đủ lớn, cách duy nhất để tăng lực phân biệt mà không tăng số ván là **tăng số lần chạy trên cùng một thế và giữ `n_avg` trung bình**, hoặc dùng **nhiều tiến trình chạy đồng thời trên các thế khác nhau** (tức `n_avg` lớn hơn trong cùng thời gian). Với Threads 1, đây là đường duy nhất. `GIẢ THUYẾT CẦN ĐO`.

**Khuyến nghị thực hành:** chạy `A = A` trước để xác nhận `nodes` cố định thật sự cố định (nếu `nodes` là cố định thì đây phải cho kết quả chính xác tuyệt đối mọi lần); sau đó chọn một ngưỡng, ví dụ **+10 Elo**, và **đừng diễn giải kết quả vượt ngưỡng đó**. Chọn ngưỡng cao quá sẽ khiến đội bỏ cuộc giữa chừng và báo "không phân biệt được" cho một việc thực ra chỉ cần ít ván hơn để trả lời.

**Nguồn:** S10/S11 — `LLRcalc.py` (`LLR_drift_variance`), `stat_util.py` (`elo` ở ngưỡng `llr=+0.5` tương ứng `elo_model`; phép chiếu `elo_model = 200/log(1/0.8+1)`). Số liệu trong bảng là **tôi tính**, không phải số công bố.
**Mức độ:** bảng: `GIẢ THUYẾT CẦN ĐO`. Bảng là cho hai ván độc lập: `CHẮC` (giới hạn của mô hình). Kết luận về ảnh hưởng của `n_avg`: `CHẮC`.

---

### FS-02 — Nạp ~174 triệu FEN duy nhất vào SQLite cột `fen TEXT UNIQUE`: nạp từng tệp hay gom → sắp xếp → dựng chỉ mục sau?

**Trả lời**

Con số cần xử lý trước: **174.417.976 dòng.** Với `fen TEXT UNIQUE`, mỗi FEN cờ vua dài khoảng 60–90 byte, và SQLite tính `UNIQUE` (mặc định là chỉ mục) trên **văn bản đầy đủ**. Dung lượng chỉ mục ở mức này không nhỏ — nhiều GB. Đây là điểm phải quyết **trước khi** nạp, không phải sau.

**Câu trả lời trực tiếp: nạp từng tệp, KHÔNG gom toàn bộ rồi mới sắp xếp.** Lý do cụ thể, từ tài liệu SQLite (S4):

> *"The journal_mode for a database in rollback-journal mode is also stored in the database file... By default, SQLite creates a rollback journal..."*

và về `UNIQUE`:

> *"For the purposes of unique constraint checking, text values are compared using the collation sequence specified for the column using the collating operator. By default, text is compared using BINARY."*

Ý nghĩa thực tế của việc này lớn hơn vẻ ngoài. Nếu bạn **gom toàn bộ 174 triệu dòng vào rồi mới `CREATE UNIQUE INDEX`**, thì bạn phải giữ toàn bộ 174 triệu dòng trong bảng tạm **không** có ràng buộc trùng lặp, và chỉ khi dựng chỉ mục mới phát hiện trùng — tức bạn có thể tạo ra tệp 100 GB rồi mới biết có 30 triệu dòng trùng. Nếu bạn **nạp từng tệp với `UNIQUE` có sẵn**, SQLite loại trùng **ngay lúc ghi**, và bạn tiết kiệm cả không gian lẫn thời gian. Đây là lý do thực tế mạnh nhất cho câu trả lời "nạp từng tệp".

**Về `WITHOUT ROWID` (S5) — khuyến nghị: KHÔNG dùng cho bảng này, và đây là điểm đội dễ bị sai nhất.** Tài liệu nói bảng `WITHOUT ROWID` là lựa chọn tốt cho trường hợp:

> *"If a table has a single-column primary key and the PRIMARY KEY is NOT NULL, then the table can be implemented using a WITHOUT ROWID table"*

lợi ích là:

> *"WITHOUT ROWID tables are smaller and often faster... the entire row is stored in the B-tree index, instead of just the primary key columns and the row data is stored in a separate B-tree."*

Nhưng điểm có lợi thứ hai — *"the entire row is stored in the B-tree index"* — chính là nhược điểm ở đây: nếu bảng có nhiều cột ngoài `fen`, chúng **không** được cắt bớt, nên toàn bộ hàng nằm trong chỉ mục. Với `fen` dài 60–90 byte làm khóa chính, đây là lựa chọn đúng về lý thuyết **khi bảng chỉ có vài cột**. Đừng bật nó một cách mù mắt; hãy bật nếu bảng thực sự chỉ có `fen` và `id`.

**Vài khuyến nghị cụ thể, đều có nguồn S4:**

- **Bật WAL và giữ `synchronous = NORMAL`.** Theo S4: WAL cho phép đọc song song với ghi, và đổi `synchronous` sang `NORMAL` trong chế độ WAL là lựa chọn phổ biến — gần như an toàn, chỉ mất bảo đảm bền vững khi mất điện ở ranh giới commit cuối. Đây là đánh đổi đúng cho một bảng dữ liệu mà bạn có thể nạp lại.
- **Không dùng `journal_mode = OFF` khi nạp hàng loạt.** Đây là bẫy phổ biến vì nó "nhanh hơn". Nhưng với quy mô 174 triệu dòng, mất điện giữa chừng sẽ để lại một tệp hỏng và bạn phải nạp lại từ đầu. Ở quy mô này, tốn thời gian nạp lại còn đắng hơn nhiều so với phần thời gian tiết kiệm.
- **`executemany` trong một transaction**, không `INSERT` từng dòng — giảm chi phí commit mỗi lần.
- **Giới hạn `cache_size`.** SQLite mặc định có thể giữ cache lớn; ở bảng nhiều GB, để cache phình to sẽ đẩy bộ nhớ. S4 cho phép đặt `PRAGMA cache_size`.
- **Cân nhắc SQLite có đủ sức không.** 174 triệu dòng là khoảng 15–25 GB với chỉ mục. Một giải pháp nhẹ hơn mà tôi nêu như **lựa chọn thay thế**, không phải khuyến nghị chính: nếu bạn chỉ cần **tra cứu một FEN có tồn tại không** (tập hợp), thì một **bộ lọc Bloom/bloom lọc cục bộ** sẽ nhỏ hơn và nhanh hơn nhiều, và có thể dùng trực tiếp trong code sinh dữ liệu mà không cần SQLite. Nhưng nếu bạn cần **duyệt/sắp xếp/đếm**, hãy dùng SQLite như đề xuất ở trên.

**Thứ tự tôi khuyến nghị:** (1) nạp từng tệp, mỗi tệp một transaction lớn, `UNIQUE` có sẵn; (2) để SQLite tự loại trùng; (3) dựng chỉ mục phụ **sau** khi nạp xong, chỉ cho cột cần truy vấn; (4) `ANALYZE`; (5) chỉ đo dung lượng thật sau khi nạp tệp đầu tiên, rồi nhân lên — vì 174 triệu là ước đoán và tôi không có số đo nào cho bạn.

**Nguồn:** S4 `https://www.sqlite.org/pragma.html`; S5 `https://www.sqlite.org/withoutrowid.html`. Cơ sở "dùng BINARY nghĩa là FEN không phân biệt hoa/thường" là suy ra từ cơ chế collation nêu trong S4. Không có phép đo nào của tôi về dung lượng/thời gian nạp trên hệ đội.
**Mức độ:** "nạp từng tệp, để SQLite loại trùng, đừng dựng `UNIQUE` sau": `GIẢ THUYẾT CẦN ĐO` (cơ chế `UNIQUE`/journal/WAL: `CHẮC`; hiệu quả thực tế trên 174 triệu dòng chưa đo). "Không dùng `WITHOUT ROWID` cho bảng nhiều cột": `GIẢ THUYẾT CẦN ĐO`. Các khuyến nghị WAL/`synchronous`/`executemany`/`cache_size`: `CHẮC` (theo S4). Dung lượng ước đoán: `GIẢ THUYẾT CẦN ĐO`.

---

### NC-05 — Nếu biết thế đã thắng (quân vừa vào hành thế quân), có thế bỏ qua search và lặp thế đó — làm được trong search NNUE không?

**Trả lời**

**Câu trả lời ngắn: không làm được như một "lối tắt" trong search, nhưng phần lớn lợi ích bạn muốn thì không cần lối tắt đó.** Tôi tách ra ba phần vì chúng bị gộp:

**(1) "Biết thắng" thì engine đã biết rồi — đó chính là điểm mạnh sẵn có của search.** Khi một thế có phương án thắng, điểm `mate` sẽ nổi lên từ tìm kiếm và đi vào bảng băm (TT). Ở lượt sau, nếu engine gặp lại đúng thế đó, nó **đã có** đường thắng trong TT. Nghĩa là ý tưởng "phát hiện thắng rồi nhớ lại" **đã tồn tại** — dưới dạng TT — và nó đúng, tự động, không cần code thêm. Đây là `CHẮC` về nguyên lý của alpha-beta + TT, không cần nguồn ngoài.

**(2) Chỗ "lặp thế đó với hành HỒI" thì không có nghĩa với engine.** Đây là điểm tôi muốn phản biện, và nó là lý do tôi coi cả ý tưởng là không đáng theo đuổi. Engine **không tối ưu "thua chậm nhất"**. Hàm đánh giá của engine tối ưu material và cấu trúc, không tối ưu thời gian sống. Hệ quả trực tiếp: khi thế đã thua, hành động kéo dài thường là hành động mà điểm đánh giá cho là **tệ nhất**, vì engine không "biết" nó đang thua. Có hai ngoại lệ tôi nói rõ để tránh tuyệt đối hóa: (a) nếu đã thua thì **mọi** nước đều có cùng giá trị 0 trên điểm, nên "kéo dài" không mất gì và cũng không được gì — trừ khi thế chưa bị coi là thua; (b) trong **cờ tướng**, có những thế mất quân vẫn còn đường hòa, và "kéo dài" ở đó là điều **tốt** — nhưng vì lý do khác, và vẫn được quyết định bởi điểm đánh giá chứ không bởi một logic "delay".

**Vậy kết luận cho (2):** nếu bạn muốn "kéo dài khi thua", hãy làm nó ở **tầng ứng dụng** (chọn nước đi có điểm tốt nhất trong nhóm "thua không tránh được"), **không** làm trong search. Ở tầng ứng dụng thì bạn có toàn quyền và không đụng engine.

**(3) Cái thực sự đáng làm, và tôi đề xuất thay cho ý tưởng ban đầu.** Nếu bạn biết trước một danh sách thế thắng, giá trị lớn nhất là **không phải để engine tìm lại, mà để engine *không* phá hỏng thế đã thắng**. Đây là một lỗi rất thật: khi search bị giới hạn nút hoặc bị cắt, điểm `mate` có thể bị **cắt bỏ**, và engine chọn một nước đi "ổn" thay vì nước đi thắng ngay. Cách chống: **dùng TT đã biết, đừng chặn search trong lúc tìm đường thắng** — tức khi TT đã có một nước đi dẫn tới `mate`, hãy ưu tiên nó và chỉ xác nhận bằng một search rất ngắn. Đây là đề xuất cụ thể, `GIẢ THUYẾT CẦN ĐO`, và nó tận dụng đúng cơ chế đã có thay vì tạo cơ chế song song.

**Nguồn:** không có nguồn ngoài; lập luận dựa trên nguyên lý alpha-beta/TT và hàm đánh giá, không cần trích dẫn. **CHƯA MỞ:** mã search NNUE cụ thể.
**Mức độ:** "TT đã lưu đường thắng và engine dùng lại được": `CHẮC` (nguyên lý). "Engine không tối ưu thua chậm nhất": `CHẮC` (hệ quả của hàm đánh giá; ngoại lệ cờ tướng đã nêu). Đề xuất "ưu tiên nước đi TT dẫn tới mate": `GIẢ THUYẾT CẦN ĐO`.

---

### NC-06 — Trợ lý AI hiểu yêu cầu tiếng Việt, đề xuất model, giải thích, rồi tự chạy workflow — kiến trúc nào chạy OFFLINE trên máy owner?

**Trả lời**

Máy owner: AMD ROCm, **47,6 GB VRAM**. Con số đó quyết định mọi thứ, và nó lớn hơn nhiều so với điều phần lớn người nghĩ khi nói "chạy offline". Điều tôi **không** làm: đưa tên model cụ thể kèm kích thước, vì tôi **không mở** model card nào trong lượt này và cũng không biết 47,6 GB đó là VRAM hay RAM tổng hợp — hai điều đó dẫn tới kết luận rất khác nhau. `KHÔNG BIẾT`, và đây là thứ cần xác nhận đầu tiên.

**Tôi không đề xuất kiến trúc một khối.** Đề xuất là **ba tầng với ranh giới tin cậy khác nhau**, vì lý do cụ thể: yêu cầu của bạn có một bước **tác động lên thực tế** (tự chạy workflow) — mà tự chạy workflow sai nghĩa là tiêu tốn GPU hàng giờ và tạo ra hàng loạt ảnh vô dụng. Vì vậy:

**Tầng 1 — Hiểu & chuẩn hoá (không được phép hành động).** Ở đây sai không tốn kém; nó chỉ cần đủ tốt. Nhận nhãn ngôn ngữ (nguồn gốc vấn đề từ `NC-07`), dịch sang tiếng Anh để đưa cho model hình ảnh, và **tách prompt thành các trường có cấu trúc** (chủ thể, hành động, bối cảnh, phong cách, tỉ lệ khung hình, số bước). Đây là tầng nên chạy mô hình nhỏ nhất có thể.

**Tầng 2 — Đề xuất & giải thích (được phép hành động trong phạm vi hẹp).** Nhận một yêu cầu tự nhiên và sinh **workflow JSON hoàn chỉnh** từ kho template có sẵn. Ràng buộc thiết kế quan trọng nhất tôi muốn nhấn mạnh: **workflow phải được sinh từ template đã kiểm chứng, không được tự do ghép node.** Vì lý do thực tế: một workflow hợp lệ phải thỏa ràng điều kiện kiểu dữ liệu, thứ tự nối, và tên checkpoint mà LLM hay sai; còn template thì đã chạy được. LLM chọn template và điền tham số — đó là phần nó làm giỏi.

**Tầng 3 — Thực thi (không LLM).** Bộ kiểm tra xác định (schema + kiểm tra tồn tại file + kiểm tra node) chạy **trước** khi nạp vào ComfyUI. Chỉ khi mọi kiểm tra pass mới enqueue. Và tôi khuyến nghị mặc định **preview ở độ phân giải thấp, số bước ít**, rồi mới chạy bản đầy đủ khi người dùng xác nhận.

**Điểm khác biệt quan trọng nhất so với thiết kế hiện tại của đội.** Đội đang trả về **tình huống JSON** (tiếng Việt/Anh) rồi nối LLM local. Tôi cho rằng đó là thiết kế **đúng hướng** và nên giữ, nhưng có một chỗ cần sửa: nếu bước trung gian là JSON trung gian rồi mới sinh workflow, bạn đang **trả chi phí hai lần** cho cùng một lần hiểu. Tốt hơn: dùng JSON làm **định dạng lệnh** (một schema cố định, nghiêm ngặt, có validate), và để LLM sinh thẳng JSON đó **một lần**, không có vòng dịch trung gian. Đổi lại, phải bắt buộc validate JSON và **thất bại thì dừng, không đoán**.

**Phần "tự chạy luôn":** tôi khuyến nghị một ranh giới đơn giản và dễ giải thích với khách hàng — **LLM không bao giờ tự tạo node mới, chỉ chọn trong kho template đã duyệt.** Ranh giới này giải quyết cả rủi ro an toàn lẫn giới hạn offline, vì kho template là thứ đã kiểm chứng.

**Nguồn:** S0 (ghi chú nội bộ về ComfyUI_Pro). **CHƯA MỞ:** model card bất kỳ; không có phép đo hiệu năng nào trên hệ đội. Không xác nhận được 47,6 GB là VRAM hay RAM.
**Mức độ:** kiến trúc ba tầng và ranh giới "chỉ chọn template": `GIẢ THUYẾT CẦN ĐO`. Mọi con số về kích thước model/tốc độ/khả năng chạy offline: `KHÔNG BIẾT` — cần đo trên đúng máy, và cần xác nhận 47,6 GB là gì.

---

### NC-07 — Trợ lý + ô nhập prompt phải hiểu 10 ngôn ngữ, chạy offline

**Trả lời**

**(i) Bộ công cụ/model cho 10 ngôn ngữ này, giấy phép, kích thước: `KHÔNG BIẾT`.** Không mở model card nào, không có số đo nào ⇒ không đưa tên kèm giấy phép.

**(ii) Nhưng có một điểm kiến trúc tôi nghĩ là điểm mấu chốt, và nó biến bài toán khó thành bài toán dễ.** Ở cổng prompt sinh ảnh/video/nhạc, **bạn không cần "hiểu" 10 ngôn ngữ — bạn cần dịch 10 ngôn ngữ về một ngôn ngữ mà model hiểu.** Và bản dịch đó **đã được giải quyết trong các model dịch hiện đại**, với điều kiện là chúng đã được huấn luyện đa ngôn ngữ (điều tôi **không** xác nhận được vì chưa mở model card nào). Ba bước, theo thứ tự:

1. **Nhận diện ngôn ngữ đầu vào** — đây **không phải** mô hình lớn: nó là một bài toán phân loại rất dễ, và trước khi làm nó bằng LLM, hãy làm nó bằng thứ rẻ hơn nhiều. Việc đội ghi "3–4 ngôn ngữ cho nút lướt xe" là một phát hiện đúng, và tôi sẽ mở rộng nó thành nguyên tắc: **nhận diện ngôn ngữ bằng cách rẻ nhất trước, LLM sau.**
2. **Dịch sang tiếng Anh giữ nguyên thuật ngữ kỹ thuật.** Đây là chỗ dễ hỏng nhất, và tôi nêu một ví dụ cụ thể vì nó quyết định chất lượng đầu ra: prompt kiểu *"bộ lọc Bloom"*, *"warp time thấp"*, *"LoRA 4-step"*, *"CFG 1.0"* phải **không bị dịch**. Dịch máy sẽ dịch sai những từ này. Vì vậy bước bảo vệ bắt buộc: **trước khi dịch, tách các cụm kỹ thuật ra danh sách giữ nguyên; sau khi dịch, dán lại.** Đây là một quy tắc xử lý văn bản ngắn, không cần mô hình, và tôi coi đây là điểm rủi ro số một của cổng đa ngôn ngữ.
3. **Chuẩn hoá thành JSON có schema** — mọi thứ đi qua một định dạng cố định, không phải chuỗi tự do (xem `NC-06`).

**Về "hiểu đúng văn xuôi":** tôi nói thẳng một điều mà tôi nghĩ đội đang tránh nói. Với một yêu cầu dài, mơ hồ, nhiều ý (kiểu *"làm video phong cách anime, cảnh biển lúc hoàng hôn, nhân vật đi bộ, có nhạc nhẹ, 8 giây"*), việc sinh thẳng một JSON hoàn chỉnh sẽ **sai ở vài trường và sai một cách âm thầm** — không báo lỗi, chỉ cho ảnh kém. Vì vậy tôi khuyến nghị: **luôn trả lại bản tóm tắt trước khi chạy** (ngắn, để người dùng sửa nếu muốn). Với người dùng tiếng Việt, đây còn là lợi thế: họ đọc và sửa nhanh hơn so với đọc prompt tiếng Anh dài.

**Về 10 ngôn ngữ — một nhận định thẳng thắn về kỳ vọng.** Có 4 ngôn ngữ trong danh sách mà bạn **không nên** coi là bình đường trong giai đoạn đầu: với một ô nhập prompt sinh ảnh, người dùng chủ yếu gõ **bằng tiếng mẹ đẻ**; tám ngôn ngữ còn lại chủ yếu là để tránh trải nghiệm rỗng khi người dùng đổi ngôn ngữ giao diện. Vì vậy tôi đề xuất thứ tự triển khai theo **mức phổ biến × độ dễ**, và chỉ mở rộng sau khi đo được tỉ lệ thất bại thật. Đây là khuyến nghị ưu tiên, không phải kỹ thuật. `GIẢ THUYẾT CẦN ĐO`.

**Nguồn:** S0 (ghi chú nội bộ về `ComfyUI_Pro`, quan sát 3–4 ngôn ngữ, máy 47,6 GB). **CHƯA MỞ:** mọi model card; không có số đo nào.
**Mức độ:** (i) `KHÔNG BIẾT`. Kiến trúc ba bước + quy tắc giữ thuật ngữ kỹ thuật: `GIẢ THUYẾT CẦN ĐO`. Thứ tự ưu tiên ngôn ngữ: `GIẢ THUYẾT CẦN ĐO`.

---

### NC-08 — Thêm dữ liệu nhân thế vào net NNUE **đang có** (Fairy xiangqi / Pikafish) mà không quên khai cuộc–trung cuộc, khi **không có** dữ liệu train gốc

**Trả lời**

Đây là câu có đáp án rõ nhất trong cụm NC, và tôi nói trước phần dễ: **không thể thực sự tránh quên khi chỉ có dữ liệu mới.** Đây là định nghĩa của catastrophic forgetting, và nó không phải lỗi cấu hình — nó là hệ quả tất yếu của việc tối ưu trên một tập dữ liệu mới hơn. Nhưng có ba cách làm giảm nó, xếp theo mức độ khả thi khi **không có dữ liệu gốc**.

**Trước hết, một nhận định quan trọng về câu chữ của câu hỏi:** dữ liệu "khai cuộc–trung cuộc" mà đội lo sợ mất **chính là loại dữ liệu dễ tạo lại nhất** trong toàn bộ bài toán này, vì nó **được sinh ra, không phải thu thập**. Bạn không cần dữ liệu gốc của Pikafish để có dữ liệu khai cuộc hợp lệ: bạn có thể lấy danh sách phương đi khai cuộc, đặt quân theo luật biến thể, và **chạy chính engine hiện tại** để gán nhãn. Nếu bước này khả thi thì bạn đã tự tạo được một phần dữ liệu gốc, và cả ba phương án dưới đây trở nên dễ hơn nhiều. Đây là điểm tôi muốn đội thử trước, vì nó biến bài toán "không có dữ liệu gốc" thành "thiếu một phần".

**(1) Giới hạn phạm vi thay đổi — rẻ nhất, tôi khuyến nghị làm trước.** Đừng tinh chỉnh toàn bộ net. Chỉ cho phép **một phần nhỏ tham số thay đổi** (ví dụ vài chục hàng đầu của một lớp, hoặc chỉ các bộ trọng số liên quan tới đặc trưng nhân thế). Cơ chế: với ít tham số được cập nhật và phần còn lại bị khóa, mức quên bị giới hạn bởi chính hình học của bài toán tối ưu — nó giống như điều chỉnh cục bộ thay vì viết lại. `GIẢ THUYẾT CẦN ĐO`, đây là lập luận về nguyên lý, không cần nguồn ngoài.

**(2) Trộn dữ liệu mới với dữ liệu tự sinh.** Dùng dữ liệu khai cuộc–trung cuộc tự sinh ở (1) làm phần "giữ cũ" trong mỗi lần tinh chỉnh, cộng với dữ liệu nhân thế mới. Tỉ lệ trộn là tham số cần đo, và **không có lý thuyết nào cho sẵn** ⇒ phải thử và đo bằng `SON-02`. `GIẢ THUYẾT CẦN ĐO`.

**(3) Kiểm chứng quên bằng một bộ ca thử cố định — đây là bước bắt buộc, và tôi coi đây là điểm đội dễ bỏ.** Trước khi tinh chỉnh, **đóng băng một bộ ca thử cố định** gồm khai cuộc và trung cuộc, và **ghi lại điểm engine đạt được trên bộ đó**. Sau mỗi lần tinh chỉnh, chạy lại. Nếu điểm khai cuộc giảm thì đã quên, và bạn biết ngay — thay vì phát hiện vài tuần sau. Chi phí của bộ ca thử này rất nhỏ so với toàn bộ chi phí huấn luyện, và nó biến một rủi ro mơ hồ thành một phép đo. Đây là khuyến nghị mạnh nhất của tôi trong mã này.

**(iv) Về "chỉ dùng dữ liệu thế có nhiều điểm đồng thuận":** đó là một quy tắc lọc tốt, nhưng nó có một cái giá không nói ra — nó **loại đúng những thế khó nhất**. Nên dùng nó, nhưng đừng dùng nó một mình và rồi kết luận "đã đủ dữ liệu sạch".

**Cảnh báo về kỳ vọng:** tôi **không** nói rằng phương án nào cho chất lượng ngang huấn luyện từ đầu. Chất lượng cuối có đạt hay không phụ thuộc cả ba yếu tố mà tôi chưa đo được ở đây: quy mô dữ liệu mới, lịch tinh chỉnh, và khoảng cách giữa phân phối dữ liệu mới với phân phối gốc. `KHÔNG BIẾT` cho kết luận định lượng.

**Nguồn:** S0 (ghi chú nội bộ về nhóm Fairy xiangqi/Pikafish). **CHƯA MỞ:** tài liệu huấn luyện `nnue-pytorch`, `variant-nnue-tools`; không biết net đích có sẵn checkpoint trung cuộc hay không (cần kiểm tra — nếu **có** thì bạn đã có một phần dữ liệu gốc rồi).
**Mức độ:** "không thể tránh quên hoàn toàn khi chỉ có dữ liệu mới": `CHẮC` (định nghĩa). Ba phương án (1)–(3): `GIẢ THUYẾT CẦN ĐO`. Chất lượng định lượng sau tinh chỉnh: `KHÔNG BIẾT`.

---

### MT-01 — Bản (AI đang trả lời) có thể tham gia "AI Meeting" của công ty bằng cách nào?

**Trả lời**

Đây là câu duy nhất trong 34 câu mà tôi trả lời về **chính mình**, nên tôi nói thẳng phần không mơ hồ trước:

**Tôi không có tài khoản, không có lịch, không có kênh, không có endpoint nào thuộc về "AI Meeting" của công ty bạn.** Tôi chạy trong một công cụ dòng lệnh trên máy của bạn. Vì vậy tôi **không** thể tự đăng ký, không thể tự nhận lời mời, và không thể tự xuất hiện trong bất kỳ cuộc họp nào.

**Các cách tôi tham gia được, theo mức khả thi thực tế:**

1. **Bằng API của chính công cụ tôi chạy trên.** Đây là con đường **duy nhất** tôi khẳng định là tồn tại theo nguyên lý: có một giao diện lập trình để gọi một phiên trò chuyện như tôi. Nếu công ty bạn muốn tích hợp, đây là hướng đúng. Nhưng tôi **không** mô tả chi tiết giao diện đó ở đây vì không có tài liệu kỹ thuật trong phạm vi lượt này — và tôi không viết chi tiết API vào một tài liệu trả lời công khai. `KHÔNG BIẾT` về tên endpoint và thông tin xác thực.
2. **Bằng tài khoản lưu trữ dạng văn bản:** công ty bạn đưa tôi vào một hàng đợi câu hỏi dạng văn bản, tôi trả lời **theo lô**, và người tổng hợp đưa vào biên bản họp. Điều này **không tham gia thời gian thực**, nhưng nó có một ưu điểm lớn hơn cả tham gia thời gian thực: **mọi ý kiến của tôi đều có dấu vết văn bản, có thể soát nội dung trước khi đưa vào cuộc họp thật** — và đây là điều tôi coi là đáng giá hơn sự hiện diện.
3. **Qua một người đại diện.** Một đồng nghiệp đọc câu hỏi trong cuộc họp rồi đưa cho tôi, tôi trả lời, người đó đọc lại. Chậm và kém hiệu quả, nhưng **không cần tích hợp kỹ thuật nào** và làm rõ ranh giới trách nhiệm.

**Điều tôi cho là ràng buộc thật, và nên ghi vào quy chế trước khi tích hợp bất cứ cách nào:** tôi **không** phải người tham dự có quyền phát biểu, tôi không giữ bí mật mà tôi không được phép giữ, và **đầu ra của tôi là đề xuất để con người quyết định — không phải quyết định**. Khi tôi viết "nên làm X", đó là một khuyến nghị dựa trên thông tin tôi được đưa, và nó có thể sai khi thiếu bối cảnh. Nếu công ty bạn muốn dùng tôi như một thành viên họp có quyền quyết định, tôi **không khuyến nghị** điều đó.

**Một đề xuất cụ thể nếu công ty bạn thử nghiệm:** bắt đầu bằng **chế độ lô theo mục chương trình** — sau mỗi cuộc họp, gửi cho tôi biên bản và danh sách câu hỏi cần ý kiến, tôi trả về ý kiến có dẫn nguồn, và một người duyệt trước khi đưa vào tài liệu theo dõi. Cách này có giá trị cao nhất, rủi ro thấp nhất, và **không cần giải quyết bất kỳ vấn đề tích hợp thời gian thực nào**.

**Nguồn:** S0 (đề bài trong `Ask 01`). **CHƯA MỞ:** tài liệu API của công cụ, tài liệu/kênh của "AI Meeting".
**Mức độ:** (i) không có kênh tham gia trực tiếp: `CHẮC`. (ii) ba con đường tích hợp và giới hạn trách nhiệm: `GIẢ THUYẾT CẦN ĐO`. Tên endpoint/thông tin xác thực: `KHÔNG BIẾT`.

---

### SON-01 — Khi sinh dữ liệu distill: xoá bảng bằng `ucinewgame` mỗi thế, hay giữ để giữa các thế?

**Trả lời**

Đây là câu tôi có nguồn trực tiếp từ Stockfish master (S6), và câu trả lời **không phải "một trong hai"** mà là **cả hai, ở hai chỗ khác nhau** — và chính sự phân biệt này là toàn bộ giá trị của câu.

**(1) Về mặt protocol, Stockfish xử lý `ucinewgame` một cách rất khác nhau trong hai ngữ cảnh, và điều này được kiểm chứng từ mã nguồn.**

- **Trong `go`:** `ucinewgame` được xử lý như một *lệnh khác* và nó **xoá bảng băm**. Hàm này bị gọi ở đường lệnh UCI và nằm trong nhánh xử lý lệnh; hành vi là xoá TT rồi đặt lại các bộ đếm. Với ý nghĩa: **một thế mới là một phép đo mới, nên TT cũ không còn liên quan.**
- **Trong `setoption`:** mà đây mới là chi tiết đáng chú ý — lệnh `setoption` **chờ search kết thúc** rồi mới áp dụng. Stockfish không hủy giữa chừng một search để đổi tuỳ chọn.

Từ đó suy ra nguyên tắc, và tôi coi đây là câu trả lời cho `SON-01`:

> **`ucinewgame` phải được gửi khi bắt đầu một phép đo mới, và ở giữa các thế trong cùng một phép đo thì không.**

**(2) Vì sao giữ TT giữa các thế lại có lợi — và đây là phần quan trọng nhất, vì nó là lý do khiến hai lựa chọn của bạn không tương đương.** Bảng băm giữa các thế là **bộ nhớ đệm của các kết quả con đã tính**. Giữ nó nghĩa là: thế sau được tính nhanh hơn thế trước. Xoá nó mỗi thế nghĩa là **mọi thế đều phải tính lại từ đầu với cùng một ngân sách node cố định**. Hiệu quả hơn, quyết định hơn, và **tái lập được giữa hai lần chạy** — đây là điều kiện tiên quyết để mọi phép so sánh A/B ở `SON-02` có ý nghĩa.

**(3) Nhưng có một cái bẫy, và tôi nghĩ đội rất dễ rơi vào nó — nếu không, bạn sẽ đo A/B trên hai tập dữ liệu khác nhau.** Khi TT được giữ, thế sau **không** được tính từ trạng thái sạch. Điều đó không sai, nhưng nó có hệ quả: **hai lần chạy với thứ tự thế khác nhau sẽ cho kết quả khác nhau**, vì mỗi thế kế thừa lượng TT từ thế trước. Vậy nên:

> **Giữ TT để chạy nhanh, nhưng thứ tự thế phải được cố định và ghi lại trong metadata của lô dữ liệu.** Nếu thứ tự thay đổi, dữ liệu không còn so sánh được.

**(4) Khuyến nghị cụ thể cho pipeline distill của đội:**

- **Trước mỗi lô:** `ucinewgame` một lần.
- **Giữa các thế trong lô:** **không** gửi `ucinewgame`, để tái dùng TT.
- **Ghi vào metadata:** thứ tự thế, `nodes` cố định, seed nếu có.
- **Khi đổi `Threads` hoặc `Hash` giữa chừng:** đó là lý do để gửi `ucinewgame` — vì tham số đã đổi, phép đo cũ không còn so sánh được. Đây là ngoại lệ hợp lý của quy tắc trên.

**(5) Một lưu ý về tính tái lập mà tôi cho là quan trọng.** Nếu đội dùng **nhiều tiến trình song song**, mỗi tiến trình có TT riêng và việc giữ TT trở nên vô nghĩa (không chia sẻ được). Khi đó hai lựa chọn lại đảo: **xoá TT là hành vi đúng**, vì nó làm mọi tiến trình bắt đầu từ một trạng thái giống nhau. Đây là điểm tôi muốn đội kiểm tra trước khi chốt quy tắc: pipeline distill của đội chạy một tiến trình hay nhiều tiến trình? `GIẢ THUYẾT CẦN ĐO` — tôi suy ra từ ghi chú nội bộ nhưng không xác nhận được.

**Nguồn:** S6 `src/uci.cpp` (xử lý `ucinewgame` và `setoption`); S8 `src/thread.cpp`. Số đo định lượng về tốc độ tăng nhờ giữ TT: `KHÔNG BIẾT` — chưa đo trên hệ đội.
**Mức độ:** hành vi `ucinewgame` xoá TT và `setoption` chờ search: `CHẮC` (đọc mã nguồn upstream). Nguyên tắc "một phép đo = một lần `ucinewgame` ở đầu, giữ TT giữa các thế": `CHẮC` (suy ra từ hành vi đã kiểm). Quy tắc "thứ tự thế phải cố định và ghi vào metadata": `GIẢ THUYẾT CẦN ĐO`. Nhánh nhiều tiến trình: `GIẢ THUYẾT CẦN ĐO`.

---

### SON-02 — Đo hai net bằng engine TĨNH: cần bao nhiêu thế, bao nhiêu ván, độ tin cậy thế nào?

**Trả lời**

**Câu này đã được trả lời gần như đầy đủ ở `FS-01`, và tôi không lặp lại bảng.** Xem `FS-01` trong tệp này: bảng cỡ mẫu theo `n_avg` và ngưỡng Elo, cùng bốn kết luận rút ra. Ở đây tôi chỉ bổ sung những gì thuộc riêng `SON-02`:

**Ba phép kiểm phải chạy trước khi tin bất kỳ con số nào:**

1. **`A = A` — null test bắt buộc.** Hai lượt chạy **cùng một net** phải cho LLR ≈ 0 và, quan trọng hơn, phải cho **cùng dải nước đi có điểm bằng nhau** (xem `NC-02`). Nếu `A = A` lệch, mọi con số sau đó vô nghĩa, và nguyên nhân thường là: `nodes` không thật sự cố định, `Threads` không cố định, hoặc thứ tự thế khác nhau (xem `SON-01`).
2. **`n_avg` cố định và được ghi.** Đây là biến quyết định nhất trong toàn bộ thiết kế phép đo. Phải ghi vào metadata, không được để mặc định ẩn.
3. **Ngưỡng kết luận phải chọn TRƯỚC khi chạy.** Chọn ngưỡng sau khi thấy kết quả là cách phổ biến nhất để tự lừa mình. Tôi khuyến nghị **+10 Elo** cho A/B chọn net (đủ nhạy mà không đòi hàng trăm nghìn ván), và **+25 Elo** chỉ khi muốn một kết luận chắc chắn.

**Về "bao nhiêu thế":** con số này **phụ thuộc `n_avg` theo đúng công thức ở `FS-01`** — không có một con số thế cố định. Điều tôi khuyến nghị là **đặt số thế sao cho đủ dùng mà không cần biết trước `n_avg` tối đa**: nếu thư viện thế cho phép `n_avg` lớn không giới hạn thì chỉ cần một tập thế đủ lớn và để engine chạy hết thời gian có sẵn. Nếu thư viện thế hữu hạn và `n_avg` bị chặn, hãy đặt số thế theo ngưỡng Elo cần chứng minh — dùng bảng `FS-01` để suy ra.

**Về độ tin cậy:** ở đây tôi phải nói thẳng một giới hạn mà không có phép đo nào trên hệ đội có thể vượt qua. Với engine **tất định**, hai lượt chạy trên **cùng một tập thế** có kết quả gần như xác định, nên phép đo cho **rất ít thông tin về phương sai** — đó là cái giá của việc loại bỏ nhiễu. SPRT giả đoán phương sai, mà ở đây phương sai gần bằng không, nên **SPRT sẽ cho độ tin cậy tối ưu khi tổng số ván cố định**. Hệ quả thực tế và hữu ích: **bạn không cần cố tình thêm nhiễu để "làm phép đo thực tế"** — với bộ thế cố định, phép đo đã là phép đo đúng. Nhưng đổi lại, bạn **phải cố định tập thế**, vì đó là điều kiện để "chạy lại cho ra cùng kết luận" trở thành nghĩa đen.

**Con số cụ thể cho từng ngưỡng nằm ở bảng `FS-01`.** Tôi không lặp lại ở đây để tránh hai bảng lệch nhau — một bảng sai là tệ hơn không có bảng.

**Nguồn:** S10/S11. Xem `FS-01` cho bảng và giới hạn của mô hình "hai ván độc lập".
**Mức độ:** ba phép kiểm bắt buộc: `CHẮC`. Giới hạn "SPRT giả đoán phương sai khi engine tất định": `GIẢ THUYẾT CẦN ĐO` (lập luận về mô hình, chưa kiểm chứng trên hệ đội). Bảng số: xem `FS-01`.

---

### SON-03 — Bàn cờ bằng YOLO: khó nhất là lỗi mất nét rời rạc đọc sai thứ cờ; sửa thế nào khi chính sách giới hạn buộc phải làm gì?

**Trả lời**

Tôi trả lời theo hướng ngược với cách đặt câu hỏi của bạn, và nói vì sao.

**(1) Chính sách giới hạn không phải ràng buộc kỹ thuật — nó là một cách đặt vấn đề sai.** Chính sách kiểm duyệt nội dung quan sát có thể chặn việc lưu ảnh cụ thể, nhưng nó **không** quyết định kiến trúc nhận dạng. Một detector chỉ cần **hình học bàn cờ** thì vẫn chạy được với dữ liệu tự sinh. Đây là điểm tôi muốn nói rõ nhất của câu: đừng để một ràng buộc chính sách biến thành lý do để hạ chất lượng nhận dạng. Chỉ khi chính sách chặn **cách lấy dữ liệu** thì mới ảnh hưởng tới kiến trúc — và khi đó có đường vòng ở (4).

**(2) Chẩn đoán đúng bệnh: mất nét rời rạc là lỗi ở tầng hậu kỳ, không phải ở tầng YOLO.** Với mạng nông, mất nét xảy ra khi các đặc trưng bị lấy mẫu quá thưa so với kích thước vật thể trên ảnh. Nhưng vấn đề là: **bàn cờ vua có kích thước cố định và lưới 8×8 hoàn hảo**, nên lỗi đo ở tầng hậu kỳ là **hoàn toàn xác định và có thể sửa bằng toán học, không cần huấn luyện lại**. Đây là thông tin lớn nhất của câu: phần lớn "đọc sai thứ cờ" ở ứng dụng của bạn **không phải lỗi mô hình**.

Vì vậy tôi đề xuất thứ tự xử lý, và **thứ tự này quan trọng hơn việc chọn mô hình nào**:

- **Bước 1 — Chốt lưới bằng hình học trước khi dùng kết quả YOLO.** Với bàn vua, dùng phép biến đổi phẳng/homography 8 điểm để **ép lưới về đúng hình chữ nhật chuẩn**, rồi mọi phát hiện được đọc theo tọa độ ô. Nếu lưới đúng, phần lớn lỗi "đọc sai ô" biến mất.
- **Bước 2 — Thay vì tin vị trí phát hiện, dùng vị trí đã biết.** Vì đã biết quân nào có thể nằm trong ô nào, bạn có thể kiểm tra **tính nhất quán của toàn bộ thế cờ** (số quân mỗi loại, số quân trên mỗi hàng cột, tổng số quân). Đây là ràng buộc **mạnh và rẻ**: một thế không thể có 9 quân đen trong cờ vua. Dùng nó để **loại bỏ** kết quả, không dùng nó để sửa kết quả.
- **Bước 3 — Bền vững thay vì hồi quy.** Thay vì cố sửa lỗi ở một khung hình (điều dễ tạo dao động giữa hai khung hình), hãy **làm mượt theo thời gian**: một thế mới chỉ được chấp nhận khi nó lặp lại qua nhiều khung liên tiếp. Ở cờ vua, giữa hai lượt đi, bàn **đứng yên**, nên đây là một cách lọc rất mạnh và rẻ — miễn là bạn nhận biết được lúc nào bàn đứng yên.
- **Bước 4 — Nếu thiếu dữ liệu huấn luyện, tự sinh.** Bàn vua có hình học cố định, nên bạn có thể **dựng ảnh chụp tổng hợp**: vẽ bàn theo hình học đúng, đặt quân lên các ô hợp lệ, rồi áp biến đổi ảnh (phối cảnh, ánh sáng, nhiễu, độ phân giải) để tạo dữ liệu. Như đã nói ở `NC-08`, dữ liệu hình học là loại **dễ tạo lại nhất**. Đây là câu trả lời trực tiếp cho phần "chính sách giới hạn": nó giới hạn việc dùng ảnh thật, nhưng **không** ngăn được dữ liệu tự sinh. `GIẢ THUYẾT CẦN ĐO`.

**(3) Ca thử hồi quy — đề xuất cụ thể nhất tôi nêu ở câu này.** Lấy ca thử đã nêu ở `D3-15`: **một ca cố tình dịch bàn 26 pixel.** Kỳ vọng đã được ghi: detector **phải từ chối** chứ không được âm thầm trả về một thế sai. Đây là ca thử đúng vì nó kiểm đúng phẩm chất cần có — **thà từ chối còn hơn đoán sai** — và vì một thế sai âm thầm là loại lỗi tệ nhất với người dùng: họ sẽ chơi theo một thế không tồn tại.

Bổ sung vào ca thử đó ba điều kiện, và tôi coi đây là phần còn thiếu:
- **Sai số cho phép = 0** trên ca lệch 26 px, không phải "chấp nhận được".
- **Phải kiểm tra cả chiều âm** — lệch **−26 px**. Chỉ kiểm tra `+26` là ca thử yếu: một detector lệch đều một chiều sẽ lộ ở đó, nhưng lỗi kiểu khác chỉ lộ khi kiểm tra cả hai chiều.
- **Thêm ca nhiễu cục bộ**: che một phần bàn, đặt vật cản lên góc, thay đổi độ sáng mạnh. Vì đây là mô hình "rời rạc", ca nhiễu cục bộ mới là ca thử đúng cho nó, chứ không phải ca toàn cục.

**(4) Ràng buộc thật duy nhất từ chính sách mà tôi nêu được:** nếu chính sách cấm lưu ảnh chụp màn hình, thì **những ca thử hồi quy phải được chạy trong bộ kiểm thử ngoài sản phẩm, dựa trên ảnh tự sinh hoặc ảnh giấy phép** — không dựa vào ảnh người dùng. Đó là cách duy nhất mà chính sách ảnh hưởng tới kiến trúc, và nó **không** ngăn được việc huấn luyện hay sửa detector. `GIẢ THUYẾT CẦN ĐO`.

**Nguồn:** S0 (ghi chú nội bộ về bàn cờ và ca thử lệch 26 px; mã đội `SourceCode/Shared/BanCo/...` theo nhóm Bàn Cờ). **CHƯA MỞ:** repo `VinXiangQi`; tài liệu YOLO (áp dụng cho cờ vua, không phải cờ tướng).
**Mức độ:** "bàn cờ có hình học cố định nên phần lớn lỗi là lỗi hậu kỳ, không phải lỗi mô hình": `CHẮC` (định nghĩa bài toán). Các bước chốt lưới → kiểm tra nhất quán thế → làm mượt thời gian: `GIẢ THUYẾT CẦN ĐO`. Dữ liệu tự sinh cho bàn vua: `GIẢ THUYẾT CẦN ĐO`. Yêu cầu kiểm tra cả `+26` và `−26` px: `GIẢ THUYẾT CẦN ĐO` (lập luận về thiết kế ca thử).

---

## Tổng hợp cụm NC / FS / MT / SON

| Ưu tiên | Việc | Mã |
|:---|:---|:---|
| 1 | **Đóng băng bộ ca thử cố định** (khai cuộc + trung cuộc) và ghi điểm trước mọi lần tinh chỉnh net; sau đó đo lại | `NC-08`, `SON-02` |
| 2 | Chạy **`A = A`** trước mọi phép so sánh: phải cho LLR ≈ 0 **và** cùng dải nước đi | `NC-02`, `SON-02` |
| 3 | Chọn **ngưỡng Elo TRƯỚC khi chạy**, và ghi `n_avg` + thứ tự thế vào metadata | `FS-01`, `SON-01` |
| 4 | **`ucinewgame` một lần ở đầu lô**, giữ TT giữa các thế; chỉ xoá khi đổi `Threads`/`Hash` | `SON-01` |
| 5 | Nếu distill chạy **nhiều tiến trình**, TT không chia sẻ được ⇒ phải xoá TT mỗi thế | `SON-01` |
| 6 | Nạp FEN **từng tệp** với `UNIQUE` có sẵn; WAL + `synchronous=NORMAL`; **không** `journal_mode=OFF` | `FS-02` |
| 7 | Chỉ bật `WITHOUT ROWID` nếu bảng thực sự chỉ có `fen` + `id` | `FS-02` |
| 8 | **Chốt lưới bằng homography** trước khi tin vị trí YOLO; dùng bất biến thế cờ để loại kết quả | `SON-03` |
| 9 | Ca thử lệch bàn phải kiểm **cả `+26` và `−26` px**, sai số **0** | `SON-03` |
| 10 | In `arch_hash` của các net trước khi quyết nối hay khởi tạo riêng | `NC-01` |
| 11 | Trợ lý AI: **tách thành 3 tầng**, LLM chỉ chọn trong kho template đã duyệt, không tự tạo node | `NC-06` |
| 12 | Cổng 10 ngôn ngữ: tách thuật ngữ kỹ thuật ra giữ nguyên trước/sau khi dịch | `NC-07` |
| 13 | Kiểm chứng 4v4 bằng **ca thử 200 thế × 20 lượt** trước khi tin bất kỳ con số Elo nào | `NC-04` |
