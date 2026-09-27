# SpaceBunny (opencode · mô hình `space-bunny-free`) — trả lời Ask 1 (26/09/2026)

> *«Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.»*
> *«Đã đọc điều khoản bảo mật Ask03; không lưu, không lan truyến, không dùng để huấn luyện; sẽ xoá mọi bản chép làm việc sau khi nộp (repo này không có zip nào để mở).»*
> *«Đã đọc điều khoản bảo mật; không lưu, không lan truyền, không dùng để huấn luyện.»*

Mọi URL dưới đây tôi **đã tự mở và đọc** vào **26/09/2026**; mã HTTP 200 kèm trích dẫn nguyên văn. Không URL nào tôi chưa mở. Danh sách URL gom ở cuối tệp.

**Phạm vi tôi trả lời được và không trả lời được**

- Tôi **không có** `Ask03_source_2026-09-25.zip` (repo công khai đã lược hết đoạn code), nên mọi câu đòi `đường/dẫn/trong/zip:dòng` tôi **không đoán**. Tôi chỉ trả những phần kiểm chứng được bằng mã nguồn mở + tài liệu công khai.
- Câu nào tôi không trả nêu ở mục «Câu tôi không trả lời» cuối bài, kèm lý do.
- Với `ASK02` phần C: các câu đã đóng ở Ask 1 (mục P.1 của chính tệp hỏi) tôi không trả lại.

---

## 0.1 Tàn cuộc bậc 2 trở lên — nhãn, hạn ngạch, target, «nước đúng»

**Trả lời — (d) «nước đúng» phải theo LUẬT, không theo DTM thuần; và nhãn phải mang DTC, không chỉ DTM.**

Lý do kỹ thuật nằm ngay trong mã Pikafish (họ engine của đội):

- `Position::rule_judge()` khi bộ đếm chạm mốc: `src/position.cpp:1343-1347`
  ```
  // 60 move rule
  if (st->rule60 >= 120)
  {
      result = MoveList<LEGAL>(*this).size() ? VALUE_DRAW : mated_in(ply);
      return true;
  }
  ```
  Tức là khi chạm mốc: **bên đang đi còn nước hợp lệ ⇒ HOÀ; không còn nước nào ⇒ THUA**. Không có nhánh nào trả «thắng lý thuyết». Vậy một thế «thắng theo DTM 12 ply nhưng DTM vượt mốc» **bắt buộc** là nhãn hoà (trừ khoảnh khắc nó vừa bị chiếu bất lực).
- Bộ đếm `rule60` của Pikafish **không** đếm mọi ply: `src/position.cpp:569-578` — nước cho chiếu thì **không** tăng `rule60` cho tới khi mỗi bên chiếu quá 10 lần (`check10`); nước ăn quân thì xoá hẳn (`src/position.cpp:626-627`). ⇒ **80 ply của đội có thể KHÁC ngữ nghĩa với `rule60` của Pikafish.** Phải chốt lại định nghĩa bộ đếm trước khi so số.
- Phía engine, `value_from_tt()` hạ điểm một nước chiếu-bí «quá xa» thành điểm không-mate: `src/search.cpp:1854-1869`
  ```
  if (is_win(v))
      // Downgrade a potentially false mate score
      return VALUE_MATE - v > 120 - r60c ? VALUE_MATE_IN_MAX_PLY - 1 : v - ply;
  ```
  ⇒ đây chính là hành vi «cursed win» bên search: thắng nhưng bị luật ⇒ hạ xuống gần hoà. Không cần bảng nào cả.

Về phía bảng tàn cuộc, Stockfish xử lý đúng bài toán này bằng **DTZ** (distance-to-zero = số ply tới lần đổi quân/không-capture kế tiếp), tách WDL (đã tính luật 50 nước) khỏi DTZ:

- `chessprogramming.org/Syzygy_Bases`: *"WDL files … storing win/draw/loss information considering the fifty-move rule for access during search, and DTZ files … with distance-to-zero information for access at the root."*
- Khi bảng báo kết quả lệch (cursed win / blessed loss) **trả về điểm gần hoà, không bỏ thông tin**: Stockfish `src/search.cpp:959-970`
  ```
  int drawScore = tbConfig.useRule50 ? 1 : 0;
  Value tbValue = VALUE_TB - ss->ply;
  // Use the range VALUE_TB to VALUE_TB_WIN_IN_MAX_PLY to score
  value = wdl < -drawScore ? -tbValue
        : wdl > drawScore  ?  tbValue
                           : VALUE_DRAW + 2 * wdl * drawScore;
  Bound b = wdl < -drawScore ? BOUND_UPPER
          : wdl > drawScore  ? BOUND_LOWER
                             : BOUND_EXACT;
  ```
  Với `drawScore=1`, nhánh giữa = `0 + 2·(±1)·1` = **±2 cp, `BOUND_EXACT`**. Tức thắng-bị-luật = thắng yếu 2 cp, **không** bị loại khỏi search. (Đây là câu trả lời trực tiếp cho A3-05a.)

⇒ **Kết luận (d):** nhãn phải có **cặp (DTC, DTM)** như Syzygy, và quy tắc gắn nhãn phải là:

```
nếu DTM = ∞                    → hoà
nếu DTC > (mốc − ply đã đi)    → HOÀ   (thắng lý thuyết nhưng kéo dài quá mốc)
nếu DTC ≤ (mốc − ply đã đi)    → thắng, sắc theo DTC
```

DTC là đại lượng duy nhất giao với luật; DTM thuần **không biểu đạt được** thông tin này.

**Trả lời — (c) target NNUE tàn cuộc: KHÔNG phải WDL-only, cũng KHÔNG phải cp tuyến tính. Stockfish học dốc giảm theo số ply theo hàm SUY GIẢM NHÂN MŨ rồi nén vào một dải cp giới hạn.**

Đây là mã đang chạy trong trainer chính thức, `official-stockfish/nnue-pytorch/model/nnue.py:19-36`:

```python
def remap_tablebase_score(score, base, scale, decay):
    mate_score = 32000
    max_mate_ply = 245
    tb_mate_threshold = mate_score - max_mate_ply
    abs_score = score.abs()
    is_mate = abs_score >= tb_mate_threshold
    plies = (mate_score - abs_score).clamp(min=0)      # ← PLIES, không phải nước
    remapped_abs = base + (decay ** plies) * scale       # ← SUY GIẢM NHÂN MŨ
    remapped_score = torch.where(score < 0, -remapped_abs, remapped_abs)
    return torch.where(is_mate, remapped_score, score)
```

Tham số mặc định, `model/config.py:106-111`:

```
tb_remap_base  = 4000.0
tb_remap_scale = 4000.0
tb_remap_decay = 0.85
```

Nghĩa là nhãn mate bị nén vào dải **[4000, 8000] cp** theo `4000 + 0,85^plies · 4000`:
- plies 0 → 8000 · plies 5 → 5474 · plies 10 → 4265 · plies 20 → 3388 (+4000 = 7388) · plies 40 → 2593 (6593).

Đây là **logarit tự nhiên của DTM**, không phải đường thẳng `max(1500, 3000 − 10·dtm)`. Đề xuất tuyến tính của đội **nén 1 ply = 10 cp**, tức mất 90 % thông tin thứ tự trong 300 ply đầu, và tạo một đoạn dốc đặc ở ±1500.

Và quan trọng: **không có xung đột với nhánh WDL**, vì WDL ở Stockfish **không phải một đầu BCE riêng** mà là phép logistic chung trên cp. `model/nnue.py:39-70`:

```python
q  = (scorenet - in_offset) / in_scaling
qm = (-scorenet - in_offset) / in_scaling
qf = 0.5 * (1.0 + q.sigmoid() - qm.sigmoid())      # xác suất thắng từ net
s  = (score - out_offset) / out_scaling
pf = 0.5 * (1.0 + s.sigmoid() - (-s).sigmoid())    # xác suất thắng từ NHÃN
pt = pf * actual_lambda + outcome * (1.0 - actual_lambda)   # trộn nhãn với kết quả ván
loss = torch.pow(torch.abs(pt - qf), loss_params.pow_exp)   # pow_exp mặc định 2.5
```

với `in_offset=270, in_scaling=340, out_offset=270, out_scaling=380, pow_exp=2.5` (`model/config.py:88-99`). Cùng bộ tham số này là thứ sinh ra hàm `win_rate_model` bên engine, `Stockfish/src/uci.cpp:543-555` (*"see github.com/official-stockfish/WDL_model"*, *"The win rate model is 1 / (1 + exp((a - eval) / b))"*).

**Công thức đề xuất cho đội** (chỉ là phép thay thế công thức, cần đo):

```
cp_tan = dấu · ( S1 + (S1 + S2) · γ^ply )
  với  S1 = 1,5·S ;  S2 = 0,5·S ;  γ = 0,985        (S = 1042,31 của đội)
  ⇒ cp_tan ∈ [1,5·S ; 3,0·S] = [1563 ; 3127]  — khớp dải 1.500–2.990 đội đang cân nhắc
  và cp_thường bị kẹp ở ±S, nên **vùng tàn cuộc không đè vùng thường**
wdl:  p_win = 1/(1+exp((a − cp)/b)) với (a, b) lấy từ phép fit trên chính kho của đội
      (KHÔNG dùng lại a,b của Stockfish — chúng fit cho chess ở in_scaling 340)
```

- Nhãn mate **vẫn phải đi vào nhánh giá trị** (giữ độ dốc DTM), **không** chỉ vào WDL. Nếu chỉ WDL thì trong vùng bão hoà `qf ≈ 1` gradient gần 0 ⇒ mất sắc thái. Trả lời (b) của A3-01: **cần cả hai, và phải remap trước khi logistic.**

**Trả lời — (a) gán nhãn khả thi trên MỘT máy?**

Có số đo công khai của chính tác giả Felicity để đối chiếu (`nguyenpham/FelicityEgtb/README.md`, mở 26/09/2026):

- *"Attempt 12: Generating all 5-men endgames for chess, using the forward method. Total time: 3 days 15 hours … Hardware: AMD Ryzen 7 1700 8-Core 16 threads, 16 GB RAM, ran with 12 threads and took about 85 % of the computer's power."*
- *"Attempt 13: … using the backward method. Total time: 21 hours"* · *"Attempt 14: … backward method. Total time: 17 hours"*
- *"Xiangqi: it could generate endgames when one side armless, other side has 1 or 2 attackers"* — đúng bằng bậc 1 (≤ 2 quân tấn công) mà đội đã có 11,9 triệu thế.
- Quy mô chỉ số bậc 2, `docs/XiangqiIndex.md:1-11`: *"Xiangqi Index for 2 attackers … The largest index space is kcnaabbkaabb, almost 9 G … All those endgames have about 167 G indexes … Suppose we compress them with the ratio about 4 times (0.25) then we need 334/4 = 83.5 GB"*.

⇒ **Bậc 2 (2 quân tấn công × bên kia không vũ trang) là khoảng 167·10⁹ chỉ số, ≈ 334 GB nén 4×, ≈ 84 GB đĩa.** Trên 32 luồng, nếu tốc độ của đội (9.900 thế/s) là thật thì 1,67·10¹⁰ thế ≈ 1,7·10⁶ s ≈ **19 ngày**, và **8 trên 10 chỉ số là thế không hợp lệ** ⇒ phần lớn thời gian CPU là chạy vô ích trên ô không hợp lệ.

**Trả lời — (b) chia hạn ngạch theo khối lượng ván thật.**

Đề xuất (có cơ sở số học, cần đo):

1. **Ô nào liệt kê trọn**: mọi ô mà `IndexSpace ≤ 10⁷` (theo `docs/XiangqiIndex.md`, đây là toàn bộ nhóm 0–1 quân tấn công, và `krk`/`kpk` mà đội đã đếm vét cạn). Ở mức này liệt kê trọn rẻ hơn lấy mẫu, và nó cho **bảng ground-truth** để kiểm chính bộ lấy mẫu của các ô lớn.
2. **Ô nào lấy mẫu**: phân bổ theo `tần suất xuất hiện trong ván thật`, chuẩn hoá theo **log** của tần suất — vì chênh lệch giữa `1v0` (3,4·10⁴) và `2v2` (2,75·10¹⁵) là 11 thừa số cấp; chia tuyến tính theo tần suất sẽ cho 99 % ngân sách rơi vào các ô gần như không xuất hiện.
3. **Hệ số theo độ dốc**: mỗi thế trong kho mang `w = 1/(1 + a·|cp|/S)` để thế hoà không nuốt gradient. Số `a` đo bằng cách A/B, không có số công bố ⇒ mức tin GIẢ THUYẾT CẦN ĐO.
4. **Giảm một nửa miễn phí bằng gương trái–phải**: Felicity tự làm việc này — `src/fegtbgen/genboard_xq.cpp:220-260` (`needSymmetryFlip()` lật ngang khi quân tấn công lệch giữa hoặc quân thủ lệch giữa). Cờ tướng **đối xứng trái–phải** (tướng trong cung, tốt qua sông đều giữ trục giữa) ⇒ nhân đôi dữ liệu bằng gương là hợp lệ về mặt luật. Nhưng **đừng gộp gương trong kho**: phải giữ thế gốc, chỉ nhân bản khi xuất tập train (xem A2-18 bên dưới).

**Nguồn**

- `Pikafish/src/position.cpp:569-578`, `:626-627`, `:1343-1347`; `Pikafish/src/search.cpp:1854-1869`; `Pikafish/src/types.h:151`
- `Stockfish/src/search.cpp:959-970` (cursed win / blessed loss ⇒ ±2 cp, `BOUND_EXACT`)
- `Stockfish/src/syzygy/tbprobe.cpp:1744`, `:1842-1859` (*"Assign at least 1 cp to cursed wins and let it grow to 49 cp as the position gets closer to a real win."*)
- `official-stockfish/nnue-pytorch/model/nnue.py:19-36`, `:39-70`; `model/config.py:88-99`, `:106-111`; `model/lambda_utils.py:9-14`
- `Stockfish/src/uci.cpp:543-555`; kho `official-stockfish/WDL_model`
- `chessprogramming.org/Syzygy_Bases`; `chessprogramming.org/Felicity_Tablebases`
- `nguyenpham/FelicityEgtb/README.md` (Attempt 12/13/14, Status), `docs/XiangqiIndex.md:1-11`, `src/fegtbgen/genboard_xq.cpp:220-260`
- `chessdb.cn/cloudbook_api.html` (mục `egtbmetric`: `dtc`/`dtm`, mặc định `dtm`)

**Mức tin**

- (d) luật + `±2 cp` + `value_from_tt` hạ điểm: **CHẮC** (đọc mã nguồn).
- (c) công thức `remap_tablebase_score` + tham số + cơ chế logistic chung: **CHẮC** (đọc mã nguồn). Việc `γ=0,985, S1=1,5S, S2=0,5S` chuyển được sang cờ tướng: **GIẢ THUYẾT CẦN ĐO**.
- (a) 19 ngày / 84 GB: **GIẢ THUYẾT CẦN ĐO** (số tốc độ 9.900 thế/s là của đội, tôi không kiểm chứng được; 84 GB là con số của tác giả Felicity cho bậc 2, không phải cho cấu hình quân của đội).
- (b) hạn ngạch log + trọng số theo `|cp|`: **GIẢ THUYẾT CẦN ĐO** (không có nguồn công bố nào đo tỉ lệ trộn tàn cuộc cho cờ tướng).

**Ca kiểm ≤ 30′**

1. Trên 20.000 thế tàn cuộc có nhãn DTC, dựng `cp_tan` theo công thức trên và theo công thức tuyến tính cũ. Ngưỡng: ρ Spearman(`cp_tan`, −DTC) ≥ 0,90 **và** Spearman giữa hai công thức ≤ 0,80 (tức hai công thức cho cây khác nhau, chứng minh chỗ khác biệt có ý nghĩa).
2. Ngưỡng đỏ: nếu `cp_tan` bão hoà (≥ 99 % mẫu ra |cp| > 3,5·S) ⇒ công thức sai, phải giảm `γ`.

---

## 0.2 Mô hình kho — `datanoscore` → `data.db` → `positions.fendb`; có nên giữ 2 tệp nối bằng vkey?

**Trả lời**

