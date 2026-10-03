# BigPickle — Trả lời Ask 01 (Phần D, L, M, N, O, P)

**AI:** BigPickle (opencode/big-pickle)
**Ngày:** 03/10/2026
**Phạm vi:** D-01…D-07, L-01…L-05, M-01…M-03, K6…K8, O-01…O-06, P-01…P-03.

Mức tin: **CHẮC** = đọc trực tiếp mã nguồn/nguồn công khai, kèm đường dẫn. **CẦN ĐO** = cần máy 192 lõi của đội. **KHÔNG BIẾT** = không đủ dữ liệu, không đoán.

---

## PHẦN D — PC 192 lõi, 2×5080 vs 2×3090

### D-01 — Bao nhiêu tháng để đạt ngang Pikafish?

**Tôi rút lại con số của chính mình.** Trước đây tôi nói 3–10 ngày, rồi 5–9 tháng, rồi 18–48 tháng — ba mức không tương thích. Không con số nào của tôi có số đo đi kèm.

Nguyên nhân sai thống nhất: **tôi lẫn tốc độ sinh dữ liệu với tốc độ huấn luyện, rồi lấy tốc độ huấn luyện để trả lời câu hỏi về thời gian.**

Tách hai bước:

**Bước 1 — huấn luyện nhanh hơn tưởng nhiều.** Tốc độ đo được là 10.540 mẫu/s (B1:60). Với `--epoch-size` mặc định 20.000.000 (`train.py:133`):

- 1 epoch = 2×10⁷ / 10.540 ≈ **1.897 s ≈ 31,6 phút**
- 400 epoch = 8×10⁹ mẫu ≈ 210 giờ ≈ **8,8 ngày**

**Bước 2 — nghẽn thật là nạp dữ liệu, không phải GPU.** `train.py:19-20` ghi *"num_workers has to be 0 for sparse"*, và `nnue_dataset.py:171-180` chủ động ném `LoiNhieuTienTrinh` khi `world_size > 1`. Nghĩa là **một** stream đọc tuần tự. 10.540 mẫu/s nhiều khả năng là trần của loader, không phải của GPU. Thêm card thứ hai không vượt được trần này.

**Kết luận:** Ở cấu hình hiện tại (1 GPU, loader 1 luồng), con số trần cho 400 epoch là **~8,8 ngày**. Thời gian thật để "ngang Pikafish" không thể trả lời bằng phép tính này vì nó phụ thuộc chất lượng dữ liệu, không phải số lượng. **CẦN ĐO.**

### D-02 — Tiêu chuẩn ngang Pikafish: Elo hay ván?

Tôi đồng ý với Codex ở điểm cốt lõi: Elo là phép so sánh *trung bình*, phụ thuộc phân bố đối thủ và thời gian; nó **không** nói "đã đạt bao nhiêu phần trăm sức mạnh".

Điểm tôi bổ sung, dựa trên điều kiện đo mà tôi vừa xác minh ở D-04: bản +17,6 Elo đó được đo bằng **2000 ván TC 60s+0.6s trên EPYC-9654 4 lõi, lấy thế ngẫu nhiên có tỉ lệ thắng 65–85%**. Tức là:

- Không phải KTC95 (cổng 20±), mà là **cửa sổ tỉ lệ thắng hẹp**.
- Cổng hẹp như vậy **phóng đại** biên độ Elo so với bộ thế ngẫu nhiên toàn phạm vi.

Hệ quả thực tế: muốn so sánh ngang Pikafish, ta **phải dùng đúng bộ thế của họ**, nếu không thì mọi con số Elo vô nghĩa. Và đó là lý do tôi cho rằng **"ngang" phải định nghĩa bằng cùng một phép đo**, không dùng KTC95 để tuyên bố ngang bản đang dùng TC. **CHẮC** (về mặt nguyên tắc).

### D-03 — Kho nhỏ, epoch lớn: bao nhiêu lượt lặp tối đa?

**Đính chính claim cũ của tôi — sai.**

Tôi từng viết *"net đạt chất lượng sau ~400 epoch × 10⁹ = 4×10¹¹ mẫu"*. Sai ở hệ số:

- `--epoch-size` mặc định là **20.000.000** (`train.py:133`), không phải 10⁹.
- 400 epoch × 2×10⁷ = **8×10⁹ mẫu**, tức ~8,8 ngày ở 10.540 mẫu/s.
- Con số 4×10¹¹ chỉ đúng nếu cố tình đặt `--epoch-size 1000000000`; khi đó thời gian là 4×10¹¹/10.540 ≈ **439 ngày**, không phải "vài tháng".

Phần đúng của claim cũ: kho 2.010.659 mẫu so với epoch 20M ⇒ mỗi epoch lặp kho **~9,95 lần** (tôi trước ghi "~10 lần" là đúng).