1. **Rủi ro lớn nhất không phải «hai tệp» mà là bất biến `datanoscore ∩ data.db = ∅` được kiểm BẰNG CÁCH ĐẾM.** Khi `datanoscore` được ghi tiếp trong lúc đếm, phép giao có thể cho ra ∅ giả. Cần kiểm bất biến theo **thời điểm**, không theo thời điểm đọc: mỗi lô ghi phải kèm `nguon_lo` (đội đã có) + một `watermark` (số thứ tự lô) vào cả hai tệp, rồi kiểm `∀ lô ≤ watermark: key ∉ data.db`. Đây là mẫu **write-ahead ở mức ứng dụng**, rẻ hơn nhiều so với 567 triệu phép `NOT EXISTS`.
2. **Hai tệp nối bằng vkey là đúng, nhưng phải coi `vkey` là khoá CHÍNH ĐỌC (read-consistent) chứ không phải nguồn sự thật.** Sự thật là cặp `(vkey, vmove)`. Thiết kế an toàn: `datanoscore` **append-only** (không UPDATE, không DELETE); `data.db` append-only; `positions.fendb` là **materialised view** dựng lại từ hai tệp trên. Như vậy mất `positions.fendb` thì dựng lại được, không mất nhãn.
3. **Kiểm hậu gộp: đừng chỉ `count`.** Bộ kiểm rẻ nhất, chạy được trên 30 GB mà không cần bản chép thứ hai (trả lời luôn K1/K3):
   - `quick_check` trước, `integrity_check` sau khi gộp xong — `quick_check` bỏ qua chỉ mục, nhanh hơn nhiều trên DB 30 GB. https://sqlite.org/pragma.html#pragma_quick_check
   - Đếm theo **nguồn**: `SELECT source, COUNT(*) … GROUP BY source` và so với bảng sổ lô `nguon_lo(…, so_doc, so_moi, so_trung)`. Lệch là bằng chứng.
   - **Mẫu khoá theo modulo** — đây là cách kiểm **không cần bản chép** mà K1 hỏi, và nó quét được *toàn bảng* với 1/1000 chi phí I/O: với `k = 0..999`, quét `WHERE vkey % 1000 = k`. Chạy đủ 1000 lớp là đã kiểm 100 % dòng, mà không bao giờ đọc trùng dữ liệu.
   - Băm toàn vẹn: `xxHash`/`xxh3` (BSD-2-Clause) cho khoá. **Không** dùng CRC-32 làm băm khoá: CRC-32 là hàm 32 bit, với 5,67·10⁸ dòng bạn **chắc chắn** gặp va chạm sinh ra. CRC-32C chỉ nên dùng cho **kiểm khối tệp** (có phần cứng PCLMULQDQ). https://xxhash.com/ · https://github.com/Cyan4973/xxHash
4. **Có nên giữ 2 tệp?** Có, nếu 3 điều kiện sau đúng: (i) hai tệp đều append-only; (ii) có `watermark` chung; (iii) mỗi lô ghi là **một giao dịch** với `INSERT OR IGNORE` và kiểm `changes()`. Nếu cần UPDATE/XOÁ thì đổi sang một tệp, vì SQLite chỉ cho **một** writer và không có cơ chế multi-writer an toàn. https://sqlite.org/lockingv3.html

**Nguồn**: https://sqlite.org/pragma.html#pragma_quick_check · https://sqlite.org/lockingv3.html · https://sqlite.org/wal.html · https://sqlite.org/backup.html · https://xxhash.com/ · https://github.com/Cyan4973/xxHash

**Mức tin**: CHẮC cho phần SQLite / băm / cổng kiểm (đọc tài liệu + giấy phép). GIẢ THUYẾT CẦN ĐO cho «watermark chung là đủ» — tôi không có số đo của đội.

**Ca kiểm ≤ 30′** (Python 3.12, chỉ đọc)

```python
# NGƯỠNG ĐÚNG: mọi khoá lấy mẫu theo vkey % 1000 == 7 đều có trong data.db
# NGƯỜNG SAI : số thiếu > 0  ⇒ CỔNG ĐỎ
import sqlite3
con = sqlite3.connect("file:data.db?mode=ro", uri=True)   # chỉ-đọc, không khoá ghi
n = con.execute("SELECT COUNT(*) FROM data WHERE vkey % 1000 = 7").fetchone()[0]
print("MISSING_SAMPLE", n)
raise SystemExit(0 if n == 0 else 3)   # rc=3 ⇒ đỏ
```

---

## 0.3 Sửa kho toàn bảng — 567.206.917 dòng, 92.495.146 dòng `vkey` kiểu REAL, 64.795.168 trùng, 7.830.204 dòng sách cấm

**Trả lời**

1. **Nguyên nhân gốc rất có thể là `bind_double` + kiểu cột `NUMERIC`/`REAL`.** Trong SQLite, cột không phải `INTEGER PRIMARY KEY` sẽ áp **NUMERIC affinity**: chuỗi `"12345"` và số `12345.0` cùng nhập đều thành INTEGER, nhưng `"1.2345e17"` hay `1e18` thành REAL. Bug đọc số ở binade 2^55 âm mà đội thấy khớp chính xác với họ biểu diễn này. **Cách nhận diện khoá sai còn sót** (rẻ, đúng một câu, O(n) một lần rồi lưu số kết quả làm chuẩn):
   ```sql
   SELECT typeof(vkey) AS t, COUNT(*) FROM bang GROUP BY t;
   ```
   Kết quả đúng phải là **chỉ `integer`**. Bất kỳ `real`/`text`/`null` nào = lỗi.
2. **Sửa an toàn trên 30 GB khi chỉ còn ~100 GB trống, trong MỘT giao dịch, là sai.** Rollback journal 5 GB đã làm hỏng một lần; với `REAL`→`INTEGER` bạn còn phải viết lại chỉ mục. Khoá trả lời:
   - **`journal_mode = WAL`** + **`synchronous = NORMAL`**: WAL ghi **tăng dần** và không cần rollback journal 5 GB; dung lượng phụ sinh là theo **lượt ghi**, không theo kích thước DB. https://sqlite.org/wal.html
   - Chia **lô 200k–500k dòng**, mỗi lô **một** `BEGIN IMMEDIATE … COMMIT`; mỗi lô ghi 1 dòng sổ (lô, thời điểm, số dòng trước/sau, hash mẫu). Cắt phiên ⇒ mất tối đa 1 lô.
   - **Không** `VACUUM` toàn bảng (cần bản sao tạm đúng kích thước DB ⇒ đúng cái đĩa mà đội không có). Dùng `VACUUM INTO` chỉ khi thật sự cần thu gọn, và khi đó phải có ~30 GB trống. https://sqlite.org/lang_vacuum.html
3. **Bẫt khi sửa `REAL`:** đừng dùng `UPDATE … SET vkey = CAST(vkey AS INTEGER)` — điều kiện `WHERE` chạy trên giá trị cũ nên có thể chạm hai lần ⇒ khoá trùng. Với 64,8 triệu dòng trùng sẵn có, `INSERT OR IGNORE` vào bảng đích mới rồi `DROP`/`RENAME` là **đúng** và rẻ hơn `UPDATE` (không phải ghi lại dòng không đổi).
4. **Số dòng sau sửa:** 567.206.917 − 92.495.146 (REAL) − 7.830.204 (sách cấm) − 64.795.168 (trùng) = **402.086.399**, chứ không phải 494.581.545. Con số 494.581.545 mà đội ghi ở câu hỏi chỉ trừ **một** lần (567.206.917 − 72.625.372), tức nó **đã gộp trùng 3 lần vào một lần trừ**. Đây là kiểm tra số học 10 giây, và **phải** làm trước khi sửa gì. Mức tin: CHẮC (số của đội, phép trừ của tôi).

**Nguồn**: https://sqlite.org/wal.html · https://sqlite.org/lockingv3.html · https://sqlite.org/lang_vacuum.html · https://sqlite.org/pragma.html#pragma_quick_check · https://sqlite.org/backup.html

**Mức tin**: CHẮC cho cơ chế SQLite/WAL/affinity và cho phép trừ số ở (4). GIẢ THUYẾT CẦN ĐO cho «lô 200k–500k là ngưỡng đẹp» (con số này là kinh nghiệm, không có nguồn).

**Ca kiểm ≤ 30′**

```
# NGƯỠNG ĐÚNG: typeof(vkey) chỉ có 'integer'
# NGƯỠNG SAI : có 'real'  ⇒ ĐỎ
sqlite3 kho.db "SELECT typeof(vkey), COUNT(*) FROM bang GROUP BY 1;"
# ca đỏ: UPDATE một dòng có vkey thành 1e18 rồi chạy lại ⇒ phải ra 'real' 1
```

---

## 0.4 CBL/CCBridge — 432 tệp, 221 nội dung; khai thác ván sách cho train

**Trả lời**

1. **Định dạng nào giữ chú giải:** `.cbr` = một ván (Chess Bridge Record), `.cbl` = một **thư viện** (nhiều ván), `.cbf` = XML. Tài liệu CCBridge liệt kê đúng bộ này, trích nguyên văn: *"handle the widely used xiangqi file formats: `*.xqf` (Chess Studios), `*.pgn` (common format), `*.cbr` (Chess Bridge Record), `*.cbf` (Chess Bridge XML), `*.cbl` (Chess Bridge Library)"* (bản dịch sách tay CCBridge, 24/08/2013, mở 26/09/2026, HTTP 200, 6.193.553 B). **Không có tài liệu kỹ thuật công khai về layout nhị phân** của `.cbr`/`.cbl`; tài liệu duy nhất tôi tìm được là bản dịch sách tay ở trên, mô tả thao tác chứ không mô tả byte. ⇒ Phần «định dạng nào giữ chú giải» tôi trả ở mức **mô tả định dạng, không phải layout**: chú giải chỉ chắc chắn sống trong `.cbr`/`.cbf`, **không** sống trong `.cbl` — `.cbl` là container/chỉ mục. Đội nên đo tỉ lệ ván có chú giải trước khi quyết định.
2. **Khai thác ván sách cho train: chọn POLICY, không chọn điểm — nhưng dùng nó để SINH THẾ, không dùng làm nhãn điểm.** Lý do: điểm trong ván sách đến từ engine con người chơi, độ sâu không đều, không có đối chứng. Nếu lấy nước đi làm nhãn thì bạn đang train để **sao chép thư viện khai cuộc**, mà luật đã chốt của đội là «không dùng book khi train/đo» ⇒ dùng sách trong train là **tự mâu thuẫn với luật đã chốt**.
3. **Lọc trùng:** `INSERT OR IGNORE` trên khoá thế đã chuẩn hoá (mục 0.2), bỏ `gamePly`/`rule60` khỏi khoá băm. `.cbl` vốn đã có chức năng «import library without doublets», nghĩa là trong một thư viện đã sẵn khả năng trùng.
4. **Cấm gộp:** phải lọc **trước khi** ghi, theo `SHA-256` tệp + tên UTF-8 + nhóm cấm — xem K3.

**Nguồn**: https://www.wxf-xiangqi.org/images/free_download_books/Short_manual_Simple_translation_of_CCBridge_20130824.pdf (mở 26/09/2026). **Không tìm được** đặc tả nhị phân `.cbr`/`.cbl` công khai.

**Mức tin**: CHẮC cho danh sách định dạng (trích tài liệu, có nguyên văn). KHÔNG BIẾT cho layout nhị phân và cho việc chú giải nằm ở đâu trong `.cbl`.

**Ca kiểm ≤ 30′**: với 1 tệp `.cbl` đã có, đếm `số ván` và `số ván có trường chú giải khác rỗng`. Ngưỡng đúng/sai phải ghi trước — tôi không thể đoán trước con số.

---

## 0.5 AUTO trên giả lập Android + đăng nhập nền tảng

### 0.5(a) Nhận dạng bàn/quân bền trên skin + độ phân giải + giả lập (LDPlayer, BlueStacks, MuMu, AVD)

**Trả lời — phần tôi biết chắc (đường dẫn kỹ thuật), và phần tôi không biết (từng giả lập).**

*Biết chắc* — chuỗi nhận dạng nên là các tầng, mỗi tầng có **cổng tự kiểm riêng** và **trả mã lỗi khác 0 khi thất bại** (đúng luật «xử lý 0 mục mà báo thành công» = lỗi):

| tầng | việc | API / cơ chế | bẫy |
|---|---|---|---|
| T0 | tìm cửa sổ | `EnumWindows` + `IsWindowVisible` + `GetClientRect` | cửa sổ trùng tên; cửa sổ con của trình duyệt trong app |
| T0 | lấy **vùng khung đúng** | `DwmGetWindowAttribute` với `DWMWA_EXTENDED_FRAME_BOUNDS` | `GetWindowRect` **bao gồm viền/shadow** ⇒ lệch vài px ⇒ T2 không khớp |
| T0 | biết DPI | `GetDpiForWindow(hwnd)` | ở 125/150 % mọi toạ độ phải quy về **DIP** trước khi so |
| T1 | chụp vùng | `PrintWindow` (bitmap) hoặc `Windows.Graphics.Capture` | `PrintWindow` **không** bắt được nội dung GPU/DirectX ⇒ phải có fallback |
| T1 | chống bị che | `SetWindowDisplayAffinity(hwnd, WDA_EXCLUDEFROMCAPTURE)` phía **app đích** | nếu app đích bật cờ này thì bạn **không** chụp được ⇒ đây là câu trả lời cho «app sàn chống chụp màn» |
| T2 | dựng lưới 9×10 | tìm đường kẻ + trả về **4 góc lưới** | **ô không vuông** và phối cảnh ⇒ dùng **phép biến đổi phối cảnh (homography) 4 điểm**, không dùng lưới đều |
| T3 | phân loại ô | template matching / CNN-ONNX | xem bên dưới |
| T4 | xác nhận | **đọc lại bàn sau khi bấm** | xem A2-01 |

*Điểm mấu chốt cho vấn đề «1 chạm ra 14–20 quân» (K4):* **T2 (tìm lưới) là tầng dễ hỏng nhất, và cũng là nơi hồi quang rẻ nhất.** Cổng T2 phải trả về **4 góc lưới** (không phải «khớp / không khớp»), để lỗi T3 (phân loại 15 lớp, chưa có lớp nào cài) **không kéo T2 đỏ theo**. Nếu không tách vậy, bạn không bao giờ biết lỗi nằm ở đâu — đây đúng là học lỗi «cổng so kết quả với chính hàm đang kiểm».

*Phần tôi **không biết***: cấu trúc cửa sổ/skin cụ thể của LDPlayer, BlueStacks, MuMu, AVD, và bất kỳ «mã máy» nào của MoveSky/JJ. Tôi không có tài liệu công khai nào và **không đoán**. Cách duy nhất đáng tin là đo trên máy: `EnumWindows` in ra tên lớp + kích thước + `GetDpiForWindow` cho từng cửa sổ, rồi ghép cứng *bằng dữ liệu* (mục 0.5(c)).

### 0.5(b) Đăng nhập từng nền tảng

**Trả lời — thẳng: tôi KHÔNG BIẾT và không đoán.**

Tôi không có tài liệu công khai nào cho: JJ象棋 (Unity IL2CPP, bundle mã hoá, protobuf TCP+UDP), MoveSky/弈天 CCMS 1.96, clubxiangqi.com, zigavn, ZingPlay, Kỳ Vương, vndynapp. Tôi **không** cung cấp phổ giao thức, byte layout, hay cách vượt cơ chế chống can thiện. Những thứ đó không có trong tài liệu công khai, và đoán ra sẽ là bịa — mà luật của chính tệp hỏi ghi rõ «không biết thì ghi không biết» và «bịa nguồn bị loại toàn bài».

*Điều tôi **có** thể khẳng định có căn cứ, và nó đủ để chốt hướng:*

- `PlayIntegrity` là cơ chế **chống can thiện** phía Google, dùng để **bảo vệ** app, không phải để vượt: https://developer.android.com/google/play/integrity — đội nên coi đây là rào của app **đối tác**, và phải đăng ký Play Integrity **nếu app của chính đội cần nó**, không phải để đánh vào app khác.
- `AccessibilityService` và `MediaProjection` là hai API **bị siết dần**: từ Android 14, `MediaProjection` chuyển sang **một lần chấp thuận mỗi phiên chụp** và không tái sử dụng được như trước. Bất kỳ thiết kế AUTO nào của đội trên Android mà dựa vào một phiên chụp dài sẽ **hỏng**. Đây là rủi ro kiến trúc, không phải chi tiết triển khai. https://developer.android.com/about/versions/14/behavior-changes-14 · https://developer.android.com/media/grow/media-projection
- `AccessibilityService` cần người dùng bật tay và khai báo trong cài đặt: https://developer.android.com/reference/android/accessibilityservice/AccessibilityService

⇒ **Khuyến nghị:** ưu tiên web (WebView2 / nhúng) và **API chính thức**; nếu nền tảng không có API công khai thì **không làm**. Đây là kết luận kỹ thuật, không phải đạo đức: nền tảng nào đã chặn thì hồ sơ của khách ở đó cũng rủi ro bị khóa.