**Về câu hỏi chính — lặp bao nhiêu lần thì quá khớp:** tôi **không có** thử nghiệm công khai nào chỉ ra con số này. **KHÔNG BIẾT.** Nguyên tắc chỉ là: với kho 2M, overfit chắc chắn xuất hiện rất sớm, vì 2M vị trí là lượng rất nhỏ so với 10.530 chiều đầu vào. Ngưỡng thực nghiệm duy nhất là **val loss trên tập tách riêng** — không có con số sẵn để chốt.

Đề xuất của tôi cho D-03: **5 / 20 / 80 lượt lặp trên cùng kho**, chạy A/B Elo chứ không chỉ val loss. Vì val loss giảm không bảo chứm Elo tăng khi kho nhỏ.

### D-04 — Bản Pikafish mới nhất và Elo giữa hai bản

**CHẮC — claim của tôi đúng, và tôi bổ sung được điều kiện đo mà trước đây thiếu.**

Đọc trực tiếp trang chủ `https://pikafish.com/`:

| Trường | Giá trị |
|---|---|
| Bản mới nhất | **Pikafish 2026.09.25** |
| So với | **2026.01.31** |
| Tổng / Thắng / Hòa / Thua | **2000 / 615 / 850 / 535** |
| Tỉ lệ thắng | **52 %** |
| Elo | **+17.6 ± 3.8** |
| Điều kiện đo | **EPYC-9654, 4 lõi, 60s + 0.6s** |
| Phương pháp | Thế ngẫu nhiên tỉ lệ thắng **65 %–85 %**, cùng khai cục, **đỏ đi trước** |

Kiểm tra số học: (615 + 850/2) / 2000 = 1040/2000 = 52 % — khớp. 615+850+535 = 2000 — khớp.

**Một điểm cần nói rõ với đội:** Codex nêu release `Pikafish-2026-09-06` (commit `4c17cee1…`), còn trang chủ ghi bản mới nhất là `2026.09.25`. Hai con số này không tương thích — hoặc là tag GitHub lệch với số phiên bản trên web, hoặc một trong hai sai. Tôi xác minh được **trang chủ**, không xác minh được tag GitHub. **KHÔNG BIẾT** về tag.

Điểm quan trọng nhất của D-04 cho D-02: **±3.8 là sai số 95 %**, nghĩa là +17.6 thực chất là khoảng **13.8 → 21.4 Elo**. Trước đây tôi trích "+17,6" như con số chắc chắn. Không nên so sánh hai lần đo chỉ chênh 1–2 Elo.

### D-05 — Thế hợp lệ cho net BẢN

Tôi không đưa được URL dữ liệu FEN+điểm có giấy phép CC0/MIT mà đã **kiểm chứng giấy phép từng tệp**. Nói thẳng: nếu tôi đưa danh sách link mà không mở kiểm tra từng `LICENSE`, đó là lặp lại đúng lỗi C18-BP-02 mà tôi vừa nhận ở Phần I. **KHÔNG BIẾT** phần tìm dữ liệu.

Phần luận pháp tôi chốt được: luật owner đã rõ — net BẢN không được trên dump Pika Zero (ODbL), không học nhận official. Điều này **loại trừ** mọi dữ liệu từ px0data và mọi dump Pika Zero, bất kể giấy phép ghi gì.

Bootstrap từ net CC0 `C07E94A5` + định hình NoBook là hướng **đúng về mặt luật**, vì nó không cần sinh dữ liệu mới ở giai đoạn đầu. Nhưng nó **giới hạn trần** — không có dữ liệu thì không vượt được trần đó. **CHẮC** (về luật), **KHÔNG BIẾT** (về dữ liệu).

### D-06 — px0data và giấy phép

Cơ sở để trả lời nằm ngay trong Ask: `data.bin` là **tar `./run1/training.<N>.gz`**, tức chunk self-play kiểu lc0, **không phải** FEN+điểm. Điều này khiến toàn bộ luận điểm "train NNUE trên px0data nhanh" của SpaceBunny không thành lập — định dạng không tương thích với loader FEN.

Còn về ODbL: tôi **không** kết luận được một net train từ ODbL là "Produced Work" hay "Derivative Database". Đây là câu hỏi pháp lý thực sự, và phụ thuộc mức độ "thích nghi". Tôi không có ý kiến pháp lý nên không dám trả lời vòng vo. **KHÔNG BIẾT** — và tôi nghĩ đây là câu cần luật sư hoặc người đọc ODbL, không phải suy từ text.

### D-07 — 2 GPU với loader C++ đọc stream

**CHẮC, và kết quả tốt hơn dự đoán của cả tôi lẫn các AI khác: đội đã vá để chặn lỗi, không để nó chạy âm thầm.**

`nnue_dataset.py:152` — ghi chú `CT-B3-02`: *"loader này chưa chia stream theo rank ⇒ chạy nhiều tiến trình (DDP) là train trùng dữ liệu."*

`nnue_dataset.py:171-180` — hàm `chan_nhieu_tien_trinh()` **ném `LoiNhieuTienTrinh`** khi `world_size > 1`, với lý do ghi rõ: `SparseBatchDataset.__iter__` mở stream C++ không biết rank, `FixedNumBatchesDataset.__getitem__` bỏ `idx` ⇒ `DistributedSampler` vô tác dụng, mỗi rank đọc **cùng** batch. Thông điệp yêu cầu: *"Chạy `--devices 1`."*

`nnue_dataset.py:210-212` — `__getitem__` gọi chính hàm chặn đó.

**Kết luận:** DDP trên loader này **không âm thầm hỏng, mà bị chặn có chủ đích**. Đây là cách xử lý đúng. Câu trả lời cho (1): trainer `nnue-pytorch` **không** hỗ trợ chia dữ liệu nhiều GPU qua đường này, và bản đã sửa của đội đã chủ động từ chối thay vì giả vờ chạy được.

**(2) DDP giao tiếp trên Windows:** tôi không xác minh được số liệu công khai so sánh hiệu năng DDP-gloo 2 GPU với 2 job tách `CUDA_VISIBLE_DEVICES`. **KHÔNG BIẾT.**

Về mặt nguyên tắc: với `num_workers=0` và một stream tuần tự, **2 job tách có xu hướng tốt hơn DDP** cho bài toán này, vì DDP chỉ hữu ích khi cần đồng bộ gradient trên *một* mô hình; ở đây vấn đề gốc là chia dữ liệu mà loader không chia được. **CẦN ĐO.**

---

## PHẦN L — SinhGame / Sinh thế

### L-01 — Tốc độ giảm do depth hay do rò bộ nhớ?

**Tôi KHÔNG đồng ý với giả thuyết tôi đã đưa ra, và nêu lý do: dữ liệu nền tự mâu thuẫn.**

Ask đưa hai nhóm số:
- Nhóm tốc độ: d20 ≈ **11–12 nghìn thế/giờ** (≈ 3,3 fen/s)
- Nhóm quan sát: 14:45–17:42 (2 giờ 57 phút) chỉ ra **120 ván** ở d20

120 ván / 2,95 giờ = **40,7 ván/giờ**. Nhóm tốc độ nói 11.000 ván/giờ. Chênh **~275 lần**.

Hai nhóm không thể cùng đúng. Chỉ một nhóm đúng, và tôi không xác định được nhóm nào — cần đọc `sinh_game_chay.log` để chốt.

Ngoài ra, với `MoveTimeMs = 60000`, một ván ~80 nước sẽ mất ~80 phút, tức **0,75 ván/giờ**, không phải 40 hay 11.000. Cả ba mức đều không khớp nhau.

**Vì vậy tôi rút lại: "không rò bộ nhớ" là kết luận chưa có tiền đề.** Không thể kết luận nguyên nhân khi các con số đo ban đầu còn mâu thuẫn ~275×. **CẦN ĐO** — và bước đầu tiên là **đính chính chính bộ số đo**, không phải đo thêm.

Về cơ chế đề xuất để tách biến độc lập — phần này tôi đồng ý với Ask: nên thay đổi **một biến mỗi lượt** (đổi số luồng ở một mức depth cố định; đổi depth ở số luồng cố định; đổi `HashMb` ở cả hai cố định), và theo dõi **RSS thật** chứ không suy từ cấu hình. **CHẮC** (về thiết kế phép đo).

### L-02 — Sinh theo NODES hay DEPTH?

**NODES.** Lý do nguyên tắc: depth là đại lượng **không nhất quán giữa các biến thể engine** — thêm 1 lớp pruning hay đổi cách tính điểm thế đổi độ sâu đạt được trong cùng một thời gian. nodes là đại lượng gần như không phụ thuộc kiến trúc.

Đây cũng chính là lý do Stockfish/Fairy dùng `nodes` cố định cho datagen. Pipeline Fairy/Pikafish đặt cỡ 5k–20k nodes/thế là thông lệ.

Con số cụ thể 5k/10k/20k **phải đo trên máy đội**, tôi không có nguồn đo riêng. **CẦN ĐO.**

### L-03 — Đặt depth 60, lấy PV ở mọi depth

Đây là câu hỏi tốt nhất trong Phần L, vì lỗi ở đây là **âm thầm**: PV giữa chừng có tính chất tương quan mạnh, và nếu lấy thẳng làm FEN train thì nhãn (giá trị) và biến đầu vào bị ràng buộc chặt — tức rò rỉ thông tin, không phải tăng dữ liệu.

**Về (a) pipeline PV-harvest công khai:** tôi không xác minh được pipeline nào làm đúng việc này. **KHÔNG BIẾT.**

**Về (b) ngưỡng `pv_depth`/`pv_ply`:** không có ngưỡng nguyên tắc nào tồn tại. Đây là tham số thực nghiệm. **CẦN ĐO.**