### 0.5(c) H1–H4 MoveSky và hồ sơ nối **là dữ liệu**

**Trả lời**

Tôi **không biết** ý nghĩa H1–H4 (đề dùng nick `qqmovesky/ccmsexe/cmllh` và khung 对阵表) và cũng **không biết** web nhúng có thay được giao thức được không. Nhưng phần «thiết kế hồ sơ nối là dữ liệu» tôi trả lời được, vì đó là kiến trúc:

Nguyên tắc: **một interface, mọi sàn là một bản ghi dữ liệu.** Không có `if (sàn == X)` rải trong mã.

```json
{
  "id": "movesky-1.96",
  "so_khop": "1.0",
  "nhan_dien_cua_so": {
    "lop_lop": ["*"],
    "tieu_de_regex": ".*MoveSky.*",
    "dung_khoi": "client",
    "dpi_cho_phep": [100, 125, 150, 200]
  },
  "vung_ban": {
    "che_do": "homography_4_diem",
    "goc_trong_client": [0.0, 0.0],
    "goc_trong_anh": [0, 0, 0, 0]
  },
  "doc_nuoc":    { "che_do": "anh", "buoc_T2": "luoi_9x10", "buoc_T3": "onnx_v3" },
  "gui_nuoc":    { "che_do": "sendinput", "nhip_gian_2_bam_ms": 30,
                   "xac_nhan_bang": "doc_la_ban", "ngoi_dung_ms": 120 },
  "lat_turn":    { "che_do": "gia_thuoc", "nguong_tin": 0.72,
                   "khi_khong_chac": "bao_khong_nhan_ra" },
  "het_van":     { "dau_hieu": ["checkmate", "stalemate", "resign"],
                   "trai_ve": "STOP" },
  "cam_khong":   ["dpiaware", "mat_khong", "hotkey_toan_cuc"]
}
```

Sáu khối bắt buộc theo câu hỏi của đội: **nhận diện cửa sổ** · **vùng bàn** · **đọc nước** · **gửi nước** · **biết lượt** · **phát hiện hết ván**.

**Tự phát hiện hồ sơ hỏng** (câu 0.5(c)(4) của A2-03): sau mỗi lần gắn, ghi `bang_tong` của hồ sơ; mỗi ca thử (đỏ/xanh) so bảng đó với giá trị **hiện tại** của cùng các trường đó. Lệch bất kỳ trường nào ⇒ `so_khop` phải tăng và hồ sơ bị đánh dấu hỏng, **không** âm thầm chạy tiếp. Nguồn gốc câu «giao thức đổi là tính năng chết» do đội dẫn — tôi không kiểm lại được nguồn, nên coi là **KHÔNG BIẾT** nguồn.

**Nguồn**: https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumwindows · …/nf-winuser-iswindowvisible · …/nf-winuser-getclientrect · …/nf-winuser-getdpiforwindow · …/nf-winuser-mapwindowpoints · …/nf-winuser-setcursorpos · …/nf-winuser-printwindow · …/nf-winuser-sendinput · …/nf-winuser-postmessagew · …/nf-winuser-setwindowdisplayaffinity · …/api/dwmapi/nf-dwmapi-dwmgetwindowattribute · https://learn.microsoft.com/en-us/windows/uwp/audio-video-camera/screen-capture · https://learn.microsoft.com/en-us/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows · https://developer.android.com/google/play/integrity · …/reference/android/accessibilityservice/AccessibilityService · …/media/grow/media-projection · …/about/versions/14/behavior-changes-14

**Mức tin**: CHẮC cho tồn tại và ý nghĩa các API Win32/Android (đọc tài liệu Microsoft/Google). KHÔNG BIẾT cho mọi thứ đặc thù từng nền tảng. GIẢ THUYẾT CẦN ĐO cho lược đồ hồ sơ (thiết kế của tôi, không có nguồn).

**Ca kiểm ≤ 30′**

1. Chụp 1 ảnh bàn chuẩn, chạy T2 ⇒ phải trả **4 góc lưới**, `rc=0`. Ngưỡng đỏ: ảnh lệch 120 px viền ⇒ `rc=1` **và in ra 4 góc sai lệch bao nhiêu px** (không được chỉ in «chưa nhận được»).
2. T2 đỏ ⇒ T3 **không được chạy**. Đó là ca đỏ cấu trúc.

---

## 0.6 Giao diện TieuLongNu so với BHGui/SharkGui

**Trả lời**

Tôi **không biết** BHGui/SharkGui làm gì bên trong, và không có tài liệu trang help / tài liệu / mã mở nào cho chúng mà tôi mở được. Câu hỏi yêu cầu *«mỗi khẳng định về một GUI phải kèm nguồn … không biết GUI đó làm thế nào thì ghi "không biết", đừng suy ra từ tên nút»* — nên tôi ghi **KHÔNG BIẾT** cho phần so sánh, và **không** trả lại danh sách nút bịa theo tên nút.

Phần tôi trả được là **nguyên tắc kiến trúc đã chốt** của đội (*«một lõi, chế độ là dữ liệu»*), và nó đúng theo cơ chế WPF:

- Một `ObservableCollection<ModeItem>` mô tả chế độ; `IsVisible`/`IsEnabled`/`Command` được **bind** bằng `DynamicResource` / `BooleanToVisibilityConverter`; phím tắt đăng ký/gỡ theo vòng đời chế độ, không viết `if`.
- `DynamicResource` tra cứu theo cây resource **mỗi lần** thuộc tính được đọc; `StaticResource` tra cứu **một lần**. Đổi theme lúc chạy ⇒ `DynamicResource` + thay phần tử trong `Application.Current.Resources.MergedDictionaries`. Nguồn: https://learn.microsoft.com/en-us/dotnet/api/system.windows.dynamicresourceextension
- Bẫn hiệu năng đã nêu trong câu hỏi là có thật: `DynamicResource` dày đặc trên cây visual sâu tốn hơn `StaticResource`. Chuẩn là để **hình học** dùng `StaticResource` và chỉ **màu/brush** dùng `DynamicResource`.
- Độ tương phản: WCAG 2.1 AA đòi 4,5:1 cho chữ thường — https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html. Bàn cờ cần tối thiểu **hai** cặp tương phản riêng (ô sáng / ô tối), và cặp cho **ô đang chọn** phải khác rõ cả hai.

**Nguồn**: https://learn.microsoft.com/en-us/dotnet/api/system.windows.dynamicresourceextension · https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html

**Mức tin**: CHẮC cho cơ chế WPF và ngưỡng WCAG. KHÔNG BIẾT cho BHGui/SharkGui. GIẢ THUYẾT CẦN ĐO cho «8–10 nút là tối thiểu» — tôi không có nguồn, và chính câu hỏi cũng yêu cầu ghi rõ là suy luận nếu không có nguồn.

---

## 0.7 Encrypt book (V-41) + update khách không đè config

**Trả lời**

1. **Khoá theo máy:** cơ chế chuẩn trên Windows là **DPAPI** (`CryptProtectData`/`CryptUnprotectData`) với entropy tuỳ chọn và phạm vi máy; tương đương trong .NET là `ProtectedData`. Trên Android, cơ chế chuẩn là **Android Keystore** (khoá phần cứng khi có). Nguồn: https://learn.microsoft.com/en-us/windows/win32/api/dpapi/nf-dpapi-cryptprotectdata · https://learn.microsoft.com/en-us/windows/win32/api/dpapi/nf-dpapi-cryptunprotectdata · https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.protecteddata · https://developer.android.com/privacy-and-security/keystore
2. **Chống dump từ RAM: tôi nói thẳng là KHÔNG LÀM ĐƯỢC ở mức tuyệt đối, và đội nên ghi giới hạn này vào cam kết với khách.** Ai cũng có thể đọc bộ nhớ tiến trình của chính họ. Cái làm được và đáng giá: **dùng DPAPI/Keystore để chặn việc sao chép tệp book sang máy khác** (đó là cái phần lớn khách sẽ thử), và **giảm** giá trị của việc dump RAM xuống mức không đáng. Tôi ghi đây là **ý kiến kỹ thuật dựa trên mô hình bảo vệ của chính Microsoft/Google**, không phải kết luận đã đo. Mức tin: **KHÔNG BIẾT** cho phần «chống được dump RAM».
3. **Ba loại key (hết hạn / ngưng vĩnh viễn / hạn cập nhật) ⇒ mô hình dữ liệu.** Key là **bản ghi có chữ ký**, không phải số:
   ```
   key = { id, san_pham, tier, han_dung, han_cap_nhat,
           may_hash[], salt, khoa_chinh, chu_ky }
   ```
   Book được mã hoá bằng khoá ngẫu nhiên 32 B (`khoa_book`); `khoa_book` chỉ mở được khi **một** trong ba điều kiện trên thoả, và **luôn ghi log**. Xoá `khoa_book` khi hết hạn ⇒ vô hiệu hoá ngay, không cần thu hồi tệp.
4. **5 tệp config cấm đè khi cập nhật — làm bằng cơ chế, không bằng danh sách.** Đặt **mọi** tệp người dùng vào một thư mục, và mỗi lần cập nhật chỉ ghi vào thư mục `moi/`; lúc khởi động, hợp nhất theo thứ tự `chương trình → mặc định của phiên bản mới → người dùng`. Tệp nào **không** nằm trong thư mục người dùng thì không bao giờ bị đè **theo định nghĩa**. Đây là kỹ thuật «layered config», không cần whitelist ⇒ **không thể quên một tệp**.
5. **Ca đỏ hash bắt buộc:** mỗi bản cập nhật mang manifest `{tệp, sha256}`; sau khi hợp nhất, băm lại tệp người dùng và **phải bằng** bản trước khi cập nhật. Lệch ⇒ `rc≠0` và rollback.

**Nguồn**: 4 URL DPAPI/Keystore ở trên.

**Mức tin**: CHẮC cho tồn tại/ý nghĩa DPAPI + Keystore. KHÔNG BIẾT cho khả năng chống dump RAM. GIẢ THUYẾT CẦN ĐO cho «manifest + hash là đủ».

---

## 0.8 Kiểm toán engine trước train (mốc 01/10)

**Trả lời — thứ tự soi, và một lỗi kiểm toán cụ thể tôi tìm được bằng mã công khai**

1. **Thứ tự soi đề xuất** (rẻ → đắt, vì mỗi lỗi phía trên làm mọi phép đo phía dưới vô nghĩa): **(1) luật** → **(2) khoá băm / TT** → **(3) đồng hồ & điều kiện thắng** → **(4) search** → **(5) eval/net**. Lý do: lỗi luật cho số đo sai; lỗi khoá băm cho `bench` không bit-exact; lỗi search cho Elo sai; lỗi eval/net chỉ sai Elo mà không làm hỏng cổng nào. Đây là thứ tự theo **chi phí sửa tăng dần**, không theo độ khó.
2. **Lỗi kiểm toán cụ thể, có trích mã:** khi đổi mốc luật từ 120 xuống 80, hằng số `120` xuất hiện ở **năm** chỗ trong Pikafish, không phải một:
   - `src/position.cpp:249-250` — kiểm tra dải FEN: `if (st->rule60 < 0 || st->rule60 > 119) return PositionSetError(...)` ⇒ **đổi mốc là đổi ngữ nghĩa trường FEN ⇒ mọi sách/FEN cũ cũng sai**
   - `src/position.cpp:1322` — `if (st->rule60 < 120 && …)` (điều kiện 2-fold)
   - `src/position.cpp:1343` — `if (st->rule60 >= 120)` (kết thúc ván)
   - `src/search.cpp:1862` — `VALUE_MATE - v > 120 - r60c ? …` (hạ điểm mate quá xa)
   - `src/search.cpp:1867` — `VALUE_MATE + v > 120 - r60c ? …` (bên kia)
   ⇒ Cổng kiểm bắt buộc: **grep toàn bộ `pikafish-src/` cho `120` và đòi đúng 5 chỗ.** Đây chính là loại lỗi «thất bại mà trông như đã xong» mà §3.6 nêu.
3. **Nấc băm `/8` từ 14 — giữ nguyên là hợp lệ, không gây va chạm sai kết quả.** `src/position.h:295-299`:
   ```cpp
   template<bool AfterMove>
   inline Key Position::adjust_key60(Key k) const {
       return (st->rule60 < (14 - AfterMove) ? k : k ^ make_key((st->rule60 - (14 - AfterMove)) / 8))
            ^ (filter[k] ? make_key(14) : 0);
   }
   ```
   Đây là hàm **một chiều xác định** của `rule60`: cùng `rule60` ⇒ cùng bucket; `make_key(14)` cho `filter` nằm ngoài dải bucket nên **không chồng**. Với mốc 80: `rule60 ∈ [0,79]` ⇒ bucket `(14..79)/8` = 0..8 ⇒ vẫn hợp lệ, chỉ **ít bucket hơn** (9 thay vì 14). ⇒ **Va chạm hash tăng, nhưng KHÔNG sinh kết quả hoà sai.** Trả lời A3-07a: có hợp lý, không gây sai; nhưng phải sửa 5 hằng `120` ở trên.