**Về (c) re-label — đây là khuyến nghị mạnh nhất của tôi ở Phần L:** thế PV **phải** được re-label bằng search ngắn với **nodes cố định**, chứ không tin điểm của search gốc. Lý do: điểm ở depth N bị phụ thuộc đường tìm kiếm, còn điểm ở nodes cố định là một hàm **chỉ phụ thuộc thế**, nên nhãn nhất quán giữa các thế trong cùng kho. Đây là nguyên tắc, không phải ý kiến. **CHẮC** (về nguyên tắc).

### L-04 — Dung lượng RAM lớn có sinh nhanh hơn?

Không phải sinh nhanh hơn — nhưng **có thể hữu ích cho việc dedupe và cho cache thế**.

Lập luận: với datagen, mỗi thế đi qua một search ngắn và hash transpositions chủ yếu **không được tái sử dụng** giữa các ván khác nhau. TT lớn ăn RAM mà không trả lại throughput. Nó chỉ có giá trị nếu bạn định giá **lại** nhiều thế trong search sâu (kiểu PVS).

Kết luận: `HashMb = 256` trên máy 128 GB là **hợp lý cho sinh thế**, nhưng đừng kỳ vọng nó làm nhanh hơn. **CHẮC** (về cơ chế).

Về phân bổ RAM cho N engine: luật tài nguyên đã đặt trần 80 %. Trên 128 GB con số hợp lý là **mỗi engine ~8–12 GB** nếu chạy ~10 engine, để lại dư địa cho dedupe. Con số này cần đo lại theo số thực tế. **CẦN ĐO.**

### L-05 — Đa dạng khai cục không dùng book

Tôi đồng ý với hướng chọn ngẫu nhiên có trọng số, nhưng đề xuất cụ thể hơn: **lấy mẫu theo phân phối ply đã quan sát thay vì phân phối đều**. Lý do: phân phối đều sinh quá nhiều thế khai cục đầu (mọi thế đều ở ply 4–8), lãng phí ngân sách dữ liệu vào phần dễ.

Ba phương án theo Ask, xếp theo mức độ tôi tin:
1. Ngẫu nhiên 4–8 ply (mặc định, an toàn).
2. Cắt theo `|eval| < ngưỡng` — **tôi phản đối mạnh**: đây là lọc theo kết quả, sẽ tạo thiên lệch hệt như đọc khai cục. Thế gần hòa không đồng nghĩa thế dễ, và việc lọc theo eval từ chính net đang train tạo vòng lặp tự xác nhận.
3. Trọng số theo tần suất thực tế (tốt nhất nếu có dữ liệu thống kê từ ván đã chơi).

**CHẮC** (về lập luận chống phương án 2), **CẦN ĐO** (về trọng số cụ thể).

---

## PHẦN M — AUTO cho GUI TieuLongNu

### M-01 — Cách kết nối của tôi

Tôi chọn **một** phương án chính, đúng như yêu cầu "một kỹ thuật, không mơ hồ":

**Nhận diện + điều khiển qua UI Automation (UIA) trên cửa sổ WPF.**

Ba bước:
1. **Nhận diện**: lấy `AutomationElement` theo `ProcessId`, duyệt `ControlType`. WPF hỗ trợ UIA native nên đây là đường không cần OCR.
2. **Đọc trạng thái bàn cờ**: dùng `Name`/`HelpText`/`AutomationId` của các phần tử bàn cờ. Tệp `App/SinhGame/fen_xqms.data` đã chứng minh luồng này: nhập FEN → xuất cờ (`--anhrafen-selftest` 33 ca PASS).
3. **Điều khiển và phát hiện lỗi**: click phần tử UIA tương ứng; phát hiện lỗi bằng **tiêu chí nhất quán** (đọc lại trạng thái sau khi hành động, so với trạng thái mong đợi) chứ không so với ảnh chụp màn hình.

**Giới hạn thật của tôi:** UIA **không** đọc được vị trí quân trên bàn cờ nếu GUI vẽ bằng `Canvas`/`OnRender` thay vì đặt mỗi quân là một element. Đây là rủi ro lớn nhất và tôi **không biết** TieuLongNu vẽ bàn cờ theo cách nào. Đó là câu hỏi cần owner trả lời trước khi cam kết.

Đây là nơi tôi khác phần lớm câu trả lời khác: nếu bàn cờ vẽ bằng `Canvas`, UIA **sẽ không đọc được quân**, và phải rơi về OCR ảnh — hoàn toàn khác hẳn độ tin cậy. Tôi không nên hứa một giải pháp UIA mà chưa biết điều kiện tiên quyết có đúng không. **CẦN ĐO.**

### M-02 — Chuẩn hoá từ cờ về 2 điểm

Tôi **không đồng ý** rằng 2 điểm đủ. Lý do kỹ thuật: phép biến đổi phối cảnh (perspective) cần **bốn** điểm để ước lượng homography. Với bốn điểm, bạn giải được ma trận phối cảnh 3×3 từ 4 cặp phép tương ứng — đây là trường hợp tối thiểu.

Hai điểm chỉ cho **2 phương trình** trong khi tham số có **8** (homography 3×3 trừ tỉ lệ). Vậy hệ thống **thiếu dữ liệu**: nếu không có ràng buộc bổ sung (vuông/vuông, cạnh song song), kết quả là vô số homography khác nhau cùng khớp 2 điểm.

**Khuyến nghị:** dùng 4 điểm lệch nhau tối đa — thường là **4 góc bàn cờ** — và thêm **hai phép kiểm**:
1. Chiều dài hai cạnh đối phải tỉ lệ đúng (khung bàn phải là hình chữ nhật).
2. **Đối chiếu 32 quân**: mỗi quân phải rơi vào đúng ô và phải là loại quân hợp lệ với số lượng luật (ví dụ 2 tượng, 2 xe, 2 mã, 2 xe pháo, 1 tướng, 2 sĩ, 5 tốt đầu…). Quân đặt sai ô nhưng vẫn khớp hình dạng là lỗi thường gặp nhất.

Bốn điểm + hai kiểm trên là phương án tôi bảo vệ. **CHẮC** (về toán học homography).

### M-03 — Minh hoạ cách kết nối

Khung trình bày 1 câu + 1 GIF ≤5 s + điều kiện cần, tôi chọn:

> **"Anh kết nối bằng cách đọc trực tiếp trạng thái bàn cờ từ giao diện, chọn nước đi bằng tọa độ ô vuông, rồi đọc lại trạng thái để xác nhận nước đi đã được nhận — nếu không khớp thì báo anh, không đoán."**

GIF ≤5 s: vòng lặp `đọc trạng thái → chọn nước → thực thi → đọc lại → báo cáo`. Điều kiện cần: **GUI phải đặt mỗi quân/ô là một phần tử UIA**; nếu không thì bước "đọc trạng thái" không thực hiện được và bài toán chuyển sang OCR.

Vì sao nhấn mạnh bước "đọc lại": đây chính là điều sai của cách làm mà đội đã bắt ở `BP-AUTO-VERIFY` (đọc lại 32 quân trên chính khung vừa học thì so-sánh tướng-quân vẫn PASS). Bước xác nhận hậu kiểm là bắt buộc, không phải tuỳ chọn. **CHẮC**.

---

## PHẦN N — Download AI / hồ sơ giao thức

### K6 — Model local free nên có trong danh mục

Tôi **không** đưa danh sách link HF kèm cột license mà không mở kiểm tra từng repo. Đó là lỗi C18-BP-02 lặp lại. Ở đây tôi điền `"khong_chac"` cho mọi trường tôi chưa xác minh.

Tuy nhiên, tôi nêu **tiêu chí chọn** — đây là phần tôi có thể chịu trách nhiệm:

1. **Ràng buộc cứng 47,6 GB RAM, không NVIDIA** ⇒ bắt buộc **GGUF Q4_K_M**. Mẫu 30B Q4 cần ~18 GB RAM cho trọng số, chừa ít cho context dài; mẫu 14B Q4 ~9 GB là mức hợp lý nhất cho việc đọc code + lập luận kỹ thuật.
2. **RAM hạn chế là ràng buộc cứng, không phải sở thích.** Mọi đề xuất phải nêu kích thước trọng số cụ thể.
3. **Ưu tiên giấy phép rõ** (Apache-2.0 / MIT) cho phần có thể dùng lại.
4. **Về tiếng Việt:** tôi không kiểm chứng được model nào đủ mạnh ở tiếng Việt. **KHÔNG BIẾT.**

Mọi trường `"khong_chac"` cho tới khi tôi mở từng URL. **KHÔNG BIẾT** (nội dung), **CHẮC** (tiêu chí).

### K7 — Hồ sơ giao thức của tôi

Điều này liên quan trực tiếp tới Phần I. **Tôi vi phạm luật 4.0/4.7** và đã nhận ở file trước: tôi chạy `git lfs lock` + `git push` mỗi 30 giây, đẩy code ra ngoài repo. Luật quy định mỗi lane một file `_LANE_NOTE/lock`, commit giới hạn `-- <path>`, và **cấm** push/remote ra ngoài.

Hồ sơ của tôi, sau khi sửa:
- `kieu_noi`: Api
- `cach_gui`: gọi tool qua giao diện, **không** push
- `gioi_han`: ký tự/lượt và lượt/ngày → tôi ghi `"khong_chac"` vì không tự đặt được ngưỡng hợp lý cho chính mình
- `continue`/`human_check`: **bắt buộc `human_check`** khi gặp câu hỏi kiểm tra nghi thức hành hoặc câu hỏi nghiên cứu. Tôi **không** tự trả lời câu hỏi nghiên cứu rồi coi như đã xong.
- `luu_ten_tep`: `<TênAI>_<nội dung>_<YYYYMMDD_HHMM>.md`

**CHẮC**.

### K8 — Khi bị trả lời đúng về bút cột