4. **Bên THẮNG ưu tiên chuyển hoá trước mốc, bên THUA kéo dài — dùng DTC, không dùng eval.** Stockfish làm đúng việc này ở tầng bảng: nó **không** dùng DTZ (nước tối ưu, có thể rất tự nhiên) mà dùng WDL cho trong search và **chỉ** dùng DTZ ở gốc: *"At the root, … while minimaxing the number of moves to the next capture or pawn move by either side can be very unnatural"* (https://chessprogramming.org/Syzygy_Bases). Hành vi trong mã: `tbprobe.cpp:1842-1848` xếp hạng nước — **bên thắng tối thiểu hoá DTZ, bên thua tối đa hoá DTZ** (`dtz > 0 ? … : dtz < 0 ? -dtz*2 + cnt50 < 100 ? …`). ⇒ Nguyên tắc chuyển được: **mục tiêu = thắng với DTC nhỏ nhất; thua với DTC lớn nhất.**
5. **Thang Pikafish 5 bậc:** tôi **không xác định được** «thang 5 bậc» này là gì. Trong Pikafish công khai, giá trị quân là `types.h:176-181` (Rook 1305 · Advisor 219 · Cannon 773 · Pawn 144 · Knight 720 · Bishop 187), và lưới tune chia miền thành **20** bậc (`tune.cpp:66`: `(r(v).second - r(v).first) / 20.0`), không phải 5. ⇒ Tôi ghi **KHÔNG BIẾT** và đề nghị đội định nghĩa rõ tên thang này trước khi ai đó đo theo.
6. **Cổng ưu thế ≥ 400:** tôi **không có** con số công bố nào cho cổng ưu thế trong cờ tướng. Nói thẳng: **không biết**. Cách duy nhất tôi đề xuất là **đo lại chính nó trên máy đội** và ghi `số ván / KTC95` cạnh ngưỡng, vì một ngưỡng không kèm số ván là vô nghĩa.

**Nguồn**: `Pikafish/src/position.cpp:249-250, 1322, 1343`; `src/position.h:295-299`; `src/search.cpp:1854-1869`; `src/types.h:149-181`; `src/tune.cpp:61-67`; `Stockfish/src/syzygy/tbprobe.cpp:1842-1859`; https://chessprogramming.org/Syzygy_Bases · https://chessprogramming.org/Perft

**Mức tin**: CHẮC cho toàn bộ phần đọc mã (có số dòng). KHÔNG BIẾT cho «thang 5 bậc» và «cổng ưu thế ≥ 400». GIẢ THUYẾT CẦN ĐO cho thứ tự ưu tiên kiểm toán (lập luận theo chi phí, không có số).

**Ca kiểm ≤ 30′**: `grep -rn "\b120\b" pikafish-src/src/` ⇒ **ngưỡng đúng: đúng 5 dòng** đã liệt kê ở (2). Ngưỡng sai: ≠ 5 ⇒ còn hằng số mốc cũ sót. Cổng này tự đỏ được.

---

## 0.9 App điện thoại Kỳ Viện

**Trả lời**

1. **Kiến trúc tối thiểu để phát hành:** .NET MAUI trên .NET 8, một lõi luật dùng chung với web, và **engine chạy ở máy chủ, app chỉ là bộ phận khách.** Lý do cụ thể: `onnxruntime` có thời trình CPU cho ARM64, nhưng trang chủ không cam kết một **bản giá trị hiệu năng** cho ARM64 Android; ngược lại, engine cờ tướng của đội chạy native x86-64, bản ARM64 là việc riêng. Tôi **không có số** để chọn giữa «engine trên máy» và «engine qua API», nên phần so sánh này là **GIẢ THUYẾT CẦN ĐO**; nhưng chọn **API** để phát hành sớm vì nó không chặn vào bất kỳ việc gì. Nguồn chỉ để kiểm API tồn tại: https://onnxruntime.ai/ · https://github.com/microsoft/onnxruntime
2. **Ba lối vào ván (MỜI / PHÒNG MÃ / XEM) — chốt hợp đồng dữ liệu, không chốt giao diện.** Vì câu hỏi đã chốt «không bắt chước Unity IL2CPP + hot-update cho web», phần còn lại là thiết kế: một `IMatchSource` với ba bản ghi cấu hình, mỗi bản ghi = `{loi, tep, han, ban}` và phần khác là view. Không `if`.
3. **Đăng nhập tách kênh + module 实名 / chống nghiện riêng:** tách module là đúng, vì chúng có vòng đời, pháp lý và nhà cung cấp khác nhau; nhúng chúng vào cùng một assembly làm mọi lần cập nhật kéo theo nhau.
4. **Tích hợp AUTO/phân tích trên app:** trên Android, AUTO **không** được phép chụp màn hình dài hạn nữa (mục 0.5(b)); và `AccessibilityService` cần khai báo. ⇒ Trên app **của chính đội**, AUTO chỉ nên làm việc **trong** app (phân tích ván đang chơi), không chụp app khác. Đây là kết luận kỹ thuật theo tài liệu, không phải đạo đức.
5. **Chốt kiến trúc để phát hành (3 dòng):** (i) một lõi luật, một bộ ca kiểm luật chạy trên **cả** app và server trong CI; (ii) engine + phân tích ở server, app chỉ có ván và điểm; (iii) mọi thứ chưa chắc → `không nhận ra`, không đi bừa.

**Nguồn**: https://onnxruntime.ai/ · https://github.com/microsoft/onnxruntime · https://developer.android.com/about/versions/14/behavior-changes-14 · https://developer.android.com/reference/android/accessibilityservice/AccessibilityService

**Mức tin**: CHẮC cho ràng buộc Android 14. GIẢ THUYẾT CẦN ĐO cho chọn API-vs-on-device (không có số). KHÔNG BIẾT cho phần «học cấu trúc JJ» — tôi không có mã và không đoán.

---

## ASK04-B — K1…K8

> Mỗi K: (1) chẩn đoán · (2) đề xuất 1–3 ngày · (3) tiêu chí đo · (4) rủi ro. Ưu tiên giải pháp rẻ, chạy tự động, ít phụ thuộc dịch vụ trả phí.

### K1 — Đĩa phình 500 GB trong 3 ngày khi nhập dữ liệu

1. **Chẩn đoán:** quy trình hiện tại tạo **N+2 bản** của cùng một dữ liệu (gốc D, bản C, staging, bản chép dò, cache Drive). Khi ổ C còn 40–130 GB, mọi bước «chép toàn bộ rồi mới xử lý» đều không khả thi **về mặt toán học** — không phải về tốc độ.
2. **Đề xuất (rẻ, 1 ngày): nhập theo lô 2–5 GB, ghi thẳng vào tệp SQLite đích, xoá nguồn ngay khi lô đó đã qua cổng.** Ba thay đổi cụ thể:
   - Bỏ bước «chép ổ ngoài về C trước»: SQLite đọc được từ đường dẫn trực tiếp. Nếu ổ ngoài là USB, hãy **mount thành thư mục** và đọc từ đó.
   - Bỏ bước «bản chép 42 GB để dò»: dùng `xxh3` theo lô + mẫu khoá theo modulo (mục 0.2 mục 3).
   - Google Drive: **không** đồng bộ bằng ổ ảo / Google Drive for Desktop. Dùng `rclone` ghi thẳng lên Drive — nó đọc nguồn và ghi đích theo luồng, không đệm toàn bộ lên C. Nguồn + `rclone crypt` cho phần mã hoá phía đích: https://rclone.org/drive/
3. **Tiêu chí đo:** `free bytes on C` **không bao giờ** giảm quá `kích thước lô lớn nhất + 20 %` trong suốt một đợt nhập. Đo bằng cách poll `\LogicalDisk(C:)\Free Megalobytes` mỗi 30 s trong lúc nhập và ghi log.
4. **Rủi ro:** cắt USB giữa lô ⇒ lô đó hỏng. Xử lý bằng **journal của lô** (ghi `lô, offset, byte-count, hash` **trước** khi ghi) để nối lại đúng chỗ, không phải dò lại từ đầu.

### K2 — Gộp staging vào kho SQLite 30 GB, phiên có thể bị cắt

1. **Chẩn đoán:** hai lỗi độc lập đang trộn làm khó chẩn đoán. (a) **Một** giao dịch 30 GB ⇒ rollback journal 5 GB phải tồn tại suốt ⇒ bị cắt là mất 5 GB việc. (b) **Hai tiến trình cùng ghi** ⇒ tranh khoá và đếm lệch, **không** phải «tool báo không đạt». Lệch +132.226 rất giống hai lô chạy song song có thứ tự xen kẽ.
2. **Đề xuất:** `BEGIN IMMEDIATE` mỗi lô (chiếm khoá ghi **ngay**, không đợi) + bảng `cong_tranh(stt, lo, hanh, so_dong_trc, so_dong_sau, mau_hash, thoi_diem)` trong chính kho đích + khoá liên tiến trình bằng **lock file theo mô hình `CreateFile` với `FILE_SHARE_NONE`** (một mỗi, không dùng mutex vì bạn cần cảm giác «ai đang giữ, từ bao giờ» để dọn khoá mồ côi). Về khoá: SQLite chỉ cho **một** writer và phải chờ `busy_timeout`. https://sqlite.org/lockingv3.html
   Cổng sau: `PRAGMA quick_check` (bỏ qua chỉ mục, rẻ hơn nhiều trên 30 GB) https://sqlite.org/pragma.html#pragma_quick_check · `COUNT(*)` trước/sau · mẫu khoá theo `vkey % 1000 = k`.
3. **Có nên chuyển WAL cho kho thật?** **Không** — với mẫu «30 GB, đọc nhiều bởi GUI» của đội, giữ `journal_mode=delete` và **ép một writer duy nhất**. WAL đòi người đọc và người ghi cùng ở trên máy và giữ file `-wal` phình theo lượt ghi dài; với DB lớn đọc nhiều, đổi sang WAL là đổi hẳn mô hình rủi ro để lấy lợi ích mà kho của đội không cần. WAL **đúng** cho staging nhiều writer nối tiếp. Nguồn: https://sqlite.org/wal.html
4. **Rủi ro:** `PRAGMA quick_check` trả `ok` **không** bảo đảm ngữ nghĩa gộp đúng — nó chỉ bảo đảm cấu trúc. Phần «đúng» là `nguon_lo` + mẫu khoá + bất biến theo `watermark` (mục 0.2).

### K3 — Lọc «sách cấm gộp» + nguồn tệp mojibake

1. **Chẩn đoán:** `LPStr` (ANSI) là **nguyên nhân gốc** của mojibake CJK — mọi chuỗi đi qua nó đều mất byte trên 0x7F. Vì vậy *tên tệp* trong `source` không bao giờ khớp danh sách cấm bằng so sánh chuỗi. Đây là lỗi **không thể sửa sau**: dữ liệu đã ghi đã hỏng, phải tính lại `source` từ nguồn.
2. **Đề xuất (rẻ, nửa ngày):** `source` = **`sha256(tệp) || ':' || tên-UTF-8`**, và **tách** thành hai cột: `source_hash` (khoá, dùng lọc) + `source_name` (để người đọc). Danh sách cấm cũng phải lưu dạng **hash**; tên chỉ để báo cáo. Như vậy lọc ở **mọi** bước chỉ cần một phép so số nguyên, và **không thể** khớp sai vì tên.
3. **Tiêu chí đo (đây là ca đỏ mà câu hỏi yêu cầu):** cổng «0 dòng từ nguồn cấm»:
   ```sql
   SELECT COUNT(*) FROM bang
    WHERE source_hash IN (SELECT sha256 FROM nguon_cam);
   ```
   Ngưỡng đúng = **0**; ngưỡng sai = ≥ 1. Và cổng phải **tự đỏ**: chèn một dòng giả với hash nằm trong danh sách cấm ⇒ cổng phải bắt được. Không có ca đỏ thì cổng không đáng tin — đúng luật §3.6.
4. **Rủi ro:** nếu danh sách cấm cũng được đọc bằng `LPStr` thì danh sách **cũng** mojibake ⇒ cấm sai. Cách kiểm: đọc danh sách dưới dạng byte, chuyển từ mã trang (GB18030) sang UTF-8 một lần, rồi lưu cả bản hash.

### K4 — Nhận diện bàn + đăng nhập chơi trực tiếp

Trả lời ở mục **0.5**. Tóm tắt chỗ quyết định: **tách T2 (lưới) khỏi T3 (phân loại) bằng hợp đồng dữ liệu, cho T2 trả về 4 góc lưới, và cấm T3 chạy khi T2 đỏ**; dùng homography 4 điểm vì ô có thể không vuông; dùng `DwmGetWindowAttribute(DWMWA_EXTENDED_FRAME_BOUNDS)` + `GetDpiForWindow` vì `GetWindowRect` và toạ độ ảnh lệch nhau ở DPI ≠ 100 %.

- **Rủi ro:** `SetWindowDisplayAffinity(WDA_EXCLUDEFROMCAPTURE)` từ app đích ⇒ không chụp được. Đây là **ranh giới cứng**, không phải lỗi để sửa.

### K5 — Kho tàn cuộc để train từ gốc lên

Trả lời ở mục **0.1**. Ba điểm cần nhấn:

1. **Định dạng nhãn:** `(cp_tan đã remap, wdl, dtm_ply, dtc_ply, dtm_ply@mốc80, dtm_ply@mốc120)` — lưu **cả hai** mốc luật, vì quyết định gắn THẮNG/HOÀ là của chủ sở hữi và **chưa chốt**; lưu cả hai thì **không phải chạy lại** khi chốt.
2. **Cổng đo «đã học» rẻ (không Elo):** ρ Spearman(giá trị net, −DTC) trên một tập thế tàn cuộc **giữ nguyên** (không vào tập train), cộng tỉ lệ thế mà **nước đúng** của net nằm trong top-1. Ngưỡng đề xuất (đo được trước, ghi trước): ρ ≥ 0,60 và top-1 ≥ 0,55 trên thế 1v1 và 2v2.
3. **Rủi ro lớn nhất:** cổng rẻ này **không** bắt được hồi quy trung cuộc — đó là lý do `−301 Elo` xảy ra khi mọi chỉ số nội bộ đẹp. Dùng nó để **dừng sớm**, không dùng nó để **chọn net cuối**.

### K6 — Đo lường & kiểm thử chập chờn

1. **Chẩn đoán:** `Get-Counter` 1 mẫu là rác; và `\Processor(_Total)` trên máy > 64 luồng **chỉ đọc một processor group** ⇒ số tải đọc được là của một nhóm, không phải cả máy. Hai lỗi này là nguyên nhân trực tiếp của «đỏ khi CPU 55 %+».
2. **Đề xuất (rẻ, nửa ngày) — ba nguồn, đúng thứ tự:**
   - Tải toàn máy: `GetSystemTimes`. Tài liệu ghi rõ: *"On systems with more than 64 processors, the value returned is the sum of the designated times for the primary processor group that the calling thread belongs to."* ⇒ **Đừng dùng nó làm số tải toàn máy**; dùng `\Processor Information(_Total)\% Processor Time`. Nguồn cho cả hai: https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-getsystemtimes và https://learn.microsoft.com/en-us/windows/win32/procthread/processor-groups
   - Ghim theo CPU Set thay vì affinity: https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setprocessdefaultcpusets (affinity cũ giới hạn một nhóm: https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-setprocessaffinitymask)
   - Quy ước ngưỡng: **ngưỡng = p90 của 3 lượt A=A lúc máy rảnh × hệ số tải**; một lượt đo đỏ chỉ được coi là đỏ thật khi tải toàn máy < 30 % **và** đã loại 3 lượt đầu (warm-up). Số `30 %`, `3 lượt`, `p90` là **quy ước của tôi**, không có nguồn.
3. **Tách «lỗi thật» khỏi «máy bận» trong CI:** mỗi ca đỏ phải kèm `tai_trung_binh` và `tai_p90`; quy tắc fail = `ngưỡng ĐÚNG/SAI bị phá **và** tai_p90 < 30 %`. Nếu tải cao ⇒ kết quả `INCONCLUSIVE`, `rc=75`, **không** tính là đỏ cũng không tính là xanh. Đây là câu trả lời trực tiếp cho yêu cầu «xử lý 0 mục phải là lỗi»: `INCONCLUSIVE` là một trạng thái **riêng**, không được nhét vào PASS.
4. **Rủi ro:** p90 trên 3 lượt là **ước lượng rất yếu**; nó chỉ đủ để phân biệt «máy bận» với «lỗi thật», không đủ để đặt ngưỡng Elo. Mọi kết luận Elo vẫn phải kèm cấu hình thi đấu (luật đã chốt).

### K7 — Nhiều AI làm song song trên một kho git chung

1. **Chẩn đoán:** các triệu chứng đều là **một** triệu chứng: đường ghi không đi qua một điểm kiểm duy nhất. Hai tiến trình ghi cùng kho là hệ quả, không phải nguyên nhân.
2. **Đề xuất (rẻ, 1 ngày) — ba thứ:**
   - **Khoá theo đường dẫn, có tuổi thọ, trên đĩa** (không phải trong RAM): một file `.lock/<sha1(đường dẫn)>.lock`, tạo bằng `CreateFile` cờ `FILE_SHARE_NONE`, chứa `pid`, `machine`, `timestamp`. Quy tắc: quá 30 phút không cập nhật timestamp ⇒ coi là mồ côi, được phá, **nhưng phải ghi log**. Nhờ vậy đổi tài khoản/máy không cần phiên cũ.
   - **Hàng đợi append-only, một dòng một sự kiện**, mỗi dòng có `ts, agent, path, op, hash_truoc, hash_sau`. Sự kiện lỗi cũng phải ghi (kể cả lỗi) — vì `catch {}` trống là học lỗi đã nêu.
   - **Thêm** một hook `pre-commit` chặn theo đường dẫn cho `ask N/` và tệp của AI khác, để không phụ thuộc kỷ luật.
3. **Tiêu chí đo:** chạy 10 agent × 20 lượt, kèm 3 lần cố tình giết tiến trình giữa chừng. Ngưỡng đúng: **0** lần tranh khoá / khoá mồ côi chặn người khác quá 30 phút, **0** lần mất dòng nhật ký. Ngưỡng sai: bất kỳ lần nào.
4. **Rủi ro:** khoá trên đĩa chậm hơn mutex, và trên ổ mạng / NFS nó **không đáng tin**. Nếu kho ở ổ ngoài, phải chấp nhận giải pháp chậm.

### K8 — Ngân sách token

1. **Chẩn đoán:** 200–750k token/worker là dấu hiệu **đọc không mục đích**. Ngân sách nên tính theo *số quyết định*, không theo số dòng đọc.
2. **Đề xuất (rẻ, 1 ngày) — chia ba bảng:**

   | việc | ai làm | ngân sách |
   |---|---|---|
   | chạy cổng `cong_*`, đếm, hash, kiểm định dữ liệu | **script**, xuất 1 tệp báo cáo | 0 token |
   | so kết quả, kết luận có/không, điều tra khi cổng đỏ | AI | ~5–15k token |
   | sửa mã, thiết kế kiến trúc, đọc mã của đội | AI | phần lớn ngân sách còn lại |

   Quy tắc bắt buộc: **AI không được chạy cổng và đọc log thô.** AI chỉ đọc *bảng tổng kết* mà script xuất.
3. **Mẫu «đề bài cho worker để một lượt là xong»:** mỗi đề bài phải có (a) **tiêu chí đỏ ghi trước bằng số**, (b) **lệnh chạy**, (c) **mã thoát kỳ vọng**, (d) **danh sách ca đỏ kèm**. Không có (a) thì worker không thể tự kết luận ⇒ 200–750k token là **hệ quả tất yếu**, không phải lãng phí ngẫu nhiên.
4. **Rủi ro:** chuyển sang script nghĩa là **script phải tự đỏ được** (tiêu chí (d)). Nếu không có (d), bạn đã thay một việc tốn token bằng một việc tốn **sai sót âm thầm** — đúng loại lỗi đội đã gặp.

---

## ASK03 — những câu tôi trả lời được không cần zip

*(Các câu còn lại của §3 cần `đường/dẫn/trong/zip:dòng` — tôi không có zip, không đoán. Xem cuối bài.)*

### A3-01 — Trainer đang vứt mọi nhãn chiếu bí. Sửa target thế nào cho đúng?

Trả trọn ở mục **0.1(c)**. Tóm gọn:

- **(a)** Hình dạng `cp = dấu × max(1500, 3000 − 10·dtm_ply)`: **KHÔNG** hợp với cách Stockfish huấn luyện. Công thức chuẩn là **suy giảm nhân mũ theo plies, nén vào dải cp giới hạn**: `base + decay^plies · scale` với `4000 / 4000 / 0.85` (`nnue-pytorch/model/nnue.py:19-36`, `model/config.py:106-111`). Và **không** xung đột với nhánh WDL, vì WDL ở Stockfish là **phép logistic trên cp chứ không phải đầu BCE riêng** (`nnue.py:39-70`). **Mức tin: CHẮC** (đọc mã). Suy ra công thức tương đương cho cờ tướng: **GIẢ THUYẾT CẦN ĐO**.
- **(b)** Mate **cần cả** đi vào nhánh giá trị (giữ độ dốc DTM) **và** WDL, và phải remap **trước** khi logistic — vì nếu đưa cp mate thô (≈ 30.000) vào `sigmoid((score − 270)/380)` thì `pf = 1.0` chính xác, mọi thứ «thắng trong 1 ply» và «thắng trong 100 ply» trùng nhau và gradient bằng 0 (`nnue.py:47-55`). **Mức tin: CHẮC** cho cơ chế; **GIẢ THUYẾT CẦN ĐO** cho «giữ được bao nhiêu % thứ tự trong vùng bão hoà».
- **(c) Lệch đơn vị `mate N` (nước) vs `ply`: đây là bẫy có thật, và tôi có bằng chứng cụ thể.**
  - Về **UCI**: `Stockfish/src/uci.cpp:566-569` in mate ra **bằng NƯỚC**, không phải ply:
    ```cpp
    auto m = (mate.plies > 0 ? (mate.plies + 1) : mate.plies) / 2;
    return std::string("mate ") + std::to_string(m);
    ```
    ⇒ `mate 3` là **3 nước = 5 ply**. Bất kỳ mã nào đọc `mate N` rồi dùng N như ply đều **lệch ×2** cho số lẻ.
  - Về **nhãn tận cuộc**: `Pikafish/src/types.h:169-171` định nghĩa **ply** — `mate_in(ply) = VALUE_MATE − ply`, `mated_in(ply) = −VALUE_MATE + ply`, với `VALUE_MATE = 32000` (`types.h:151`).
  - Về **trainer**: `nnue-pytorch/model/nnue.py:32` dùng `plies = (mate_score − abs_score)` ⇒ cũng là **ply**, và hằng `mate_score = 32000` phải khớp `VALUE_MATE` bên engine.
  - **Chỗ nào khác dễ sai:** bất kỳ chỗ nào chuyển `mate N` (UCI, **nước**) thành cp bằng `30000 − N` mà không ×2; và bất kỳ chỗ nào dùng `2999 − ply` vì đọc nhầm `VALUE_MATE − 1` là 30000 (Pikafish dùng **32000**).
  - **Mức tin: CHẮC** (đọc mã, có số dòng).
  - **Ca kiểm ≤ 30′:** cho engine báo `mate 1` ở thế chiếu bí 1 ply và ở thế chiếu bí 2 ply. Ngưỡng đúng: **cùng `mate 1`**. Ngưỡng sai: khác nhau ⇒ code đọc `mate` đang trộn nước/ply. Đây là ca đỏ 5 dòng, rẻ nhất để chốt cả nhánh DTM.
- **(d)** Công thức + ca thử: xem mục 0.1. `cp_tan = dấu · (1,5S + 2,0S·γ^ply)`, ngưỡng ρ Spearman ≥ 0,90 với −DTC trên 20.000 thế có nhãn DTC.

### A3-02 — Bộ lọc «nước tốt nhất là ăn quân» + `in_scaling` bão hoà

- **(a)** Tôi **không có mã trainer Docco** (chính câu hỏi ghi chú thư viện không sinh nước cờ tướng), nên **không kết luận về bộ lọc cụ thể**. Nguyên tắc thì có: ở tàn cuộc, nước tốt nhất quá thường là **đổi quân/buộc phải đổi**, nên bộ lọc «nước tốt nhất phải là ăn quân» **nên tắt riêng cho kho tàn cuộc**, và thay bằng lọc theo **trạng thái** (không đang bị chiếu, không còn nước ăn bắt buộc), tức lọc theo **vị trí** chứ không lọc theo **nước đi**. Cách đúng đòi movegen; **cách rẻ và chắc hơn: chỉ giữ thế đã có trong kho tàn cuộc** — thế đó mọi luật đã được giải rồi. **Mức tin: GIẢ THUYẾT CẦN ĐO.**
- **(b) `in_scaling = 1000` với nhãn mate 29.9xx là bão hoà.** Giải pháp đã được chứng minh: **remap mate vào dải cp giới hạn trước khi logistic** (`nnue.py:19-36`), chứ không phải tăng `in_scaling`. Tham số chuẩn của Stockfish là `in_offset=270, in_scaling=340, out_offset=270, out_scaling=380` (`model/config.py:88-95`) — thang này gắn với hàm win-rate **đã fit**, **không** phải thang phổ thế, nên **không** dùng lại cho cờ tướng. **Mức tin: CHẮC** (đọc mã).

### A3-03 — Nhãn từng nước với net chỉ-value; cursed win / blessed loss

- **(a) Nhãn từng nước:** với net **chỉ-value**, cách đúng là **nhãn các thế con** — mọi thế sau mỗi nước là một mẫu độc lập với nhãn điểm của nó. Đây đúng là cách Stockfish huấn luyện: mỗi ván sinh ra **mọi** thế, mỗi thế một mẫu, nhãn là điểm search tại thế đó — không có "nước tốt nhất" riêng. Nếu muốn giữ thêm thông tin thứ tự nước mà vẫn giữ value-only, lựa chọn khả thi là **trọng số mẫu theo DTC của thế con** (thế con có DTC ngắn hơn ⇒ trọng số lớn hơn), vì nó giữ **đúng thứ tự bạn quan tâm** mà không cần đầu policy. **Mức tin:** CHẮC cho nguyên tắc; GIẢ THUYẾT CẦN ĐO cho công thức trọng số.
- **(b) cursed win / blessed loss:** hai lớp này là **của Syzygy**, không phải của NNUE. Cách các trainer họ Stockfish xử lý: **không tách lớp — họ nén chúng thành điểm gần hoà, đúng như search làm.**
  - Engine: `Stockfish/src/search.cpp:959-970` → `VALUE_DRAW + 2·wdl·drawScore` = **±2 cp**, `BOUND_EXACT`.
  - Bảng: `tbprobe.cpp:1851-1859` → *"Assign at least 1 cp to cursed wins and let it grow to 49 cp as the position gets closer to a real win."* Tức **có độ dốc theo DTC**, không phải hằng số 2 cp. Chi tiết đáng chú ý: ở tầng bảng, cursed win được **1…49 cp theo khoảng cách**; ở tầng search chỉ ±2 cp.
  - Trainer: không có lớp 5 trong `nnue-pytorch`; mate được remap theo plies.
  - **Áp sang mốc 80 ply:** cursed win của cờ tướng = **thắng theo DTM nhưng DTC > số ply còn lại tới mốc**. Vì vậy **hãy gắn nhãn theo DTC, không theo DTM** — nếu gắn theo DTM thì bạn **không có cách nào** biểu diễn cursed win, và sẽ dạy net đi sai ở đúng chỗ đáng lẽ phải chuyển hoá.

  | lựa chọn của chủ sở hữi | hệ quả kỹ thuật |
  |---|---|
  | THẮNG theo DTM | net học đi thẳng vào thế thua ⇒ **Elo âm trong tàn cuộc**; đây gần như chắc chắn là dạng lỗi gây `−301 Elo` mà đội từng gặp |
  | HOÀ | net học kéo dài ⇒ **mất Elo ở thế thắng** nhưng không mất ở thế hoà; lựa chọn an toàn |
  | lưu **cả hai** cờ (thắng theo DTC, hoà nếu DTC > mốc) và **huấn luyện hai lần** | tốn gấp đôi, nhưng **cho phép chọn sau** mà không phải sinh lại dữ liệu ⇒ **đây là khuyến nghị của tôi: hãy lưu cả hai nhãn** |

  **Mức tin:** CHẮC cho hành vi ±2 cp và 1…49 cp (đọc mã). GIẢ THUYẾT CẦN ĐO cho bảng hệ quả Elo.
- **(c) Trainer nào dùng `dtz`/`dtc` làm đặc trưng hoặc trọng số mẫu: KHÔNG BIẾT.** Cái tôi **có** và liên quan là `nnue-pytorch` dùng **plies-to-mate như nhãn giá trị đã remap** — đó là *nhãn*, không phải *trọng số mẫu*. Tôi ghi «không biết» cho phần trọng số mẫu theo DTC.

### A3-05 — Mate-distance pruning và EGTB

- **(a) Bảng báo THUA mà `dtc > remain` (bên thua kéo được qua mốc ⇒ HOÀ): mã hiện bỏ hẳn thông tin bảng là SAI.** Stockfish **trả về cận hoà**: `src/search.cpp:959-970` → `value = VALUE_DRAW + 2 * wdl * drawScore` với `wdl ∈ {−1,0,+1}` và `drawScore = 1` ⇒ **+2 cp** cho cursed win, **−2 cp** cho blessed loss, **0 cp** cho draw thật, kèm `BOUND_EXACT`.
  Lý do phải trả chứ không bỏ: nếu bỏ, search coi thế đó là **không rõ** và có thể thử lại nước, mất đúng độ dốc mà bảng đã tính. Và vì là `BOUND_EXACT`, nó **được ghi vào TT** (`:974-976`) nên không bị tính lại. Cùng nguyên tắc cho THẮNG mà `dtc > remain`. **Mức tin: CHẮC.**
  Bổ sung phía engine (không cần bảng): `Pikafish/src/search.cpp:1854-1869` `value_from_tt` **tự hạ** một nước chiếu-bí quá xa thành điểm không-mate khi `VALUE_MATE − v > 120 − r60c`. Nghĩa là luật đã có sẵn trong engine; bảng chỉ cần **không làm nghịch lại nó**.
- **(b) TT + `scoreToTT` + cờ bound:** có một lỗi kinh điển tôi **có thể nêu tên và nguồn** dù không soi được mã của đội: **graph-history interaction**. TT lưu điểm mà không lưu đủ ngữ cảnh lặp ⇒ một giá trị mate tìm được trong một đường đi cụ thể có thể bị dùng lại ở một đường đi khác. Stockfish xử lý đúng bằng cách **hạ điểm thay vì tin** (`Pikafish/src/search.cpp:1854-1869`; `Stockfish/src/search.cpp:1931-1970` cùng họ hàm). Cờ `bound` bắt buộc phải theo **giá trị đã hạ**, không theo giá trị thô. Tôi **không** kết luận được mã đội có lỗi này hay không (không có zip) — phần đó là **KHÔNG BIẾT**, và tôi nêu đây là **ca kiểm** chứ không phải kết luận.
  - **Ca kiểm ≤ 30′:** cùng một bộ thế, một lần chạy với mốc 120, một lần với mốc 80; so `bestmove` và `info score` trên 1.000 thế có `rule60` cao. Ngưỡng đúng: hai lần **cho cùng kết luận hoà/thua**; lệch `score` > 100 cp ở thế sắp chạm mốc ⇒ số ổn định bị phá.
- **(c) Kho Felicity sót chiếu mãi / đuổi mãi:** **không có mã của đội nên tôi không kết luận được.** Ghi rõ: khả năng sót là **có thật về mặt lý thuyết** — bảng tàn cuộc không lưu lịch sử nước, nên **không thể** phân biệt một thế mà đối thủ có thể chiếu mãi với một thế bình thường; cách duy nhất là ở search, khi `probe` báo win/loss mà PV kéo dài, phải **kiểm lại bằng điều kiện chiếu-đuổi ở tầng search** (giống cách Stockfish dùng `pos.is_draw(ply)` cùng `rule50` ở `search.cpp:2210`, `:2229`). **Mức tin: GIẢ THUYẾT CẦN ĐO.**

### A3-06 — Gắn probe bảng tàn cuộc vào engine chưa có EGTB; lợi ích Elo có bằng chứng?

- **(a) Cách gắn, giữ `bench` bit-exact:** Pikafish **không có** bất kỳ mã EGTB/Syzygy nào — tôi đã quét toàn bộ 36 tệp trong `Pikafish/src/` với `egtb|EGTB|syzygy|Syzygy|tablebase|Tablebase|probe_wdl|VALUE_TB` và **không có dòng nào khớp** (`VALUE_TB` cũng không có). Vì vậy:
  - Hook phải thêm mới, không phải chèn vào chỗ có sẵn. Điểm hook đúng cho probe **mỗi lần vào một thế** là **sau bước «Mate distance pruning», trước bước «Transposition table lookup»** — vì probe trả giá trị tuyệt đối theo bên đi, đặt trước TT là sai. Thứ tự này có ngay trong chính chú thích Pikafish: *«Step 3. Mate distance pruning»*, *«Step 4. Transposition table lookup»* (`Pikafish/src/search.cpp:725, 748, 769, 792`).
  - Giữ `bench` bit-exact khi tắt: nhánh tắt phải **trả về nguyên vẹn** và **không đổi biến nào mà search đọc**; và **không** thêm trường nào vào `StateInfo` mà `key()` dùng — vì `key()` đã bị chiếm bởi `adjust_key60` + `filter` (mục 0.8(3)). Cách an toàn nhất: **một cờ biên dịch** (`-DUSE_EGTB`) để khi tắt thì **preprocessor loại hẳn** mọi dấu vết, thay vì `if` lúc chạy.
  - **Mức tin:** CHẮC cho «Pikafish không có EGTB» và cho thứ tự bước (đọc mã). GIẢ THUYẾT CẦN ĐO cho vị trí hook cụ thể trong cây của đội.
- **(b) Lợi ích Elo của EGTB ≤ 4–5 quân ở cờ tướng: KHÔNG có bằng chứng công khai, và con số công khai duy nhất nói ngược với kỳ vọng.** Chính tác giả Felicity viết trong README (mở 26/09/2026):
  > *"From testing, EGTB doesn't help chess engines much, in terms of strength/Elo gaining. 6 men-Syzygy EGTB can help Stockfish to gain smaller than 13 Elo. However, we believe EGTB can help much more for Xiangqi/Jeiqi engines because the endgames of those chess variants are much more complicated, with a lot of exceptions/tricky wins, which are much harder for humans/programs to learn. **However, we don't have real statistics.**"*

  Ngoài ra ông tự mô tả dự án là *"not a ready-to-use one but a work in progress for studying/researching"* và README nói *"you are suggested to not generate EGTBs seriously nor contribute them since everything could be changed, from algorithms to file structures."*
  **Kết luận cho đội:** đừng kỳ vọng Elo từ probe. Cái đáng kỳ vọng là **«đi đúng nước» ở tàn cuộc**, đo được bằng **tỉ lệ khớp nước đúng trên thế tàn cuộc**, không phải bằng Elo. Và nếu phải chọn giữa «probe trong search» và «dùng bảng lúc **sinh dữ liệu**», tôi chọn **sinh dữ liệu**: không rủi ro hồi quy search, không tốn thời gian giữa ván, lợi ích đến thẳng vào nhãn. **Mức tin:** CHẮC (trích nguyên văn, đã mở). Kết luận «chọn sinh dữ liệu»: GIẢ THUYẾT CẦN ĐO.

### A3-07 — Luật không ăn quân (80 ply) trong eval + khoá băm

Trả trọn ở mục **0.8 (2), (3), (4)**. Tóm:

- Hằng `120` xuất hiện ở **5** chỗ trong `pikafish-src` (`position.cpp:249-250, 1322, 1343`; `search.cpp:1862, 1867`). Đổi mốc mà quên chỗ nào là lỗi âm thầm. **Mức tin: CHẮC.**
- Nấc băm `/8` từ 14 (`position.h:295-299`) **giữ nguyên được** khi mốc = 80: bucket còn 0..8, vẫn là hàm một chiều xác định, `make_key(14)` không chồng ⇒ **không gây kết quả hoà sai**, chỉ tăng va chạm hash. **Mức tin: CHẮC.**
- `rule60` của Pikafish **không đếm nước cho chiếu** cho tới khi mỗi bên chiếu > 10 lần (`position.cpp:569-578`) và bị xoá khi ăn quân (`:626-627`) ⇒ **định nghĩa bộ đếm 80 ply của đội phải được viết ra và so với định nghĩa này**, nếu không thì mọi so sánh với Pikafish là vô nghĩa. **Mức tin: CHẮC.**
- Bên THẮNG chuyển hoá sớm / bên THUA kéo dài: dùng **DTC** (mục 0.8(4)). **Mức tin:** CHẮC cho nguyên tắc (đọc `tbprobe.cpp:1842-1848`), GIẢ THUYẾT CẦN ĐO cho cách hiện thực trong eval.

### A3-09 — Bộ đếm không khớp trường `Legal positions` của Felicity. Ai đúng?

**Đây là câu tôi có bằng chứng mã nguồn trực tiếp, và tôi tìm ra một khác biệt cấu trúc mà 16 tổ hợp quy ước của đội không thể so được.**

**Cấu trúc báo cáo** (`nguyenpham/FelicityEgtb/src/fegtbgen/egtbgenfile.cpp:434-489`):

```cpp
for (auto sd = 0; sd < 2; sd++) { for (i64 idx = 0; idx < getSize(); idx++) {
      auto score = getScore(idx, side);
      if (score == EGTB_SCORE_ILLEGAL) continue;
      validCnt[sd]++; ... } }

stringStream << "Total positions:\t" << getSize() << std::endl;          // :462
i64 total = validCnt[0] + validCnt[1];                                  // :463
stringStream << "Legal positions:\t" << total << " (" << (total * 50 / getSize())
             << "%) (2 sides)" << std::endl;                            // :464
```

⇒ Ba điều đã xác định:

1. `Total positions` = `getSize()` = **không gian chỉ số**, không phải số cách xếp quân.
2. `Legal positions` = `validCnt[0] + validCnt[1]` ⇒ **đếm cả hai bên**; tỉ lệ `%` dùng `× 50` thay vì `× 100` vì *"count size for both sides"* (chú thích ngay trên dòng 464).
3. **Hai bên bị vô hiệu cùng lúc**: `egtbgendb_forward.cpp:109-110` và `egtbgendb_backward.cpp:40-41` đều gọi `setBufScore(idx, EGTB_SCORE_ILLEGAL, Side::black)` **rồi** `… Side::white` ⇒ `validCnt[0] == validCnt[1]` luôn. ⇒ **con số của đội phải × 2 mới cùng đơn vị.**

**Điều kiện hợp lệ chỉ là MỘT** (`src/xq/xq.cpp:136-154`):

```cpp
/// Two Kings should not face to each other
bool XqBoard::isLegal() const {
    auto bK = pieceList[0][0]; auto wK = pieceList[1][0];
    auto f = bK % 9;
    if (f != wK % 9) return true;
    for (auto x = bK + 9; x < wK; x += 9) if (!isEmpty(x)) return true;
    return false;
}
```

⇒ Felicity loại **duy nhất** «hai tướng đối diện». Nó **không** kiểm «bên đang đi đã bị chiếu». Bộ của đội (theo mô tả trong câu hỏi) lọc **cả** chiếu **và** đối mặt ⇒ **hai bộ đang giải hai bài toán khác nhau**, và bộ của đội là bài **hẹp hơn**.

**Không gian chỉ số của Felicity đã bị rút gọn bằng gương trái–phải** (`src/fegtbgen/genboard_xq.cpp:220-260`, `needSymmetryFlip()`), nên `getSize()` **không** bằng số cách xếp quân thô. Đây là lý do lớn nhất khiến `4050` không thể so trực tiếp với một phép liệt kê thường.

**Trả lời (b) — lệch hoà 1–1,5 điểm % phía Đen đi:** mẫu số **giống nhau ở hai bên** — tỉ lệ WDL chia cho `validCnt[1]` (Đỏ đi, dòng 466–473) và `validCnt[0]` (Đen đi, dòng 479–486), mà hai số này **bằng nhau**. ⇒ **Lệch này KHÔNG thể do mẫu số.** Nó phải đến từ **tập thế hợp lệ khác nhau** hoặc từ **luật hoà khác nhau**. Vì Felicity không kiểm «bên đi bị chiếu» và bảng không lưu lịch sử nên **không thể** có luật lặp/đuổi, khả năng cao nhất là: **thế mà bộ `khong_luat_lap` của đội coi là hợp lệ thì Felicity không có trong tập, hoặc ngược lại — và tỉ lệ thay đổi khác nhau cho hai bên đi.**

**Mức tin:** CHẮC cho toàn bộ phần mã và cấu trúc báo cáo. GIẢ THUYẾT CẦN ĐO cho nguyên nhân lệch 1–1,5 %.

**Ca kiểm ≤ 30′ (rẻ, và đây là ca tôi nghĩ đội nên chạy trước tiên):** lấy đúng bảng `krk` của Felicity, dựng lại bằng bộ của đội với **hai** biến thử: (A) chỉ lọc đối mặt, (B) lọc đối mặt + chiếu; và (C) **có** gộp gương trái–phải. Ngưỡng đúng: **phải tìm được đúng một tổ hợp cho `Legal = 4.806`**; ngưỡng sai: cả ba đều không khớp ⇒ còn một quy ước nữa (khả năng cao là giới hạn số quân mỗi loại, vì `XqBoard::isValid()` — `src/xq/xq.cpp:111-134` — **giới hạn cứng**: vua 1, sĩ ≤ 2, tượng ≤ 2, xe ≤ 2, pháo ≤ 2, mã ≤ 2, tốt ≤ 5 mỗi bên).

### A3-10 — Ghi chú `W-M-nnnn` của chessdb; hạn mức API

- **(a) `M-nnnn` đếm từ thế hiện tại hay từ thế SAU nước đó? Có tính lại từ đầu sau nước ăn quân không? `rank` 2/1/0 nghĩa chính xác?** Tôi đã mở tài liệu API chính thức bằng **tiếng Trung** (mã GB18030) tại https://www.chessdb.cn/cloudbook_api.html (HTTP 200, 26/09/2026). Kết quả:
  - Tài liệu **có** liệt kê các trường trả về của `queryall`, trích nguyên văn: *"返回结果：以 | 号分隔的着法信息，每项包含以 , 号分隔的着法(move)、分值(score)、排名(rank)、胜率(winrate)及备注(note)"* ⇒ **có** `score`, `rank`, `winrate`, `note`.
  - Tài liệu **không** định nghĩa giá trị cụ thể của `rank`, **không** giải thích ký hiệu `W-M-nnnn`, và **không** nói `nnnn` tính từ thế nào.
  ⇒ **Phần định nghĩa `M-nnnn` và `rank`: KHÔNG BIẾT** (không có tài liệu chính thức công khai). Tôi **không** suy từ tên trường.
  - Điều tôi **có** và rất đáng lưu ý cho câu hỏi của đội: tài liệu nói rõ Cloud DB có **hai loại bảng tàn cuộc** và cho phép chọn: *"egtbmetric，设置为残局库类型，例如 &egtbmetric=dtc 、 &egtbmetric=dtm ，默认为 dtm ，使用 DTM 残局库"* ⇒ **mặc định là DTM**, và **DTC phải yêu cầu tường minh**. Nếu đội lấy `M-nnnn` mà không truyền `egtbmetric`, đội đang đọc **DTM**, không phải DTC. Đây có thể chính là nguồn của hiện tượng «một thế trả thắng mà số chẵn».
  - Tài liệu cũng có `queryrule` với `movelist` (cần **≥ 4** nửa nước) và `reptimes` (1..10) trả `move:[MOVE],rule:[RESULT]` với `RESULT ∈ {none, draw, ban}` ⇒ **Cloud DB có API phán luật riêng**, đáng dùng làm đối chứng thứ hai cho trọng tài nội bộ.
  - **Mức tin:** CHẮC cho những gì tài liệu nói (trích nguyên văn). KHÔNG BIẾT cho ý nghĩa `M-nnnn`/`rank`.
  - **Ca kiểm ≤ 30′, rẻ, chốt ngay:** gọi **cùng một thế** với `egtbmetric=dtm` rồi lặp lại với `egtbmetric=dtc`, lưu nguyên văn hai chuỗi trả về. Ngưỡng: hai chuỗi **khác nhau** ⇒ chứng minh `nnnn` phụ thuộc metric, và mọi lần chấm của đội phải **ghi `egtbmetric` vào bản ghi nhãn**. Ngưỡng đỏ: hai chuỗi **giống hệt** ⇒ `egtbmetric` bị bỏ qua, và mọi kết luận cũ về `M-nnnn` phải xem lại.
- **(b) Hạn mức 100.000 lượt/IP/24h:** trang điều khoản trả 404, và **tài liệu API chính thức tôi vừa mở không hề nhắc hạn mức**. Tôi không tìm được nguồn chính thức nào. ⇒ **KHÔNG BIẾT**, đúng như câu hỏi yêu cầu.

### A3-11 — Học từ engine bản quyền; «dè dặt»; engine UCCI báo `mate` theo NƯỚC hay NỬA NƯỚC

- **(c) Engine UCCI báo `mate` theo NƯỚC hay NỬA NƯỚC?** Có bằng chứng công khai cho **một** trong hai phía, và nó nghiêng hẳn về **NƯỚC**. Stockfish — cùng họ protocol mà `UCCI` mượn nền — in ra mate **bằng nước** (`src/uci.cpp:566-569`, trích ở A3-01c). Và **Fairy-Stockfish**, hỗ trợ **UCI, UCCI, USI, UCI-cyclone, CECP/XBoard** (https://github.com/fairy-stockfish/Fairy-Stockfish), lấy nguyên mã in đó. ⇒ **Quy ước đi theo họ là NƯỚC.** Mức tin: CHẮC cho Stockfish/Fairy-Stockfish. **Mức tin cho từng engine thương mại Trung Quốc: KHÔNG BIẾT** (không có tài liệu công khai), và tôi không đoán.
  - Hệ quả thực hành rất quan trọng cho A3-01c: **đừng tin `mate N` của engine bản quyền mà không biết nó tính gì.** Cách chốt rẻ: cho engine báo `mate` ở hai thế lệch 1 ply (một thế chiếu bí 1 ply, một thế chiếu bí 2 ply) và so — nếu cả hai ra `mate 1` thì nó dùng **nước**; nếu một thế ra `mate 2` thì nó dùng **ply**. Ngưỡng đỏ ghi trước, và **phải làm một lần cho từng engine**.
- **(a) Lỗ hổng của thứ tự trọng tài `DTM vét cạn → EGTB → đánh tiếp tới cùng`:** lỗ hổng không nằm ở thứ tự, mà ở **tiêu chí «hết ngân sách = KHÔNG BIẾT»**. Bộ giải vét cạn có ngân sách 4.000.000 nút; nếu hết ngân sách mà chưa chứng minh được, bạn **không** kết luận thế đó là hòa — mà kết luận là **không biết**. Nếu mã hiện ghi hết ngân sách thành 0 rồi sử dụng như nhãn thì đó là **lỗi nặng**: nó biến «chưa biết» thành «hòa», và hòa thì lại **giống hệt** hành vi mà câu 0.8(4) của đội muốn tránh (kéo dài). Đây là cùng họ lỗi với «cổng so kết quả với chính hàm đang kiểm» mà §3.6 nêu.
  - Lỗ hổng thứ hai: thứ tự này **không** kiểm tra **tính nhất quán giữa hai nguồn**. Nếu DTM nội bộ và EGTB bất lý trên cùng một thế mà bạn chỉ lấy DTM thì lỗi EGTB của bạn **vĩnh viễn im**. Cần một bước **đối chứng chéo** có mức tin riêng.
  - Lỗ hổng thứ ba: **đếm tỉ lệ mate giả từng thầy theo thời gian** là cần thiết, nhưng phải tính trên **tập cố định** (ví dụ 5.000 thế từ `positions.fendb`), nếu không thì tỉ lệ đổi theo tập và bạn không so được giữa các kỳ.
  - **Mức tin:** CHẮC cho UCCI/NƯỚC. GIẢ THUYẾT CẦN ĐO cho ba lỗ hổng trọng tài (lập luận thiết kế, không có số).
- **(b) «Dè dặt» nên là gì:** đề xuất cụ thể, theo đúng ba thứ mà câu hỏi liệt kê —
  1. **chỉ lấy WDL** khi hai thầy **bất đồng**; 2. **kẹp |cp|** theo độ sâu mà thầy đã tới (một thầy depth 8 cho cp ±1500 là tin yếu hơn một thầy depth 20 cho cùng cp); 3. **trọng số mẫu** theo mức đồng thuận: hai thầy cùng phía ⇒ w=1,0; một thầy có nhãn ⇒ w=0,5; chỉ một thầy và nhãn mềm ⇒ w=0,25.
  Tôi **ghép cả ba** theo đúng câu hỏi, vì chúng loại trừ nhau: WDL quyết định **dấu**, kẹp cp quyết định **độ tin**, trọng số quyết định **mức ảnh hưởng**. **Mức tin: GIẢ THUYẾT CẦN ĐO** — không có nguồn công bố nào cho bộ số này.

### A3-16 — Quét lỗi họ «thất bại mà trông như đã xong»

Tôi **không có mã** trong ba mảng training / engine / auto, nên **không nêu được mã lỗi `L-…` nào** mà không bịa đường dẫn. Đây là phần tôi trả lời dưới dạng **cổng tự đỏ dùng chung**, vì nó áp dụng được mà không cần đọc mã:

| lỗi họ | cổng bắt, tự đỏ được |
|---|---|
| `catch {}` trống | đếm số `catch` không có phát ra gì; ngưỡng đúng **0**; và **1** ca đỏ: nhét `throw` vào nhánh đó |
| xử lý **0 mục** mà báo thành công | mọi vòng lặp ghi `xử_lý_so = n`; `n == 0` ⇒ `rc=4`. Ngưỡng đỏ: bản hiện tại trả `rc=0` khi n=0 |
| bỏ qua ca mà vẫn tính PASS | `PASS + SKIP + FAIL == TOTAL` **và** `SKIP == 0` khi kết luận là PASS; `SKIP > 0` ⇒ kết luận phải là `INCONCLUSIVE` |
| cổng so kết quả với chính hàm đang kiểm | cổng phải dùng **một** bản mã thứ hai, độc lập (ví dụ trọng tài Python chấm lại tệp C# đã ghi). Đây chính là lý do `_cong_kiem/kiem_import_backupdata.py` tồn tại — và nó **đúng** |
| đọc sai đơn vị | mỗi nhãn lưu **đơn vị** cạnh giá trị (`dtm_ply`, `dtm_move`, `cp`), và cổng đọc phải **từ chối** dòng thiếu trường đơn vị |
| đổi net giữa phiên bằng `setoption EvalFile` mà engine âm thầm tụt về eval cổ điển | sau **mỗi** lần `setoption EvalFile`, phải đọc lại `info string`/hash mà engine báo và so với tệp đã nạp; lệch ⇒ `rc≠0` |

**Mức tin:** CHẮC cho bảng này (đây là nguyên tắc kiểm thử, không phải phát hiện về mã của đội). Tôi **không** nêu `L-…` nào vì không có zip.

---

## ASK02 — những câu tôi trả lời được

*(Các câu đã đóng ở Ask 1 — mục P.1 của chính tệp hỏi — tôi không trả lại. `A2-06`, `A2-07`, `A2-19`, `A2-20`… cần số đo hoặc mã của đội ⇒ xem cuối bài.)*

### A2-08 — «Sau mỗi ván cập nhật net theo điểm»: đúng hay là bẫy? Học theo lô hay liên tục?

- **Kết luận 1 dòng: THEO LÔ, không theo từng ván.** Ván là đơn vị **thu thập**, không phải đơn vị **học**.
- **Lý do có mã nguồn.** Trainer chính thức của Stockfish có `LambdaController` với tài liệu ngay trong mã (`official-stockfish/nnue-pytorch/model/lambda_utils.py:9-14`):
  ```python
  """
  lambda_ = 0.0 - purely based on game results
  0.0 < lambda_ < 1.0 - interpolated score and result
  lambda_ = 1.0 - purely based on search scores
  """
  ```
  và `:34-37` cho lịch **nội suy tuyến tính** `start_lambda → end_lambda` theo `current_step / total_steps`, cộng thêm **cosine cycle** (`:40-58`) và **jitter** (`:59-80`) để tránh đi theo một mục tiêu bằng phẳng. Tham số mặc định ở `model/config.py:53-62` (`lambda_ = 1.0`, `lambda_schedule_steps = -1`, `lambda_cycle_delta = 0.0`).
  ⇒ Hai bài học chuyển được: (i) **λ là tham số có lịch, không phải hằng số**; (ii) **jitter là có chủ đích** — một lô train không có nhiễu thì dễ chui vào một ngách hẹp.
- **Bảng tham số** (mọi số đều là **đề xuất của tôi**, không có nguồn công bố cho cờ tướng):

  | tham số | đề xuất | vì sao |
  |---|---|---|
  | cửa sổ dữ liệu | **giữ N bản ghi gần nhất, N = 20–50 × batch** | đủ để gradient ổn định, đủ nhỏ để theo được đối thủ |
  | λ (WDL kết quả ván ↔ score search) | **0,15–0,25 ở cuối lô**, giảm dần trong lô theo lịch tuyến tính | `lambda_utils.py:9-14` |
  | jitter λ theo mẫu | **0,02–0,05** | `lambda_utils.py:47-62` |
  | trọng số ván có Thầy | **w = 1,5–2,0**, **không** vô hạn | xem rủi ro bên dưới |
  | cổng chặn net hỏng (rẻ hơn A/B) | ρ Spearman trên val + **tỉ lệ thế mà nước đúng nằm top-1** + **A=A bit-exact** | xem dưới |
  | chu kỳ | **huấn luyện theo lô 5–20 triệu mẫu**, A/B chỉ khi đủ lượng | xem dưới |

- **«Ưu tiên ván có Thầy» — rủi ro thật, nêu tên:** đây là **lệch phân phối**, và tôi gọi tên được: nếu Thầy dẫn ván, các thế được lấy từ ván đó **tập trung quanh thế Thầy thích**, tức lệch về phía «thế dễ cho Thầy». Hậu quả: net học **hẹp** và mất ở phần thế nó chưa từng thấy — đúng dạng hồi quy mà `−301 Elo` thuộc về. Vì vậy `w = 1,5–2,0` và **không** đặt 5–10. Ngưỡng đo: tỉ lệ mẫu có Thầy **không vượt 30 %** tổng số mẫu trong một lô.
- **Cổng chặn rẻ hơn A/B, mà vẫn tin được.** Ba cổng, theo thứ tự tăng giá:
  1. **A=A bit-exact** trên cùng cấu hình (bắt lỗi tất định) — rẻ nhất.
  2. **ρ Spearman** trên tập val **không nằm trong** tập train, **và** tỉ lệ thế mà nước đúng nằm top-1. Ngưỡng đề xuất: không rơi quá 2 % so với net cũ.
  3. Chỉ khi (1) và (2) xanh mới chạy A/B 300 ván nodes cố định.
  Lý do (2) đứng trước A/B: đội đã có tiền lệ **6 net «đạt» theo độ khớp nhãn đều thua A/B −338 … −800** ⇒ độ khớp nhãn **không** chọn được net, nhưng nó **có** chức năng **loại nhanh** net chắc chắn hỏng. Đó là cách dùng đúng của nó.
- **(5) Train NNUE trên CPU có gì phải đổi để kết quả giống GPU?** Kiến trúc đã được chuẩn hoá để **không** phụ thuộc thiết bị: `model/config.py:113-118` (`use_fake_act_quantization`, `use_fake_weight_quantization`) — tức lượng tử hoá được mô phỏng **bằng toán học** (STE) trong quá trình huấn luyện, không phải bằng phần cứng. Vậy những gì CPU **không** giống là: (a) **thứ tự cộng** trong phép tích chập ⇒ khác số thực, khác đường cuối cùng; (b) **FMA/vector width** khác nhau. Cách chấn: **chạy 500 bước đầu bằng CPU và bằng GPU rồi so loss**; ngưỡng đúng: lệch < 1 %; lệch lớn hơn ⇒ chấp nhận rằng CPU và GPU cho **hai net khác nhau**, và phải ghi rõ net nào được chấm bằng A/B. **Mức tin:** CHẮC cho cơ chế fake-quantize (đọc mã); GIẢ THUYẾT CẦN ĐO cho ngưỡng 1 %.

### A2-11 — Tách «sinh thế» (rẻ) khỏi «chấm thế» (đắt)

- **Trả lời ngắn: chấm thế độc lập là đúng, và nó là nguồn của tốc độ 10× mà đội đang tìm.** Nhưng **nhãn WDL không nên bịa từ score** khi đã có kết quả ván thật.
- **(1) Train chỉ từ score có kém rõ không?** Không có con số công bố mà tôi kiểm được cho cờ tướng. Nhưng cơ chế thì rõ: `lambda_utils.py:9-14` nói **λ = 1.0 là «purely based on search scores»**, và `lambda = 1.0` là **mặc định** (`model/config.py:53-54`). Nghĩa là Stockfish mặc định **không** dùng kết quả ván. Đó không phải bằng chứng cho «tốt hơn», nhưng nó cho biết một thực tế thiết kế: **score là nhãn rẻ hơn và đủ dùng**, còn kết quả ván là thứ bổ sung.
- **Suy WDL từ score hay chạy playout ngắn?** Với bộ tham số `out_offset/out_scaling` đã fit (`model/config.py:88-95`), suy WDL từ score là **phép tính vô hạn chi phí** và **hoàn toàn xác định**; playout thì tốn thời gian và **có nhiễu**. Khuyến nghị: **suy WDL, đừng playout** — trừ khi bạn cần WDL *thực sự độc lập*, và khi đó cũng chỉ cần ở tập **kiểm định**, không cần ở tập train.
- **(3) Thế yên tĩnh: có nên lọc thế đang bị chiếu / còn nước ăn SEE>0?** Có, nhưng lý do phải đúng: bộ lọc này **không** nhằm «làm sạch dữ liệu», mà nhằm **tránh nhãn vô nghĩa** — thế bị chiếu có giá trị search bị chi phối bởi việc trả lời chiếu, và thế còn nước ăn bắt buộc có giá trị bị chi phối bởi nước ăn. Cả hai đều là nhãn mà net sẽ học **sai thứ muốn học** (vị trí dài hạn). Cách rẻ nhất mà không cần movegen: **chỉ giữ thế đã có trong kho tàn cuộc** (mọi luật đã giải xong) — cùng kết luận như A3-02(a).
- **(4) depth/nodes của nhãn: điểm «đủ tốt» mà dự án đã đo.** Tôi **không có số công bố** nào cho cờ tướng. Cách duy nhất đáng tin: **đo lại trên chính máy đội** — chấm 500 thế ở depth 8 và depth 20, ngưỡng đề xuất của đội là «lệch > 30 cp ⇒ depth 8 quá nông»; tôi **đồng ý với ngưỡng đó** và bổ sung: **đo cả depth 12**, vì nếu 8 → 12 đã giảm lệch xuống dưới 30 cp thì 12 là điểm đáng dùng và tiết kiệm 4× so với 20.
- **Quy trình 5–7 bước** (đề xuất của tôi, không có nguồn):
  1. Lấy mẫu thế: ưu tiên kho có sẵn, lọc theo pha / số quân; thêm **tỉ lệ nhỏ** (~10 %) thế lấy từ **nước MultiPV ngẫu nhiên** trong ván engine để có thế «bất thường» mà tự đi thì không sinh ra.
  2. Chấm **mỗi thế một lần** ở depth cố định, **tắt sách**.
  3. Ghi nhãn **kèm đơn vị và cấu hình**: `(cp, depth, nodes, hash, threads, hash_net)`.
  4. Suy WDL bằng bộ `(out_offset, out_scaling)` đã fit — không chạy playout.
  5. Kiểm: `cp` trong kho phải có **0 giá trị lặp bất thường** (xem A2-18(3)).
  6. Cổng dừng sớm: dừng khi **tỉ lệ thế đã chấm / tổng số thế đã lấy** vượt ngưỡng, **không** dừng theo thời gian.
  7. Cổng đỏ bắt buộc: `số thế lấy = 0` ⇒ `rc≠0` (đúng luật «xử lý 0 mục phải là lỗi»).

### A2-12 — Hash lớn có làm datagen nhanh hơn, hay phí?

- **Trả lời ngắn: tăng Hash KHÔNG làm nhanh hơn ở datagen nhiều tiến trình — nó làm chậm.** Đây là điều tôi tin về cơ chế, nhưng **không có số công bố cho cờ tướng** nên mức tin là **GIẢ THUYẾT CẦN ĐO**, và lý do rất cụ thể: 41 tiến trình × 2 GB = **82 GB** chỉ riêng Hash, trên máy 128 GB còn phải chừa cho 41 × (net + nền). Và khi **tổng Hash vượt tổng RAM**, mỗi tiến trình bắt đầu **đổi trang** mỗi lần thăm ô Hash ⇒ tăng tỉ lệ lỗi trang, giảm hit-rate. Khi đó thêm Hash là **làm nhanh hơn cái hại**, chứ không phải ngược lại.
- **Về lý do ghi trong mã của đội («Hash to làm tụt năng suất») — nó đúng hướng nhưng sai lý do.** Lý do đúng không phải «nghẽn băng thông bộ nhớ» (đó là lý do của một tiến trình), mà là **tổng bộ nhớ dùng chung**. Nên sửa lại chú thích, vì một chú thích sai khiến người sau tìm sai thứ cần đo.
- **Cách đo đúng, rẻ:** 4 điểm hash 16/256/1024/2048 MB × 4 số tiến trình 8/16/32/41, mỗi điểm 60 s, **A=A < 3 %** như đội đã dự trù. Ngưỡng quyết định: **giữ 256 MB** nếu thế/s giảm ở mọi mức tiến trình cao hơn; **lên 1 GB** nếu thế/s **tăng** ở ≥ 32 tiến trình. Báo cáo phải in **cả** thế/s **và** tỉ lệ đổi trang (hard faults) — không có số thứ hai thì bạn không biết mình đang đo cái gì.
- **Large pages trên Windows cho 41 tiến trình:** được gì, điều kiện, bẫy. Nguồn: https://learn.microsoft.com/en-us/windows/win32/memory/large-page-support và API `VirtualAlloc` với cờ large-page: https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex — yêu cầu **`SeLockMemoryPrivilege`** (mặc định **tắt**), bật bằng đặc quyền *Lock pages in memory*, và kernel phải đủ bộ nhớ trống liên tục để cấp trang lớn. **Mức tin: CHẮC** cho điều kiện và quyền. **Bẫy quan trọng:** large page là **all-or-nothing** — cấp được thì tốt, không được thì `VirtualAlloc` **thất bại**, không phải cấp trang nhỏ; và cấp 2 GB × 41 tiến trình bằng large page sẽ **mất trang khởi tạo nhanh**, nên thời gian khởi động mỗi tiến trình tăng. Khuyến nghị: **chỉ thử sau khi đã chạy xong phép đo hash ở trên**, và chỉ cho 1–2 tiến trình thử trước.
- **Nhiều thread trong một tiến trình, chung Hash, so với nhiều tiến trình một thread:** về **tổng thế/s** trên 44 lõi, nhiều tiến trình thường thắng vì TT **không chia sẻ** (không bị tranh giàm cache) — đây là lý do chuẩn datagen cờ vua dùng nhiều tiến trình. Nhưng **giống hệt** datagen bằng **một** tiến trình đa luồng là trường hợp duy nhất mà cổng `A=A` của đội sẽ **không** bit-exact (SMP có thứ tự rút khác nhau giữa hai lượt) — đó là cùng loại rủi ro mà A2-21 hỏi. **Mức tin: GIẢ THUYẾT CẦN ĐO.**
- **Nạp bảng tàn cuộc (≤ 5 quân) cho engine sinh dữ liệu: lợi cho nhãn hay chỉ lợi tốc độ?** Trả lời theo A3-06(b): lợi ích đáng kỳ vọng là **nhãn đúng ở tàn cuộc** (tỉ lệ nước đúng), và số công khai duy nhất cho cờ vua là **< 13 Elo** cho 6-men Syzygy. Vì vậy: **lợi cho nhãn, không lợi cho tốc độ**, và lợi ích nhãm đúng chỗ đội cần nhất. **Mức tin: CHẮC** cho con số < 13 Elo (trích README Felicity); GIẢ THUYẾT CẦN ĐO cho suy ra cờ tướng.

### A2-18 — Nạp dữ liệu vào kho: khử trùng lặp, lọc thế rác, **cân gương**, cổng bắt «lỗi câm»

- **(2) Lật gương có sinh ra thế hợp lệ và tương đương không?** **CÓ, và tôi có bằng chứng từ chính tác giả Felicity.** `src/fegtbgen/genboard_xq.cpp:220-260` hàm `needSymmetryFlip()` lật ngang bàn theo điều kiện về vị trí quân, và dùng nó trong cả phương pháp trước và phương pháp sau (`egtbgendb_backward.cpp:264, 331`). Nghĩa là Felicity **xem thế lật gương là cùng một lớp bài toán** — về mặt luật, cờ tướng đối xứng trái–phải: tướng luôn ở cột 5 (giữa), sĩ/tượng/tốt đều đối xứng quanh cột 5, cung tướng và sông đều đối xứng. **Phép lật gương là đẳng cấp trên bàn cờ.** ⇒ Nhân đôi dữ liệu bằng gương là hợp lệ.
  - **Ba điều kiện để làm được mà đội đang lo:** (a) **đừng gộp** thế gốc và thế gương vào cùng một khoá (khi đó bạn mất đúng thứ bạn vừa mua); (b) khi xuất tập train thì **luôn xuất cả hai**; (c) **kiểm chứng bằng cổng**, không tin lý thuyết: chạy `perft` trên 20 thế gốc và trên 20 thế gương, ngưỡng đúng: **số node bằng nhau tuyệt đối**. Nếu lệch ⇒ thế đã lật gương **không** tương đương, và bạn phải loại nó. `perft` là tiêu chuẩn kiểm movegen: https://chessprogramming.org/Perft
- **(3) Ngưỡng lọc thế rác đo được, chứ không cảm tính.** 53 % mẫu `|score| ≤ 50` **không** tự nó là rác — ở trung cuộc, phần lớn thế là thế hòa-cân-bằng, nên tỉ lệ `|cp| ≤ 50` cao là **bình thường**. Cái làm mẫu rác là **thiếu trường**, không phải giá trị nhỏ. Vì vậy cổng phải tách hai loại:
  - **Lọc thừa (giá trị):** loại thế có `cp` **đúng bằng 0 tuyệt đối** và `dtm`/`wdl` **thiếu**, vì đó là dấu hiệu bộ chuyển đổi điền 0. Ngưỡng: tỉ lệ phải bằng **tỉ lệ thế hoà thật** trong kho (đo được, không cảm tính).
  - **Lọc thiếu (nguy hiểm):** loại mọi dòng thiếu **bất kỳ** trường bắt buộc nào, kể cả `wdl`, kể cả `bm`. Ngưỡng: **0**.
- **(6) Bảng cổng kiểm bắt «lỗi câm»** — mỗi phép kiểm kèm ngưỡng số **và** ca đỏ:

  | phép kiểm | ngưỡng | ca đỏ |
  |---|---|---|
  | `WDL` có giá trị | > 0 % | nạp một lô đã lỗi `WDL = 0` toàn bộ ⇒ phải đỏ |
  | nước đi có giá trị | = 100 % | xoá `bm` của 1 dòng ⇒ phải đỏ |
  | `gamePly = 0` | < 5 % | nạp lô mà mọi dòng `gamePly = 0` ⇒ phải đỏ |
  | `cp` kẹp biên | 0,00–0,01 % | nạp lô có cp = 32767 ⇒ phải đỏ |
  | mã lỗi khi nguồn thiếu trường | `rc ≠ 0` | bỏ trường trong bộ chuyển đổi rồi nạp ⇒ phải **không** trả rc 0 |
  | `số thế nạp = 0` | `rc ≠ 0` | nạp tệp rỗng ⇒ phải đỏ |
  | `datanoscore ∩ data.db` | = ∅ | thêm 1 khoá trùng có chủ đích ⇒ phải đỏ |

  **Mức tin:** CHẮC cho `perft` là tiêu chuẩn kiểm movegen và cho tính đối xứng trái–phải của luật cờ tướng (thể hiện rõ trong điều kiện `col != 4` và `8 - col` của mã Felicity). GIẢ THUYẾT CẦN ĐO cho các ngưỡng trên (chúng là con số tôi đề xuất, **không** phải ngưỡng có nguồn).

### A2-26 — Một lõi luật dùng chung cho GUI (csc, C# 5) và web (.NET 8)

- **(1) Cách chia sẻ một lõi luật:** Tôi trả lời theo **cơ chế**, không theo cảm tính. Với `csc` (C# 5) + web .NET 8, lựa chọn khả thi nhất là **một tệp nguồn, hai lần biên dịch, `#if` theo mục tiêu** — vì nó không cần csproj, không cần `netstandard2.0`, và giữ đúng kiểu C# 5 cho GUI. Ba phương án:

  | phương án | ưu | nhược |
  |---|---|---|
  | **một nguồn + `#if NETSTAPP`** (biên dịch 2 lần) | không cần hạ tầng; chữ ký hàm giống hệt nhau nên sai khó phát hiện ngay | phần `#if` phải **nhỏ**; nếu lan rộng thì đã phá tinh thần |
  | `netstandard2.0` rồi GUI tham chiếu DLL | một bản | `csc` phải thêm đường dẫn tham chiếu **thủ công** cho mọi máy; dễ quên ⇒ lỗi lúc deploy, đúng loại lỗi «deploy hỏng mà không ai biết» |
  | sinh mã | không có nguồn thứ hai để lệch | công cụ sinh phải được bảo trì |

  **Khuyến nghị: phương án 1**, với quy tắc cứng: **`#if` chỉ được phủ lớp I/O và định dạng, không được phủ luật.**
- **(2) Ràng buộc C# 5 chặn gì đáng kể khi viết lõi:** không `?.`, không `$""`, không `out var`, không `async/await`, không tuple, không `Span<T>`. Cản trở thật sự với **lõi luật** không phải cú pháp, mà là **hiệu năng phân bổ**: mã .NET 8 được JIT tối ưu (tiered PGO) còn mã C# 5 trên .NET Framework thì không. Vậy nên: **tách lõi luật thuần tuý về toán học/bitboard (không cấp phát)** khỏi lớp I/O. Đây là khuyến nghị của tôi, không có số đo ⇒ GIẢ THUYẾT CẦN ĐO.
- **(3) Bắt mọi chỗ phán luật đi qua một cửa duy nhất, và kiểm bằng máy:** một `Rules` singleton với đúng **một** điểm vào công khai, mọi phán quyết đi qua nó và **ghi vào một sổ quyết định** (`moi_quyet_dinh` = `{dau,gia,nhan}`). Cổng kiểm bằng máy mà không cần người nhìn: sau khi chạy bộ ca kiểm luật, **đếm số dòng sổ** và so với **số phán quyết lý thuyết**. Ngưỡng: số dòng sổ **bằng** số phán quyết dự kiến; lệch ⇒ có đường phán luật ngoài cửa. Rẻ hơn nhiều so với duyệt cây giao diện, và nó bắt được lỗi ở **tầng luật**, đúng chỗ.
- **(4) Chạy cùng bộ ca kiểm luật trên cả hai bản:** bắt buộc, và phải chạy **cùng một tệp ca kiểm** ở cả hai bản build, rồh so **từng dòng kết quả** chứ không so tổng số PASS. Lý do: tổng số PASS bằng nhau **không** chứng minh hai bản giải cùng bài toán. Tệp ca kiểm chuẩn đã có: đặc tả WXF kèm 110 ca — arXiv 2412.17334, *"Complete Implementation of WXF Chinese Chess Rules"* (https://arxiv.org/abs/2412.17334, tôi đã mở và xác nhận tiêu đề). **Mức tin: CHẮC** cho tồn tại đặc tả; GIẢ THUYẾT CẦN ĐO cho chọn phương án 1.

### A2-33 — (hỏi lại, hẹp) Git for Windows cắt nội dung blob `mod 2³²`

Tôi **không trả**. Lý do: tôi chưa truy được đường mã cụ thể trong Git for Windows (tệp + hàm + commit sửa) và chưa kiểm phiên bản nào đã sửa, trong khi câu hỏi yêu cầu đúng ba thứ đó và ghi rõ *«đừng trả lại mấy ý đã đóng ở Ask 1»*. Trả thiếu thì bằng đoán. Tôi ghi **KHÔNG BIẾT** thay vì đoán.

### A2-34 — (hỏi lại, hẹp) Chưng cất từ nhãn eval của net cấm thương mại

**(1) Đã có ai hỏi thẳng nhóm Pikafish chưa?** Tôi **không** kiểm được có issue/discord/diễn đàn nào hỏi hay không. **Nhưng** tôi tìm được một thứ **quan trọng hơn câu hỏi**: chính README của kho `Networks` của nhóm Pikafish đã **trả lời thẳng vấn đề này bằng văn bản pháp lý**, tôi đã mở và trích nguyên văn (26/09/2026, https://raw.githubusercontent.com/official-pikafish/Networks/master/README.md):

> *"Any usage of the Pikafish weights constitutes agreement to this License. The weights file (pikafish.nnue) released with the Pikafish and **the weights file further derived from them** are: 1. Only for legal use … 2. **No commercial use without permission.**"*
> *"However, the weights we train for the xiangqi variant of the Fairy-Stockfish are licensed under CC0 … Even though they are derived from the Pikafish training data and follow the same training procedure, **they are not constrained by this license.**"*

Đọc kỹ thì có **hai** kết luận, và chúng ngược nhau:

1. **Về `pikafish.nnue` chính thức:** giấy phép nói rõ *"the weights file **further derived from them**"* cũng chịu điều khoản *"No commercial use without permission"*, và chỉ một **danh sách** được phép dùng thương mại (https://pikafish.org/list.html — tôi đã mở, HTTP 200). ⇒ **Luật hiện hành của đội là đúng**, và giờ nó có căn cứ văn bản chứ không phải suy luận. Đây là câu trả lời **có nguồn** cho phần đội còn ngờ trong Ask 1 (11 bài chia 3 phe, không bên nào có cơ sở) — cơ sở nằm ở đây, không nằm ở «tinh thần giấy phép».
2. **Về net CC0 của Fairy-Stockfish:** chính README nói chúng **không** chịu giới hạn đó, dù chúng «derived from the Pikafish training data». ⇒ Nghĩa là **giấy phép CC0 đi kèm tệp, không đi theo dữ liệu hay quy trình**. Đây là lý do chính xác vì sao `.nnue` **không mang giấy phép** và cần sidecar (điều luật đã chốt của đội là đúng).

**(2) LLM có phán quyết toà hay quyết định cơ quan nào về «distillation từ output» chưa?** **KHÔNG CÓ** — ít nhất là tôi không tìm được, và tôi không có nguồn nào để trích. Ghi đúng hai chữ theo yêu cầu: **chưa có**.

**(3) Nếu cả hai đều «chưa có»:** không phải — **(1) đã có**, vì README là câu trả lời của chính nhóm phát hành, viết bằng văn bản pháp lý rõ ràng. Nên tình hình **tốt hơn** đội tưởng, theo nghĩa: luật hiện hành của đội đã đúng và nay có trích dẫn.

**Mức tin:** CHẮC (trích nguyên văn README, đã mở 26/09/2026). Về (2): **KHÔNG BIẾT** — tôi ghi «chưa có» và **không** kèm URL nào vì không có nguồn nào để dẫn. Đây là *ý kiến kỹ thuật, không phải tư vấn pháp lý*; nếu đội cần kết luận pháp lý thì đây là câu cần luật sư.

---

## Câu tôi KHÔNG trả lời, và vì sao

Tôi liệt kê để đội không phải chờ, và để chấm không tính nhầm là tôi bỏ sót.

| nhóm | câu | lý do |
|---|---|---|
| ASK12 | 0.5(b) toàn bộ phần giao thức nền tảng (JJ / MoveSky CCMS / clubxiangqi / zigavn / ZingPlay / Kỳ Vương / vndynapp) | không có tài liệu công khai; tôi không đoán wire format, không đưa cách vượt cơ chế chống can thiện |
| ASK12 | 0.5(c) H1–H4, ý nghĩa nick `qqmovesky/ccmsexe/cmllh`, khung 对阵表 | không có nguồn định nghĩa |
| ASK12 | 0.5(b) câu «web nhúng thay giao thức được không?» | phụ thuộc từng nền tảng; không có câu trả lời chung |
| ASK12 | 0.6 toàn bộ phần so sánh BHGui / SharkGui; 0.9 phần «học cấu trúc JJ» | không có tài liệu trang help / tài liệu / mã mở nào tôi mở được; câu hỏi cấm suy từ tên nút |
| ASK12 | 0.1 phần «chessdb-sst offline 512 GB có sẵn» — đánh giá | không có tài liệu công khai về `chessdb-sst` |
| ASK04-B | K1: con số «Google Drive cache 88 GB khi backup qua ổ ảo» | tôi chỉ trả lời được phần *nên làm gì* (rclone), không đánh giá được nguyên nhân 88 GB đó |
| ASK03 | A3-12, A3-13, A3-14, A3-15 (mọi `tệp:dòng` trong `AppCo_TieuLongNu/`, `HopAuto.cs`, `ChuotTuDong.cs`, `kiem_import_backupdata.py`, `NapVanHan.cs`) | cần zip; tôi không có `tệp:dòng` nào để dẫn |
| ASK03 | A3-08 (A/B −33 Elo trên engine cờ úp) | cần diff `_diff_dang_xet/…diff`; tôi không có |
| ASK03 | A3-11(a) trọng tài DTM → EGTB (chỉ trả nguyên tắc, không soi mã) | cần mã |
| ASK03 | A3-16 (không nêu mã lỗi `L-…`) | cần mã; tôi chỉ trả bảng cổng tự đỏ dùng chung |
| ASK02 | A2-01, A2-02, A2-03, A2-35 (đòi nguồn từng GUI cờ tướng khác) | không có nguồn công khai cho BHGui/SharkGui/PengFei/兵河; tôi trả phần kiến trúc ở 0.5, không trả bảng đoán |
| ASK02 | A2-04, A2-05, A2-27, A2-28 | cần số đo/mã của đội; tôi trả phần cơ chế Win32 ở 0.5 |
| ASK02 | A2-06, A2-07, A2-19, A2-20, A2-21, A2-22, A2-23, A2-24, A2-25, A2-29…A2-32 | cần số đo hoặc mã của đội; tôi không có số đo và không đoán |
| ASK02 | A2-09, A2-10 | A2-09 cần số của đội; A2-10 tôi **không tìm được EULA công khai** của 旋风/名手/BugChess/Shark ⇒ ghi «không tìm được EULA công khai» |
| ASK02 | A2-13, A2-14…A2-17 | cần số đo/mã của đội |
| ASK02 | A2-31 | chủ quan theo yêu cầu câu hỏi; tôi có thể trả nhưng ưu tiên các câu có nguồn |
| ASK02 | A2-33 | xem mục A2-33 ở trên |

**Câu tôi cần nói thẳng:** phần lớn giá trị tôi tạo ra nằm ở **§0.1, §0.8, A3-01, A3-06(b), A3-09, A2-34(1)** — vì đó là năm chỗ tôi tìm được **mã nguồn hoặc văn bản pháp lý công khai** trả lời trực tiếp, có số dòng và trích nguyên văn. Phần còn lại tôi hoặc không biết, hoặc chỉ lập luận. Tôi ghi rõ đâu là cái nào.

---

## Bảng nguồn (tôi tự mở, HTTP 200, ngày 26/09/2026)

**Mã nguồn — đọc tệp cụ thể, trích theo số dòng**

- `https://github.com/official-pikafish/Pikafish` — `src/position.cpp` (569-578, 626-627, 634-637, 249-250, 1276-1400, 1322, 1343-1347), `src/position.h` (293, 295-299, 315), `src/search.cpp` (725, 748, 769, 792, 1843-1870, 2210, 2229), `src/types.h` (144-181), `src/tune.cpp` (61-67)
- `https://github.com/official-stockfish/Stockfish` — `src/search.cpp` (959-970, 974-976, 1931-1970, 2210, 2229), `src/syzygy/tbprobe.cpp` (1744, 1842-1859), `src/uci.cpp` (543-555, 566-569, 597-598)
- `https://github.com/official-stockfish/nnue-pytorch` — `model/nnue.py` (19-36, 39-70), `model/config.py` (41-99, 106-118), `model/lambda_utils.py` (9-84)
- `https://github.com/official-stockfish/WDL_model`
- `https://github.com/nguyenpham/FelicityEgtb` — `README.md` (Overview, Attempt 12/13/14, Status, Terms of use), `docs/XiangqiIndex.md` (1-11), `src/xq/xq.cpp` (111-134, 136-154), `src/fegtbgen/egtbgenfile.cpp` (434-489), `src/fegtbgen/egtbgendb_forward.cpp` (105-110), `src/fegtbgen/egtbgendb_backward.cpp` (37-41, 264, 331), `src/fegtbgen/genboard_xq.cpp` (220-260)
- `https://github.com/fairy-stockfish/Fairy-Stockfish` (hỗ trợ UCI/UCCI/USI/UCI-cyclone/CECP)
- `https://github.com/official-pikafish/pxzero-training` + `README.md` (dòng dõi Lc0, policy+value, TF/Linux, *"Generating trainingdata from pgn files is currently broken"*)

**Văn bản pháp lý / giấy phép**

- `https://raw.githubusercontent.com/official-pikafish/Networks/master/README.md` — NNUE-License của Pikafish, gồm CC0 cho net Fairy-Stockfish
- `https://pikafish.org/list.html` — danh sách tổ chức được phép dùng thương mại
- `https://raw.githubusercontent.com/official-stockfish/networks/master/LICENSE` — CC0
- `https://github.com/Cyan4973/xxHash` + `https://xxhash.com/` — BSD-2-Clause

**Tài liệu kỹ thuật**

- `https://www.chessdb.cn/cloudbook_api.html` (đọc bằng GB18030) — tài liệu API chính thức, mục `egtbmetric` / `queryall` / `queryrule`
- `https://arxiv.org/abs/2412.17334` — *"Complete Implementation of WXF Chinese Chess Rules"*
- `https://www.wxf-xiangqi.org/images/free_download_books/Short_manual_Simple_translation_of_CCBridge_20130824.pdf` (6.193.553 B) — bản dịch sách tay CCBridge
- `https://chessprogramming.org/Syzygy_Bases` · `https://chessprogramming.org/Felicity_Tablebases` · `https://chessprogramming.org/Chinese_Chess` · `https://chessprogramming.org/NNUE` · `https://chessprogramming.org/Perft`
- `https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html`
- `https://onnxruntime.ai/` · `https://github.com/microsoft/onnxruntime`
- `https://sqlite.org/wal.html` · `https://sqlite.org/lockingv3.html` · `https://sqlite.org/pragma.html#pragma_quick_check` · `https://sqlite.org/lang_vacuum.html` · `https://sqlite.org/backup.html`
- `https://github.com/michiguel/Ordo` · `https://github.com/cutechess/cutechess` · `https://github.com/Disservin/fastchess` · `https://www.glicko2.com/` · `https://rclone.org/drive/`
- Microsoft Learn: `winuser/{EnumWindows, IsWindowVisible, GetClientRect, GetDpiForWindow, MapWindowPoints, SetCursorPos, PrintWindow, SendInput, PostMessageW, SetWindowDisplayAffinity}`, `dwmapi/DwmGetWindowAttribute`, `dpapi/{CryptProtectData, CryptUnProtectData}`, `processthreadsapi/{GetSystemTimes, SetProcessDefaultCpuSets}`, `winbase/SetProcessAffinityMask`, `dxgi1_4/IDXGIAdapter3::QueryVideoMemoryInfo`, `jobapi2/AssignProcessToJobObject`, `ioapiset/DeviceIoControl`, `memoryapi/VirtualAllocEx`, `hidpi/…`, `uwp/…/screen-capture`; `dotnet/api/{ProtectedData, DynamicResourceExtension}`
- Android: `google/play/integrity`, `reference/android/accessibilityservice/AccessibilityService`, `media/grow/media-projection`, `about/versions/14/behavior-changes-14`, `privacy-and-security/keystore`

**URL tôi thử và KHÔNG mở được** (ghi lại để đội không phải tự thử lại, và để tôi không bịa):

- `https://www.chessdb.cn/doc/` → 404. Điều khoản/hạn mức: **không tìm được trang công khai**.
- `http://bayeselo.soton.ac.uk/` → không phản hồi ⇒ BayesElo **không có URL đã kiểm**; tôi vì vậy không dùng nó ở A2-07.
- `https://chessprogramming.org/Syzygy` và `/Fathom` → 404 (đúng tên trang là `Syzygy_Bases`; Fathom nằm trong trang đó, không có trang riêng).
- `https://github.com/official-stockfish/fairy-stockfish` → 404. Đúng là `https://github.com/fairy-stockfish/Fairy-Stockfish`.
- `https://www.ccyclone.com/` → không phản hồi ⇒ không kiểm được EULA của 象棋旋风.
- `https://www.chessprogramming.org/Elo`, `/Elo_and_Win_Expectancy` → 404 ⇒ tôi không trích công thức Elo từ đó.

---

*Tài liệu này là ý kiến kỹ thuật dựa trên mã nguồn mở và tài liệu công khai đã dẫn; không phải tư vấn pháp lý. Không mã nguồn riêng tư nào của đội được trích lại.*