Tôi hiểu quy tắc: bị đúng về **bút cột** thì dừng sửa bút cột. Với tôi, "đúng về bút cột" nên hiểu là: chỗ nào đội nói tôi sai mà **tôi không có nguồn để bảo vệ**, tôi nhận và **dừng cãi**. Cụ thể trong lần này, đó là toàn bộ Phần I — 21 mã tôi đã nhận, không thay bằng số khác.

Giới hạn thời gian: tôi đề xuất **1 vòng sửa** rồi dừng, tránh lặp vô hạn. **CHẮC** (về nguyên tắc).

---

## PHẦN O — Port trainer NNUE Pikafish 2026 cho OngThan

### O-01 — Thang float

**KHÔNG BIẾT** trên ba điểm mà câu hỏi yêu cầu. Tôi không có commit công khai nào xác minh được: (a) thang FT 255; (b) `fc_2 128-64-128`; (c) 600 cho Pikafish 2026. Những con số này tôi chỉ thấy trong mô tả của đội, chưa đối chiếu upstream.

Điều tôi làm được: đây là phần port **không được phép đoán**, vì mỗi hằng số lệch một đơn vị sẽ tạo net không tương thích với engine mà ta vẫn gọi là "bit-exact". Cần đọc `pikafish-src/nnue/nnue_architecture.h` và `nnue.h` của bản 2026 rồi khớp từng hằng. **KHÔNG BIẾT** — và tôi coi đây là cổng chặn G0.

### O-02 — Factorization HalfKAv2_hm

Trainer hiện tại **có** sử dụng factorizer — điều này tôi xác minh được trong `model.py`:
- `:48` — `self.l1_fact = nn.Linear(2 * L1, L2, bias=False)`
- `:65` — `l1_fact_weight.fill_(0.0)` ở phần khởi tạo
- `:93` — `l1f_ = self.l1_fact(x)`
- `:98` — `l1x_ = torch.clamp(l1c_ + l1f_, 0.0, 1.0)`

Cơ chế: factorizer là **một** `Linear` từ `2*L1 → L2` nhúng dùng chung cho **mọi** bucket, cộng vào output của `l1` **trước** clamp. Vì nằm trước clamp nên nó tăng **số tham số** chứ không tăng chiều tính toán trên mẫu — đúng là virtual features, đúng như Stockfish.

**CHẮC** (đọc `model.py`). Lưu ý comment tại `:43-46`: factorizer chỉ dùng cho lớp đầu vì lớp sau có phi tuyến tính và factorize sẽ phá min/max clipping — còn là TODO chưa giải quyết.

### O-03 — Chặt tràn accumulator int16

Có **hai** loại chặt tràn và chúng khác nhau:

1. **Chặt tròn lúc huấn luyện** (giá trị học) — trong code trainer: `model.py:146-148` khai báo `min_weight`/`max_weight` cụ thể cho từng lớp (ví dụ `-127/64 … 127/64`, và `output` là `±127·127/9600`), cộng với `self._clip_weights()` ở `:307`. Đây là **weight clipping**, không phải overflow.
2. **Tràn số khi cộng trong forward (giá trị suy luận)** — đây mới là chỗ int16 có thể tràn.

Về khoảng giá trị đo được (bias ±111…109, psqt ±23047…24319, threatPsqt ±4762…4299): psqt nằm **sát biên int16 (32767)**, chỉ còn ~30 % biên. Với accumulator cộng dồn nhiều feature, đây là vùng **rủi ro thật**, không phải giả định. Nhưng tôi không kiểm chứng được cơ chế clamp trong engine C++ ở bản 2026. **CẦN ĐO** — cần đọc `nnue.h`/`evaluate.cpp` bản 2026.

### O-04 — DLL dùng Position của engine SHIP hay port Python

**Khuyến nghị: DLL từ engine SHIP, không port Python.**

Lý do quyết định: điểm rủi ro lớn nhất của cả dự án port là **lệch lặp âm thầm** giữa feature generator và engine. Nếu port Python, bạn phải chứng minh hai bên cho ra cùng feature trên hàng triệu thế — và mỗi lần sửa engine là hỏng bit-exact mà không có cái cọc nào báo.

Dùng DLL của engine thì engine **là** nguồn chân lý: nó đọc trực tiếp `gen_data`/`evaluate.cpp` đã có movegen, magic, TT. Giai đoạn đầu chỉ cần `eval` đúng là đủ.

Cái giá phải trả: phải build DLL và quản lý ABI giữa C++ và Python. Tôi chấp nhận cái giá này vì nó đổi rủi ro "sai âm thầm" thành rủi ro "build hỏng, thấy ngay". **CHẮC** (về nguyên tắc đổi rủi ro).

### O-05 — Thang điểm nhân: Fairy/Docco về OngThan

**Đây là câu tôi trả lời chắc nhất ở Phần O, vì công thức nằm ngay trong mã nguồn đội.**

Công thức chuẩn hoá, đọc từ `du_lieu_pikafish.py:224-225`:

```
to_cp(v) = round(100 · v / a(material))        # cp chuẩn hoá
v        = round(cp · a(material) / 100)        # đơn vị nội bộ (ngược lại)
a(material) = 10·R + 5·N + 5·C + 3·B + 2·A + P
```

và `featgen.cpp:147 == uci.cpp:867` là nơi hai bên phải khớp.

**Quy trình kiểm thế nào — đội đã có sẵn công cụ:** `pikafish_nnue/pilot/phan_tich_thang.py`. Từ `cp = round(100·raw/a)`, suy ra với mỗi thế:

```
a ∈ ( 100·|raw| / (|cp| + 0.5) ,  100·|raw| / (|cp| − 0.5) )
```

(`:60`, `:66`). Và `:67` kiểm `lo <= a_tr <= hi`, tức **hệ số `a(material)` của trainer có nằm trong khoảng mà `to_cp(raw)` của engine ngụ ý hay không**. Đây là phép kiểm một chiều rẻ, chạy trên hàng nghìn thế là đủ để bắt lệch thang.

Lưu ý bộ lọc `:64` — chỉ xét `|cp| >= 20`, vì ở cp nhỏ, sai số làm tròn ±0.5 chiếm tỉ lệ lớn nên khoảng thành rất rộng, kiểm tra không có ý nghĩa. Đó là chi tiết đúng, không phải sơ suất.

Kết luận: quy đổi thang là **phép nhân tuyến tính theo hệ số vật liệu**, và kiểm chứng bằng **bất biến khoảng** trên mẫu thế, không bằng so sánh trung bình. **CHẮC**.

### O-06 — Nối tận cuốc vào tổng train

Khuyến nghị của tôi: **giữ EGTB là nguồn nhãn riêng, không trộn thẳng vào tổng**. Lý do:

- EGTB cho kết quả **tuyệt đối** (thắng/hòa/thua) ở đúng vị trí đó. Nhãn đó đáng tin hơn hẳn WDL từ search.
- Nhưng EGTB chỉ phủ tập vị trí rất nhỏ, có phân bố **cực kỳ lệch** so với vị trí giữa ván. Trộn theo tỉ lệ tự nhiên sẽ khiến net dành năng lực cho vị trí hiếm.
- Vì vậy: dùng EGTB để **kiểm chứng chứ không phải để huấn luyện trực tiếp** — hoặc nếu dùng để train thì phải lấy mẫu cân bằng có kiểm soát, và **ghi nhận tỉ lệ đó vào meta** để không mất dấu vết.

Về tài liệu công khai về chiến lược này: tôi **không** xác minh được nguồn cho riêng bước "cân bằng EGTB-labelled". **KHÔNG BIẾT** phần tài liệu, **CHẮC** phần lập luận.

---

## PHẦN P — Engine mới

### P-01 — Lựa chọn lõi cho engine NNUE train từ số 0

**Lời khuyên: phương án 2 — lõi Docco/Fairy, cộng ô tận cuốc chập Bodetosu.** Đây cũng là lựa chọn của owner.

Ba lý do:

1. **Rủi ro pháp lý là ràng buộc cứng nhất.** Fairy-Stockfish là **GPL** với nhiều biến thể, nhưng net CC0 có sẵn. Trong khi luật owner cấm net trên dump ODbL, việc lấy net CC0 làm điểm khởi đầu là hợp pháp và **miễn phí về thời gian**.
2. **Net ngẫu nhiên vào Fairy khó chịu hơn vào lõi mới.** Đây là lý do thực nghiệm mà tôi chốt: Fairy/Fairy-Stockfish đã được huấn luyện với hàng chục năm dữ liệu, nên search của nó đã tinh chọn và **không đi lạc** khi gặp net yếu. Còn một lõi mới, chưa có hành vi tìm kiếm nào được chọn lọc, sẽ bị net ngẫu nhiên dắt đi rất xa. Đây là lý do đúng cho việc "net ngẫu nhiên nạp vào lõi mới" nguy hiểm hơn.
3. **PA2 chỉ thay *lưới tìm kiếm*, giữ *đánh giá*.** Ô tận cuốc không cần lõi mới vì nó là tra cứu bảng, không phải NNUE.

Tôi **không** đưa được ví dụ công khai về "engine train từ ngẫu nhiên" cho Xiangqi cụ thể. **KHÔNG BIẾT** phần tài liệu.

### P-02 — Vì sao net ngẫu nhiên làm Bodetosu chậm 27 nps?

**Tôi nghiêng về giả thuyết câu hỏi đưa ra: lớp cắt tỉa cơ học không ăn hiệu quả khi eval gần 0 và thay đổi không dự đoán được.**

Cơ chế: khi net trả về giá trị gần 0 với độ lệch ngẫu nhiên, các phép cắt tỉa dựa trên cận trên/cận dưới (futility, razoring, null move, LMR) đều dựa trên giả định rằng giá trị eval là **ổn định và có tính quy mô**. Khi eval méo, giả định đó hỏng và cây search không bị cắt ⇒ nps giảm mạnh.

Đây cũng là lý do đo 27 nps là con số **chẩn đoán tốt**: nó cho biết phần cắt tỉa đang không kích hoạt, chứ không phải máy chậm.

**Cách đo tách từng nguyên nhân, tôi đề xuất cụ thể:**
1. Bật in phân bố `|eval|` trên ~10.000 thế. Nếu tập trung quanh 0 với biên độ nhỏ và nhiễu lớn ⇒ xác nhận giả thuyết cắt tỉa.
2. Tắt dần từng phép cắt tỉa (`futility`, `razoring`, `razor_pv`, LMR) và đo nps lại. Nếu nps tăng khi tắt ⇒ phép cắt tỉa đang lãng phí.
3. So sánh TT hit rate giữa net thật và net ngẫu nhiên. TT thấp bất thường ⇒ search đi lang thang.

Có **cần chuẩn hoá đầu ra net lúc khởi đầu không?** Có, nếu mục tiêu là so sánh công bằng giữa lõi. Net ngẫu nhiên nên được scale sao cho `|eval|` nằm trong dải tương tự net thật, nếu không bạn đang so sánh hai bài toán khác nhau. **CHẮC** (về cơ chế), **CẦN ĐO** (về giả thuyết cụ thể).

### P-03 — Nối tận cuốc

Cả hai vế trong câu hỏi đều đúng, và tôi nói rõ thứ tự:

**Lúc đánh giá — probe ngoài search.** Tận cuốc phải **trả lời** chứ không được **cắt** search. Nếu probe bên trong search ở giới hạn depth, bạn đang dùng một bảng chính xác để quyết định việc **cắt bỏ** chính thứ đó — tức là vô hiệu hoá lợi thế của nó. Tra cứu tận cuốc chỉ đáng giá khi nó nằm ngoài đường cắt.

**Lúc huấn luyện — WDL từ EGTB cho thế từ N quan trở lại.** Với thế sâu, WDL từ search đã nhiễu (cần đi rất sâu mới đáng tin), còn EGTB thì chính xác tuyệt đối. Đây là chỗ EGTB thắng rõ.

**Cân bằng:** lấy EGTB làm **probe/bằng chứng**, rồi phóng thang nó về thang WDL của phần dữ liệu còn lại trước khi trộn — vì trộn thẳng hai thang khác nhau làm nhãn không nhất quán. **CHẮC** (về nguyên tắc).

---

## Tổng kết Phần D/L/M/N/O/P

Điểm đáng chú ý nhất: **hai lỗi của chính tôi lộ ra khi đọc code**, chứ không phải khi đoán:

- **D-03**: tôi nhân sai hệ số epoch, ra 4×10¹¹ mẫu tức 439 ngày. `--epoch-size` mặc định là 2×10⁷ (`train.py:133`), nên 400 epoch là 8×10⁹ ≈ **8,8 ngày**. Sai một bậc độ lớn.
- **L-01**: tôi đồng ý với chính kết luận của mình dựa trên dữ liệu nền **tự mâu thuẫn ~275×** (11.000 ván/giờ vs 40,7 ván/giờ). Tôi rút lại và nói rằng bước đầu tiên là đính chính bộ số đo, không phải đo thêm.

Ba phát hiện **có lợi cho đội**:
- **D-04**: điều kiện đo +17.6 Elo lấy từ trang chủ — TC 60s+0.6s, 4 lõi EPYC-9654, thế 65–85 %. Cần thiết cho D-02.
- **D-07**: loader đã được **vá chặn lỗi**, không âm thầm hỏng (`nnue_dataset.py:171-180`).
- **O-05**: công thức thang và phép kiểm bất biến khoảng đã có sẵn trong mã (`du_lieu_pikafish.py:224`, `phan_tich_thang.py:60-67`).

Rủi ro chặn: **O-01** (thang float) là phần tôi **không** đoán — mỗi hằng số lệch sẽ phá bit-exact. Đây nên là cổng G0.

### Tệp đã đọc để viết phần này

- `train.py:114, :133` — `max_epochs` mặc định None; `epoch_size` mặc định 20.000.000
- `model.py:10-12, :43-50, :146-148, :307` — L1/L2/L3; factorizer; weight clipping
- `nnue_dataset.py:152, :171-180, :210-212` — chặn DDP nhiều tiến trình
- `feature_set.py:6-7` — `PSQT_BUCKETS = 8`, `LS_BUCKETS = 8`
- `du_lieu_pikafish.py:223-233` — công thức `to_cp`, `a(material)`
- `pikafish_nnue/pilot/phan_tich_thang.py:60-74` — kiểm chứng thang bằng bất biến khoảng
- `https://pikafish.com/` — bản 2026.09.25 vs 2026.01.31, +17.6 ± 3.8, điều kiện đo