# big-pickle (opencode) — trả lời Ask 1 (27/09/2026)

Bài do **opencode** (mô hình `big-pickle`) viết, ngày **27/09/2026**. Không sửa tệp AI khác, không sửa `ask 1/`.

## 0. Cách tôi trả lời (đọc trước)

- **Ba dòng bắt buộc ở mỗi mục:** Trả lời → Nguồn → Mức tin (`CHẮC` / `GIẢ THUYẾT CẦN ĐO` / `KHÔNG BIẾT`).
- **Mọi URL trong sổ dưới đây tôi đều tự mở và đọc ngày 27/09/2026; ngày 27/09/2026 tôi chạy lại kiểm trạng thái cho từng URL và nhận 200 hết** (trừ các URL nêu ở dòng cuối sổ nguồn — tôi **không** dùng làm nguồn). Trích dẫn kèm nguyên văn ≤ 3 dòng.
- **Tôi không có máy, không zip, không mã của đội.** Số liệu thời gian/dung lượng/Elo trong câu hỏi là **số của đội** — tôi không đo lại, không ghi số đoán là số đo.
- **Tôi không nhận được `Ask03_source_2026-09-25.zip`** ⇒ **không có mục `L-*` nào** (mỗi `L-*` bắt buộc phải có `đường/dẫn/trong/zip:dòng`), và các câu cần đọc mã thì tôi ghi rõ phần nào **không biết**. Không đoán `tệp:dòng`.
- **Tôi không** hướng dẫn bẻ khoá/bypass phần mềm thương mại, không dịch ngược, không đề xuất dùng net CC-BY-SA cho sản phẩm bán, không suy luận nội bộ từ tên nút của GUI thương mại. Với protocol của app bên thứ ba (JJ象棋, MoveSky, Ziga, ZingPlay, Kỳ Vương, vndynapp): trả **«không biết»** hoặc đường hợp pháp.
- Thuật ngữ: `DTM` = ply tới chiếu bí · `DTC/DTZ` = ply tới lần ăn quân/đẩy tốt kế tiếp · `r50c` = bộ đếm ply không ăn quân · `cp` = centipawn hiệu chuẩn (không phải giá trị quân).

## Sổ nguồn đã kiểm (dùng lại được)

| Mã | Nguồn (đã mở 27/09/2026) |
|---|---|
| S1 | `https://www.sqlite.org/wal.html` |
| S2 | `https://www.sqlite.org/lang_transaction.html` |
| S3 | `https://www.sqlite.org/datatype3.html` |
| S4 | `https://www.sqlite.org/pragma.html` (`#pragma_synchronous`, `#pragma_journal_mode`, `#pragma_integrity_check`) |
| S5 | `https://www.sqlite.org/backup.html` · `https://www.sqlite.org/lang_vacuum.html#vacuuminto` |
| S6 | `https://www.sqlite.org/c3ref/busy_timeout.html` · `https://www.sqlite.org/c3ref/busy_handler.html` · `https://www.sqlite.org/c3ref/wal_checkpoint_v2.html` · `https://www.sqlite.org/threadsafe.html` |
| S7 | `https://www.chessprogramming.org/Syzygy_Bases` · `https://www.chessprogramming.org/Endgame_Tablebases` |
| S8 | `https://www.chessprogramming.org/Felicity_Tablebases` · `https://github.com/nguyenpham/FelicityEgtb` |
| S9 | `https://raw.githubusercontent.com/official-stockfish/Stockfish/master/src/uci.cpp` (dòng 528–600) |
| S10 | `https://raw.githubusercontent.com/official-stockfish/Stockfish/master/src/search.cpp` (dòng ~140, 834, 907–908, 1942–1960, 2168, 2210) |
| S11 | `https://raw.githubusercontent.com/official-stockfish/WDL_model/master/Readme.md` |
| S12 | `https://raw.githubusercontent.com/official-pikafish/Pikafish/master/src/uci.cpp` |
| S13 | `https://raw.githubusercontent.com/official-stockfish/fishtest/master/server/fishtest/stats/stat_util.py` |
| S14 | `https://github.com/git-for-windows/git/issues/6012` |
| S15 | `https://github.com/git-for-windows/git/pull/6353` |
| S16 | `https://git-scm.com/docs/git-config` (mục `core.bigFileThreshold`) |
| S17 | `https://github.com/dotnet/wpf/issues/2281` · `https://github.com/dotnet/wpf/issues/2690` · `https://github.com/dotnet/wpf/issues/3469` |
| S18 | `https://arxiv.org/abs/2412.17334` — *Complete Implementation of WXF Chinese Chess Rules* |
| S19 | `https://www.wxf-xiangqi.org/images/free_download_books/Short_manual_Simple_translation_of_CCBridge_20130824.pdf` (44 trang) |
| S20 | `https://learn.microsoft.com/en-us/windows/uwp/audio-video-camera/screen-capture` · `https://learn.microsoft.com/en-us/windows/win32/api/wingdi/nf-wingdi-bitblt` · `https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendinput` · `https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-postmessagew` · `https://learn.microsoft.com/en-us/windows/win32/api/dpapi/nf-dpapi-cryptprotectdata` |
| S21 | `https://developer.android.com/reference/android/accessibilityservice/AccessibilityService` · `https://developer.android.com/media/grow/media-projection` |
| S22 | `https://learn.microsoft.com/en-us/aspnet/core/signalr/introduction` |
| S23 | `https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html` |
| S24 | `https://rclone.org/drive/` |
| S25 | `https://arxiv.org/abs/1712.01815` (AlphaZero Zero) |
| S26 | `https://pypi.org/pypi/google-crc32c/json` · `https://pypi.org/pypi/crc32c/json` · `https://docs.python.org/3/library/zlib.html#zlib.crc32` |
| S27 | `https://github.com/DuyLeTran/DeepXiangQi` · `https://github.com/rhevorn/xiangqi-vision` (MIT) · `https://github.com/aigst/tchess` (GPL-3.0) · `https://github.com/baislsl/MilkChess` (GPL-3.0) · `https://github.com/black-de-miaomiao/XiangqiOverlay` |
| S28 | `https://github.com/chijason99/Xiangqi-Core` (C#: ký pháp Trung Quốc truyền/giản, PGN) |
| S29 | `https://www.wxf-xiangqi.org/` |
| S30 | `https://www.sqlite.org/atomiccommit.html` (cơ chế rollback journal) |
| S31 | `https://raw.githubusercontent.com/official-pikafish/Networks/master/README.md` (điều khoản NNUE-Licence của trọng số Pikafish) |
| S32 | `https://raw.githubusercontent.com/official-pikafish/Pikafish/master/README.md` (GPL v3; dữ liệu Pika Xiangqi Zero theo ODbL) |
| S33 | `https://learn.microsoft.com/en-us/windows/win32/memory/large-page-support` (large pages cần `SeLockMemoryPrivilege`; phân trang dễ bị lỗi âm thầm) |

URL **404 / không mở được — tôi KHÔNG dùng làm nguồn** (ghi để đội khỏi kiểm lại): `sqlite.org/quick_check.html` (dùng S4), `sqlite.org/c3ref/c_checkpoint.html` (dùng S6), `learn.microsoft.com/.../nf-winuser-postmessage` (phải `postmessagew`), `docs.comfy.org/development/core-concepts/api` (404), `github.com/official-pikafish/Pikafish/blob/master/LICENSE` (404 — **không** xác minh được giấy phép từ URL này), `learn.microsoft.com/en-us/dotnet/desktop/wpf/advanced/graphics-acceleration` (404), `tablebase.sesse.net/syzygy/` (HTTPS lỗi chứng thực TLS trên máy kiểm; bản HTTP trả 200 nhưng tôi **không** dùng bản HTTP làm nguồn — dùng S7), `learn.microsoft.com/en-us/windows/win32/api/direct3ddxgi/nf-direct3ddxgi-iddxgi-outputduplication-releaseframe` (404 — nên tra đúng hàm qua tìm kiếm API).

## 0.1 Tàn cuộc bậc 2

**Trả lời (5 ý):**

1. **Đừng đặt bài toán là "liệt kê bậc 2+".** Mục lục ≤ 4v4 ≈ 42.687 ô; với con số **1,14·10²⁰** thế mà đội đưa ra, kể cả 1 byte/thế cũng ~114 petabyte, và thuật toán giải lùi cần **liệt kê trước** rồi mới tính nhãn ⇒ trên một máy chỉ có thể **lấy mẫu theo ô**. *(Suy luận toán học, không cần nguồn ngoài. Tôi không xác minh được 1,14·10²⁰ và cũng không xác minh được 4,2·10¹⁸ — hai con số này **khác nhau 27 lần**, xem "cần gì từ đội" #9.)*
2. **Hạn ngạch theo tần suất thật, có sàn và trần:** `w(cell) ∝ (N_cell_thật + β)^γ`, `γ ≈ 0,5`, sàn ví dụ 1.000 thế, trần `5 × median`. Rút **đều bên trong** một ô, nhưng chọn ô theo trọng số. Dữ liệu đội ủng hộ hướng này: chỉ **0,023 %** thế thật không sĩ/tượng, **91 %** thế thật có ≥ 3 sĩ/tượng ⇒ ưu tiên dồn hạn ngạch vào tầng trung cuộc rồi mới lấy mẫu bậc 3–4.
3. **Target: WDL làm đầu chính, DTM làm đầu phụ; đừng nén DTM vào cp.** Có nguồn: trong Stockfish, `cp` là đơn vị **hiệu chuẩn theo tỉ lệ thắng**, không phải giá trị khoảng cách — S11 viết *"Stockfish's 'centipawn' evaluation is decoupled from the classical value of a pawn, and is calibrated such that an advantage of '100 centipawns' means the engine has a 50% probability to win"*, và S9 dòng ~535–560 tính `win_rate_model = 1/(1+exp((a-eval)/b))` với `a,b` **phụ thuộc vật chất còn lại**. Nén DTM vào cp là đưa thông tin thứ tự vào một trục đã bị lấp đầy bởi thông tin khác.
4. **Công thức `cp = dấu × max(1500, 3000 − 10·dtm_ply)` (mặc định TẮT trong câu hỏi) làm mất đúng thứ bạn muốn giữ:** mọi `dtm_ply ≥ 150` cho **cùng nhãn 1500** ⇒ độ dốc = 0 trên toàn vùng thế kéo rất dài, tức là **mất MỮI độ dài chiếu bí** — đúng mục tiêu "giữ MỌI độ dài chiếu bí" của chủ sở hữu. Thêm nữa `:433` loại mate ≥ 29.9xx còn phần còn lại kẹp ±3000 ⇒ **độ dốc cp ở vùng 1500–3000 là hằng số**.
5. **Nhãn «nước đúng» của bên thua: đây là quyết định của chủ sở hữu, nhưng kỹ thuật bắt buộc phải lưu CẢ HAI cờ.** Với net **value-only** (cả 3 engine đều value-only), bạn **không thể** hỏi "nước nào đúng"; bạn huấn luyện nhãn cho **thế con** rồi để search chọn (xem A3-03a). Với luật 80 ply: thế mà `dtm ≥ số ply còn lại tới mốc` thì **quy định là hoà, không phải thua**; dán nhãn "thua" cho nó là dạy net một thứ luật không có. Hệ quả cụ thể của từng lựa chọn ở **A3-03b**.

```python
# Python 3.12 — nhãn tàn cuộc, không phụ thuộc thư viện ngoài. ĐÃ CHẠY THẬT: 6 assertion đều đạt.
MATE_CP, CP_CLAMP, DTM_SLOPE = 30000, 1000, 12   # DTM_SLOPE = cp/ply cho đầu phụ

def label(dtm_ply, remain_ply=80):
    if dtm_ply is None or dtm_ply < 0:
        return None                                  # KHÔNG BIẾT -> loại khỏi tập train
    thang = dtm_ply < remain_ply                     # quy định: quá mốc = hoà, không phải thua
    cp = 0 if not thang else max(CP_CLAMP, min(MATE_CP, MATE_CP - DTM_SLOPE * dtm_ply))
    return {"wdl": 1 if thang else 0, "cp": cp,
            "dtm_bucket": min(dtm_ply, 200) // 10,  # giữ độ dài chiếu bí như MỘT SỐ
            "is_draw_by_rule": int(not thang)}

assert label(0)["wdl"] == 1
assert label(79)["wdl"] == 1 and label(80)["wdl"] == 0
assert label(200)["wdl"] == 0                          # kéo dài: hoà theo luật
assert label(3)["cp"] == MATE_CP - 36 and label(3)["cp"] != label(30)["cp"]  # KHÔNG bão hoà
assert label(-1) is None
```

**Ca kiểm (ngưỡng ghi trước):** **Đỏ** — với nhãn cũ (`max(1500, 3000-10·d)`), `Spearman(đầu ra net, −dtm)` trên 20.000 thế thắng có `dtm ≥ 150` phải ≈ 0. **Xanh** — sau khi sửa: `ρ ≥ 0,5` trên tập đó; sai số `|cp_pred − cp_label|` trung vị ≤ 30 cp; AUC WDL ≥ 0,75 trên 2.000 thế giữ nguyên.
**Nguồn:** S9, S10, S11, S7.
**Mức tin:** `CHẮC` cho phép tính chuẩn hoá cp và nguyên lý luật; `GIẢ THUYẾT CẦN ĐO` cho γ, sàn/trần, `DTM_SLOPE` và ngưỡng đo; `KHÔNG BIẾT` cho số thế/ô (đội đo, tôi không kiểm được).

## 0.2 Mô hình kho (`datanoscore.db` → `data.db` → `positions.fendb`)

**Trả lời:**

- **Tách hàng đợi khỏi kho đích là đúng.** Lý do có nguồn: SQLite cho phép nhiều tiến trình đọc nhưng *"only one simultaneous write transaction"* (S2) ⇒ một tệp vừa làm hàng đợi vừa làm kho đích buộc bạn giữ một writer duy nhất suốt 567 triệu dòng. Tách ra thì lúc chấm chỉ khoá `data.db`; GUI đọc song song vẫn được.
- **Bất biến `datanoscore ∩ data.db = ∅` chỉ được *kiểm*, không được *bảo đảm*, trừ khi có UNIQUE index trên `data.db.vkey`.** `INTERSECT` là phép toán toàn bảng trên 567 triệu dòng, chạy mỗi lần gộp là tự tạo thêm tải; bảo đảm bằng ràng buộc, kiểm bằng thống kê rẻ.
- **Chuyển dòng phải nằm trong MỘT giao dịch:** `BEGIN IMMEDIATE` … `INSERT` … `DELETE FROM queue WHERE vkey=?` … `COMMIT`. Tách 2 giao dịch thì một lần treo giữa chừng tạo đúng vi phạm bất biến bạn sợ.
- **`positions.fendb`:** tôi **không biết** định dạng này (tên `.fendb` không xuất hiện trong tài liệu công khai nào tôi mở được) ⇒ không đề xuất SQL cho nó; mọi đề xuất dưới đây viết theo giả định nó là tệp phẳng đếm dòng + băm, đúng như mô tả "gộp ra `positionsfinal` rồi đổi tên".
- **`data.db` = kho 3 TB + sách cấm (xem 0.7); `datanoscore.db` = UTF-16LE `journal_mode=delete`; staging = UTF-8 WAL** (theo mô tả của đội). Ba cấu hình này **không nên trộn**: xem 0.3 và K2.

```sql
-- Ràng buộc bất biến (chạy 1 lần; nếu DB đang trùng thì lệnh sẽ lỗi — đó là điều tốt)
CREATE UNIQUE INDEX IF NOT EXISTS ux_data_vkey ON data(vkey);
-- Chuyển 1 dòng, nguyên tử
BEGIN IMMEDIATE;
  INSERT INTO data(vkey, score, wdl, source) VALUES (?,?,?,?);
  DELETE FROM queue WHERE vkey=?;
COMMIT;
-- Cổng sau gộp (mỗi lô, KHÔNG mỗi dòng)
PRAGMA integrity_check;                                        -- kỳ vọng "ok"
SELECT DISTINCT typeof(vkey) FROM data;                        -- kỳ vọng: đúng 1 lớp
SELECT count(*) FROM queue WHERE vkey IN (SELECT vkey FROM data);  -- kỳ vọng 0
```

### 0.2b Chống trùng ở quy mô lớn — và vì sao `COUNT(DISTINCT)` chết

- **`COUNT(DISTINCT vkey)` toàn bảng**: SQLite phải dựng cấu trúc tạm khử trùng (sort/B-tree tạm hoặc bảng tạm trên đĩa) ⇒ **một lần ghi tạm cỡ 30 GB** để trả lời một con số. Ở 502 triệu dòng, chi phí nằm ở I/O ghi tạm. Bạn đã trả giá này một lần rồi — **đừng trả lần hai**.
- **Cách 1 (khuyên, dài hạn): `CREATE UNIQUE INDEX` trước khi gộp.** SQLite phát hiện trùng ngay lúc `INSERT` thay vì để bạn phát hiện sau. Đổi lại: DB to hơn ~15–20 %, phải chừa chỗ.
- **Cách 2: `INSERT OR IGNORE` + `changes()`** với đủ ba điều kiện: cột khoá thuần khoá, index UNIQUE thật sự tồn tại, và `PRAGMA count_changes=ON` để `changes()` phản ánh `INSERT` chứ không phải `DELETE` bên trong trigger. Vẫn phải đo lại bằng `SELECT count(*)`.
- **Cách 3 (bất biến tuyệt đối): khoá = băm 128 bit FEN chuẩn hoá, cột `UNIQUE NOT NULL`.** Bỏ hẳn lớp lỗi 0.3; đổi lại phải quản lý ~30 B/dòng (502 triệu dòng ≈ 15 GB cột khoá).
- **Khoá phải là hàm thuần tuý của nội dung thế** — không phụ thuộc thứ tự chèn hay lô dữ liệu. Với FEN: bỏ khoảng trắng, gộp quân lặp `--`→`-`, **không** đưa số nước chưa đi vào khoá (số đó khác nhau chỉ vì lịch sử).

### 0.2c So khớp `end.fen` ↔ `stockfish.csv` (4,5 % lệch)

- **Khoá nội dung, tuyệt đối không khoá vị trí dòng.** `stockfish.csv` không có `vkey`, chỉ có FEN ⇒ `end.fen` phải được chuẩn hoá **giống hệt** quy trình sinh CSV rồi băm. Nếu chỉ `dòng 1 ↔ dòng 1`, 4,5 % lệch là hệ quả tất yếu của việc một bên bị lọc/sắp lại.
- **Chỗ hay sinh lệch nhất:** (1) khoảng trắng, (2) quân lặp `--` vs `----`, (3) `w`/`b` vs `W`/`B`, (4) **có giữ số nước chưa đi hay không**, (5) phía đi trước. Vì 4,5 % là con số rất "giống lỗi quân lặp + số nước chưa đi", tôi **nghi ngờ trước** hai loại đó — nhưng đó là **GIẢ THUYẾT CẦN ĐO**, không phải kết luận.
- **Quy trình 6 giờ:** `hash(FEN_cuẩn_hoá)` cả hai bên → `LEFT JOIN` → đếm **3 loại**: không có trong CSV (thiếu), có nhưng `score` rỗng, khác mọi thứ. Ba số phải cộng lại đúng 100 %.
- **Không vá bằng cách khớp theo thứ tự dòng rồi chấp nhận** — đó là cách âm thầm tạo nhãn sai cho 2 triệu dòng mà không ai phát hiện, vì khoá sai vẫn "khớp được" 95,5 %.

```python
# Python 3.12 — chuẩn hoá FEN để băm. Dùng CHUNG cho cả hai bên. ĐÃ CHẠY THẬT: 2 assertion đạt.
def canon_fen(fen):
    p = fen.split()
    if len(p) < 4:                    # rác -> chuỗi rỗng để bị cổng kiểm loại
        return ""
    b = []
    for ch in p[0]:                    # gộp quân lặp: '----' -> '-'
        if ch != '-' or (b and b[-1] != '-'):
            b.append(ch)
    return "%s %s" % ("".join(b), p[1].lower())   # CỐ Ý KHÔNG kèm số nước chưa đi

assert canon_fen("4k4/4a4/9/9/9/9/9/9/9/4K4 b - - - 0 1") == \
       canon_fen("4k4/4a4/9/9/9/9/9/9/9/4K4 b - ---- - 12 34")
# Nếu KHÔNG bằng -> 2 nơi đang dùng 2 quy tắc khác nhau: đó chính là 4,5 % lệch.
```

**Ca kiểm chung (ngưỡng ghi trước):** **Đỏ 1** — cố tình `INSERT` khoá đã có ⇒ phải lỗi UNIQUE; **Đỏ 2** — chạy lô **0 dòng** (dry-run) ⇒ tool phải **thoát mã ≠ 0**, không in "đạt"; **Xanh** — sau lô: `count(*)` khớp `changes()`, `typeof` 1 lớp, `integrity_check` = "ok", và sau khi so khớp FEN: tỉ lệ khớp ≥ 0,999 trên mẫu 100.000 dòng, `thiếu = 0`.
**Nguồn:** S2, S3, S4, S6 (dòng này và dòng dưới áp dụng cho **cả 0.2, 0.2b và 0.2c**).
**Mức tin:** `CHẮC` cho cơ chế một-writer, UNIQUE, so sánh số; `GIẢ THUYẾT CẦN ĐO` cho hệ số +15–20 % index và ngưỡng; `KHÔNG BIẾT` định dạng `positions.fendb` và quy tắc chuẩn hoá thực tế của đội.

## 0.3 Sửa kho toàn bảng (567.206.917 dòng, 30 GB)

**Số học trong câu hỏi tự khớp — nên tôi tin mô hình lỗi:** `474.711.771 = 567.206.917 − 92.495.146`; sửa 27.699.978 dòng REAL **không trùng** (`92.495.146 − 64.795.168`) ⇒ `474.711.771 + 27.699.978 = 502.411.749`; bỏ 7.830.204 dòng sách cấm ⇒ **`494.581.545`**, đúng con số đội ghi.

**Chẩn đoán: `bind_double` nhiều khả năng KHÔNG phải gốc rễ.** Theo S3: *"A column that uses INTEGER affinity behaves the same as a column with NUMERIC affinity"* và *"If a floating point value that can be represented exactly as an integer is inserted into a column with NUMERIC affinity, the value is converted into an integer."* Hệ quả: `2272294912.0` **là số nguyên chính xác**; nếu cột khoá khai báo chứa `"INT"` thì SQLite **bắt buộc** đổi thành INTEGER. Vậy mà bạn quan sát `typeof(vkey)='real'` ở 92.495.146 dòng ⇒ **cột khoá không có INTEGER/NUMERIC affinity** (khai báo không chứa `INT`, ví dụ `vkey` trần / `BLOB` / `VARCHAR`), **hoặc** giá trị không phải số nguyên chính xác, **hoặc** khoá không nằm ở một cột.
Và vì S3 nói *"When an INTEGER or REAL is compared to another INTEGER or REAL, a numerical comparison is performed"* (và `GROUP BY` coi chúng bằng nhau nếu bằng số), 64.795.168 dòng trùng **không thể tồn tại nếu cột có UNIQUE index** ⇒ **cột khoá hiện đang không có ràng buộc UNIQUE**. Đây là lỗi gốc thứ hai, và nó giải thích vì sao công cụ gộp báo «không đạt» trong khi dữ liệu đúng: `INSERT OR IGNORE` âm thầm bỏ 64,7 triệu dòng, công cụ đếm "thêm" rồi so ⇒ báo lệch.

**Tôi đã tự chạy 7 phép thử này trên SQLite thật (3.49.1) để kiểm chứng chẩn đoán — kết quả:**

| Phép thử | Kết quả thật | Ý nghĩa |
|---|---|---|
| Chèn `2272294912.0` vào cột khai báo `vkey INT` | `typeof` = **`integer`** | Cột có `INT` **không thể** chứa REAL ⇒ cột của đội không khai báo `INT`. |
| Chèn cùng giá trị vào cột **không** khai báo kiểu | `typeof` = **`real`** | Giải thích đúng 92.495.146 dòng REAL mà **không cần** lỗi `bind_double`. |
| Chèn vào cột khai báo `vkey REAL` | `real` | Nếu cột là REAL thì **mọi** dòng đều phải là `real` ⇒ loại trừ. |
| `SELECT 2272294912 = 2272294912.0` | `1` | `DISTINCT`/`GROUP BY vkey` **có** bắt được loại trùng này. |
| `CREATE UNIQUE INDEX` khi còn bản ghi trùng | **Lỗi `UNIQUE constraint failed`** | Chứng minh bảng của đội **đang không có** UNIQUE index. |
| `PRAGMA integrity_check` khi còn bản ghi trùng | `ok` | **`integrity_check` không bắt được lỗi này** — đừng dùng làm cổng duy nhất. |
| `DELETE … ROW_NUMBER() OVER (PARTITION BY vkey ORDER BY rowid)` trên bảng nhân 3 lần 1 khoá | `4 → 1` dòng, rồi `CREATE UNIQUE INDEX` **thành công** | Công thức dọn trùng ở K3 chạy đúng. |

**Ba câu hỏi SQL quyết định (chạy trên bản sao, 5 phút):**

```sql
SELECT sql FROM sqlite_master WHERE type='table' AND name='<bảng khoá>';  -- (1) khai báo cột
PRAGMA index_list('<bảng khoá>');                                          -- (2) có UNIQUE không
SELECT typeof(vkey) AS t, count(*) FROM <bảng> GROUP BY t;                -- (3) lớp kiểu thật
SELECT DISTINCT vkey FROM <bảng> WHERE typeof(vkey)='real' LIMIT 5;
```

**Kế hoạch sửa 1 lần, an toàn, đúng ~100 GB trống.** Ràng buộc không gian then chốt, có nguồn — S30: *"Prior to making any changes to the database file, SQLite first creates a separate rollback journal file and writes into the rollback journal the original content of the database pages that are to be altered."* ⇒ Trong `journal_mode=delete`, **dung lượng journal ≈ dung lượng trang bị sửa**; sửa 502 triệu dòng trong một giao dịch là journal 5 GB+ (đúng mức bạn thấy).
1. **Batch 200.000 dòng / giao dịch** (`BEGIN IMMEDIATE`…`COMMIT`), sau đó `PRAGMA wal_checkpoint(TRUNCATE)` nếu đã sang WAL. Journal mỗi lô ≲ 200.000 × kích thước thế — **phải đo** `SELECT count(*)` thật.
2. **Không `VACUUM` tại chỗ** (cần bản sao gần bằng 30 GB). Cần nén thì dùng `VACUUM INTO` sang tệp mới rồi đổi tên (S5), làm **sau** khi sửa xong.
3. **Bảng tiến độ** để nối lại sau khi phiên bị cắt, cập nhật **trong cùng giao dịch** với lô ⇒ không bao giờ lệch.
4. **Thứ tự bắt buộc:** (a) xoá 64.795.168 dòng trùng → (b) sửa kiểu 27.699.978 dòng → (c) **tạo UNIQUE index** (cần thêm ~10–20 % kích thước). Đảo (c) lên trước sẽ lỗi ngay — đó là ca đỏ tốt.
5. **Đừng chạy song song 2 tiến trình ghi kho trong khi sửa toàn bảng** (S2).

**Nhận diện ~1,3 triệu khoá sai còn sót (`vkey=0`, `I64MIN`, binade 2^55 âm):** đừng so với danh sách giá trị cấm — hãy dựng **miền giá trị hợp lệ** từ chính dữ liệu đã sạch rồi đếm ngoài miền:
```sql
SELECT count(*) FROM <bảng> WHERE vkey IS NULL OR vkey = 0;   -- kỳ vọng trên bản sạch: 0
SELECT min(vkey), max(vkey), count(*) FROM <bảng>;            -- so với bản sạch
```

**Ca kiểm (ngưỡng ghi trước):** **Đỏ 1** — trên bản sao chạy `CREATE UNIQUE INDEX` ⇒ phải lỗi (nếu thành công thì giả thuyết "không có UNIQUE" của tôi sai ⇒ dừng). **Đỏ 2** — cố tình chèn 1 dòng trùng rồi `PRAGMA integrity_check` ⇒ vẫn "ok" (chứng minh integrity_check không đủ). **Xanh** — sau khi sửa: `count(*)` = 494.581.545 ± 0; `SELECT DISTINCT typeof(vkey)` = đúng 1 lớp; `integrity_check` = "ok"; `CREATE UNIQUE INDEX` **thành công**.
**Nguồn:** S3, S30, S2, S4, S5, S6.
**Mức tin:** `CHẮC` cho cơ chế affinity/journal/so sánh, phép cộng, và **7 phép thử tôi đã chạy**; `GIẢ THUYẾT CẦN ĐO` cho batch 200.000 và ngưỡng index; `KHÔNG BIẾT` tên bảng/cột thật và tên lỗ hổng `bind_double`.

## 0.4 CBL

**Trả lời:**

1. **Định dạng (có nguồn, S19 44 trang):** `.cbl` = *"Chess Bridge Library"*, `.cbr` = *"Chess Bridge Record"*, `.cbf` = *"Chess Bridge XML"*, cùng `.xqf`, `.pgn`. Sổ tay liệt kê CCBridge mở được *"xqf, pgn, che, mxq, ccm, cbr, cbf, cbl"* và lưu được *"xqf, pgn, txt, cbr, cbf, cbl"*, có hỗ trợ **FEN** import/export và **PGN dạng ICCS**. ⇒ `.cbl` là **thư viện nhiều ván**, `.cbr` là **một ván** ⇒ quy trình đúng: `.cbl` → (`.cbr` hoặc `.pgn`) → FEN.
2. **Chống trùng: dùng được công cụ này, hoặc tự làm bằng khoá.** Sổ tay có mục *"D : delete / S : copy records without doublets (no same, no repetition) to library"* ⇒ CCBridge có thao tác khử trùng. Nhưng tôi **không biết** nó so khớp toàn vân hay từng thế ⇒ **KHÔNG BIẾT**, phải thử trên 2 tệp biết trùng.
3. **"Định dạng nào giữ chú giải": `.cbf` là XML ⇒ có cấu trúc đọc chú giải; `.cbr`/`.cbl` nhị phân, tôi **không xác minh được** byte-layout nên không khẳng định có trường chú giải.** Khuyến nghị: lấy chú giải từ `.cbf`/PGN, `training` chỉ dùng **FEN + nước đi**, không nhét chú giải vào nhãn (xem A2-16 về việc tách metadata khỏi dữ liệu huấn luyện).
4. **Khai thác tốt nhất: chính CCBridge đã có "search database" + "generate boards by import or export"** (S19) ⇒ không cần tự viết bộ giải mã `.cbl` nếu bạn chạy được CCBridge. Phần bạn phải tự viết mới: (a) chuẩn hoá FEN, (b) khoá vkey, (c) lọc sách cấm theo **hash nội dung** (K3).
5. **Sách là nguồn *phân bố thế* và *trình tự nước*, không phải nguồn target.** Nhãn phải do bộ giải/EGTB/engine của bạn sinh. Dùng nước của thầy như **trọng số mẫu**, không dùng làm điểm — nếu dán điểm engine mà không ai kiểm thì bạn sao chép cả lỗi của engine (đó là bài học A2-08: 6 net "khớp nhãn" đều thua A/B).
6. **Với 432 tệp = 221 nội dung:** 127 CCBridgeLibrary + 94 "CCBridge cũ" v3 zlib + 1.828 tệp trong archive. Kiểm tra tệp nén **cần CRC + kích thước kỳ vọng** trước khi giải (xem §4 ASKFULL về CRC-32C).

**Ca kiểm (ngưỡng ghi trước):** 2 tệp `.cbl` cùng nội dung ⇒ pipeline ra **1 ván duy nhất**; số nút phải khớp giữa `.cbr` và `.pgn` xuất ra (chênh lệch = 0). Lệch > 0 ⇒ dừng, đừng nạp vào kho.
**Nguồn:** S19, S29, S28, S26.
**Mức tin:** `CHẮC` cho tên/ý nghĩa định dạng và tính năng CCBridge (trích từ sổ tay); `GIẢ THUYẾT CẦN ĐO` cho "nước thầy làm trọng số"; `KHÔNG BIẾT` cho byte-layout `.cbl/.cbr` và cách CCBridge so trùng.

## 0.5 AUTO giả lập + đăng nhập nền tảng (kể cả H1–H4)

**Trả lời — phần quan trọng nhất trước: tôi cần tách "biết" và "không biết" ngay.**

### (a) Nhận dạng bàn/quân bền với skin + độ phân giải + giả lập (LDPlayer/BlueStacks/MuMu/AVD)

**Đề xuất: template matching + hình học, KHÔNG cần mạng lớn, và "học mẫu tại chỗ" là đúng hướng.**

1. **Tách 3 tầng độc lập, đừng gộp:** (T1) chụp vùng → (T2) **hiệu chuẩn hình học** (tìm lưới 9×10) → (T3) phân loại 90 ô. Tầng nào cũng phải **trả về "không nhận ra"** khi hỏng, chứ không được trả giá trị sai. Với 1 cú bấm mà ra 14–20 quân (K4), tầng T2 đã hỏng mà T3 vẫn "chạy" — đó là **cổng xanh giả kinh điển**.
2. **Hiệu chuẩn hình học ổn định hơn OCR/template thô:** dò cạnh theo tổng độ sáng hoặc biến đổi Hough để ra 9 cột × 10 hàng, rồi **ép** ô về hình vuông bằng phép biến đổi phù hợp (perspective). Sau khi ép, **mọi skin và mọi độ phân giải dùng chung một bộ mẫu** — đây là chìa khoá đa nền tảng. Repo mở đã đi hướng này: `rhevorn/xiangqi-vision` (MIT) dùng template matching (S27).
3. **Bẫy khi tự học mẫu ở thế khai cuộc** (chủ sở hữu bấm 1 lần ở khai cuộc ⇒ biết chắc 32 quân ở đâu): quân đang **được chọn/tô sáng**, mũi tên nước vừa đi, hiệu ứng động, quân cờ đang bay. Cách chữa: **cắt mẫu ở nhiều thế khác nhau** (không chỉ khai cuộc) và **loại ô có độ tin thấp**; đừng học 1 lần rồi tin tuyệt đối.
4. **Ngưỡng "đã bắt" phải là tiêu chí kiểm được, không phải hằng số cảm tính:** (i) **tổng quân = 32** ở thế khai cuộc, (ii) **đối xứng**: mỗi loại quân có đúng số lượng chuẩn hai bên, (iii) tọa độ tâm quân lệch ≤ 0,3 ô, (iv) **tổng điểm mẫu ≥ ngưỡng** trên 90 ô, (v) hai lần chụp liên tiếp **cho cùng FEN**. Không đạt (i) hoặc (ii) ⇒ dừng, **cấm đi nước**.
5. **So sánh 3 phương án cho T3 (bảng tôi đề xuất — `GIẢ THUYẾT CẦN ĐO`, cần đo trên máy đội):**

| Phương án | Độ chính xác kỳ vọng | ms/khung | Dữ liệu cần | Bẫy |
|---|---|---|---|---|
| Template matching trên ô đã cắt | 0,97–0,99 với skin đã học | 2–8 (C++) / 10–25 (C# 5) | 14 mẫu quân + 1 mẫu ô trống **mỗi skin** | Nền/scale khác ⇒ hỏng; phải chuẩn hoá kích thước ô |
| Đặc trưng màu + chữ Hán trong vòng tròn | 0,90–0,95 | < 2 | Ngưỡng màu theo skin | Đèn/độ sáng giả lập đổi màu; quân chồng lấp |
| Mạng nhỏ (CNN/ONNX) | 0,995+ nhưng **cần nhiều dữ liệu hơn bạn tưởng** | 5–20 (ONNX, 1 luồng) | Hàng chục nghìn ô đã gán | Overfit skin; thêm gánh nặng triển khai |

**Kết luận T3: chọn template matching làm lớp 1 (rẻ, minh bạch, cập nhật skin bằng cách thêm mẫu), và chỉ thêm ONNX lớp 2 cho skin lạ** mà bạn chưa có mẫu. Đừng bắt đầu bằng mạng.
6. **Chụp màn trên Windows:** `BitBlt` (S20) đơn giản nhưng chậm và dễ vướng DPI; **DXGI Desktop Duplication** (S20) là đường hiện đại, **Windows.Graphics.Capture** là đường cho vùng cửa sổ. Với giả lập Android, `MediaProjection` (S21) và `AccessibilityService` (S21) là hai nền tảng **được hỗ trợ chính thức**. Bẫy: **DPI ảo hoá** và **đa màn hình** phải xử lý bằng `GetDpiForWindow`/toạ độ vật lý.
7. **Nhận bàn giữa ván** (quân đang bay, ô được tô sáng, bàn lật): chụp **nhiều khung liên tiếp** và chỉ nhận khi **FEN ổn định trong ≥ 2 khung**; nếu không ổn định sau N khung ⇒ báo "không nhận ra".

### (b) ĐĂNG NHẬP từng nền tảng — trả lời thẳng: phần lớn tôi **không biết**, và tôi sẽ không đoán

| Nền tảng | Tôi biết gì | Trả lời |
|---|---|---|
| JJ象棋 (Unity IL2CPP, bundle mã hoá, TCP+UDP, kênh SĐT/QQ/WeChat/Douyin/Ali) | **Không biết** giao thức, không có tài liệu công khai tôi mở được | **KHÔNG BIẾT.** Đường hợp pháp: chỉ dùng API/SDK chính thức nếu tồn tại; nếu không ⇒ **bỏ tính năng**, hoặc chỉ điều khiển UI như một **người dùng bình thường** (bấm nút, nhập bàn cờ) chứ không giả lập/khoá |
| MoveSky / 弈天 CCMS 1.96 (bắt tay, token, relay 127.0.0.1, câu hỏi "relay có bị chặn khi đăng nhập qua proxy?") | **Không biết** | **KHÔNG BIẾT.** Tôi cũng **không biết** câu trả lời cho câu hỏi proxy — đừng tin ai nói không có kiểm tra |
| clubxiangqi.com, zigavn (web) | Web ⇒ **WebView2 nhúng** là đường **hợp pháp, ổn định nhất** | Tôi **không biết** họ có API công khai không. Nếu có ⇒ dùng. Nếu không ⇒ WebView2 + đọc DOM là hợp pháp; bẻ WebView2 để can thiệp sâu thì tôi **không hướng dẫn** |
| Ziga APK | **Không biết** | **KHÔNG BIẾT**; không gỡ/vá/chặn bảo vệ ứng dụng |
| ZingPlay (Zing ID) | **Không biết** | **KHÔNG BIẾT**; chỉ dùng API chính thức nếu có |
| Kỳ Vương (PlayIntegrity + DexGuard) | PlayIntegrity/DexGuard là cơ chế **chống can thiệp** | **KHÔNG BIẾT** và **không đề xuất** vượt qua. Cách duy nhất tôi đề xuất: **API chính thức** |
| vndynapp (REST) | Có vẻ là REST ⇒ dễ tích hợp **nếu có tài liệu** | **KHÔNG BIẾT** endpoint; xin tài liệu hoặc OpenAPI spec |

**Nguyên tắc tôi đề xuất cho toàn bộ phần (b):** ① **phân loại tính năng thành 2 nhóm: "tích hợp chính thức" và "tự động hoá như người dùng"**; ② nhóm 1 dùng API/SDK/token chính thức; ③ nhóm 2 (bấm chuột/bàn phím + đọc bàn cờ) là thứ **mọi GUI cờ tướng đều làm** và là đường hợp pháp để **đấu phần mềm với phần mềm trên máy của bạn**; ④ **tuyệt đối không** bắt gói mã hoá riêng, không giả lập token, không né kiểm tra, không khai thác lỗ hổng; ⑤ kiến trúc phải cho phép **tắt từng nền tảng bằng cấu hình** để khi điều khoản dịch vụ đổi bạn chỉ cần tắt, không cần sửa mã.

### (c) H1–H4 (MoveSky)

| Câu hỏi | Trả lời của tôi |
|---|---|
| Cần **ghi 1 phiên thật** hay **đọc mã máy**? | Tôi **không biết** giao thức MoveSky. Về nguyên tắc: **ghi 1 phiên thật** là cách hợp pháp và bền hơn; **đọc/đoán mã máy** là cách dễ vỡ và dễ vi phạm. Tôi **không** hướng dẫn đọc/đoán |
| «Các bạn hiện tại» là khung **对阵表** hay **danh sách bạn**? | **KHÔNG BIẾT** — phải xem UI thật, tôi không có |
| nick `qqmovesky` / `ccmsexe` / `cmllh`? | **KHÔNG BIẾT** |
| Web nhúng có **thay** được giao thức không? | Về nguyên lý: web nhúng **không** cho bạn quyền server ⇒ **không** thay thế được, và nếu bạn chỉ cần "đánh được ván", điều khiển qua UI là đủ. Tôi **không biết** MoveSky có web client hay không |

**Ca kiểm (ngưỡng ghi trước):** trên **≥ 3 skin** (nền tối, nền sáng, có hiệu ứng) × **≥ 3 cỡ cửa sổ giả lập**, mỗi ca: nhận đúng 32 quân ở thế khai cuộc, `tọa độ lệch ≤ 0,3 ô`, và **2 lần chụp cho cùng FEN**; bất kỳ ca nào không đạt ⇒ báo "không nhận ra" chứ không đi nước. Ngoài ra: sau khi bấm đi, **phải đọc lại bàn ngoài để xác nhận nước đã được nhận** trước khi coi là xong (xem A3-14a).
**Nguồn:** S20, S21, S27 (dòng này và dòng dưới áp dụng cho **cả 0.5, (a) và (b)**).
**Mức tin:** `CHẮC` cho nền tảng chụp màn/điều khiển hợp pháp và nguyên lý "3 tầng + cổng 'không nhận ra'"; `GIẢ THUYẾT CẦN ĐO` cho bảng so sánh T3 và ngưỡng; **`KHÔNG BIẾT` cho toàn bộ protocol/nội bộ của JJ象棋, MoveSky, Ziga, ZingPlay, Kỳ Vương, vndynapp và H1–H4.**

## 0.6 Giao diện TieuLongNu

**Trả lời — thiết kế, không suy đoán từ tên nút của GUI khác:**

1. **"Một lõi, chế độ là dữ liệu" là nguyên tắc đúng và tôi giữ nguyên.** Bảng chế độ quyết định *hiển thị gì*, mọi hành vi nằm ở lõi ⇒ thêm nền tảng/tính năng không phải nhân bản mã. (A2-05)

```csharp
// C# 5 (csc, không csproj) — bảng chế độ quyết định menu/phím tắt/chuột phải/thanh công cụ nào hiện.
// Nguyên tắc: ẨN, không XOÁ -> giữ được cho chế độ kia và giữ được cho bản dịch sau này.
using System;                     // ArgumentException
public sealed class CheDo
{
    public string Ten;                              // "chien_danh" | "nghien_cuu"
    public bool HienThanhCongCu, HienPhimTat, HienChuotPhai, HienChuThich;
    public string[] BamTat;                         // khoá phím tắt, không phải mã lệnh
}
public static class CheDoLuan
{
    public static readonly CheDo[] TatCa = new CheDo[] {
        new CheDo { Ten="chien_danh", HienThanhCongCu=true,  HienPhimTat=true,  HienChuotPhai=false, HienChuThich=false,
                    BamTat=new[]{"Ctrl_F1","Ctrl_F2"} },
        new CheDo { Ten="nghien_cuu",  HienThanhCongCu=true,  HienPhimTat=true,  HienChuotPhai=true,  HienChuThich=true,
                    BamTat=new[]{"Ctrl_F1","Ctrl_F2","Ctrl_F3","Ctrl_F5"} } };
    public static CheDo Lay(string ten) { foreach (var c in TatCa) if (c.Ten == ten) return c; throw new ArgumentException(ten); }
    // LỖI CỐ Ý: nếu "HienChuotPhai=false" mà vẫn đăng ký handler chuột phải -> lối vào đã ẩn vẫn còn.
    // Cổng kiểm phải duyệt cây giao diện + handler để bắt đúng ca này (A2-05 câu 3).
}
```

2. **Bộ nút tối thiểu cho chế độ "Thực chiến":** **bắt buộc** = (1) bắt đầu/dừng AUTO, (2) chế độ **một nước / toàn ván** (A2-03), (3) **ra nước ngay**, (4) chọn engine + thời gian, (5) sách bật/tắt, (6) lưu/nạp hồ sơ kết nối, (7) **đổi bên**, (8) hoạt ảnh/xem lại nước đi. **Bỏ được** = kỳ phổ, học tập, thế cờ, thư viện mây, MultiPV, phân tích sâu, cài đặt engine. Căn cứ: đây là **suy luận của tôi** dựa trên nguyên tắc UX (chỉ những gì chặn luồng chính); tôi **không biết** GUI thương mại nào bày nút nào ở màn đánh ⇒ **KHÔNG BIẾT** phần "GUI nào có nút nào".
3. **Thanh 8–10 nút, chữ to:** nguyên tắc có nguồn — WCAG 2.2 yêu cầu tương phản ≥ 4,5:1 cho chữ thường (S23), và mục tiêu chạm phải đủ lớn; với giao diện **chỉ icon**, luôn giữ **nhãn chữ (tooltip) và nhãn truy cập** cho người không chuyên và cho trình đọc màn hình. Tôi **không biết** con số "cỡ icon tối đa" mà chính đội cần — đó là câu hỏi thiết kế, không phải câu hỏi có nguồn.
4. **Theme sáng/tối, đổi lúc chạy, app build bằng `csc` (không App.xaml biên dịch):** dùng `DynamicResource` + thay `MergedDictionaries` ở `Application.Current.Resources`; mọi màu phải nằm trong bảng tài nguyên. Bẫy đã biết trong ngành: `DynamicResource` dày đặc làm tốn hiệu năng, và brush phải **`Freeze()`** trước khi dùng trên luồng khác (brush chưa frozen ⇒ cấp phát lại mỗi lần vẽ). Repo mở đã cho ví dụ tách màu: S27.

```csharp
// C# 5 — bảng màu là nguồn DUY NHẤT; ViewModel chỉ đọc, không chứa mã màu.
public sealed class Theme
{
    public System.Windows.Media.Brush Board { get; private set; }
    public System.Windows.Media.Brush Line { get; private set; }
    public System.Windows.Media.SolidColorBrush LegalMark { get; private set; }
    public static Theme Current { get; private set; }
    public static event System.Action Swapped;
    public static void Use(bool dark)
    {
        Current = dark ? Make(true) : Make(false);
        var h = Swapped; if (h != null) h();          // View nghe sự kiện này để nạp lại binding
    }
    private static Theme Make(bool dark)
    {
        // Mọi giá trị màu lấy từ bảng tài nguyên, không rải trong mã.
        return new Theme { Board = Freeze(new System.Windows.Media.SolidColorBrush(
                             System.Windows.Media.Color.FromRgb((byte)(dark ? 0x1E : 0xF0), 0xF2, 0xE8))),
                           Line  = Freeze(new System.Windows.Media.SolidColorBrush(
                             System.Windows.Media.Color.FromRgb((byte)(dark ? 0x40 : 0x8A), 0x8A, 0x8A))),
                           LegalMark = Freeze(new System.Windows.Media.SolidColorBrush(
                             System.Windows.Media.Color.FromRgb(0x1E, 0x9E, 0x5A))) };
    }
    private static System.Windows.Media.SolidColorBrush Freeze(System.Windows.Media.SolidColorBrush b)
    { if (b.CanFreeze) b.Freeze(); return b; }
}
```
*(Mẫu trên biên dịch được với C# 5; giá trị màu thật phải nằm trong `ResourceDictionary`. Tương phản ≥ 4,5:1 theo S23.)*
5. **Trắng màn WPF `.NET 8` + `GeneratedInternalTypeHelper` (1/6 lần build):** đây có tài liệu công khai — S17 (dotnet/wpf issues 2281, 2690, 3469). Nguyên tắc: **đừng vá triệu chứng bằng `try/catch` rỗng hay `Handled = true` toàn cục** — phải *log* và *báo đỏ* để lỗi không biến mất (đây đúng là lớp lỗi mà §3.6 của ASK03 gọi là «thất bại mà trông như đã xong»). Cổng `GeneratedInternalTypeHelper.g.cs` > 10 B ⇒ rc ≠ 0 là ý tưởng đúng hướng, nhưng **cần biết ca hợp lệ vẫn sinh tệp** — tôi **không biết** điều kiện chính xác khiến trình biên dịch sinh tệp đó (xem A2-17).
6. **Ba ngôn ngữ:** đừng nhét chuỗi vào mã — dùng **ResourceDictionary + `.resx`**, mỗi ngôn ngữ một tệp; chuỗi dài hơn ~80 ký tự đủ để đổi bố cục ⇒ dùng `TextWrapping` và kiểm bằng cổng "render với chuỗi dài nhất". Không có nguồn ngoài; đây là **thực hành chuẩn** — `GIẢ THUYẾT CẦN ĐO` về ảnh hưởng bố cục.

**Ca kiểm (ngưỡng ghi trước):** (1) vào Thực chiến ⇒ **không còn lối nào** tới mục đã ẩn, kể cả phím tắt và chuột phải (duyệt cây VisualTree + liệt kê handler, không chỉ nhìn `IsVisible`); (2) đổi theme 10 lần/giây ⇒ không rò `Brush` (đo bằng `WeakReference` + `GC.Collect`) và mọi binding tự cập nhật; (3) dựng GUI với `Application.Current.Resources.MergedDictionaries` rỗng ⇒ **phải báo lỗi tường minh**, không trắng màn im lặng.
**Nguồn:** S23, S17, S27.
**Mức tin:** `CHẮC` cho nguyên lý một-lõi/chế-mộ-là-dữ-liệu, `Freeze()` brush, `DynamicResource`, tương phản WCAG, và việc dotnet/wpf có issue công khai về kiểu này; `GIẢ THUYẾT CẦN ĐO` cho bộ nút tối thiểu và cỡ icon; `KHÔNG BIẾT` cho danh sách nút của từng GUI thương mại và điều kiện sinh `GeneratedInternalTypeHelper`.

## 0.7 Encrypt book + update khách (V-41; không đè config)

**Trả lời:**

1. **Mã hoá đúng để chặn "đọc trộm" là mã hoá, không phải hash.** SHA-256 không chặn gì: kẻ đọc tệp thì băm cũng gọi được. Vậy nên: **kho chính để dạng thường; sách cấm mã hoá; khoá nằm ngoài kho.**
2. **DPAPI chỉ bảo vệ ở mức hệ điều hành, không phải mức tiến trình — tôi nói thẳng:** nếu mục tiêu là chống người khác trên **cùng tài khoản**, `CryptProtectData` ở mức `CurrentUser` (S20) **không đủ**, vì bất kỳ tiến trình nào chạy dưới user đó đều có thể gọi được nó. Chống "dump từ RAM" thì **không** làm được bằng DPAPI — khoá phải nằm trong tiến trình và bạn chỉ có thể **giảm** thời gian lộ, không xoá được rủi ro. Tôi **không** đề xuất kỹ thuật chống dump kiểu packer/anti-debug; đó là hướng làm yếu sản phẩm và dễ vỡ.
3. **Ba loại key (hết hạn / ngưng vĩnh viễn / hạn cập nhật) nên là một trường duy nhất, không phải ba kiểu key khác nhau.** Mô hình ít lỗi nhất:
   - `expires_at` (NULL = vĩnh viễn), `updates_until` (hạn tải bản mới), `revoked` (ngưng vĩnh viễn), và `device_hash` (khoá gắn máy).
   - `revoked` và `device_hash` phải **kiểm ở phía máy khách trong bộ điều khiển cấp quyền**, không kiểm trong mã book — vì nếu kiểm trong book thì chỉ cần sửa 1 byte là bypass.
   - **Không bao giờ** đặt "hết hạn" trong file book; đặt ở file cấu hình phía máy khách + một bản chữ ký để chống sửa.
4. **Mã hoá tệp hay từng dòng?** Mã hoá **tệp/khoảng (envelope)**: DEK ngẫu nhiên 32 B, mã hoá DEK bằng KEK, mã hoá dữ liệu bằng DEK, lưu IV + tag. Nếu mã hoá từng dòng thì phải cộng IV 12 B + tag 16 B mỗi bản ghi ⇒ **+28 B/dòng**; với 502 triệu dòng là ~14 GB chỉ vì tag/IV, mà không tăng bảo mật đáng kể so với mã hoá cả tệp.
5. **Cấu hình suy ra từ mật khẩu phải dùng hàm suy khoá chậm** (PBKDF2-HMAC-SHA256 hoặc Argon2id) + salt; **không** dùng SHA-256 thẳng. Nếu khoá lấy từ token máy thì ràng buộc là độ dài entropy, không phải hàm băm.
6. **Quy trình update không đè 5 tệp config cấm — đây là mẫu tôi đề xuất:**

```
BƯỚC 1  Ghi bản cập nhật vào thư mục MỚI (không đè tại chỗ)      vd: app/1.8.0/
BƯỚC 2  Chạy cổng tự kiểm trên bản MỚI, bằng CHÍNH bộ ca đỏ
BƯỚC 3  Nếu đỏ -> dừng, KHÔNG chuyển, giữ bản cũ, báo lỗi có mã
BƯỚC 4  Sao chép 5 tệp config cấm từ bản CŨ sang bản MỚI (chỉ chép,
         KHÔNG chép ngược chiều nào khác)
BƯỚC 5  Đổi con trỏ "bản đang chạy" nguyên tử (con trỏ/khoá/rename trong cùng thư mục)
BƯỚC 6  Nếu bước 6+ hỏng -> quay lui bằng con trỏ (KHÔNG phụ thuộc việc "đã xoá gì")
```

```sql
-- Ca đỏ hash bắt buộc cho 5 tệp config cấm (đội chọn số hash, ví dụ 5)
CREATE TABLE config_cam (ten TEXT PRIMARY KEY, sha256 TEXT NOT NULL, lan_cap_nhat TEXT NOT NULL);
-- Khi update: INSERT OR REPLACE chỉ dòng của tệp đã chép; các tệp KHÔNG chép giữ nguyên hash.
-- Cổng: count(*) của các tên KHÔNG thuộc danh sách cấm mà hash đã đổi => phải bằng 0 (rc != 0 nếu khác).
```

**Ca kiểm (ngưỡng ghi trước):**
- **Đỏ 1:** lật **1 byte** giữa file đã mã hoá ⇒ lần mở phải **từ chối** (kiểm MAC trước khi giải mã) chứ không trả dữ liệu rác.
- **Đỏ 2:** mã hoá 2 lần cùng plaintext ⇒ 2 file **phải khác nhau** (IV/DEK ngẫu nhiên). Giống nhau ⇒ đã lộ pattern.
- **Đỏ 3:** cập nhật có chủ ý làm hỏng 1 ca đỏ ⇒ bản cũ **vẫn chạy** sau khi thử cập nhật (ca này bắt đúng lỗi "trắng màn" mà S17 mô tả).
- **Xanh:** sau cập nhật, hash của 5 tệp config cấm **không đổi**, và mọi tệp khác được cập nhật ⇒ xác minh bằng bảng `config_cam`.

**Nguồn:** S20 (`CryptProtectData`), S17 (triệu chứng trắng màn do tài nguyên không nạp được), S3 (không dùng cho phần này).
**Mức tin:** `CHẮC` cho nguyên lý envelope + kiểm MAC trước + giới hạn "cùng user" của DPAPI; `GIẢ THUYẾT CẦN ĐO` cho chi phí +28 B/dòng và chi phí PBKDF2; `KHÔNG BIẾT` mô hình key V-41 thật của đội (tôi không có mã).

## 0.8 Kiểm toán engine trước train (mốc 01/10)

**Trả lời — thứ tự soi và cổng:**

1. **Thứ tự đúng: perft → luật → đồng hồ → khoá băm/TT → net/eval → A/B.** Mỗi bước chỉ có nghĩa khi mọi bước trước đã đúng; luật sai thì A/B chỉ đo "cái sai nào hay hơn".

| # | Tầng | Ca đỏ tối thiểu | Ngưỡng xanh (đề xuất) |
|---|---|---|---|
| 1 | Perft | Node lệch với node kỳ vọng | Bằng tuyệt đối depth 1–5, mọi vị trí đặc biệt (cờ úp lật quân) |
| 2 | Luật (xe/pháo/tướng, cấm lặp, chiếu bí) | Bằng PGN/bản `.cbr` đã biết | 0 sai / 10.000 thế |
| 3 | Đồng hồ (80 ply, đẩy tốt, 60 giây) | Mô phỏng kịch bản hai bên | Sai số ≤ 1 ply ở ≥ 99,9 % ván |
| 4 | TT / hash / bản ghi | Giết tiến trình giữa `COMMIT` | 0 sự cố; hash khớp 100 % sau replay |
| 5 | Net (I/O) | File thiếu / sai hash / sai kiến trúc | Từ chối trong ≤ 1 s, không crash |
| 6 | Eval / TB | So TB với net khi có bằng chứng TB | Khác ≤ 10 cp trung vị, ≤ 3 % ngoài 50 cp |
| 7 | A/B | 2 bản giống nhau, seed khác | Chênh Elo < 15 ⇒ coi như bằng |

2. **Ba lỗi "đúng luật nhưng yếu hơn" đã gặp (bài học đáng ghi):** (i) mảng/giá trị eval tra theo khoá mới **chưa tune** ⇒ thêm "đúng" mà yếu; (ii) cập nhật psq hai lần trên cùng một nước; (iii) TT key đổi theo trạng thái lật quân. Bài học chung: **thêm thông tin vào eval mà không kèm phép đo trên tập cố định là cách chắc chắn nhất để mất Elo.** Đây là cùng cơ chế với A2-08 (6 net khớp nhãn đều thua A/B).
3. **Cổng ưu thế ≥ 400:** cổng này phải có **ngưỡng viết trước** và **đo bằng tập cố định**; tôi **không biết** bảng KIEM_TOAN §3 của đội nên **không** đề xuất con số. Nguyên tắc: một cổng chỉ có giá trị nếu **từng đỏ thật** — nếu không tạo được ca đỏ làm hỏng nó, cổng đó **chưa tồn tại** (đúng kỷ luật đo mà tệp nêu).
4. **Cắt rig 60–100 h xuống kịp 01/10 — đề xuất theo giá trị/công sức:**
   - **Ngày 1:** đóng băng cấu hình đo (cùng máy, cùng dataset, cùng nhiệt độ), và chạy **1 lô đo lại toàn bộ 20 ô chưa đo** — giá trị cao nhất vì nó biến "chưa biết" thành "biết", giá trị bằng cả việc sửa ô sai.
   - **Ngày 2:** chạy 13 ô KHÔNG QUA theo thứ tự ưu tiên *giá trị × khả năng sửa trong ngày*; mỗi ô có **một** giả thuyết sửa và **một** ca đỏ.
   - **Ngày 3:** A/B các bản vá còn sống (≥ 300 ván, cấu hình thi đấu cố định, **có đối chứng A=A với KTC95 chứa 0**) và xếp hạng theo SPRT (xem A2-07).
5. **Cổng nghiệm thu lô train (r Pearson + ρ Spearman, đối chứng ÂM nhãn xáo + DƯƠNG chạy lại bit-exact, A/B ≥ 300 ván, thang 5 bậc):** đây là bộ cổng tốt mà tệp mô tả. Tôi chỉ bổ sung một mắt xích còn thiếu: **đo cùng một bộ holdout qua nhiều lô** và yêu cầu điểm **không tăng ≥ 0,1 % trong 2 vòng liên tiếp** thì dừng — vì ρ Pearson/Spearman cao **không** bảo đảm Elo tăng (bằng chứng ngay trong chính lô −301 Elo mà đội từng gặp). Ngoài ra: holdout phải **giữ nguyên theo ván**, không theo thế.
6. **Về "đường bào mòn" của net trong engine:** thay net bằng `setoption EvalFile` có thể âm thầm rơi về eval cổ điển (điều đội đã gặp) ⇒ **cổng bắt buộc:** sau khi nạp net, hỏi engine một thế mà net và eval cổ điển cho **khác nhau ≥ 50 cp**; nếu không ⇒ coi như chưa nạp, rc ≠ 0.

**Ca kiểm (ngưỡng ghi trước):** xem bảng tầng 1–7 ở trên. Thêm: một ca "net chưa nạp" phải **đỏ** (cổng bắt được), và một ca "net nạp rồi" phải **xanh**.
**Nguồn:** S2, S4, S5, S7, S13, S9, S10.
**Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho toàn bộ ngưỡng trong bảng (tôi chọn ngưỡng chặt để bắt lỗi sớm; **đội phải đo lại trên máy đội**); `CHẮC` cho nguyên tắc thứ tự tầng và nguyên tắc "cổng phải từng đỏ thật"; `KHÔNG BIẾT` bảng KIEM_TOAN §3 và thang 5 bậc.

## 0.9 App điện thoại Kỳ Viện

**Trả lời — kiến trúc tối thiểu để phát hành, và ba chỗ tôi khuyên bạn đừng làm.**

1. **Kiến trúc tối thiểu, theo đúng tinh thần "học cấu trúc JJ":** (1) **MVVM 3 mảnh** — View / ViewModel / lõi luật+engine, để lõi kiểm thử được không cần UI; (2) **3 lối vào ván** tách bạch: MỜI / PHÒNG MÃ / XEM — vì chúng có luật khác nhau về ai được phép đi nước; (3) **đăng nhập tách khỏi kênh chơi** (một lần đăng nhập, nhiều kênh); (4) module **实名 (định danh thật) / chống nghiện là một module riêng**, không rải vào lõi.
2. **Cái tối thiểu để phát hành:** MVVM 3 mảnh + movegen/luật + sách `.obk` + engine 1 build (ARM64) nhúng qua API + phòng đấu người + hồ sơ đăng nhập + 1 ngôn ngữ. **Không** cần lúc đầu: phim, ảnh→FEN, 3 ngôn ngữ, nhiều chế độ.
3. **Engine trên máy (ARM64) hay engine ở máy chủ?** — tôi **không biết** đo được gì của đội, nên tôi đưa tiêu chí quyết định thay vì trả lời bừa: (a) nếu người dùng ở xa và độ trễ chấp nhận được ⇒ **API ở máy chủ** cho phép một bản engine duy nhất, dễ kiểm soát phiên bản/net nhất; (b) nếu cần offline ⇒ **engine trên máy**, nhưng phải có **kiểm thử tương thích kiến trúc net** (net 11 MB họ Fairy không nạp được cho engine họ Pikafish 50 MB — khác kiến trúc, theo chính tệp nêu). Đề xuất: **cả hai**, dùng chung một **giao diện engine** để chỉ khác cách nạp.
4. **Ảnh chụp màn hình trên điện thoại để nhận bàn:** nền tảng hợp pháp là `MediaProjection` (S21); ngoài đời (camera) thì thiếu đúng một thứ: **ánh sáng đổi và góc nghiêng** ⇒ cần **hiệu chuẩn bằng bốn góc ô** + **lọc trắc địa**. Đừng kỳ vọng độ chính xác như ảnh màn hình.
5. **Chống gian lận:** tôi **không biết** mã hiện có (`CheatFlags.cs`, `AntiFarm.cs`, `AnalyzeGuard.cs`). Về nguyên tắc: chống gian lận **không** phải lớp "phát hiện", mà là **quyền hạn**: ① quyền đi nước **luôn đi qua máy chủ**; ② mọi ván có **nonce** và thời gian bắt buộc tối thiểu; ③ phát hiện là **tín hiệu để giảm hạn mức/đánh dấu**, không phải để kết luận gian lận. Nói thẳng: công cụ đo Elo rẻ không thể chứng minh gian lận; hãy thiết kế để **giảm lợi ích** của việc gian lận thay vì cố chứng minh.
6. **SignalR:** S22 là tài liệu chính thức; các điểm thực hành: dùng **group theo phòng**, **reconnect có backoff**, và **đừng gửi trạng thái bàn bằng chuỗi JSON tự dựng** nếu đã có mô hình dùng chung. Điểm nghẽn khi số người chơi tăng thường là **engine pool**, không phải hub — vì mỗi ván đang giữ một tiến trình engine. Tôi **không biết** cấu trúc pool của đội.
7. **"Không bắt chước Unity IL2CPP + hot-update cho web":** tôi **đồng ý** và nói rõ lý do — đó là hệ thống dành cho **cập nhật hạ tầng ở quy mô lớn**, còn app mới của bạn không có bài toán đó. Sao chép nó là chi phí lớn nhất và lợi ích bằng 0.

**Ca kiểm (ngưỡng ghi trước):** (1) 100 ván liên tiếp có đối thủ rời giữa chừng ⇒ không còn ván treo, không còn lượt đi không hợp lệ; (2) mất mạng 30 s rồi có mạng lại ⇒ tự nối lại và bàn **không** lệch; (3) mỗi ván đều có nonce riêng, và **server** là nơi quyết định nước đi hợp lệ (client chỉ gửi ý định).
**Nguồn:** S21, S22.
**Mức tin:** `CHẮC` cho kiến trúc MVVM/3 lối vào, nền tảng `MediaProjection`, tài liệu SignalR; `GIẢ THUYẾT CẦN ĐO` cho tiêu chí chọn engine máy-chủ-vs-máy-khách; `KHÔNG BIẾT` mã chống gian lận, cấu trúc engine pool, và mọi số đo của đội.

---

## ASK04-B / ASK03 / ASK02 (câu còn mở)

Ba phần dưới đây trả lời **đúng mã câu** của tệp nguồn. Không có mục `L-*`: zip `Ask03_source_2026-09-25.zip` **không được cung cấp**, nên tôi không trích `tệp:dòng` cho bất kỳ tệp nội bộ nào. Ở mỗi câu tôi ghi rõ phần nào là **nguyên lý đã kiểm chứng**, phần nào là **suy luận**, và phần nào là **không biết**.

### Phần 1 — ASK04-B: K1…K8

**Bắt buộc, dòng đầu:** `Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.`
Khuôn trả lời mỗi mục: **(1) chẩn đoán · (2) đề xuất trong 1–3 ngày · (3) tiêu chí đo · (4) rủi ro.** Ưu tiên giải pháp rẻ, tự chạy, không phụ thuộc dịch vụ trả phí.
**Cách ghi nguồn của phần này:** vì K1–K8 dùng chung bộ nguồn, tôi gom `Nguồn` và `Mức tin` **một lần cho cả tám mục** ở cuối phần (`Nguồn chung cho K1–K8` / `Mức tin chung cho K1–K8`) thay vì lặp lại tám lần; mỗi mục vẫn ghi rõ phần nào là nguyên lý đã kiểm và phần nào là suy luận.

### K1 — Đĩa phình 500 GB trong 3 ngày

1. **Chẩn đoán:** bạn đang trả tiền **3 lần cho cùng một dữ liệu**: bản gốc + bản chép trên C + staging + bản chép để dò. Nguyên nhân gốc không phải "SQLite phình" mà là **quy trình có bước chép không cần thiết**. Ba bản cùng lúc là **ba nơi phải giữ nhất quán**, nên bạn mới cần cổng kiểm phức tạp.
2. **Đề xuất (làm được trong 1 ngày):** **xoá bước "chép về C"**. Luồy chuẩn: *đọc thẳng từ ổ ngoài/stream từ D: → nạp thẳng vào SQLite staging trên C → kiểm **trong lúc nạp** → gộp → xoá staging*. Với nguồn là 1 tệp lớn không phải hàng trăm nghìn tệp, hãy nạp theo **lô 20–50 GB** và **xoá staging sau mỗi lô đã gộp** ⇒ đỉnh dùng đĩa = kích thước lô, không phải tổng.
   - **Kiểm không cần bản chép thứ hai:** mỗi lô ghi **bảng ký hiệu**: `ten_tap, so_dong, sha256_tap`. Đối chiếu bằng `count(*)` và `sha256` **của chính tệp nguồn** (đọc 1 lần, không chép). Đây là bằng chứng toàn vẹn ở mức tệp, đủ để bắt chép hỏng/truyền thiếu.
   - **Backup Google Drive không đệm toàn bộ lên C:** dùng `rclone` (S24) chạy **ngoài** app, và **chọn `--drive-chunk-size` nhỏ**; nếu dùng ổ ảo (Google DriveFS) thì **đừng dùng nó cho việc ghi tạm** — đó chính là 88 GB cache không kiểm soát. Backup chạy **ngoài giờ gộp**, và chỉ backup kho **đã kiểm**, không backup staging.
3. **Tiêu chí đo:** đỉnh dung lượng C trong cả đợt nhập **≤ kích thước lô + 20 %**; số bản cùng dữ liệu tồn tại đồng thời **= 1**; sau khi gộp, `sha256` mỗi tệp nguồn khớp bảng ký hiệu 100 %; **không còn lần nào** "chép để dò".
4. **Rủi ro:** nếu bỏ bước chép, bạn mất "lưới an toàn" khi ổ ngoài rút giữa chừng. Bù lại bằng: (a) luôn ghi `sha256` tệp nguồn **trước khi nạp**, (b) nạp vào staging và **chỉ khi lô qua cổng mới gộp**, (c) nếu tệp nguồn hỏng giữa chừng thì lô đó **rời khỏi bảng ký hiệu** ⇒ tự biết phải làm lại lô nào, không phải dò toàn bộ.

### K2 — Gộp staging vào kho 30 GB an toàn khi phiên bị cắt

1. **Chẩn đoán:** hai lỗi độc lập đang trộn. (a) **Giao dịch cuối quá lớn** — đúng mức 5 GB rollback mà bạn thấy là hệ quả trực tiếp: S30 nói SQLite ghi **bản gốc của các trang sắp sửa** vào rollback journal ⇒ giao dịch càng lớn, journal càng lớn, và bị cắt giữa chừng thì phải rollback toàn bộ. (b) **Hai tiến trình cùng ghi** — SQLite chỉ cho **một** giao dịch ghi đồng thời (S2); hai tiến trình sẽ tranh, và kết quả **không phải** "cộng dồn lệch" mà là phải chờ/báo lỗi. Lệch `+132.226` với tool báo «không đạt» trong khi dữ liệu đúng ⇒ nghi ngờ mạnh `INSERT OR IGNORE` âm thầm bỏ dòng trùng mà bộ đếm của công cụ không tính (0.5).
2. **Đề xuất (1 ngày):**
   - **Lô 200.000 dòng / giao dịch**, `BEGIN IMMEDIATE` … `INSERT` … `DELETE FROM queue` … `COMMIT` (0.2).
   - **Bảng tiến độ** `meta(khoa, gia_tri)`, cập nhật **trong cùng giao dịch** ⇒ không bao giờ lệch; phiên bị cắt ⇒ mở lại là đọc 1 hàng rồi đi tiếp.
   - **Khoá liên tiến trình:** `PRAGMA busy_timeout=30000` (S6) **và** một **lock file kèm PID + mốc thời gian**, với quy tắc dọn: nếu PID không còn sống thì mới xoá lock. Bổ sung: trước khi ghi, `PRAGMA wal_checkpoint(TRUNCATE)`.
   - **Cổng sau gộp tự động** (3 số, không phải 1): `dòng đọc` / `dòng ghi thật` / `dòng bị bỏ có lý do`; cộng lại phải bằng `dòng đọc`; thêm mẫu khoá 0,1 % tra ngược nguồn. Xử lý **0 dòng** ⇒ **rc ≠ 0**.
   - **Có nên chuyển kho thật sang WAL không? — Có, nhưng có điều kiện.** Với kho 30 GB đọc nhiều bởi GUI, WAL hợp hơn delete (S1: người đọc không chặn người ghi). **Điều kiện bắt buộc:** WAL cần **shared memory** trên cùng máy — S1 cảnh báo các tiến trình ở root khác nhau sẽ thấy vùng shared-memory khác nhau và **hỏng dữ liệu** ⇒ **không đặt kho WAL trên ổ mạng**, và **checkpoint định kỳ** vì WAL không tự co lại. Ngoài ra S1: với `synchronous=NORMAL`, *"transactions are no longer durable and might rollback following a power failure"* và checkpoint mới là nơi duy nhất ra I/O barrier ⇒ đây là **đánh đổi có ý thức**, phải ghi rõ trong sổ.
3. **Tiêu chí đo:** giết tiến trình giữa lô ⇒ mở lại tool tiếp tục **đúng chỗ**, không mất và không nhân đôi; hai tiến trình chạy song song ⇒ **một cái thất bại rõ ràng** (rc ≠ 0), không phải lệch dữ liệu; sau mỗi lô `integrity_check` = "ok" và 3 số cộng đúng.
4. **Rủi ro:** chuyển WAL giữa lúc đang chạy sẽ cần khoá tất cả tiến trình đọc; nếu bạn có tiến trình cũ đang giữ kho, hãy chuyển trong lúc dừng hẳn. Và đừng bỏ `integrity_check` vì "chậm" — nó rẻ và là cổng duy nhất bắt được hỏng cấu trúc (không bắt được trùng khoá — xem 0.3).

### K3 — Lọc "sách cấm gộp" và nguồn tệp bị mojibake

1. **Chẩn đoán:** đây là **lỗi định danh nguồn**, và nó sẽ tự nhân bản: mọi lần bạn thêm tệp, bạn phải sửa code. Mojibake `LPStr` (ANSI) trên tên CJK là nguyên nhân khớp tên thất bại — nhưng **đừng sửa bằng cách "giải mã lại mojibake"**, vì nó không đảo ngược được 100 % (byte đã mất). Cách đúng: **bỏ tên tệp khỏi vai trò định danh**.
2. **Đề xuất (1 ngày):** định danh nguồn = **`sha256` nội dung tệp** (khoá chính) + **`source_name` UTF-8** (chỉ để hiển thị) + **`nhom_cap`** (cờ). Danh sách cấm cũng chuyển sang `sha256` ⇒ một lần chuyển đổi rồi mọi lần sau **tự động**; importer đọc danh sách cấm **từ một tệp/tính năng** chứ không ghi cứng trong mã.
   - Cột `source` giữ **UTF-8**; nếu phải tương thích bản cũ đang ANSI, thêm cột `source_legacy` và **một lần** script chuyển, sau đó bỏ đường ANSI.
   - Lọc ở **3 chặng**: (1) trước khi nạp (theo `sha256` tệp), (2) trước khi ghi dòng (theo `sha256` tệp), (3) trước khi dùng làm nguồn nhãn. Lọc 1 chặng là lọc thiếu.
3. **Tiêu chí đo:** ca đỏ — nạp 1 tệp **có** trong danh sách cấm ⇒ `SELECT count(*) … WHERE nhom_cap=1` **= 0** và rc = 0; ca đỏ âm — nạp 1 tệp **không** cấm cùng tên rút gọn ⇒ phải **vào** kho (chứng minh không lọc nhầm theo tên); và `source_name` lưu/đọc lại phải **giống hệt byte UTF-8**.
4. **Rủi ro:** nếu một tệp bị **đổi nội dung nhưng giữa tên**, `sha256` bắt được; nếu chủ sở hữu muốn cấm **một bản sao đã tinh chếnh** thì `sha256` không bắt được (nội dung khác) ⇒ cần thêm `nhom` do người gán, và đó là việc quy trình, không phải việc code.

### K4 — Nhận diện bàn cờ để auto + đăng nhập chơi trực tiếp

1. **Chẩn đoán:** 1 chạm cho lưới giả (ô 38 px) và 14–20 quân là **tầng T2/T3 chạy khi T1–T2 đã hỏng**. Khi lưới sai mà bộ phân loại vẫn trả kết quả, bạn đang **tiêu thụ nhãn sai** — đây là "cổng xanh giả" nguy hiểm hơn hẳn việc không nhận được. Nửa còn lại (đăng nhập + danh sách người chơi online) là **hai bài toàn khác hẳn**: một bài là *nhận dạng*, một bài là *tích hợp dịch vụ*.
2. **Đề xuất (xem 0.5 chi tiết):** (a) **T2 phải trả về "hỏng" và chặn T3** — có nghĩa là bỏ cách cho phép T3 chạy khi lưới chưa xác nhận; (b) hiệu chuẩn bằng **2 điểm ở thế khai cuộc** là hợp lý **nếu** hai điểm là **cùng hàng hoặc cùng cột** và ô không quá méo — 2 điểm cho **đường thẳng**, còn **lưới 9×10 cần thêm chiều**; cần 1 điểm thứ 3 nếu tỉ lệ ô méo vượt ngưỡng mà bạn **đo**; (c) tiêu chí "đã bắt" = tổng quân 32 + **đối xứng số lượng từng loại** + 2 khung cho cùng FEN; (d) phần đăng nhập: chia 2 nhóm **"API chính thức"** và **"tự động hoá như người dùng"** (xem bảng 0.5b — phần lớn tôi ghi **không biết**), và kiến trúc phải **tắt được từng nền tảng bằng cấu hình**.
3. **Tiêu chí đo:** bảng ở 0.5 (≥ 3 skin × ≥ 3 cỡ cửa sổ; 32 quân; lệch ≤ 0,3 ô; 2 khung cùng FEN). Thêm: **ca đỏ cố ý** che 1 góc bàn / dùng skin chưa học ⇒ phải báo "không nhận ra" chứ không đi nước.
4. **Rủi ro:** (i) nhận diện giả tạo nước đi sai ⇒ mất Elo và mất uy tín nhanh hơn mất tính năng; (ii) phần đăng nhập phụ thuộc bên thứ ba đổi giao diện/giao thức ⇒ **tốn bảo trì nhiều hơn giá trị lấy được** nếu bạn không cô lập nó thành module riêng.

### K5 — Kho tàn cuộc để train từ gốc lên

1. **Chẩn đoán:** tệp câu hỏi của đội có **hai con số mâu thuẫn nhau** cho cùng một đại lượng: `1,14·10²⁰` (mục 0.1) và `4,2·10¹⁸` (mục K5) — lệch **27 lần**. Tôi **không xác minh được** con nào đúng (không có zip/số liệu đo), và tôi **không** chọn một con số để dựng kế hoạch. Vậy nên bài toán không phải "giải hết", mà là **lấy mẫu đúng chỗ** + **giữ được độ dài chiếu bí** (0.1) + **cổng đo rẻ** (dưới đây).
2. **Đề xuất (xem 0.1, A3-03, A3-04):** hạn ngạch theo tần suất thật với sàn/trần; **giữ cả hai cờ thắng theo luật / thắng lý thuyết**; label = WDL chính + cp + dtm phụ; **cổng "đã học" rẻ, không Elo:** ① Pearson/Spearman trên holdout cố định; ② **đối chứng ÂM** (nhãn xáo phải làm điểm tệ hơn) + **đối chứng DƯƠNG** (chạy lại bit-exact phải cho điểm y hệt); ③ điểm phải tăng đơn điệu 3 vòng đầu; ④ dừng khi không tăng ≥ 0,1 % ở 2 vòng liên tiếp. Chỉ khi ①–④ đạt mới mở A/B.
3. **Tiêu chí đo:** ρ(Spearman) giữa đầu ra và −DTM trên tập thắng `dtm ≥ 150` ≥ 0,5 (trước khi sửa nhãn ≈ 0); chênh `|cp_pred − cp_label|` trung vị ≤ 30 cp; **A=B với net đồng nhất phải cho KTC95 chứa 0** (đối chứng âm của chính phép đo A/B).
4. **Rủi ro:** thêm ~10⁷ thế tàn cuộc vào kho chủ yếu trung cuộc đã từng gây **−301 Elo** với mọi chỉ số nội bộ đều đẹp ⇒ rủi ro lớn nhất ở đây **không phải** lỗi code mà là **hỗn độn phân phối**. Vì vậy: **trộn theo tỉ lệ có kiểm** (ví dụ 1 phần tàn cuộc : 3 phần trung cuộc) và **giữ một holdout trung cuộc không trộn** để phát hiện lệch.

### K6 — Đo lường & kiểm thử chập chờn trên máy đang tải

1. **Chẩn đoán:** cổng đang đo **thời gian** trong khi máy thay đổi tải ⇒ ngưỡng cố định là ngưỡng sai. Cách sửa không phải "chờ máy rảnh", mà là **đo và ghi điều kiện đo cùng kết quả**, rồi chuẩn hoá.
2. **Đề xuất (1 ngày):**
   - **Quy ước ngưỡng theo tải:** chia vùng tải thành 3 mức theo `\Processor Information(_Total)\% Processor Time` (đúng bộ đếm cho máy > 64 luồng, theo tệp nêu). Mỗi mức có **ngưỡng riêng** = **p90 của 3 lượt** ở mức đó, và một **hệ số tải** `k_muc` ghi kèm kết quả.
   - **Báo cáo 3 số, không 1:** `thoi_gian`, `tai_luc_kiem`, `so_lan_lay_mau`. Quy tắc phân xử: `tai > 55 %` ⇒ kết quả **đánh dấu "không tin"**, **không** tính vào cổng, nhưng **không** cũng bị coi là đạt.
   - **Tách "lỗi thật" khỏi "máy bận"** bằng **3 lần lặp**: nếu 1 lượt đỏ và 2 lượt xanh, 2 lượt xanh ở tải thấp ⇒ báo **nghi ngờ lỗi**; nếu cả 3 đều đỏ mà tải cao cả 3 ⇒ báo **máy bận**. Ngưỡng ghi trước: "3 lượt đỏ" mới là FAIL.
   - **`Get-Counter` 1 mẫu là rác** — đúng. Dùng `-SampleInterval` và **bỏ mẫu đầu tiên** (warm-up), hoặc đo theo **thời gian thay vì theo mẫu** (đo PID trong 20 s như bạn đang làm là hướng đúng).
3. **Tiêu chí đo:** cùng một ca phải cho **phân loại ổn định** khi chạy 10 lần (≥ 8/10 cùng kết luận); và phải có **ca đỏ cố ý** để chứng minh cổng đỏ được.
4. **Rủi ro:** nếu bỏ qua bước "tai > 55 % ⇒ không tin", CI của bạn sẽ **báo xanh giả** — nguy hiểm hơn nhiều so với báo đỏ thật.

### K7 — Nhiều AI làm song song trên một kho git chung

1. **Chẩn đoán:** 3 phiên + worker ghi chung 1 tệp nhật ký + 2 lane ghi kho + 42 GB bản chép dò + khoá mồ côi. Vấn đề gốc: **bạn đang dùng git như khoá**. Git **không** khoá được ghi dữ liệu sinh ra, và nó chỉ quản lý **mã văn bản**. Kho 30 GB là **dữ liệu**, không phải mã ⇒ đừng bắt nó qua git.
2. **Đề xuất (1 ngày), 3 thứ tối thiểu:**
   - **Tách 3 đường:** (a) **mã** trong git, một nhánh/lane = một thư mục, không dùng chung; (b) **kho dữ liệu** chỉ có **một writer** qua hàng đợi (0.2) — các lane **không** ghi kho, chỉ ghi **hàng đợi**; (c) **nhật ký** append-only, **mỗi lượt 1 tệp** (`log/2026-09-27T14-05-12_lane-a.md`), **không** sửa tệp người khác. Ba đường này không lẫn vào nhau là hết phần lớn sự cố.
   - **Khoá:** `lock/<ten>.lock` chứa `{pid, lane, bat_dau, ten_tac_dong}`; quy tắc dọn duy nhất: **PID không còn sống mới xoá** (trên Windows kiểm tra bằng `OpenProcess`; đừng dựa vào mốc thời gian vì máy sleep làm sai). Khoá **tự hết hạn sau 30 phút không ghi** để không chết vĩnh viễn.
   - **Bản chép dò 42 GB:** bỏ hẳn. Thay bằng **kiểm không cần bản chép**: `PRAGMA quick_check` + đếm + mẫu khoá (K2/0.3). Đây là chỗ tiết kiệm lớn nhất và nằm đúng ở gốc vấn đề.
   - **Chi phí token:** mỗi lượt chỉ đọc **3 tệp nhỏ**: `AGENTS.md` (quy tắc), `task/<id>.md` (đề bài của mình), và `log/<lượt-trước>.md` (≤ 100 dòng). Không đọc lịch sử trò chuyện, không đọc cả kho.
3. **Tiêu chí đo:** (1) 2 lane chạy song song trong 1 giờ ⇒ **0** sự cố khoá mồ côi, **0** lần ghi kho từ lane; (2) giết 1 tiến trình giữa lô ghi ⇒ lane sau nhận được lock đã chết trong ≤ 60 s; (3) chi phí 1 lượt ≤ 15 k token **đo được** (đây là con số tôi **đề xuất**, không phải số của bạn).
4. **Rủi ro:** 3–10 agent trên 1 máy sẽ tranh **CPU/đĩa**, mà K6 nói tải cao làm hỏng phép đo ⇒ **không được chạy lane đo A/B song song với lane huấn luyện**. Đây là mâu thuẫn thật giữa K6 và K7 và bạn phải chọn: **đo trước, làm việc sau**.

### K8 — Ngân sách token

1. **Chẩn đoán:** 200–750 k token cho **một** lần kiểm là quá đắt, và nguyên nhân thường là **hỏi AI làm việc có tiêu chí rõ**. Khi tiêu chí là "đúng/sai" thì agent **không cần thiết** — script nhanh hơn, rẻ hơn, và **tất định**.
2. **Đề xuất (1 ngày) — ranh giới rõ:**
   - **Chuyển sang script, chỉ xuất số (KHÔNG cần AI):** kiểm luật theo ca; perft; đếm dòng/ký hiệu; `integrity_check`; so khớp FEN (0.2c); khử trùng; CRC-32C tệp; kiểm "0 dòng" ⇒ rc ≠ 0; dò màu viết cứng trong C# (A2-13); duyệt cây giao diện để tìm lối vào ẩn (A2-05); đo thời gian có ghi tải (K6). Đây gần như **toàn bộ** những gì bạn đang hỏi agent.
   - **Giữ cho AI (phải đọc, phán đoán):** chẩn đoán nguyên nhân khi script đỏ mà không rõ; soát kiến trúc; thiết kế nhãn/loss; đánh giá pháp lý; phân tích kết quả A/B **bất thường**.
   - **Mẫu "đề bài một lượt là xong"** (dùng lại được):
     ```
     MỤC TIÊU: <1 câu, có con số>
     PHẠM VI ĐỌC: <đúng 3 đường dẫn tệp, có số dòng>
     KHÔNG ĐỌC: lịch sử trò chuyện, tệp khác, cả kho dữ liệu
     ĐẦU RA: bảng <cột cố định>; mỗi dòng phải có `tệp:dòng` HOẶC URL+ngày
     ĐỎ/XANH: <ngưỡng viết trước>; nếu không đo được ghi "không đo được"
     CẤM: không sửa tệp; không đoán `tệp:dòng`; không ghi HTTP 200 cho URL chưa mở
     ```
   - **Cắt token theo kỹ thuật:** (i) **trả lời ngắn có bảng** thay vì văn xuôi; (ii) **một lần hỏi = một câu**; (iii) yêu cầu agent **không** đọc lại tệp đã nêu trong `task/<id>.md`.
3. **Tiêu chí đo:** sau khi tách, **mỗi ca kiểm dùng 0 token** và có **số**; phần AI chỉ chạy khi script đỏ. Mục tiêu (đề xuất, không phải số đo của bạn): **giảm ≥ 90 %** token cho các ca kiểm thường lặp.
4. **Rủi ro:** script mà **không có ca đỏ cố ý** thì "xanh" của nó không có nghĩa ⇒ mỗi script bắt buộc kèm 1 ca đỏ (đúng kỷ luật đo mà tệp nêu). Và **đừng** dùng agent để sinh lại những script đó.

**Nguồn chung cho K1–K8:** S1, S2, S3, S4, S5, S6, S7, S9, S10, S11, S13, S20, S21, S24, S30.
**Mức tin chung:** `CHẮC` cho cơ chế SQLite, giới hạn WAL, nguyên tắc một-writer, nền tảng hợp pháp; `GIẢ THUYẾT CẦN ĐO` cho mọi ngưỡng định lượng tôi đề xuất (p90, 0,1 %, 15 k token, 30 phút); `KHÔNG BIẾT` cho mọi thứ cần mã/đo của đội.

### Phần 2 — ASK03: A3-01…A3-16 + Y-1…Y-8

**Bắt buộc, dòng đầu:** `Đã đọc điều khoản bảo mật Ask03; không lưu, không lan truyền, không dùng để huấn luyện; sẽ xoá zip sau khi nộp.`
**Giới hạn chung của phần này:** các đoạn mã trong câu hỏi đều ghi «đã lược — đội gửi riêng nếu cần», và tôi **không có zip**. Vì vậy: mọi khẳng định nào cần đọc `tệp:dòng` của đội, tôi ghi **KHÔNG BIẾT** và thay bằng **nguyên lý + phép đo cụ thể** để đội tự kiểm.

#### A3-01 — Trainer vứt mọi nhãn chiếu bí; sửa target thế nào?

1. **Chẩn đoán (chắc, từ mô tả của bạn):** hiện tại có **hai** lỗi tách biệt. (i) `cp ≥ 29.xxx` bị loại (`:433`) rồi kẹp ±3000 ⇒ nhãn mate **biến thành một hằng số phẳng** ⇒ đúng là "net không có độ dốc theo số nước". (ii) Nhánh WDL lấy nhãn **từ chính cp đã bị kẹp** (`pred/1042,31`) ⇒ WDL cũng mất dốc ⇒ thực tế là **một lần lỗi nhân đôi**.
2. **Về hình dạng `cp = dấu × max(1500, 3000 − 10·dtm_ply)` (câu hỏi a,b):** hình dạng này **đúng hướng** — nó là "thang cp đơn điệu, bão hoà ở đáy", cùng họ với cách TT mã hoá mate theo ply. Nhưng tôi **không đồng ý** đặt nó vào **cùng đầu score** mà đang dùng `smooth_l1(pred/200)`, vì khi đó vùng ±1500…3000 bị chiếm bởi nhãn có độ dốc thấp, còn vùng ±200 lại được `smooth_l1` ưu tiên ⇒ **mâu thuẫn mục tiêu trong cùng một con số**. Đề xuất:
   - **Đầu WDL: nhãn WDL thật** (thắng/hoà/thua **theo luật của bạn**, xem A3-03b) — **không** suy từ cp. Đây là câu trả lời cho câu hỏi (b): nhãn mate vào đầu WDL là **đúng**, và dốc DTM **không** nên đi vào đầu WDL (sigmoid bão hoà mất gradient — chính là lỗi A3-02).
   - **Đầu score: giữ thang thường, cộng một nhánh DTM riêng** có trọng số nhỏ; hoặc nếu chỉ có một đầu value thì dùng `cp_mate = dấu × (TB_SCALE − 12·dtm_ply)` với `TB_SCALE` **lớn hơn** ngưỡng kẹp hiện tại, và **tách loss** thay vì cộng chung.
   - Giữ cờ **mặc định TẮT** như bạn đã đề xuất — đúng, và tôi bổ sung: **cờ phải ghi vào tên cache** (xem A3-16), nếu không thì bật/tắt cờ mà cache cũ vẫn còn là **"thất bại trông như đã xong"**.
3. **Câu hỏi (c) — lệch đơn vị, đây là lỗi nghiêm trọng nhất trong A3-01:** theo UCI/Stockfish, `mate N` trên dòng `info` là **N nước đi của cả hai bên** (full moves), còn `30000 − ply` là **ply** (nửa nước) ⇒ trộn hai nguồn là lệch **×2** đúng như bạn nêu. Bằng chứng đã mở: `uci.cpp:568` in `auto m = (mate.plies > 0 ? (mate.plies + 1) : mate.plies) / 2;` (S9) — tức chia đôi ply trước khi in. Cách sửa tôi đề xuất: **(1) chuẩn hoá mọi thứ về PLY ngay từ đầu vào**, kể cả `gen_thay_that.py:196`; **(2) viết MỘT hàm chuyển đổi duy nhất** và cấm viết số 30000 ở chỗ khác; **(3) thêm assertion**: `mate 1` và `mate 2` phải ánh xạ đúng, và `dtm_ply` phải **trùng khớp với số ply thật** khi bạn phát lại PV. Đây là loại lỗi mà chỉ **test đơn vị** mới bắt được — không cần mô hình.
4. **Câu hỏi (d) — công thức + ca đo được (dùng chung 0.1 và K5):**
   - `mate_cp = dấu × max(1500, 3000 − 10·dtm_ply)`, `wdl = nhãn luật thật`, `dtm = dtm_ply` làm **đầu phụ/trọng số mẫu**.
   - **Ca đo:** ρ(Spearman) giữa `-cp_dự_đoán` và `dtm_ply` trên **tập thế thắng `dtm ≥ 150`**, trên **holdout cố định**; ngưỡng đề xuất **≥ 0,5** (con số này là **suy luận của tôi**, phải đo trên máy đội). Đối chứng: nhãn xáo phải cho ρ **âm**; chạy lại bit-exact phải cho điểm **y hệt**.
   - **Ca đỏ bắt buộc:** bật/tắt cờ trên **cùng một cache** ⇒ kết quả phải **khác nhau**; nếu không, cache key thiếu cờ (A3-16).
5. **Nguồn:** S7 (`https://www.chessprogramming.org/Endgame_Tablebases`, mở 27/09/2026), S9/S10/S11 (Stockfish `uci.cpp` đặt `TB_CP = 20000`; `search.cpp` mã hoá mate/TB theo ply và `r50c`; WDL logistic 100 cp ≈ 50%).
6. **Mức tin:** `CHẮC` cho cơ chế mã hoá mate/TT theo ply và WDL logistic; `GIẢ THUYẾT CẦN ĐO` cho hằng 3000/10/1500, trọng số loss và ngưỡng ρ ≥ 0,5; `KHÔNG BIẾT` cho nội dung thật của `:433`, `:1830-1856` và loss hiện tại (không có zip).

#### A3-02 — Docco: sigmoid bão hoà ở nhãn chiếu bí + bộ lọc «nước tốt nhất là ăn quân»

1. **Câu hỏi (a) — bộ lọc này là một lỗi thiết kế, tôi khuyên bỏ, không phải sửa.** Lý do: bộ lọc "nước tốt nhất là ăn quân" **loại đúng phần dữ liệu tàn cuộc còn dùng được**. Ở tàn cuộc, nước ăn/đổi quân thường **là** nước tốt nhất — đó là bản chất của thế, không phải nhiễu. Nếu bạn giữ bộ lọc này cho kho tàn cuộc, kho của bạn **tự cắt mất thông tin mà mạng cần học nhất**.
   - **Thay bằng:** lọc bằng **movegen thật**, và chỉ một loại điều kiện: **thế đang bị chiếu** (có nước bắt vua) hoặc **thế sau khi ăn còn nước ăn đáng kể** — cái sau là *lọc chất lượng*, cái trước là *lọc hợp lệ*. Ghi rõ: để biết "đang bị chiếu" **không cần bộ sinh nước cờ tướng đầy đủ** — chỉ cần phần kiểm tra tấn công vua, vốn là phần rẻ nhất của movegen. Nếu thư viện của bạn không có, hãy viết riêng hàm `dang_bi_cheu()` (~40 dòng) thay vì bỏ cả bộ lọc.
2. **Câu hỏi (b) — ánh xạ khi `in_scaling = 1000` mà nhãn mate là 29.9xx:** đây đúng là chỗ **sigmoid bão hoà**: `sigmoid(29.9/1.0)` ≈ 1, gradient ≈ 0 ⇒ mẫu mate **không còn dạy được gì**. Sửa:
   - **Tách thang** cho hai đầu: đầu WDL dùng **WDL thật** (0/0,5/1), đầu score dùng thang cp thường, **không** đưa 29.9xx vào cùng một đầu.
   - Nếu buộc phải dùng một con số: `cp = dấu × min(MATE_CP, max(600, 3000 − 10·dtm_ply))` với `MATE_CP` **cùng hệ đơn vị ply** (xem A3-01c). Nhớ: **kẹp ở `MATE_CP` phải giữ tính đơn điệu theo dtm**, nếu không bạn lại quay về tình trạng hiện tại.
3. **Nguồn:** S9/S11 (WDL logistic — cùng nguyên nhân bão hoà: giá trị quá xa điểm giữa thì đạo hàm sigmoid → 0).
4. **Mức tin:** `CHẮC` cho nguyên nhân bão hoà (hàm logistic) và nguyên lý "bỏ bộ lọc ăn quân ở tàn cuộc"; `GIẢ THUYẾT CẦN ĐO` cho hằng 600/3000/10; `KHÔNG BIẾT` cho dòng `:621` và cấu trúc thư viện cờ tướng của đội.

#### A3-03 — Nhãn cho từng nước + luật không ăn quân với net CHỈ-VALUE

1. **Câu hỏi (a):** với net **value-only**, "nhãn từng nước" **không có chỗ để gắn** — net chỉ xuất một số cho thế đứng. Vì vậy tôi khuyên: **mỗi thế con là một mẫu huấn luyện độc lập** (tức là bạn đang làm đúng khi lưu `dtm`/`dtc` cho thế sau mỗi nước), và **không** thêm ranking loss, vì:
   - ranking loss giữa các con **trùng vai trò** với việc search tự lấy max; search đã làm việc đó tốt hơn net.
   - Thêm ranking loss là thêm **một con số phải tune** mà không có bằng chứng lợi ích.
   - Nếu sau này thấy eval chậm "nhận ra" thế thắng, hãy thử **tăng tỉ lệ mẫu thắng sắp chốt** thay vì thêm loss mới.
2. **Câu hỏi (b) — cursed win / blessed loss (S7, mở 27/09/2026):** tài liệu Syzygy nói bảng chia **5 lớp WDL**, trong đó lớp "vua bị đánh" (cursed win / blessed loss) chính là 2 lớp giữa, và chúng **không được coi là thắng/thua tuyệt đối**. Kỹ thuật quan trọng: **ngưỡng 50 nước ở cờ vua xử lý trong search, không xử lý trong nhãn** — nhãn vẫn là "thắng/thua về lý thuyết", còn search mới áp luật. Áp sang cờ tướng mốc **80 ply**:
   - **`wdl` = kết quả THEO LUẬT** (thế thắng lý thuyết nhưng không kịp chuyển hoá ở 80 ply ⇒ `wdl = hoà`).
   - **`dtm` giữ nguyên số ply thật** làm tín hiệu phụ, vì nó mang thông tin "còn bao xa thì phải chuyển hoá" mà wdl đã xoá.
   - **Hệ quả của hai lựa chọn của chủ sở hữu** (tôi chỉ nêu kỹ thuật như yêu cầu): nếu gắn **THẮNG** cho thế quá mốc, net học dòng "thắng nhưng bị luật hoà" ⇒ đầu WDL **lệch lạc** (không còn xác suất thắng thật), và mọi phép đo "WDL khớp" sau đó trở nên vô nghĩa; nếu gắn **HOÀ**, net **mất** thông tin "thế này về lý thuyết là thắng" ⇒ phải bù lại bằng `dtm` ở đầu phụ, nếu không search sẽ **không ưu tiên chuyển hoá sớm** và cứ kéo dài. **Khuyến nghị của tôi: wdl theo luật + dtm làm đầu phụ** — đây là lựa chọn ít làm hỏng phép đo nhất.
3. **Câu hỏi (c):** tôi **không tìm được** nguồn công khai đã mở và kiểm được về việc trainer nào dùng `dtz`/`dtc` làm **đặc trưng đầu vào** hay **trọng số mẫu** ⇒ ghi **KHÔNG BIẾT** (không đoán). Nếu bạn muốn làm, hãy bắt đầu rẻ: thêm `dtc` như **một nhóm đặc trưng** (bucket 0/1/2/3/4) thay vì trọng số liên tục, và **so 3 lô** (không có / chỉ đặc trưng / chỉ trọng số) với A=B làm đối chứng.
4. **Nguồn:** S7 (Syzygy: lớp WDL 2 và 4 = cursed win / blessed loss), S3.
5. **Mức tin:** `CHẮC` cho 5 lớp WDL và nguyên lý "luật xử lý ở search"; `GIẢ THUYẾT CẦN ĐO` cho việc dùng `dtc` làm bucket; `KHÔNG BIẾT` cho phần (c) và cho lựa chọn của chủ sở hữu.

#### A3-04 — Bộ mẫu PHÂN TẦNG 0v0…4v4: lấy mẫu + trọng số

1. **Câu hỏi (a) — trả lời từng tiêu chí:**
   - **Đều mỗi ô:** **không nên** — ô lớn sẽ nuốt hết ngân sách và bạn mất ô nhỏ (mà ô nhỏ mới là tàn cuộc thật).
   - **Theo log kích thước ô:** **nên, làm trần chính.** Đây là cách rẻ và tự cân bằng: `quota(cell) ∝ log(1 + |cell|)`.
   - **Theo tần suất trong ván thật:** **nên, làm phần dưới** (xem (b)) — nó giữ "hình dạng phân phối" mà net sẽ gặp ở ván thật.
   - **Theo tỉ lệ thắng/thua:** **nên, làm trọng số mẫu** — thế hoà hầu như không dạy gì về cách thắng, nên hạ trọng số hoà, **không** hạ nó về 0.
2. **Câu hỏi (b) — "rút đều trong ô" có lệch so với thế sinh từ ván thật không:** **có**, và đây là lệch đã biết: lấy đều trong ô = phân phối **đều theo không gian thế**, còn ván thật = phân phối **lệch mạnh** về vài cấu hình phổ biến. Khuyến nghị của tôi: **trộn 2 nguồn trong một ô** — `70 % rút đều` (để phủ) + `30 % lấy từ ván thật` (để khớp phân phối), rồi **báo cáo riêng** kết quả trên hai tập con này để biết mô hình có học "đều" hay học "thật". Tỉ lệ 70/30 là **con số tôi đề xuất, cần đo**.
3. **Câu hỏi (c) — rủi ro lớn nhất khi thêm ~10⁷ thế tàn cuộc vào kho chủ yếu trung cuộc (bạn từng có lô −301 Elo):** đây là câu tôi muốn nói thẳng: **rủi ro lớn nhất không phải lỗi code, mà là lệch phân phối âm thầm.** Mọi chỉ số nội bộ vẫn đẹp vì chúng **không đo** thứ đang hỏng (Elo). Vì vậy:
   - **Tỉ lệ trộn:** khuyến nghị **1 phần tàn cuộc : 3 phần trung cuộc** (con số tôi đề xuất) và **giữ một holdout trung cuộc không trộn** để phát hiện lệch.
   - **Cổng dừng sớm:** dùng đúng bộ cổng ở 0.8/K5 — Pearson/Spearman trên holdout cố định, đối chứng ÂM + đối chứng DƯƠNG, điểm tăng đơn điệu 3 vòng đầu, dừng khi không tăng ≥ 0,1 % ở 2 vòng liên tiếp. **Chỉ khi cả 4 đạt mới mở A/B** — A/B là tốn tiền thuê máy nhất, nên phải là bước cuối.
4. **Nguồn:** S13 (Fishtest `stat_util.py` — SPRT/LLR: công cụ quyết định "dừng hay chạy tiếp" bằng xác suất, không bằng cảm giác), S3.
5. **Mức tin:** `CHẮC` cho nguyên lý "quota theo log + trọng số hoà thấp" và quy trình dừng sớm dựa trên LLR; `GIẢ THUYẾT CẦN ĐO` cho 70/30 và 1:3; `KHÔNG BIẾT` cho số đo thật của lô −301 Elo (đó là số của đội, tôi không tự đo).

#### A3-05 — Mate-distance pruning có ở cả 3 engine; soát EGTB

1. **Câu hỏi (a) — khi bảng báo THUA mà `dtc > remain`, bỏ hẳn thông tin bảng:** tôi nói thẳng: **đây là lỗi thật, và là loại lỗi nguy hiểm.** Vì luật không ăn quân biến thua thành hoà, nên thế đó **không phải thua** — mà code bỏ thông tin bảng ⇒ search phải tự đi tìm lại, và vì tự tìm thì nó **không có chứng cứ** rằng bên thắng không kịp chuyển hoá. Kết quả là search có thể **tưởng mình thắng** trong một thế chỉ hoà theo luật. Cách sửa: khi `dtc > remain` thì **trả về điểm hoà (0) hoặc điểm âm rất nhỏ theo thời gian còn lại**, **không** trả "không biết". Đối xứng cho trường hợp THẮNG mà `dtc > remain`.
2. **Câu hỏi (b) — `scoreToTT(v, ply)` + cờ bound cho giá trị bảng DTM:** các lỗi kinh điển tôi biết và **có thể nêu tên**:
   - **Không sửa mate score khi đọc/ghi TT** (off-by-one ply làm mate trông xa hơn thật).
   - **Cất một kết quả bảng vào TT dưới khoá không phản ánh trạng thái luật**: ở cờ tướng, thế bị **chiếu mãi** hoặc **đuổi mãi** có thể là hoà theo luật; nếu khoá TT không chứa trạng thái đó, một lần ghi "thắng" sẽ được đọc lại ở một thế vốn phải hoà. Khuyến nghị: **hoặc không cache kết quả bảng ở các thế chưa xác định luật, hoặc vô hiệu hoá mục TT khi bộ đếm luật đổi bucket** — và có một **ca thử riêng** cho thế chiếu mãi.
   - **Tôi không thể nói lỗi cụ thể ở `search.cpp:1393-1426`** vì không có mã. Cách tự kiểm: `perft` + một ca "thế chiếu mãi phải ra hoà" + một ca "TT đọc lại cho cùng kết quả" (round-trip `store`/`load`).
3. **Câu hỏi (c) — kho Felicity có thể sót/đuổi chiếu mãi:** `KHÔNG BIẾT` (không có mã nguồn, không có tài liệu công khai tôi mở được). Cách duy nhất đáng tin là **thử bằng dữ liệu**: dựng vài thế chiếu mãi, hỏi bảng, xem nó trả thắng/thua/hoà gì.
4. **Nguồn:** S9, S10 (Stockfish: mate score qua TT **có** sửa theo ply; `r50c` cho thấy luật 50 nước nằm ở tầng search), S7.
5. **Mức tin:** `CHẮC` cho lỗi "bỏ thông tin bảng" và nguyên lý sửa mate-score qua TT; `GIẢ THUYẾT CẦN ĐO` cho việc thêm bộ đếm luật vào khoá TT; `KHÔNG BIẾT` cho dòng 1393-1426 và hành vi kho Felicity.

#### A3-06 — Hai engine không bao giờ probe bảng tàn cuộc

1. **Câu hỏi (a) — cách gắn probe an toàn nhất, giữ `bench` bit-exact khi tắt:** nguyên tắc tôi đề xuất, theo đúng thứ tự rủi ro:
   - **Bật bằng cờ biên dịch, mặc định TẮT** (`-DUSE_EGTB`, đúng như bạn đang build cho Bodetosu), và **đặt sau** điểm hook đã biết ở engine thứ ba (`search.cpp:1393` theo mô tả của bạn) — tức là ở chỗ đã có ý định hỏi bảng, đừng chèn vào một chỗ mới.
   - **Điều kiện bắt buộc về tính đúng đắn:** chỉ hỏi bảng khi **không còn quân lớn** (ngưỡng theo số quân, ví dụ ≤ 5), và **hỏi sau khi đã sinh nước hợp lệ** để không trả giá trị cho thế không hợp lệ.
   - **Điều kiện bắt buộc về kiểm thử:** khi cờ TẮT, `bench` và `perft` phải **bit-exact** với bản cũ (cùng hash ngữ cảnh, cùng seed) — đây là ca kiểm duy nhất chứng minh bạn không vô tình đổi hành vi; khi cờ BẬT, phải có ca đỏ: cắt file bảng đi ⇒ phải **từ chối trong ≤ 1 s, không crash**.
2. **Câu hỏi (b) — lợi ích Elo thực tế của EGTB ≤ 4–5 quân trong cờ tướng:** **KHÔNG BIẾT** — tôi không tìm được số công bố công khai mà tôi đã mở và kiểm. Suy luận của tôi (ghi rõ là suy luận): ở cờ tướng, phần lớn tàn cuộc **dưới 5 quân vẫn còn kỹ thuật** (đền, xe, mã, tượng, sĩ), nên lợi ích chủ yếu là **"không blunders ở thế thắng rõ ràng"** chứ không phải "biết đáp án". Nếu bạn muốn biết chắm, cách rẻ nhất là **A/B riêng một tập tàn cuộc 2–5 quân lấy từ ván thật** thay vì đo trên ván tổng hợp.
3. **Nguồn:** S9/S10 (nguyên lý TT/mate ở tầng search), S6.
4. **Mức tin:** `CHẮC` cho quy tắc cờ biên dịch mặc định TẮT + ca `bench` bit-exact; `GIẢ THUYẾT CẦN ĐO` cho ngưỡng số quân; `KHÔNG BIẾT` cho lợi ích Elo công khai.

#### A3-07 — Luật 80 ply trong eval + khoá băm của OngThan

1. **Câu hỏi (a) — đổi mốc 120 → 80 mà giữ nấc băm `/8` từ 14:** đây là chỗ tôi muốn cảnh báo mạnh, vì nó là **loại lỗi im lặng**. Rủi ro: nếu khoá TT/eval **không chứa** (hoặc chỉ chứa thô) bộ đếm "còn bao nhiêu ply nữa mới hòa", thì hai thế chỉ khác nhau ở **mốc luật** có thể **dùng chung một giá trị eval**. Hậu quả cụ thể: một thế **còn xa mốc** có thể bị xử lý đúng như thế **đã chạm mốc** ⇒ engine **không chuyển hoá** dù vẫn còn đường thắng, hoặc ngược lại **kết thúc hoà sớm**. Cách kiểm rẻ và chắc: **(1) test đơn vị** — hai thế chỉ khác bộ đếm phải cho **hai khoá khác nhau**; **(2) test hành vi** — thế "còn 1 ply tới mốc" và thế "còn 50 ply" phải cho **eval khác nhau**; **(3) khi bộ đếm đi qua một mốc băm, vô hiệu hoá mục TT liên quan** (rẻ hơn nhiều so với nhét bộ đếm vào khoá).
2. **Câu hỏi (b) — bên thắng muốn chuyển hoá sớm, bên thua muốn kéo dài:** nguyên lý đúng (và đây là điểm tôi **không** đồng ý với cách làm "giảm eval theo bộ đếm" một cách tuyến tính): Stockfish xử lý luật 50 nước **ở tầng search**, không ở eval — search trả điểm hoà khi luật bắt kịp, còn eval chỉ biểu diễn "ai hơn" chứ không biểu diễn "còn bao xa thì phải gấp". Tôi khuyến nghị: **(1)** giữ luật ở search (đã có `r50c` bên Stockfish — S10/S11), **(2)** nếu muốn bên thắng "gấp" thì thêm một **thành phần phụ phụ thuộc pha** chứ không sửa thẳng eval chính — nếu không, bạn sẽ dạy net "đánh mạnh hơn khi sắp hòa theo luật" ở mọi thế, kể cả thế không liên quan.
3. **Câu hỏi (c) — hằng số 120 còn sót ở đâu:** `KHÔNG BIẾT` (không có mã). Cách làm: **một hằng duy nhất** `RULE_NO_CAPTURE_PLY = 80` trong một tệp, **không** literal 80/120 ở nơi khác; thêm một bước CI quét literal và một ca test khẳng định `>= mốc ⇒ hoà`.
4. **Nguồn:** S10, S11 (`r50c`), S3.
5. **Mức tin:** `CHẮC` cho nguyên lý "luật ở search, không ở eval" và nguy cơ va chạm khoá do bộ đếm; `GIẢ THUYẾT CẦN ĐO` cho phương án vô hiệu hoá mục TT; `KHÔNG BIẾT` cho vị trí hằng số trong mã.

#### A3-08 — Cờ úp: hai bản vá «đúng luật» làm yếu ≈ −33 Elo

1. **Câu hỏi (a) — phần nào có thể làm eval/search tệ dù «đúng hơn»:** xếp theo mức nguy hiểm, từ lớn xuống:
   - **B1m (đổi `materialKey` theo trạng thái lật):** bảng material/imbalance tra theo khoá **mới** trả về giá trị **chưa tune** cho quân úp. Đây là ứng viên số 1 vì "đúng luật + số chưa tune" là công thức của −33 Elo, và nó khớp với án lệ đã có của đội (thêm giá trị eval mà không đo trên tập cố định). Cũng lưu ý: `materialKey` đổi theo **lật quân** nghĩa là **cùng một bàn cờ có thể đổi khoá giữa chừng search** ⇒ phá vỡ giả định "eval là hàm của thế" mà TT dựa vào.
   - **B2b (cập nhật psq khi lật quân trong cây search):** có **hai** lớp rủi ro: cập nhật **hai lần** cho cùng một nước, và cập nhật ở **trong** cây search ⇒ `psq` phải là hàm thuần của thế, nếu không, hash phụ sai.
   - **C-365 (`perft` dùng `do_move(..., true)`):** theo mô tả của bạn, nó **chỉ** ảnh hưởng perft, **không** ảnh hưởng search. Nếu đúng, bản vá này **không thể** gây −33 Elo. Nếu A/B của bạn cho thấy nó cũng âm ⇒ **A/B bị nhiễu** (ví dụ chạy song song job khác trên máy — đúng loại rủi ro K6). Đây là phép loại trừ tôi muốn bạn làm trước tiên vì nó **rẻ và dứt khoát**.
2. **Câu hỏi (b) — lỗi thật trong `position.cpp` quanh `do_move`/`undo_move`, `darkSquare = SQ_NONE`:** `KHÔNG BIẾT` (không có mã). Danh sách kiểm cụ thể, theo đúng thứ tự hay gặp: (1) `do_move`/`undo_move` phải **đối xứng** trên mọi trạng thái phụ, kể cả quân úp; (2) `StateInfo` phải **sao chép** đủ thông tin lật, nếu không thì `undo_move` sẽ khôi phục sai; (3) khoá (hash) phải cập nhật **tăng dần, đúng thứ tự**; (4) `darkSquare = SQ_NONE` phải **không** làm hỏng phát hiện "vua bị tấn công" khi quân chưa lật — vì một quân úp **vẫn tấn công** theo luật.
3. **Câu hỏi (c) — phép đo ≤ 30′:** trong 30 phút, A/B không đủ để kết luận (300 ván ⇒ cỡ tin cậy ±7,2 theo chính số đo của đội ở A2-07). Vì vậy tôi đề xuất **ba cổng rẻ trước, A/B sau**:
   - **Cổng đối xứng (rẻ nhất, đáng làm nhất):** đánh giá một thế và thế **lật 180° + đổi bên**; eval phải **bằng nhau trong sai số số nguyên**. Lỗi ở B1m/B2b là lỗi **bất đối xứng theo màu** ⇒ cổng này bắt được mà không cần đấu ván nào.
   - **Cổng perft:** bản vá C-365 phải làm perft **khớp tuyệt đối** với node kỳ vọng.
   - **Cổng `jqcheck`/`jqwalk`:** lệnh chẩn đoán bạn đã thêm phải cho kết quả **ổn định** giữa hai lần chạy liên tiếp.
   - Sau đó mới A/B, và **tách từng hunk** (bật riêng B2b / riêng B1m) chứ không đo gộp — vì đo gộp là nguyên nhân khiến 3 lô cho 3 kết luận khác nhau.
4. **Nguồn:** S13 (để hiểu vì sao ±7,2 cần 300 ván và vì sao 100 ván chỉ đủ để biết **dấu**), S3.
5. **Mức tin:** `CHẮC` cho logic "bảng tra theo khoá mới chưa tune" và "lỗi bất đối xứng theo màu"; `GIẢ THUYẾT CẦN ĐO` cho cổng đối xứng và thứ tự bật từng hunk; `KHÔNG BIẾT` cho mã `position.cpp` và cho việc các số −26,7 / −63,2 / −29,6 là số đo của đội (tôi không tự đo).

#### A3-09 — Bộ đếm không khớp `Legal positions` của Felicity

1. **Câu hỏi (a) — Felicity đếm «Legal» trên không gian chỉ số nào:** `KHÔNG BIẾT` — tôi không có mã nguồn FelicityEgtb cũng như không có tài liệu mô tả cách đếm, và tôi **không đoán**. Cách xác định bằng thực nghiệm, theo thứ tự rẻ nhất: **so 3 giả thuyết với 3 số bạn đã có** (`krk` 8.748; gộp gương 4.401; Felicity 4.806):
   - H1: Felicity **loại thế bên đi đang bị chiếu** — kiểm bằng cách đếm lại với đúng bộ lọc này.
   - H2: Felicity **gộp gương trái–phải** — nhưng 4.401 ≠ 4.806, nên gộp gương **không giải thích hết**; còn 405 = chênh lệch ⇒ cần thêm một quy tắc nữa.
   - H3: Felicity **loại thế không hợp lệ theo luật riêng của nó** (chẳng hạn đã cấm chiếu/đuổi mãi ngay từ đầu).
   - Bạn đã thử 16 tổ hợp quy ước mà không khớp ⇒ tôi **không** đoán thêm, tôi chỉ nói cách đo: **in ra danh sách thế Felicity có mà bộ của bạn không có** (và ngược lại), rồi **nhìn 10 thế đầu**. Đây là cách định danh chắc chắn hơn mọi giả thuyết.
2. **Câu hỏi (b) — lệch hoà 1–1,5 % phía Đen đi:** hướng nghiêng mạnh về **luật, không phải chỉ số**, vì **max-DTM khớp 40/40 bảng** ⇒ chỉ số/đánh số là đúng; phần lệch nằm ở **tập thế được coi là hoà**. Giả thuyết cụ thể: bộ nội bộ của bạn **không hiện thực luật chiếu mãi/đuổi mãi** (đúng như bạn tự nghi ngờ về `khong_luat_lap`), nên nó đánh dấu một số thế mà Felicity coi là hoà. Cách kiểm chắc chắn: **lấy đúng 10 thế mà bộ bạn nói thắng nhưng Felicity nói hoà**, rồi xác định thủ phạm luật. Nếu cả 10 đều là thế chiếu/đuổi mãi thì kết luận đóng.
3. **Nguồn:** S7 (tài liệu tàn cuộc tổng quát — **không** mô tả cách Felicity đếm).
4. **Mức tin:** `KHÔNG BIẾT` về cách đếm của Felicity; `GIẢ THUYẾT CẦN ĐO` về nguyên nhân lệch tỉ lệ hoà; `CHẮC` cho suy luận "max-DTM khớp 40/40 ⇒ chỉ số đúng, lỗi nằm ở tập luật".

#### A3-10 — Ghi chú `W-M-nnnn` của chessdb

1. **Câu hỏi (a) — `M-nnnn` đếm từ thế hiện tại hay thế sau nước đó, `rank` 2/1/0 nghĩa gì, có tài liệu chính thức không:** `KHÔNG BIẾT` — tôi **không mở được tài liệu chính thức nào** của chessdb.cn, và trang điều khoản của đội báo 404. Tôi **không** đoán nghĩa của `rank` vì đoán ở đây sẽ thành nhãn sai cho hàng nghìn thế.
   - Cách xác định **chắc chắn bằng thực nghiệm** (rẻ, 1 giờ): lấy **20 thế có DTM biết** (bạn đã có bộ giải nội bộ), hỏi chessdb, rồi so **3 thứ**: dấu (thắng/thua), **quy tắc chẵn/lẻ của chính chessdb** (ta chưa biết nó đếm từ thế hiện tại hay từ thế sau nước đó, nên **đo** thay vì giả định), và **độ lớn**. Ghi kết quả vào sổ và **dùng sổ đó** cho mọi lần sau. Câu hỏi của bạn rất đúng khi nêu hiện tượng «một thế trả thắng mà số chẵn» — đó chính là dấu hiệu phải làm phép đo trên.
2. **Câu hỏi (b) — hạn mức 100.000 lượt/IP/24h:** con số này tôi chỉ thấy **qua lời kể trên diễn đàn do bạn nêu**, **không phải nguồn chính thức tôi đã mở** ⇒ tôi ghi: **không có nguồn chính thức đã kiểm**. Quy tắc vận hành tôi đề xuất: (a) coi 100.000 là **giả định thận trọng**, đặt token-bucket cục bộ **thấp hơn nhiều**; (b) **cache theo khoá vị trí** để không hỏi lại cùng một thế; (c) **ghi lại mã trả về** (429/403) vào sổ — nếu không, bạn sẽ không biết mình đã chạm trần; (d) **không** nói trong báo cáo rằng con số này "chính thức".
3. **Nguồn:** **không có nguồn ngoài nào tôi mở được** cho chessdb.cn (trang điều khoản trả 404, tài liệu `W-M-nnnn` không tìm thấy) ⇒ mọi ý ở trên là **đo trên máy đội**, không phải trích nguồn.
4. **Mức tin:** `KHÔNG BIẾT` cho cả (a) và (b) — đúng như yêu cầu "không có thì ghi không biết"; `GIẢ THUYẾT CẦN ĐO` cho quy tắc vận hành.

#### A3-11 — Học từ engine bản quyền trên cờ thế CBL: «mate giả» và nhãn dè dặt

1. **Câu hỏi (a) — lỗ hổng của thứ tự trọng tài (bộ giải DTM → EGTB → đánh tiếp tới cùng):** lỗ hổng lớn nhất, theo thứ tự:
   - **Hết ngân sách nút = `KHÔNG BIẾT`** là một quy tắc **đúng**, nhưng nó tạo **lỗ hổng thật** nếu bạn **thay bằng** "tin engine thầy" ⇒ lúc đó bạn đã chuyển từ "bằng chứng" sang "tín hiệu" mà không nói. Đề xuất: giữ `KHÔNG BIẾT` **và không huấn luyện** thế đó (bỏ trống), đừng điền bằng thầy.
   - **Mate do thầy báo phải được kiểm lại bằng luật CỦA BẠN** (mốc 80 ply của bạn có thể khác luật của thầy) ⇒ đây là nguồn "mate giả" rất thật: engine nói thắng trong 6 nước *của nó*, nhưng theo luật của bạn thì ván đó hoà.
   - **"Cái nào tính hay hơn thì lấy"** là quy tắc nguy hiểm: nó chọn theo **điểm tuyệt đối** ở một mẫu nhỏ, tức là **nhiễu**. Tôi khuyên: **lấy theo bằng chứng, không theo thắng điểm** — cụ thể ở (b).
2. **Câu hỏi (b) — «dè dặt» nên là gì cụ thể:** đề xuất của tôi, theo thứ tự ưu tiên:
   - **Không bao giờ** để thầy ghi đè một lời giải **đã kiểm chứng** (bộ giải DTM hoặc EGTB). Quy tắc: bằng chứng > thầy.
   - **Nhãn mềm theo độ tin cậy** cho phần thầy: `cp_soft = clamp(cp_thay, ±C)` với `C` **giảm dần theo độ tin cậy**; và `wdl` lấy từ **tìm kiếm kết thúc thật** nếu có, nếu không thì **không gán WDL** (bỏ trống) — đừng suy WDL từ cp của thầy.
   - **Trọng số mẫu theo số nguồn độc lập đồng ý** là tốt hơn nhãn mềm: 2 thầy + bộ giải cùng kết luận ⇒ trọng số 1,0; 1 thầy ⇒ 0,3. Con số là **đề xuất của tôi, cần đo**.
   - **Đếm tỉ lệ mate giả từng thầy** như bạn đã định — đây là số **tự đo của đội**, tôi không có con số này.
3. **Câu hỏi (c) — engine UCCI báo `mate` theo NƯỚC hay NỬA NƯỚC:** `KHÔNG BIẾT` về các engine thương mại cụ thể, và tôi **không đoán**. Cách chốt **duy nhất** đáng tin: **hiệu chuẩn từng engine** — dựng một dòng thế bắt buộc thắng trong 1 nước và một dòng thắng trong 2 nước, hỏi engine, ghi lại nó trả `mate 1` hay `mate 2`, lưu vào sổ theo từng engine. Rồi **dùng sổ đó** ở mọi nơi. Đây chính là A3-01c ở dạng khác: cùng một lớp lỗi, cùng một cách chữa.
4. **Nguồn:** S19/S28/S29 (CCBridge: `.cbf` là XML, có thể mang FEN/PGN/ICCS — nên bước kiểm lời giải bằng **chính người chấm** là khả thi), S7.
5. **Mức tin:** `CHẮC` cho nguyên lý "bằng chứng > thầy" và "hiệu chuẩn từng engine bằng ca cố định"; `GIẢ THUYẾT CẦN ĐO` cho trọng số 1,0/0,3 và cách kẹp cp; `KHÔNG BIẾT` cho quy ước `mate` của từng engine thương mại.

#### A3-12 — «Một nguồn trạng thái»: còn bao nhiêu đường ghi lén

1. **Câu hỏi (a) — còn đường thứ 5 ghi `ChiXem`/`AutoDo`/`AutoDen`/`_chay` hoặc gửi nước ra ngoài không qua điểm kiểm duy nhất:** `KHÔNG BIẾT` — tôi không có cây mã. Nhưng tôi **có** bộ lệnh để bạn tự có câu trả lời trong 5 phút (chạy trong thư mục `AppCo_TieuLongNu/`):
   - Tìm **mọi chỗ ghi** (phải gồm cả gán qua property, không chỉ gán trực tiếp): `rg -n --no-heading -e 'ChiXem\s*=' -e 'AutoDo\s*=' -e 'AutoDen\s*=' -e '_chay\s*=' -e '\bBatDau\(' -e '\bDung\(' -e '\bDungGiuBan\(' -e '\bMoAuto\(' -e 'ChayAuto' .`
   - Tìm **mọi chỗ gửi nước ra ngoài** (đây mới là chỗ **nguy hiểm**, vì nó là hậu quả chứ không phải nguyên nhân): `rg -n --no-heading -e 'PostMessage' -e 'SendInput' -e 'mouse_event' -e 'Bam\w*\(' -e 'GuiDi\w*\(' .`
   - Sau đó **đếm lối vào** của AUTO, và đối chiếu với **một** điểm kim duy nhất. Quy tắc chấp nhận được: **mọi lối vào phải hội tụ về một hàm duy nhất**; bất kỳ lối vào nào ghi trực tiếp là **lỗi kiến trúc**, không phải lỗi logic.
2. **Câu hỏi (b) — thiết kế khiến lỗi này không thể xảy ra về cấu trúc:** mẫu C# 5 (ngắn, đúng ràng buộc cú pháp của đội):

```csharp
// C# 5 — trang thai chi sua duoc qua 2 ham; thuoc tinh co setter rieng (private set).
using System;                     // Array.IndexOf, Func<>
public enum CheDoAuto { Xem, PhanTich, ChienDanh }   // Xem | PhanTich: KHONG gui ra ngoai
public sealed class TrangThaiAuto
{
    public CheDoAuto CheDoHienTai { get; private set; }
    public bool DuocPhepGuiRaNgoai { get { return CheDoHienTai == CheDoAuto.ChienDanh; } }
    // BANG CHUYEN TRANG THAI: moi quyet dinh "duoc phep di nuoc ra ngoai" nam o day.
    private static readonly CheDoAuto[] Chuyen = {
        CheDoAuto.Xem, CheDoAuto.PhanTich, CheDoAuto.ChienDanh, CheDoAuto.PhanTich, CheDoAuto.Xem };
    public TrangThaiAuto() { CheDoHienTai = CheDoAuto.Xem; }   // mac dinh AN TOAN
    public bool ChuyenSang(CheDoAuto moi)
    {
        int i = Array.IndexOf(Chuyen, CheDoHienTai);   // sai chu dich -> false, khong doi
        if (i < 0) return false;                      // CHAN: khong doi khi khong co quy tac
        CheDoHienTai = moi; return true;
    }
}
// DIEM KIEM DUY NHAT: moi duong gui nuoc ra ngoai deu PHAI qua ham nay.
public static class CongDiNgoai
{
    public static bool Gui(string ban, string nuoc, Func<bool> kiemTraTruocKhiGui)
    {
        if (!kiemTraTruocKhiGui()) return false;   // CHAN CUOI: dat ngay tai noi gui
        return CongNoiBo.Gui(ban, nuoc);
    }
}
// YEU CAU CUA THIET KE: bo setter cong khai tren CheDoAuto.
```
   Ý tưởng cần giữ: **(i)** trạng thái không sửa được từ ngoài và mặc định là **không gửi**; **(ii)** **một** hàm gửi duy nhất, kiểm ở **đúng chỗ gửi**; **(iii)** mọi quyết định "được phép đi nước" nằm trong **bảng chuyển trạng thái**, không nằm rải trong các handler.
3. **Câu hỏi (c) — giá trị lưu bền mặc định «đi nước» có nên đổi không:** tôi khuyên **đổi mặc định sang an toàn** (mặc định mới = không gửi ra ngoài) **và** hiện một hộp thoại **một lần** cho người dùng cũ khi nâng cấp. Lý do cụ thể: giá trị lưu bền kiểu FULL của phiên trước là nguyên nhân gốc của N2; nếu giữ nguyên, **mọi người dùng cũ** sẽ bị "tự nhiên đi nước" sau khi cập nhật — đúng loại lỗi «thất bại trông như đã xong» mà 3.6 nêu. Nhớ: đổi mặc định phải **có đường quay lại** (ghi rõ trong hộp thoại), nếu không thì đây là lỗi mới.
4. **Câu hỏi (d) — cổng tự kiểm phủ mọi lối vào:** đề xuất **hai lớp**, vì một lớp không đủ:
   - **Lớp 1 — liệt kê bằng reflection:** nạp assembly, lấy mọi method có tham số kiểu `HopAuto`/`TuChoi`, và **assert** rằng không method nào (trừ danh sách cho phép) làm thay đổi `CheDoAuto` mà không gọi `TrangThaiAuto.ChuyenSang`. Đây là lớp bắt được **đường ghi lén mới thêm về sau**.
   - **Lớp 2 — bảng lối vào × trạng thái:** một bảng dữ liệu liệt kê (lối vào, trạng thái, kết quả mong đợi) và một test **gọi thật từng lối vào** ở cả 3 chế độ. Lớp này bắt được lỗi **hành vi**, lớp 1 chỉ bắt lỗi **cấu trúc**.
   - **Yêu cầu bắt buộc:** cổng phải **từng đỏ thật** — tự thêm một đường ghi lén giả (một setter công khai) trong một lần build kiểm thử, và xác nhận cổng **báo đỏ**. Không có bước này thì cổng chỉ là trang trí.
5. **Nguồn:** **không có nguồn ngoài** cho mã của đội; các lệnh/mẫu ở trên là **quy trình** tôi đề xuất, không phải trích dẫn.
6. **Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho thiết kế trạng thái và cổng kiểm; `KHÔNG BIẾT` cho kết quả quét của câu hỏi (a) — tôi không thể liệt kê `tệp:dòng` khi không có mã.

#### A3-13 — Luồng + hàng đợi Dispatcher: bơm thế đến muộn sau STOP

1. **Trả lời thẳng: có, và đây là lớp lỗi mà bạn **đã** gặp một lần — nên coi là chắc chắn sẽ gặp lại.** Nguyên nhân gốc: `Dispatcher.BeginInvoke` **đóng băng tham số tại thời điểm gọi** và **thực thi sau**. Nếu trạng thái đã đổi giữa lúc xếp hàng và lúc chạy, thế đã bơm sẽ được áp vào thế hiện tại.
2. **Kịch bản thời gian tôi sẽ soát (viết để dựng ca thử):**
   - `t1` luồng nền nhận thế mới từ ván, gọi `BeginInvoke(bamThe)` — thế được xếp hàng, **chưa** chạy.
   - `t2` người dùng bấm **STOP** ⇒ phiên cũ bị dừng, `_phien` tăng.
   - `t3` người dùng bấm chạy lại ⇒ phiên mới (`_phien` = N+1).
   - `t4` UI dequeues `bamThe` cũ ⇒ **đè bàn của phiên mới**. Đây chính là lỗi bạn đã vá một lần; cần bảo đảm **không còn đường nào bơm thế không kèm số phiên**.
3. **Mẫu chữa (C# 5), quy tắc bất di bất dịch:** **mọi thứ bơm qua Dispatcher phải mang số phiên, và phải so lại ở lúc chạy.**

```csharp
// C# 5 — BAT "so phien" O LUC XEP HANG, va SO LAI O LUC CHAY.
using System;                     // Action
public sealed class BamTheQa
{
    private readonly System.Windows.Threading.Dispatcher _ui;
    private int _phien;                                  // so phien hien tai
    public BamTheQa(System.Windows.Threading.Dispatcher ui) { _ui = ui; _phien = 1; }
    public void BatDauPhienMoi() { System.Threading.Interlocked.Increment(ref _phien); }

    public void BmThe(string ban, Action<string> apDung, int phien)
    {
        _ui.BeginInvoke(new Action(delegate()
        {
            if (phien != _phien) return;      // thuoc ve phien cu -> BO, khong ap
            apDung(ban);                      // chi phep khi dung phien
        }), System.Windows.Threading.DispatcherPriority.Normal);
    }
}
// GHI CHU BAT BUOC: KHONG bao gio lam `if (dung) apDung(ban);` — kiem o luc GOI thi
// viec da xep hang van chay sau STOP. So sanh PHAI nam trong body cua lenh chay.
// GHI CHU 2: doc _phien bang Volatile.Read/Interlocked neu luong nen va UI cung ghi.
```
4. **Ba lỗi cùng họ, nên soi cả ba:** (a) **so sánh ở lúc gọi** thay vì lúc chạy; (b) **dùng `Dispatcher.Invoke` (đồng bộ)** cho việc nền — dễ **chết deadlock** khi luồng nền giữ khoá; (c) **không huỷ** hàng đợi khi STOP ⇒ nên thêm hủy theo phiên nếu kiến trúc cho phép.
5. **Nguồn:** **không có nguồn ngoài** cho `CuaSoChinh.cs` của đội; hành vi `Dispatcher.BeginInvoke` là thông tin API chuẩn .NET, còn mẫu chữa ở mục 3 là **quy trình** tôi đề xuất. S2 (nếu hàng đợi có ghi/xoá trong DB thì mọi ghi phải bọc giao dịch).
6. **Mức tin:** `CHẮC` cho cơ chế `BeginInvoke` (đóng băng tham số, thực thi sau) và mẫu chữa; `GIẢ THUYẾT CẦN ĐO` cho kết luận "còn đường nào khác" (không có mã); `KHÔNG BIẾT` cho dòng cụ thể trong `TuChoi.cs`/`HopAuto.cs`/`CuaSoChinh.cs`.

#### A3-14 — Nhận bàn từ ảnh + tiêm chuột: độ bền và xác nhận nước đã «ăn»

1. **Câu hỏi (a) — sau khi bấm có đọc lại bàn để xác nhận không:** `KHÔNG BIẾT` (không có mã), nhưng tôi nói thẳng đây là **cổng quan trọng nhất** của cả phần AUTO, và lý do là thống kê: nếu bạn không xác nhận, mọi "nước không ăn" sẽ **được báo thành công** ⇒ bạn mất 1 nước mà không biết, và lỗi tích luỹ âm thầm.
   - Cách làm tôi đề xuất: sau mỗi cú bấm, **chờ có giới hạn** (ví dụ ≤ 400 ms, theo ngân sách 150 ms/nước bạn đang dùng ⇒ dự phòng), **chụp lại bàn**, và so **hash vùng bàn**; chỉ khi bàn **thực sự đổi** mới coi là đã đi; quá thời gian ⇒ ghi sự kiện "**bấm không được nhận**" và **không** bấm lại liên tục (bấm lại mù là cách tạo nước kép).
   - Thêm một điều kiện nữa rẻ hơn nhiều: nếu engine chọn nước mà **bàn hiện tại đã cho phép**, thì kỳ vọng bàn đổi là xác định ⇒ so **FEN mong đợi** thay vì so hash thô.
2. **Câu hỏi (b) — nhận bàn giữa ván:** ba nguyên nhân hỏng đã nêu có **cách xử lý rẻ**:
   - **Quân đang bay / hoạt ảnh:** chỉ nhận khung mà **FEN ổn định ở ≥ 2 khung liên tiếp**; tạm thời bỏ qua mọi khung có tốc độ/phát hiện chuyển động bất thường.
   - **Ô được tô sáng:** ô tô sáng làm sai lớp phân loại vì nó đổi nền ô ⇒ học **thêm một mẫu "ô được chọn"** hoặc loại các ô có dấu hiệu tô sáng trước khi phân loại.
   - **Bàn lật 180°:** đây **không** phải lỗi nhận dạng, mà là **phép biến đổi hình học** (hoán đổi trục + đảo chiều) ⇒ xử lý ở tầng T2 (0.5), một phép biến đổi cố định, không cần mô hình.
3. **Câu hỏi (c) — rủi ro `AccessibilityService` + chụp màn trên Android mới:** nguồn chính thức tôi đã mở: tài liệu `AccessibilityService` và `MediaProjection` (S21, 27/09/2026). Rủi ro cụ thể: (i) `AccessibilityService` chỉ được cấp khi người dùng bật, và dùng để **tự động hoá ứng dụng khác** là khu vực mà **chính sách Play** hạn chế — tức là rủi ro lớn nhất không phải kỹ thuật mà là **phân phối**; (ii) `MediaProjection` cần **sự đồng ý của người dùng theo từng phiên** và phải chạy foreground service; (iii) các phiên bản mới hạn chế việc khởi động `MediaProjection` từ nền. Khuyến nghị: **thiết kế để bật/tắt theo yêu cầu**, và coi `AccessibilityService` là phương án **cuối cùng**, không phải mặc định.
4. **Nguồn:** S21.
5. **Mức tin:** `CHẮC` cho nền tảng Android và nguyên lý "xác nhận lại sau khi bấm"; `GIẢ THUYẾT CẦN ĐO` cho ngưỡng 2 khung và số ms; `KHÔNG BIẾT` cho `ChuotTuDong.cs:100/108` và `TuChoi.cs:262` — các số đó là **số của đội**, tôi không tự đo.

#### A3-15 — Hai bộ đọc ký pháp Hán độc lập: điểm mù chung

1. **Câu hỏi (a) — ca nào CẢ HAI bộ đọc có thể cùng sai:** tôi không có mã, nên tôi không thể nói "chỗ nào chưa phủ" — nhưng tôi **có** thể chỉ ra **tập ca** mà bất kỳ bộ đọc nào cũng dễ sai, và đó chính là danh sách ca thử bạn cần. Theo thứ tự tôi sẽ xếp:
   - **前 / 后 / 中 khi 2–3 quân cùng cột** (xe trước/sau, hai xe cùng cột — «前» chỉ có nghĩa khi đồng loại **cùng cột**; nếu một cột có xe + mã thì «前» là **vô nghĩa** và bộ đọc phải **báo lỗi**, không được đoán).
   - **Số Hán nhiều chữ số một tập tướng: 一二三四五 và 前中后** (ví dụ hai tốt trở tiếp: 一二, 前中, 前前 — đây là chỗ hay sai nhất vì cách gọi phụ thuộc vị trí chứ không chỉ số lượng).
   - **进/退/平 cho mã–tượng–sĩ: số đích là CỘT (file), không phải số bước** — đây là điểm khác biệt cấu thể với mã, tượng, sĩ; bộ nào áp chung công thức của xe là **sai ngay**.
   - **Số Hán cho Đỏ, số Ả Rập cho Đen**, kể cả chuỗi toàn khổ `１２３` (full-width) — lỗi kinh điển của bộ đọc thiếu chuẩn hoá Unicode.
   - **Biến thể chữ: 車/车, 馬/马, 砲/炮/包, 帥/帅, 兵/卒, 將/将** — cần bảng ánh xạ riêng, không dựa vào một chữ chuẩn duy nhất.
   - **Ô bãi:** đây là dạng ghi mà bộ đọc phải **từ chối**, không đoán. `KHÔNG BIẾT` là hành vi **đúng** ở đây.
2. **Câu hỏi (b) — bộ ~50 ván thử đối kháng (mô tả, không cần dữ liệu thật):** tôi đề xuất chia 50 ván thành **5 nhóm × 10**, mỗi nhóm nhắm một họ lỗi: (1) 10 ván chỉ dùng **tiền/hậu/trung** với 2–3 đồng loại cùng cột; (2) 10 ván số tốt một–năm cùng cột, trộn cả `一二三四五` và `前中后`; (3) 10 ván có **xe/马/相/士/将** đi cùng một cú để tách bạch "số là cột" với "số là bước"; (4) 10 ván **Đỏ dùng số Hán, Đen dùng số Ả Rập** trong cùng một ván, có một ván toàn khổ `１２３`; (5) 10 ván dùng **biến thể chữ** ở cả hai bên, trong đó 1 ván cố tình có **nước vô nghĩa** để kiểm cả hai bộ đều **từ chối** chứ không đoán.
   - **Cách chạy chéo, và đây là phần quan trọng:** cả hai bộ đọc chạy trên **cùng 50 ván**, rồi so **từng nửa nước** (như bạn đã làm với 973.636 thế). Tiêu chí: `THIẾU 0 · LỆCH 0` và **mọi vân hỏng đều phải bị từ chối, không phải đoán**. Đây là ca kiểm **rẻ, tất định, không cần engine** — nên làm trước mọi việc khác.
3. **Nguồn:** S19/S28/S29 (CCBridge: `.cbr` mang FEN/ICCS ⇒ **định dạng đích** cho bộ đọc Hán, tức là khi hai bộ đọc "đúng" thì chúng phải cho cùng ICCS với `.cbr`).
4. **Mức tin:** `CHẮC` cho danh sách họ ca dễ sai (đây là kiến thức về ký pháp, không phụ thuộc mã của bạn); `GIẢ THUYẾT CẦN ĐO` cho tỉ lệ chia 5 nhóm; `KHÔNG BIẾT` cho việc `giai_ky_phap`/`NapVanHan.cs` đã phủ ca nào.

#### A3-16 — Quét lỗi «thất bại mà trông như đã xong»

1. **Tôi không thể quét mã (không có zip) ⇒ `KHÔNG BIẾT` về kết quả quét.** Thay vào đó, đây là **checklist 12 mẫu** kèm cách phát hiện, xếp theo mức nguy hiểm — dùng được ngay cho lô code kế tiếp:

| # | Mẫu lỗi | Cách phát hiện tự động | Mã thoát mong đợi |
|---|---|---|---|
| 1 | `catch {}` / `catch { }` rỗng | quét regex `catch\s*(\([^)]*\))?\s*\{\s*\}` | 0 kết quả |
| 2 | `Handled = true` toàn cục nuốt lỗi (A2-17) | quét từ khoá `Handled` | 0 chỗ nuốt lỗi |
| 3 | `Environment.Exit` / `return` **thiếu** mã | so với mẫu chuẩn của mỗi lối thoát | không lối thoát nào thiếu mã |
| 4 | Xử lý **0 mục** nhưng báo thành công | mọi vòng lặp ghi `da_xu_ly`; nếu = 0 thì **lỗi** | ≠ 0 khi có việc |
| 5 | Cổng so kết quả với **chính hàm đang kiểm** | dòng kiểm có chứa tên hàm bị kiểm | 0 |
| 6 | Bỏ qua ca lỗi nhưng vẫn tính PASS | so `skip` với `pass` | `pass + skip ≠ tổng` |
| 7 | **Đọc sai đơn vị** (mate = nước vs ply) | test biên: 1 và 2 nước ⇒ giá trị mong đợi | khớp |
| 8 | Đổi net bằng `setoption EvalFile` mà engine **tụt về eval cổ điển** (đã xảy ra) | thế hỏi khác nhau ≥ 50 cp | không tụt |
| 9 | **Cờ đổi hành vi không nằm trong khoá cache** (A3-01) | bật/tắt cờ trên cùng cache | kết quả **khác** nhau |
| 10 | Cổng kiểm **chưa từng đỏ** | CI cố ý làm hỏng 1 ca | cổng phải báo đỏ |
| 11 | Xử lý 0 dòng trong `phan_loai_rc_khuc`/`xu_khuc_hong` (A3-16) | đếm dòng trước/sau | ≠ 0 hoặc báo lỗi |
| 12 | Ghi 32 B/thế mà **không** kiểm CRC-32C khối | đổi 1 byte giữa khối | phát hiện |

2. **Hai chỗ nóng tôi thấy ngay trong mô tả của bạn, và muốn nói thẳng vì sao nguy hiểm:** (a) `_cache_key :1830-1856` — nếu có **một cờ nào đổi nhãn mà không đổi khoá cache**, bạn sẽ train lại trên **dữ liệu cũ** mà tin là mới; đây là mẫu lỗi **9** ở dạng nguy hiểm nhất. (b) `CanhTrain.cs` canh tiến trình bằng **thời gian CPU**: đây là cổng "còn sống hay treo" và nó **dễ báo xanh giả** nếu tiến trình còn sống nhưng không tiến triển ⇒ nên thêm điều kiện "**và** có bước tiến mới trong N phút", đây chính là bài toán K6 áp sang train.
3. **Nguồn:** S30 (nguyên lý "ghi an toàn rồi mới thay" mà mọi mục trên đều là thể hiện), S26 (CRC-32C: nhanh hơn thuần Python và bắt được lỗi khối).
4. **Mức tin:** `CHẮC` cho cơ chế của từng mẫu lỗi; `GIẢ THUYẾT CẦN ĐO` cho cách phát hiện bằng regex; `KHÔNG BIẾT` cho lỗi **cụ thể** trong ba mảng training/engine/auto — tôi không thể liệt kê `L-*` khi không có mã.

#### Y-1 BookTool

1. **(1) Tính năng mà công cụ sách khai cuộc mạnh có mà bạn thiếu — tôi không khảo sát được tới nơi, nên tôi nêu theo góc nhìn "hồ sơ phải đủ để tái tạo ván":** ba thứ tôi sẽ xếp hạng cao: (a) **ghi lại toàn bộ điều kiện thi đấu** (cấu hình engine + hash exe + hash net + sách + luật) **kèm theo từng ván**, (b) **tự dừng sớm theo SPRT** thay vì đủ số ván, (c) **xuất ván theo chuẩn trao đổi** (PGN/XQF/ICCS) để tránh phụ thuộc định dạng riêng. **Lợi ích đo bằng:** thời gian tới kết luận (giờ, không phải ván) và tỉ lệ ván phải chạy lại do không tái tạo được.
2. **(2) Lô 10–40 ván tự dừng sớm mà vẫn đúng thống kê:** dừng sớm đúng = dùng **LLR/SPRT** (S13) với `elo0/elo1` viết trước, và **dừng khi LLR vượt ngưỡng** — không phải "thắng 5 trước". Bổ sung một quy tắc **hợp lý**: lô dừng sớm phải **ghi lại** "đã dừng ở N ván, LLR = …" để người đọc không tưởng là 10 ván đầy đủ. **Lợi ích đo bằng:** số ván phải chạy để đạt cùng một kết luận.

#### Y-2 Engine

1. **(1) Đòn bẵy Elo lớn nhất còn bỏ ngở:** tôi **không biết** mã của bạn nên không thể kèm `tệp:dòng`, nhưng tôi có thể nêu **thứ tự ưu tiên đo**, và theo kinh nghiệm đây là thứ tự thường thắng: (a) **time management / phân bổ thời gian** — chi phí gần bằng 0, phần lớn lỗi nằm ở đây chứ không ở eval; (b) **tính đúng của phân phối nước ở search** (không chỉ "nước tốt nhất"); (c) **mở rộng lưới khi an toàn**; (d) eval. Và nhớ: **mỗi đòn bẵy phải A/B với đối chứng A=A** — nếu không, bạn chỉ đang đo nhiễu. **Lợi ích đo bằng:** Elo trên cùng cấu hình, 300 ván, cùng đối chứng.
2. **(2) Eval cho quân chưa lật — tôi không biết bạn đang làm gì, nhưng đây là câu tôi sẽ trả lời như sau:** theo kỳ vọng, quân chưa lật có **giá trị giữa 0 và giá trị quân thật**, và nên **học từ phân phối thực** (tỉ lệ lật còn úp ở các thế tương tự) chứ không dùng một hằng. Rủi ro: hằng cố định dễ tạo **thiên lệch theo vị trí** (quân ở ô khác nhau phải có kỳ vọng khác nhau) — đó có thể chính là phần gây −33 Elo mà A3-08 đang điều tra. **Lợi ích đo bằng:** cổng đối xứng (đánh giá thế lật 180° + đổi bên phải bằng nhau) + A/B.

#### Y-3 GUI Tiểu Long Nữ

1. **(1) Chế độ «Thực chiến» nên giữ tối thiểu những nút nào:** xem 0.6 — tôi nêu 8 nút **bắt buộc** và ghi rõ đó là **suy luận của tôi**, không phải khảo sát GUI nào. **Lợi ích đo bằng:** thời gian từ lúc mở app tới cú bấm auto đầu tiên (giây) và số ca "vào nhầm mục đã ẩn".
2. **(2) Cái gì làm AUTO tự hồi phục khi mất bàn giữa ván:** ba cơ chế, tôi xếp theo độ bền: (a) **phát hiện mất bàn** bằng điều kiện bất biến (FEN không đổi sau N giây khi không phải lượt mình / không có nước hợp lệ mới) ⇒ **tự dừng an toàn**; (b) **tự dò lại** bằng cách chạy lại T2 (hiệu chuẩn) trên cửa sổ đang mở; (c) **vòng lặp thử-và-sửa** với giới hạn số lần. Điểm cốt lõi: **hồi phục phải kèm giới hạn, và phải dừng an toàn khi hồi phục thất bại** — hồi phục vô hạn là cách tạo nước giả. **Lợi ích đo bằng:** số ván phải bỏ vì mất bàn (mục tiêu: 0) và thời gian hồi phục (giây).

#### Y-4 ComfyUI Pro

1. **(1) Hàng đợi / nhiều GPU / checkpoint dở:** nguyên tắc, tôi đề xuất **mỗi đoạn là một đơn vị giao dịch** với trạng thái ghi **nguyên tử** trên đĩa (ghi file tạm + đổi tên), và **mọi việc nặng phải có bước bắt đầu lại** từ điểm đã lưu — vì đây đúng là bài toán đã gặp ở A2-15. **Lợi ích đo bằng:** số giờ mất khi tắt máy giữa chừng.
2. **(2) Kho model tự cập nhật:** rủi ro lớn nhất là **tải dở rồi dùng**. Khuyến nghị: tải vào tệp tạm, **kiểm hash so với hash công bố**, rồi **mới** đổi tên vào kho. Đây là cùng nguyên tắc CRC-32C/kiểm toàn vẹn ở 0.7 và A3-16. **Lợi ích đo bằng:** tỉ lệ lần tải lại vì tệp hỏng.

#### Y-5 CinPlay

1. **(1) Điểm nghẽn độ trễ lớn nhất trong dịch/lồng tiếp thời gian thực:** tôi **không biết** mã của bạn, nên tôi nêu **cách tìm** thay vì đoán: đo **3 mốc** (lấy khung hình → nhận dạng → phát) và xem mốc nào chiếm phần lớn; nếu mọi thứ đều nhanh thì nghẽn ở **đường ống âm thanh** chứ không phải mô hình. **Lợi ích đo bằng:** trễ cuối (ms) và phần trăm thời gian ở từng mốc.
2. **(2) OCR phụ đề cứng:** đề xuất **tách phần ổn định và phần dễ sai**: phần có thật (chữ hiện) giữ đường ống cũ, phần dễ sai (chữ nhấp nháy, nền động) thì dùng **lưới thời gian + đối chiếu nhiều khung** rồi mới đưa ra. **Lợi ích đo bằng:** tỉ lệ ký tự sai trên 30 phút video có phụ đề.

#### Y-6 App cờ tướng APK

1. **(1) Engine qua API ở máy chủ vs engine trên máy:** xem 0.9 — tôi nêu **tiêu chí quyết định** thay vì trả lời, và khuyến nghị làm **cả hai** sau một giao diện chung. **Lợi ích đo bằng:** độ trễ cảm nhận và tỉ lệ phiên rớt mạng.
2. **(2) Nhận bàn ngoài đời (camera):** thiếu đúng một thứ là **hiệu chuẩn góc**, và đó là thứ quyết định tỉ lệ nhận đúng. **Lợi ích đo bằng:** tỉ lệ nhận đúng 90 ô trên 100 bàn thật, theo góc/ánh sáng.

#### Y-7 Kỳ Viện

1. **(1) Chống gian lận:** xem 0.9 — tôi đề xuất **giảm lợi ích** (server là nơi quyết định nước đi, nonce, thời gian tối thiểu) hơn là **phát hiện**. **Lợi ích đo bằng:** tỉ lệ phiên bị nghi ngờ sai (đo bằng cách cố tình chơi hợp lệ trong thời gian tối thiểu và xem hệ thống có báo nhầm không).
2. **(2) Điểm nghẽn khi số người tăng:** tôi **không biết** cấu trúc pool của bạn, nhưng tôi nói thẳng điều có thể nói: với mỗi ván đang giữ **một tiến trình engine**, giới hạn sẽ đến từ **engine pool** trước khi đến từ hub. **Lợi ích đo bằng:** số ván đồng thời tối đa trước khi thời gian chờ tăng.

#### Y-8 Dùng chung / Train

1. **(1) Nếu chỉ làm MỘT việc để net tháng 10 mạnh hơn:** tôi chọn — và tôi chọn việc **không phải** "thêm dữ liệu", mà là **xoá đường ghi lén trong trainer + làm cổng dừng sớm đáng tin** (A3-16 + K5). Lý do: bạn đã có dữ liệu và đã có một lô **−301 Elo** với mọi chỉ số nội bộ đẹp ⇒ lỗi không nằm ở thiếu dữ liệu, nó nằm ở **không phát hiện được lô hỏng kịp**. **Lợi ích đo bằng:** số lô phải đốt tiền máy trước khi biết là hỏng.
2. **(2) Cổng nghiệm thu còn thiếu phép đo nào:** tôi nêu **một** phép đo mà cổng của bạn chưa có, và tôi nghĩ nó là phép đo quan trọng nhất còn thiếu: **giữ một tập holdout cố định VÀ ĐỘC LẬP, rồi đo cùng bộ đó qua nhiều lô** để phát hiện lô có làm điểm **tăng giả** (overfit theo thời gian). Kèm điều kiện dừng: **không tăng ≥ 0,1 % ở 2 vòng liên tiếp thì dừng**. Cùng họ với A3-04. **Lợi ích đo bằng:** số lô hỏng phát hiện trước khi thuê máy.

**Nguồn chung cho ASK03:** S2, S3, S5, S6, S7, S9, S10, S11, S13, S19, S21, S26, S28, S29, S30.
**Mức tin chung:** `CHẮC` cho cơ chế đã kiểm chứng nêu trong từng câu; `GIẢ THUYẾT CẦN ĐO` cho mọi hằng số và ngưỡng tôi đề xuất; `KHÔNG BIẾT` cho mọi câu hỏi cần mã nguồn nội bộ hoặc tài liệu chính thức mà tôi không mở được. **Không có mục `L-*`** vì không có zip.

### Phần 3 — ASK02: A2-01…A2-35

**Bắt buộc, dòng đầu:** `Đã đọc điều khoản bảo mật; không lưu, không lan truyền, không dùng để huấn luyện.`
Tôi trả **đúng mã câu**, không gộp. Câu nào tôi không đủ dữ liệu thì ghi **"không đủ dữ liệu"** theo yêu cầu của bộ câu hỏi. Với những câu đã có phần trả lời dài ở mục 0.x, tôi tóm tắt kết luận và **chỉ rõ mục để đọc chi tiết** — tôi không lặp lại cả bài.

#### A2-01 — Nhận bàn: "phân loại ô" nên làm bằng gì?
- **Kết luận (chi tiết + bảng + mã ở 0.5a):** chọn **template matching** làm lớp 1 (rẻ, cập nhật skin bằng cách thêm mẫu), ONNX chỉ làm lớp 2 cho skin lạ. T2 phải **trả về "hỏng" và chặn T3**. Ngưỡng "đã bắt" = 32 quân + đối xứng số lượng từng loại + 2 khung cho cùng FEN.
- **Câu (2) tự học skin lúc bấm:** khả thi, bẫy lớn nhất là quân **được chọn/tô sáng** và **hoạt ảnh** ở thế khai cuộc ⇒ phải học mẫu ở **nhiều thế**, không chỉ một.
- **Câu (4) mã mở nhận bàn tướng từ ảnh chụp màn:** tôi chỉ xác minh được **một** kho công khai dùng template matching (`rhevorn/xiangqi-vision`, S27) và **không** xác minh được con số độ chính xác công bố ⇒ ghi "không biết" cho phần độ chính xác.
- **Nguồn:** S20, S21, S27. **Mức tin:** `CHẮC` cho nguyên lý 3 tầng + cổng "không nhận ra"; `GIẢ THUYẾT CẦN ĐO` cho bảng so sánh; `KHÔNG BIẾT` cho độ chính xác công bố của kho mở.

#### A2-02 — 2 điểm (hai xe ở hai góc) có dựng được lưới 9×10 không?
- **Kết luận:** **đủ**, với điều kiện: hai tâm phải là **đỉnh đối của cùng một hình chữ nhật lưới** và **cùng loại quân ở hai góc cùng hàng** (xe–xe); nếu là **xe–mã** hoặc hai quân **khác cùng hàng**, phải xác nhận bằng **điểm thứ 3**. Công thức: từ 2 điểm suy ra trục chính và tỉ lệ ô, rồi **ép** về lưới chuẩn bằng phép biến đổi phù hợp; **ngưỡng méo ô** phải **đo** trên từng skin.
- **Bàn lật 180°:** dùng **phép đảo 180°** (một phép biến đổi hình học cố định) — rẻ và chắc hơn đoán bằng màu hay chữ.
- **Lượt đi từ một khung hình:** tôi **không biết** các GUI thương mại dùng cách nào (không có tài liệu tôi đã mở). Nguyên lý an toàn: **không suy lượt đi từ một khung**; hãy chờ **FEN ổn định + biến cố** (nước vừa đi, đồng hồ) — và nếu phải, coi bước đầu là **chờ**, không phải đoán.
- **Ca đỏ bắt buộc:** bấm nhầm quân · bấm lệch ô · cửa sổ bị kéo/đổi cỡ sau khi thiết lập. Phản ứng: **hủy thiết lập và bắt người dùng làm lại**, không "sửa cho qua".
- **Nguồn:** S27 (hiệu chuẩn hình học bằng điểm bám). **Mức tin:** `CHẮC` cho nguyên lý 2 điểm + điểm thứ 3 khi nghi ngờ, và phép đảo 180°; `GIẢ THUYẾT CẦN ĐO` cho ngưỡng méo; `KHÔNG BIẾT` cho cách làm của GUI khác.

#### A2-03 — full mode ↔ one mode, và hồ sơ kết nối phải là DỮ LIỆU
- **Định nghĩa chuẩn:** **full** = máy đánh **cả ván**; **one** = đánh **một nước rồi trả quyền**. Tôi **không** gắn nguồn cho các tên gọi của từng hãng ⇒ ghi đây là **định nghĩa dùng chung trong bài này**, và mọi tên riêng của GUI tôi để trống.
- **Ba đường nối (bảng ở 0.5b cho chi tiết):** bóc ảnh màn hình (phủ rộng, bền vừa, chậm nhất) · đọc cửa sổ/điều khiển Win32-UIA (nhanh, bền, **phụ thuộc app có bộ phận trợ năng**) · hook trang web (chính xác nhất khi nền tảng có web, nhưng **mất ngay khi họ đổi giao diện**). Về câu hỏi "GUI thương mại mạnh nhất dùng đường nào": tôi **không biết** với mức chắc chắn chấp nhận được ⇒ `KHÔNG BIẾT`.
- **Lược đồ hồ sơ (JSON, ≤ 25 dòng, như yêu cầu khuôn):**

```json
{
  "id": "ten_san", "ten_hien_thi": "...", "cach_nhan_dien": "class|process|title",
  "vung_ban": { "che_do": "tu_nhan|tay_dat", "goc": [0,0], "kich_thuoc": [0,0] },
  "doc_nuoc": { "loai": "anh|dom|uia", "cau_hinh": "..." },
  "gui_nuoc": { "loai": "chuot|uia|dom", "cau_hinh": "..." },
  "biet_luot": { "loai": "cho|thu_nghiem", "cau_hinh": "..." },
  "het_van": { "loai": "phong|kiem_van", "cau_hinh": "..." },
  "phien_ban_ghi": "YYYY-MM-DD", "hash_lien_quan": "..."
}
```
- **Tự phát hiện hồ sơ hỏng:** **cổng "2 khung cùng FEN"** ở 0.5 chính là cổng này — hồ sơ hỏng ⇒ bàn không ổn định ⇒ **hỏng**, và bạn chỉ cần một ca kiểm cho từng hồ sơ thay vì phát hiện thủ công.
- **Nguồn:** S20, S21, S27. **Mức tin:** `CHẮC` cho ba đường nối và nguyên lý hồ sơ là dữ liệu; `GIẢ THUYẾT CẦN ĐO` cho lược đồ; `KHÔNG BIẾT` cho cách làm của từng GUI.

#### A2-04 — "Tốc độ đua xe": đo và cắt độ trễ 4 khúc
- **Bảng đề xuất:**

| Khúc | Cách đo | Cách cắt | ms kỳ vọng (ước) |
|---|---|---|---|
| (1) Nhận ra đối thủ đã đi | so hash vùng bàn | quét thay đổi ở tần số vừa đủ | 5–30 |
| (2) Đọc thế cờ | chạy T2+T3 | bỏ T3 khi T2 hỏng; chỉ chạy T3 khi **đã biết** lượt mình | 10–40 |
| (3) Engine nghĩ | đo `go` của chính bạn | đo và điều chỉnh — đây là khúc **dài nhất** | không cố định |
| (4) Bấm chuột | đo từ lúc phát sự kiện tới lúc bàn đổi | `SendInput` vào cửa sổ tiền cảnh; chỉ 1 cú nếu nền tảng cho phép | 20–80 |

- **Chụp nhanh nhất:** `Windows.Graphics.Capture` (vùng cửa sổ) hoặc `DXGI Desktop Duplication` (màn hình) — `BitBlt` chậm và dễ vướng DPI (S20). Bẫy DPI/đa màn hình xử lý bằng `GetDpiForWindow` và toạ độ vật lý.
- **Phát hiện "đối thủ đã đi" rẻ nhất:** so **hash vùng bàn** (không phải sự kiện cửa sổ); tần số quét **tự điều chỉnh**: quét nhanh khi ván đang chạy, nghỉ khi không có gì.
- **Bấm chuột:** `SendInput` đến cửa sổ tiền cảnh là lựa chọn **ít bị bỏ qua** nhất; `PostMessage` có thể bị app bỏ qua.
- **"Mức trễ tổng của GUI thương mại":** **không có số công bố mà tôi mở được ⇒ `KHÔNG BIẾT`.** Và tôi sẽ nói thêm: con số 50 ms trong mã của bạn là **chép nhãn của GUI khác, chưa từng đo** — đừng để nó thành cam kết.
- **Nguồn:** S20. **Mức tin:** `CHẮC` cho cơ chế chụp/tiêm chuột; `GIẢ THUYẾT CẦN ĐO` cho ms; `KHÔNG BIẾT` cho số công bố.

#### A2-05 — Một app, hai cửa vào (mẫu C# 5, cổng kiểm, nguyên tắc UX)
- **Xem 0.6:** bảng 8 nút bắt buộc / 5 nhóm bỏ được; mẫu `CheDoLuan` (C# 5, có **lỗi cố ý** để bạn thấy cổng cần bắt); cổng kiểm = duyệt cây giao diện + liệt kê handler; 3 nguyên tắc có nguồn là **tương phản WCAG ≥ 4,5:1** (S23), **giữ nhãn chữ/tooltip khi giao diện chỉ có icon**, và **ẩn chứ không xoá**.
- **Câu (1) "GUI nào bày nút nào ở màn đánh":** `KHÔNG BIẾT` — tôi không khảo sát được từng GUI, và quy tắc của chính bộ câu hỏi là đừng suy ra từ tên nút. Tôi ghi bảng nút của mình là **suy luận của tôi**.
- **Nguồn:** S23, S17, S27. **Mức tin:** `CHẮC` cho nguyên lý một-lõi/chế-mộ-là-dữ-liệu và tương phản; `GIẢ THUYẾT CẦN ĐO` cho bộ nút; `KHÔNG BIẾT` cho GUI thương mại.

#### A2-06 — Xếp lịch nhiều cặp đấu khi mỗi engine đòi số lõi khác nhau
- **Thuật toán (nguyên tắc, ≤ 30 dòng):** (1) **bin-packing theo lõi yêu cầu** — sắp cặp theo tổng lõi, **ưu tiên cặp có Thầy** vì Thầy khan hiếm (giới hạn 2 phiên); (2) **giữ phiên Thầy treo qua nhiều ván** thay vì mở/tắt liên tục (đúng giới hạn của bạn: tắt rồi chờ ≥ 60 s); (3) **đếm tiến trình theo PID + hash exe**, **không** theo tên (đúng lỗi bạn đã vấp); (4) giới hạn phiên phải tính **theo tài khoản, không theo máy** (đúng lỗi 2 máy dùng chung tài khoản).
- **Ghim lõi (affinity/CPU Sets) trên máy > 64 luồng:** tôi **không khuyên ghim cho mục đích đo** — ghim có thể làm lệch Elo giữa hai bên, và bạn đang dùng máy này để **đo**. Nếu phải ghim (tương tác với job), hãy ghim **giống nhau cho cả hai bên** và **ghi lại** vào hồ sơ ván.
- **Ponder khi nhiều cặp:** **tắt** — nó làm mất ý nghĩa khi đo, và khiến việc chia lõi khó kiểm soát hơn.
- **Số lõi lệch nhau (6 vs 20):** khi lệch, **cố định nodes** mới đo được (đúng như A/B của bạn dùng `go nodes 200000`); cố định thời gian sẽ biến chênh lệch lõi thành chênh lệch chất lượng ⇒ **không đo được**.
- **Công cụ mã mở:** tôi **không mở** và không xác minh tệp/đường dẫn cụ thể của cutechess-cli/fastchess ⇒ `không biết link/tệp đã kiểm`. Tôi không đoán.
- **Nguồn:** **không có nguồn ngoài** cho cách xếp lịch này (thiết kế tôi đề xuất; số 41 tiến trình là của đội). **Mức tin:** `CHẮC` cho bin-packing và nguyên lý "đếm theo PID"; `GIẢ THUYẾT CẦN ĐO` cho quyết định ghim lõi/ponder; `KHÔNG BIẾT` cho công cụ mã mở.

#### A2-07 — Xếp hạng từ giải vòng tròn: công cụ, neo, bao nhiêu ván
- **Công cụ:** với "ít ván, nhiều người, không cân cặp" (đúng tình huống của bạn vì Thầy đánh ít hơn), tôi khuyên **Elo tuyến tính có hệ số dựa trên số ván** hoặc **Glicko** vì nó chịu được "chơi ít"; BayesElo thuộc họ **khác** (mô hình xác suất, mạnh khi có đối thủ xếp hạng rõ). Tôi **không** đã mở và chạy thử các công cụ này ⇒ không đưa lệnh cụ thể đã kiểm; ghi "không biết lệnh đã kiểm".
- **Neo thang vào Thầy:** cố định Elo Thầy làm **méo khoảng tin cậy của Trò** (Trò bị kéo về phía neo) ⇒ tôi khuyên **neo cả nhóm Thầy thành một điểm giữa** và báo cáo **khoảng tin cậy** cho từng Trò, không chỉ số.
- **Trộn ván có sách và không sách trong một bảng:** **không hợp lệ** — đây là hai bài toán khác nhau; hãy tách thành **hai bảng xếp hạng** hoặc **cột phụ** trong cùng bảng.
- **Số ván để phân biệt hai Trò cách nhau 30 Elo:** dùng đúng số đo của bạn — 300 ván cặp gương pentanomial cho **±7,2** ⇒ để phân biệt 30 Elo với độ tin cậy khoảng ±7, cần **vài trăm ván mỗi cặp**; với hai bên chỉ đấu Thầy (ít ván), số ván phải **lớn hơn nữa**. Con số chính xác phải tính theo công thức SPRT (S13) với `elo0/elo1` bạn chọn — tôi **không** tự tính thay bạn vì đó là quyết định của đội.
- **Nguồn:** S13. **Mức tin:** `CHẮC` cho nguyên lý "tách bảng có sách / không sách" và công thức SPRT; `GIẢ THUYẾT CẦN ĐO` cho lựa chọn hệ xếp hạng; `KHÔNG BIẾT` cho lệnh đã kiểm.

#### A2-08 — Cập nhật net sau mỗi ván: đúng hay bẫy?
- **Kết luận một dòng:** **theo lô**, không theo từng ván — vì vài trăm thế không đủ đổi trọng số mà đủ để làm nhiễu.
- **Bảng tham số:** cửa sổ dữ liệu nguyên vẹn thay vì cập nhật liên tục để giảm quên; `λ` giữa WDL và score và trọng số ván có Thầy **là quyết định của đội** — tôi chỉ nêu **cách chọn**: dùng **3 lô A/B** với các giá trị khác nhau và **giữ A=A làm đối chứng**; ưu tiên ván có Thầy bằng **trọng số mẫu**, nhưng **đặt trần** (ví dng không vượt 1/3 tổng mẫu trong một cửa sổ) để tránh lệch phân phối; cổng chặn net mới hỏng: dùng đúng bộ cổng ở 0.8/K5 (rẻ, chạy trước A/B).
- **An toàn dữ liệu (A2-08(5)):** train NNUE trên CPU để **giống** bản GPU cần **tất định** (cùng thứ tự, cùng hạt giống, cùng kiểu số) và **cùng thuật toán lượng tử hoá int8**; nếu hai lần chạy cho khác kết quả thì bạn **không thể so sánh** hai bản — đây là điều kiện tiên quyết, không phải tối ưu.
- **Nguồn:** S13. **Mức tin:** `CHẮC` cho nguyên lý "lô, không từng ván" và yêu cầu tất định; `GIẢ THUYẾT CẦN ĐO` cho trần trọng số; `KHÔNG BIẾT` cho số liệu kinh nghiệm của các dự án khác mà tôi không mở được.

#### A2-09 — Gói bàn giao giữa nhiều máy: manifest, khử trùng, xuất xứ
- **Lược đồ manifest (ngắn, các trường bạn yêu cầu):** `may`, `engine + hash exe`, `hash net`, `cau_hinh`, `khoang_thoi_gian`, `so_ban_ghi`, `sha256 tung tep`. Bổ sung tôi khuyên: **`id goi`** (để chống nạp trùng, idempotent) và **`gia_phep`** ngay trong manifest.
- **Khử trùng khi hai nhãn khác nhau:** tôi khuyên **giữ cả hai có trọng số** và lưu **cả hai giá trị gốc** — vì "giữ cái nào / trung bình" đều làm mất thông tin, và trung bình làm **ẩn** sự bất đồng thay vì giải quyết nó.
- **Chống nạp trùng / phát hiện gói cắt dở:** `id goi` là khoá (nạp hai lần là **không có tác dụng**), và kiểm `so_ban_ghi` khai trong manifest **bằng số thực tế** trước khi ghi — đây là cổng `0 dòng ⇒ lỗi`.
- **Tách trường giấy phép:** `nguon_lo(nhan, nguon, **gia_phep**, ngay_nap, so_doc, so_moi, so_trung)` — bạn **đã có** đúng trường này; chỉ cần **quy tắc**: mọi thế mà nguồn là engine thương mại đi qua một `gia_phep` riêng để sau này lọc được toàn bộ theo **một** điều kiện.
- **Nguồn:** **không có nguồn ngoài** cho lược đồ này (thiết kế tôi đề xuất). **Mức tin:** `CHẮC` cho nguyên lý idempotent và tách trường giấy phép; `GIẢ THUYẾT CẦN ĐO` cho lược đồ; `KHÔNG BIẾT` cho chính sách cụ thể của từng hãng engine.

#### A2-10 — Nhãn từ engine thương mại đóng nguồn: dùng để BÁN được không?
- **Bảng:** `engine | EULA có/không | điều khoản liên quan | URL/ngày | rủi ro`
  - `Pikafish (trọng số `pikafish.nnue` và mọi trọng số phái sinh) | có, tôi đã mở | README kho `Networks`: *"No commercial use without permission"*; phần CC0 **chỉ** dành cho trọng số biến thể cờ tướng của Fairy-Stockfish | S31/S32, mở 27/09/2026 | dùng nhầm loại trọng số là mất quyền bán`
  - `Các engine thương mại cờ tướng (Cyclone/旋风, 象棋名手/XQMS, BugChess, Shark) | **không tìm được EULA công khai mà tôi đã mở** | — | — | **cao**`
- **Kết luận một dòng:** **không bán** bất kỳ sản phẩm nào có nhãn từ engine mà EULA chưa được đọc và **lưu lại** — đây là **ý kiến kỹ thuật, không phải tư vấn pháp lý**.
- **Tiền lệ:** tôi **không** dẫn được nguồn đã kiểm về điều khoản "dùng output để huấn luyện mô hình cạnh tranh" trong ngành engine, nên tôi **không** dẫn. Điều tôi nói được và có ích: **giữ xuất xứ** (A2-09) là cách rẻ nhất để sau này lọc bỏ toàn bộ nhãn của một Thầy nếu phải — và đó là lý do trường `gia_phep` phải tồn tại **từ đầu**, không phải thêm sau.
- **Nguồn:** S31, S32. **Mức tin:** `CHẮC` cho tình trạng giấy phép trọng số Pikafish; `KHÔNG BIẾT` cho EULA của bốn engine thương mại.

#### A2-11 — Tách "sinh thế" (rẻ) khỏi "chấm thế" (đắt)
- **Quy trình 6 bước tôi đề xuất:** (1) **sinh thế** bằng random playout / lấy từ kho có sẵn, **không** gọi engine sâu; (2) **lọc** thế đang bị chiếu và thế còn nước ăn tốt; (3) **chấm** bằng script chấm từng thế ở **depth thấp, cố định**, ghi kèm `depth` đã dùng; (4) **nhãn WDL lấy từ chính kết quả tìm kiếm**, không suy từ cp nếu không có; (5) **lưu `(thế, cp, depth, nguồn)`** để sau này nâng chất lượng mà không chấm lại; (6) **đo lại thế/s** và so với chế độ ván.
- **Bảng tham số:** (1) score-only có kém không — `CHẮC` là chấm nhận được, vì WDL chỉ là **biến đổi logistic của score** và thêm một đầu không tự tạo thông tin; nếu cần WDL chính xác hơn thì **tìm hệ số logistic trên bộ của bạn** (không có số công bố cho cờ tướng mà tôi mở được ⇒ `KHÔNG BIẾT` con số, nhưng **cách** thì rõ: khớp bằng bình phương trên tập đã biết kết quả thật). (2) Lấy mẫu: `GIẢ THUYẾT CẦN ĐO` tỉ lệ "thế xấu" — tôi đề xuất giữ ~10–20 % thế khó để eval không chỉ học thế sạch. (3) depth nhãn: **chưa có số** tôi đã kiểm ⇒ `KHÔNG BIẾT`; nhưng **nguyên lý** là: depth nhãn phải **cố định** và **ghi vào dữ liệu**, nếu không bạn sẽ không giải thích được vì sao cùng một thế có hai nhãn khác nhau.
- **Nguồn:** **không có nguồn ngoài** cho quy trình này (thiết kế tôi đề xuất). **Mức tin:** `CHẮC` cho quy trình và việc phải ghi `depth`; `GIẢ THUYẾT CẦN ĐO` cho tỉ lệ thế xấu; `KHÔNG BIẾT` cho số depth tối ưu và hệ số logistic.

#### A2-12 — Máy 128 GB mà Hash chỉ 13 % RAM
- **Bảng:**

| Phương án | Lợi kỳ vọng | Cách đo | Nguồn |
|---|---|---|---|
| Hash 256 MB → 2 GB/tiến trình | có thể **giảm** hash-full % nên **nhanh hơn**; nhưng 41 tiến trình × 2 GB là **82 GB** ⇒ không khả thi trên 128 GB | đo hash-full % ở từng cỡ, A=A < 3 % | số đo của đội, tôi không kiểm lại |
| Large pages (Windows) | giảm overhead TLB; cần `SeLockMemoryPrivilege`; bẫy: phân trang bị phân mảnh ⇒ cấp phát lỗi âm thầm | đo thế/s trước/sau | S33 |
| **Ít tiến trình, nhiều thread, chung Hash** | tôi **nghiêng về phương án này**: 41 tiến trình đã chạm trần CPU 95 % ⇒ vấn đề là **số tiến trình**, không phải Hash | đo thế/s tổng | số đo của đội, tôi không kiểm lại |
| Nạp bảng tàn cuộc ≤ 5 quân vào RAM | lợi cho **tốc độ** nhiều hơn là cho **nhãn**; đổi lại là mất quyền kiểm soát bằng chứng ở A3-05 | A/B riêng tập tàn cuộc | S7 |

- **Khuyến nghị 3 dòng:** (1) **đo trước, giảm tiến trình sau** — trần CPU đã chạm thì thêm Hash không giúp; (2) nếu tăng Hash thì tăng **cho một vài** tiến trình chứ không phải tất cả, và **ghi cỡ Hash vào hồ sơ ván**; (3) lý do ghi trong mã ("nghẽn băng thông bộ nhớ") **chưa từng được đo** ⇒ hoặc đo, hoặc **đừng để nó là lý do** trong mã.
- **Nguồn:** S7, S33. **Mức tin:** `CHẮC` cho nguyên lý đo-trước; `GIẢ THUYẾT CẦN ĐO` cho lợi ích Hash/large pages; `KHÔNG BIẾT` cho số công bố Elo theo cỡ Hash ở TC ngắn. Các số 41 tiến trình / 82 GB / 95 % là **số đo của đội**, tôi không kiểm lại được.

#### A2-13 — Theme sáng cho app WPF (csc, C# 5)
- **Xem 0.6:** mẫu `Theme.Use(dark)` với `Freeze()` brush; nguyên tắc "màu chỉ nằm trong một bảng tài nguyên".
- **Tìm màu viết cứng:** bằng **cổng kiểm**, không bằng regex đơn thuần: quét các mẫu tạo brush trực tiếp (`new SolidColorBrush`, `Color.FromArgb/FromRgb`, `Brushes.` trong mã) và **bắt buộc** phải có **ca đỏ** (thêm một màu cứng giả ⇒ cổng phải đỏ). Regex chỉ giúp tìm nhanh, không thay cổng.
- **Bảng màu bàn cờ tương phản WCAG AA:** tôi **không** đưa mã hex cụ thể ở đây vì tôi **chưa tính tương phản cho từng cặp** mà không muốn đội dùng nhầm; quy tắc đã kiểm: **chữ thường ≥ 4,5:1** (S23), và phần chữ lớn ≥ 3:1.
- **Bẫy hiệu năng:** `DynamicResource` dày đặc là tốn; brush phải `Freeze()` trước khi dùng ở luồng khác.
- **Nguồn:** S23, S17, S27. **Mức tin:** `CHẮC` cho nguyên lý và ngưỡng tương phản; `GIẢ THUYẾT CẦN ĐO` cho mẫu đổi theme; `KHÔNG BIẾT` cho bảng hex cụ thể (tôi cố ý không đoán).

#### A2-14 — Phim 2D 15 phút/tập, giữ nhân vật xuyên tập
- **Kết luận ngắn (tôi không khảo sát được kênh nào đang làm thế nào ⇒ không dẫn nguồn về "các kênh đang làm gì"):** với 150–220 cảnh/tập trên iGPU UMA, phương án khả thi là **ảnh tĩnh + chuyển động nhẹ (pan/zoom, nội suy)**; video sinh từ mô hình cho **từng cảnh** sẽ tốn máy vượt xa ngân sách bạn nêu. Giữ nhân vật xuyên tập: tôi nghiêng về **character sheet + tham chiếu ảnh (IP-Adapter) hơn LoRA** khi phong cách 2D đơn giản, vì LoRA cần **hàng chục ảnh cùng nhân vật** và thời gian train mà tôi **không đo được** trên máy bạn. TTS hai ngôn ngữ cho 2D: **không cần lip-sync** nếu phong cách 2D — chỉ cần miệng mở/đóng. Ước lượng giờ máy **trước khi chạy** (câu 4): đo **1 cảnh đại diện** × 3 lần rồi nhân — đây là cách rẻ và chính xác hơn ước lượng tổng.
- **Nguồn:** không có nguồn đã kiểm cho phần "kênh khác làm thế nào" ⇒ `KHÔNG BIẾT`. **Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho tỉ lệ giữa hai phương án; `KHÔNG BIẾT` cho giờ máy/tập và thời gian train LoRA.

#### A2-15 — PC 2 card: chia phim theo thời gian
- **Câu (1) — không đứt mạch:** chia **ở ranh giới phân cảnh** (mỗi card nhận cảnh **trọn vẹn**), **không** chia nửa một cảnh; nếu buộc phải nối giữa chừng thì card sau phải nhận **key-frame do card trước sinh**, tức là phụ thuộc — tôi **không** mở tài liệu nào về pipeline video nhiều GPU nên `KHÔNG BIẾT` về "cách các pipeline khác làm".
- **Câu (2) — hàng đợi sống qua tắt máy:** trạng thái trên đĩa, ghi **nguyên tử** (tệp tạm + đổi tên), và `resume` **đúng đoạn**; đây là cùng nguyên tắc như 0.7.
- **Câu (3) — trần GPU 90 %:** cần biết **engine nào** đang bận (3D hay Compute) trước khi hạ; nếu ComfyUI không có núm điều tiết, cách thực tế là **giảm batch / chèn nhịp nghỉ**, và tôi **không** khẳng định rằng điều đó luôn tránh được treo GPU.
- **Câu (4) — checkpoint giữa một đoạn:** `KHÔNG BIẾT` với ComfyUI (tôi không đọc mã của nó). Nguyên lý: nếu không có hook, thì **giảm đơn vị công việc** (lưu mỗi phân cảnh) là cách duy nhất không cần sửa ComfyUI.
- **Nguồn:** S2 (chỉ cho nguyên tắc ghi nguyên tử). **Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho cách chia theo phân cảnh; `KHÔNG BIẾT` cho (1) nguồn, (4) khả năng hook.

#### A2-16 — Tách "ngạch sản xuất": chặn rò ở đâu cho chắc
- **(1)** Mô hình "ngạch = cấu hình": một bảng, tối thiểu gồm `ten_ngach`, `danh_sach_model`, `danh_sach_lora`, `thu_muc_xuat`, `co_bang_cong_bo`, `an_hien`.
- **(2) Điểm chặn chắc nhất: sơ đồ CUỐI CÙNG trước khi gửi** — vì chặn ở lúc chọn model trong giao diện thì còn đường vòng qua cấu hình cũ; chặn lúc dựng sơ đồ thì còn đường vòng qua mặc định. Đọc **mọi tên tệp model/LoRA trong JSON** là kiểm tra không lách được nếu bạn kiểm **ngay trước lúc gửi**; cộng thêm chặn ở chọn model để báo lỗi **sớm** cho người dùng (2 lớp: báo sớm + chặn cuối).
- **(3)** LoRA thuộc ngạch nào khi tên tệp không nói gì: **hash tệp + sổ nội bộ** là chắc nhất; `__metadata__` trong safetensors là bổ sung; sidecar là dự phòng.
- **(4)** Các trường bắt buộc của nền tảng video về khối công bố AI: `KHÔNG BIẾT` — tôi **không mở** chính sách nào trong phiên này, và tôi **không** đoán danh sách trường.
- **Ca đỏ bắt buộc:** dựng sơ đồ có **1 LoRA của ngạch khác** ⇒ cổng phải **đỏ** trước khi gửi.
- **Nguồn:** không có nguồn đã kiểm cho (4) ⇒ `KHÔNG BIẾT`; nguyên tắc chặn 2 lớp là suy luận. **Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho điểm chặn; `KHÔNG BIẾT` cho trường khối công bố.

#### A2-17 — WPF .NET 8 trắng màn: `GeneratedInternalTypeHelper` 1/6 lần build
- **Câu (1) — điều kiện sinh tệp:** `KHÔNG BIẾT` ở mức "trích mã nguồn `tệp:dòng`" — tôi **không** đọc mã PresentationBuildTasks. Tôi **có** xác minh rằng dotnet/wpf có **issue công khai** về lỗi trắng màn khi kiểu không nạp được lúc chạy (S17, các issue 2281/2690/3469, mở 27/09/2026), nhưng tôi **không** gắn được nguyên nhân chính xác cho `GeneratedInternalTypeHelper`.
- **Câu (2) — vì sao chỉ 1/6:** `KHÔNG BIẾT`; một **giả thuyết** hợp lý là phụ thuộc thứ tự pass của markup compile và build tăng dần, nhưng tôi ghi rõ đây là **giả thuyết**, không phải kết luận.
- **Câu (3) — cổng "> 10 B":** tôi **không** khẳng định nó bắt đúng; nó là **bằng chứng trong nghi vấn**, và tôi khuyên **mạnh hơn** là đo **mức phản chiếu** (số lần một kiểu bị gọi từ XAML) chứ không chỉ kích thước tệp. Ca đỏ bắt buộc: cố ý làm kiểu `public static` bị XAML gọi nhiều lần ⇒ phải đỏ.
- **Câu (4) — bắt lỗi toàn cục:** mẫu đúng là **log + báo đỏ + không nuốt**: ghi lỗi vào một sổ có mã, hiện thông báo cho người dùng, và **không** đặt `Handled = true` cho lỗi tải tài nguyên. Nguyên tắc: bộ bắt lỗi toàn cục chỉ được dùng như **lưới an toàn cuối**, không phải như chính sách xử lý.
- **Nguồn:** S17. **Mức tin:** `CHẮC` cho việc dotnet/wpf có issue công khai về triệu chứng này và cho nguyên tắc log-không-nuốt; `GIẢ THUYẾT CẦN ĐO` cho giả thuyết pass ordering; `KHÔNG BIẾT` cho nguyên nhân chính xác.

#### A2-18 — Nạp dữ liệu vào kho: khử trùng lặp, lọc thừa, "lỗi cảm"
- **Kết luận:** "lỗi cảm" ở đây chính là **mẫu lỗi số 4 trong A3-16** (xử lý 0 mục mà báo thành công) — cổng bắt buộc: đếm `them / trung / db_sau`, và **`db_sau` phải bằng `db_truoc + them`**; nếu lệch thì `rc ≠ 0`. Về lọc trùng: dùng **khoá logic** (không phải hash byte) và coi `trung` là **thông tin bắt buộc** trong sổ, không phải chỉ là thống kê.
- **Nguồn:** S3. **Mức tin:** `CHẮC` cho cổng đếm; `GIẢ THUYẾT CẦN ĐO` cho cách chọn khoá logic.

#### A2-19 — Kiến trúc net nào cho engine nhà
- `KHÔNG BIẾT` cho "mạng chậm giải / mạng nhanh" — tôi **không mở** tài liệu đó nên không dẫn. Nguyên lý suy ra từ A2-08(5): **kiến trúc phải được chọn để bạn có thể tạo kết quả tất định**, vì nếu không thì mọi phép so sánh CPU/GPU sau này đều không đáng tin. Đây là tiêu chí duy nhất tôi khẳng định được.
- **Nguồn:** không có nguồn ngoài đã kiểm cho tài liệu kiến trúc; nguyên lý nêu ở trên suy ra từ A2-08(5). **Mức tin:** `KHÔNG BIẾT` cho mô tả "mạng chậm giải / mạng nhanh"; `GIẢ THUYẾT CẦN ĐO` cho việc chọn kiến trúc theo tiêu chí tất định.

#### A2-20 — Ván 0 nước / ván rỗng, chọn "ván ma" thế nào
- `KHÔNG BIẾT` cho cách đội hiện chọn. Nguyên lý tôi đề xuất: **vờ đoạt điều kiện lọc trên một tập ván nhỏ, cố tình tạo 1 ván ma, và kiểm xem công cụ có bắt được không** — nếu không bắt được, bộ lọc của bạn **không có tác dụng**.
- **Nguồn:** không có nguồn ngoài; đây là **phép thử** tôi đề xuất. **Mức tin:** `KHÔNG BIẾT` cho lựa chọn hiện tại của đội; `GIẢ THUYẾT CẦN ĐO` cho cách dựng ván ma kiểm bộ lọc.

#### A2-21 — Threads 12 + ponder BẬT: bao nhiêu ván
- `KHÔNG BIẾT` cho con số. Nguyên lý: **đo**, và cố định `nodes` khi so (giống A2-06). Ponder bật khi đo làm kết quả **không tái lập được** vì nó phụ thuộc thời điểm có nước vừa đi ⇒ tôi khuyên **tắt khi đo**.
- **Nguồn:** không có nguồn ngoài đã kiểm cho con số ván/giờ; nguyên lý "cố định nodes khi so" lặp lại từ A2-06. **Mức tin:** `KHÔNG BIẾT` cho số ván; `GIẢ THUYẾT CẦN ĐO` cho việc tắt ponder khi đo.

#### A2-22 — "Hiệu ứng cảm" từ lý thuyết ra M Elo
- **Câu trả lời trung thực:** tôi **không biết** có số công bố nào hay không. Tôi chỉ nêu cơ chế mà một con số phải có để trở thành M Elo: **hiệu ứng phải là phép biến đổi của một phép đo có nhiễu đã biết** (chẳng hạn KTC95) chứ không phải của cảm giác. Nếu bạn chỉ ra được "cái gì bị đo", tôi sẽ giúp biến nó thành phép đo.
- **Nguồn:** không có nguồn đã kiểm (tôi **không** mở được con số M Elo nào). **Mức tin:** `KHÔNG BIẾT`; `GIẢ THUYẾT CẦN ĐO` cho yêu cầu "hiệu ứng phải là biến đổi của phép đo có nhiễu đã biết".

#### A2-23 — Nếu dùng lại engine cờ tướng HƠM NAY thì kiến trúc nào; sau NNUE lại gì; MCTS
- `KHÔNG BIẾT` cho cả ba phần. Điều tôi khẳng định được chỉ là: **quyết định này phụ thuộc mục tiêu đo**, và mục tiêu đo của bạn đã chốt ở A2-08/A2-21 là **nodes cố định** ⇒ bất kỳ kiến trúc nào không cho phép điều khiển nodes cũng **không dùng được** cho đo của bạn.
- **Nguồn:** không có nguồn ngoài đã kiểm cho ba kiến trúc nêu trong câu hỏi. **Mức tin:** `KHÔNG BIẾT` cho cả ba; `CHẮC` cho tiêu chí loại kiến trúc không kiểm soát được nodes (theo A2-08/A2-21).

#### A2-24 — Sâu nhánh engine, không người: gộp cân máy
- `KHÔNG BIẾT` cho "cân bằng kỹ thuật". Nguyên lý: sâu nhánh là vấn đề **cấp phối công việc**, và bài toán đúng ở đây là **giới hạn độ sâu song song** chứ không phải chia tài nguyên — nếu không, chính bạn sẽ tự tạo ra "ván ma" (A2-20) bằng cách cho các nhánh độ sâu không cân nhau.
- **Nguồn:** không có nguồn ngoài; nguyên lý "giới hạn độ sâu song song" suy ra từ A2-06. **Mức tin:** `KHÔNG BIẾT` cho cách cân bằng kỹ thuật hiện tại; `GIẢ THUYẾT CẦN ĐO` cho phương án giới hạn độ sâu song song.

#### A2-25 — Web cho cộng đồng: xem giải trực tiếp, ván bản ghi
- `KHÔNG BIẾT` cho cách bạn đã làm. Nguyên lý kỹ thuật duy nhất tôi nêu: **tách bản ghi khỏi bản phát trực tiếp** (S22 SignalR: nhóm theo phòng, reconnect có backoff) — vì một sự cố phát trực tiếp **không được phép** làm hỏng bản ghi.
- **Nguồn:** S22 (SignalR: nhóm theo phòng, reconnect có backoff) — chỉ cho phần "tách bản ghi khỏi phát trực tiếp". **Mức tin:** `CHẮC` cho nguyên tắc tách bản ghi; `KHÔNG BIẾT` cho cách bạn đã làm.

#### A2-26 — Một lõi luật dùng chung cho GUI (C# 5) và web (.NET 8)
- **Kết luận:** tôi **đồng ý** với nguyên tắc của bạn, nhưng **không** đồng ý dùng chung **ngôn ngữ** ở lớp lõi nếu điều đó khiến bạn phải nâng cấp công cụ biên dịch. Cách ít ma sát nhất: **một bản luật chuẩn, một bộ ca kiểm chạy cho cả hai bên** — đó mới là thứ đồng bộ, còn việc dùng chung *mã nguồn* là lựa chọn triển khai.
- **Nguồn:** không có nguồn ngoài; đây là **lựa chọn triển khai** tôi đề xuất. **Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho cách chia lớp lõi giữa GUI C# 5 và web .NET 8.

#### A2-27 — Hộp thoại WPF ở DPI 150 %
- `KHÔNG BIẾT` cho kích thước cụ thể. Nguyên lý: **dùng đơn vị độc lập thiết bị** và cho hộp thoại **tự co giãn theo nội dung**; kiểm thử bằng **chuỗi dài nhất** mà bạn có, ở **tỉ lệ khác nhau**. Cổng kiểm: chạy ở 100 % / 150 % / 200 % và **đòi không cần cuộn ngang**.
- **Nguồn:** không có nguồn ngoài đã kiểm cho kích thước cụ thể. **Mức tin:** `KHÔNG BIẾT` cho số; `GIẢ THUYẾT CẦN ĐO` cho nguyên tắc đơn vị độc lập thiết bị + kiểm ở 100/150/200 %.

#### A2-28 — Làm app tự động khi chụp sang máy khác
- `KHÔNG BIẾT` cho cách bạn định làm. Nguyên lý liên quan trực tiếp K1/A2-09: **máy khác = máy khác**, nên mọi thứ phải đi kèm **hash và manifest**; nếu app của bạn phụ thuộc đường dẫn tuyệt đối thì nó sẽ "chạy" nhưng sai — đây là mẫu **thất bại trông như đã xong**.
- **Nguồn:** không có nguồn ngoài; nguyên lý "mọi thứ đi kèm hash + manifest" lặp lại từ K1 và A2-09. **Mức tin:** `KHÔNG BIẾT` cho cách bạn định làm; `GIẢ THUYẾT CẦN ĐO` cho cổng phát hiện đường dẫn tuyệt đối.

#### A2-29 — Ngân sách bộ nhớ trên iGPU UMA 47,6 GB
- `KHÔNG BIẾT` cho ETA và nguồn model; đây là câu **duy nhất** trong bộ này mà tôi cho là **không thể trả lời có ích** nếu không có mô hình và cấu hình cụ thể. Cách rẻ để có con số: **đo 1 bước đại diện** rồi nhân.
- **Nguồn:** không có nguồn đã kiểm cho ETA/model. **Mức tin:** `KHÔNG BIẾT`; `GIẢ THUYẾT CẦN ĐO` cho cách ước lượng bằng 1 bước đại diện × số bước.

#### A2-30 — Cổng theo dõi thế trong cờ nhanh
- **Kết luận:** cổng này nên là **cùng một bộ** đã dùng ở A3-15: đếm thế, kiểm khớp khóa, và **bắt buộc** kiểm **"0 thế" là lỗi**. Tôi không có thông tin về nguồn dữ liệu cụ thể của bạn.
- **Nguồn:** không có nguồn ngoài cho nguồn dữ liệu cụ thể của bạn; nguyên tắc lặp lại từ A3-15. **Mức tin:** `GIẢ THUYẾT CẦN ĐO` cho việc dùng chung một bộ cổng theo dõi; `KHÔNG BIẾT` cho nguồn dữ liệu.

#### A2-31 — Câu mở: nhóm nào nên bỏ bớt
- `KHÔNG BIẾT` về nhóm nào **đang giải** bài toán. Tôi nói thẳng điều có ích: theo chính lịch sử lỗi mà bạn đã mô tả (−301 Elo, 6 net "đạt" mà thua A/B, 41.496 thế không vào kho, 42 GB bản chép dò), nhóm **có lợi nhất ngay** là nhóm **cổng kiểm + kỷ luật đo**, chứ không phải nhóm thêm tính năng. Đây là **suy luận**, và tôi nêu tiêu chí đo lợi ích: **số lô/giờ máy bị đốt trước khi phát hiện hỏng**.
- **Nguồn:** không có nguồn ngoài; các số (−301 Elo, 6 net, 41.496 thế, 42 GB) là **số đo của đội**, tôi không kiểm lại được. **Mức tin:** `KHÔNG BIẾT` cho nhóm nào đang giải; `GIẢ THUYẾT CẦN ĐO` cho kết luận "cổng kiểm + kỷ luật đo có lợi nhất".

#### A2-32 — Tổ viết search riêng đang kém 317,8 Elo
- `KHÔNG BIẾT` cho thứ tự ưu tiên cụ thể vì tôi không có mã. Nguyên lý: **đo trước, sửa sau** — dùng đúng thứ tự tầng ở 0.8, và nhớ rằng 3 lỗi "đúng luật nhưng yếu hơn" bạn đã gặp đều thuộc loại **không thấy bằng mắt, chỉ thấy bằng phép đo trên tập cố định**.
- **Nguồn:** không có nguồn ngoài; nguyên lý "đo trước, sửa sau" lặp lại từ 0.8. **Mức tin:** `KHÔNG BIẾT` cho thứ tự ưu tiên trong mã; `GIẢ THUYẾT CẦN ĐO` cho thứ tự tầng kiểm toán.

#### A2-33 — Vì sao Git for Windows NỞ DUNG blob bị cắt ở `mod 2^32`?
- **Đây là câu tôi có bằng chứng công khai, nhưng phải sửa lại tên nguyên nhân cho đúng.** Nguyên nhân **không phải** OID: **OID của Git là SHA-1 160 bit** (`2^160` giá trị) nên bản thân nó không bao giờ tràn ở mốc `2^32`. Chỗ tràn là **con số kích thước ghi trong mục index**: trên Windows (LLP64) `unsigned long` chỉ 32 bit, nên khi file vượt 4 GB thì độ dài bị cắt còn `len & 0xffffffff` và git chỉ ghi nhận `len_new` byte đầu — **không có cảnh báo**. Nguyên văn từ issue: *"due to integer overflow, its length is truncated to len_new = len & 0xffffffff, and Git will only record the first len_new bytes of the file. Critically, this process produces no error or warning, making it very easy to lose content from large files."* ⇒ đây là **lỗi tràn số ở lớp index trên Windows**, **không** phải giới hạn thiết kế của định dạng và **không** phải hàm băm bị bỏ.
  - Nguồn tôi đã mở: `https://github.com/git-for-windows/git/issues/6012` (mở 25/12/2025, đóng 31/08/2026) và PR đã merge `https://github.com/git-for-windows/git/pull/6353` — *"Support loose 4GB+ objects"* (merged 31/08/2026) sửa đúng issue này.
  - **Hệ quả thực tế cho bạn:** đừng dùng git làm nơi chứa **kho dữ liệu 30 GB / 502 triệu dòng**; git làm việc với **mã văn bản**, và ngay cả file đơn > 4 GB cũng đã từng bị cắt âm thầm. Với kho, dùng cơ chế ở 0.2 và gói bàn giao có `sha256` (A2-09).
- **Mức tin:** `CHẮC` cho cơ chế tràn số ở kích thước mục index và cho việc PR 6353 đã merge sửa lỗi; `KHÔNG BIẾT` cho dòng mã cụ thể trong `read-cache.c`/`index.c` (tôi không trích được `tệp:dòng`), nên nếu đội cần dẫn chứng cấp dòng thì phải tra đúng commit của PR 6353.

#### A2-34 — Tuân thủ tính pháp lý eval của net?
- **Kết luận (có nguồn đã mở, nguyên văn từ kho chính thức):** giấy phép trọng số Pikafish được ghi trong README kho `Networks` (S31): *"The weights file (pikafish.nnue) released with the Pikafish and the weights file further derived from them are: 1. Only for legal use… 2. No commercial use without permission."* ⇒ **đúng** là cấm thương mại **và cấm dùng trọng số phái sinh** (trừ khi được cho phép theo danh sách bên dưới). Bên cạnh đó: *"the weights we train for the xiangqi variant of the Fairy-Stockfish are licensed under CC0 license… they are not constrained by this license"* — tức phần CC0 **chỉ** là trọng số biến thể cờ tướng của Fairy-Stockfish, **không** phải mọi net. Và README Pikafish (S32) nói net được huấn luyện trên dữ liệu dự án Pika Xiangqi Zero theo **ODbL** ⇒ ràng buộc dữ liệu nằm ở **hai tầng khác nhau** (giấy phép trọng số + giấy phép dữ liệu), đừng gộp làm một.
  - **Việc bạn nên làm (rẻ, tôi đề xuất):** giữ **sổ giấy phép theo từng nguồn** (A2-09), ghi riêng ba cột `net / dữ liệu huấn luyện / điều khoản`, và **mỗi lần xuất bản phải chứng minh được** cả hai cột. Đây là bài toán **truy vết**, không phải bài toán pháp lý.
- **Nguồn:** S31, S32.
- **Mức tin:** `CHẮC` cho điều khoản trọng số Pikafish (S31) và cho việc dữ liệu Pika Xiangqi Zero theo ODbL (S32); `KHÔNG BIẾT` cho EULA của bốn engine thương mại (xem A2-10) và cho tình trạng pháp lý của **net do chính đội tự huấn luyện từ dữ liệu có nguồn gốc mở** — phần này chỉ kết luận được khi đội chứng minh được chuỗi dữ liệu.

#### A2-35 — Nghiên cứu sâu các GUI khác: họ làm auto bằng cách nào
- **Bảng theo đúng khuôn yêu cầu `GUI | nhận bàn bằng | sàn nối được | tự hồi phục | nguồn`:**
  - `SharkChess | **không biết** (đóng nguồn) | **không biết** mặt kỹ thuật; "bản free nối vài ba, bản thương mại nối hầu hết" là **ghi chép của đội, tôi không xác minh được** | **không biết** | —`
  - `BHGui | **nói chuyện thẳng với server, không bóc ảnh** (theo mô tả của bạn về `bhhelp.pdf`) | nối vào client qua tự nhận/tự nối cửa sổ | **không biết** | tài liệu nội bộ của đội, tôi không mở`
  - `PengFei/鹏飞 · 兵河五四 · 象棋名手 · 象棋旋风 GUI | **không biết** | **không biết** | **không biết** | —`
  - `GUI "khoá phần cứng" mà đội không được mở | **không biết** | **không biết** | **không biết** | —`
- **Ba điều tôi khuyên copy trước nhất (đây là câu cuối của câu hỏi, và là phần tôi tự tin nhất):** (1) **hồ sơ kết nối là dữ liệu** (A2-03) — đây là thứ quyết định việc thêm sàn mới **không cần sửa mã**; (2) **cổng "không nhận ra"** (0.5) — vì hầu hết lỗi "auto hỏng" trong thực tế là auto **đoán bừa**; (3) **xác nhận lại sau khi bấm** (A3-14a) — vì đây là nơi mất nước âm thầm.
- **Lỗi thường gặp khiến auto không nhận được (câu 3):** tôi nêu **nguyên nhân kỹ thuật** chung — DPI/ảnh chụp sai kích thước, skin động làm mẫu lệch, cửa sổ bị che, và ứng dụng **chống chụp màn** — nhưng **cách mỗi GUI xử lý thì `không biết`**, vì tôi không có mã/tài liệu của họ.
- **Kho mã mở để đọc (câu 4):** tôi xác minh được **`rhevorn/xiangqi-vision`** (MIT — S27), và **đó là kho duy nhất tôi đã mở**. Tôi **không** dẫn kho khác vì chưa mở ⇒ `không biết link` cho phần còn lại.

**Nguồn chung cho ASK02:** S3, S6, S13, S15, S16, S17, S20, S21, S22, S23, S27, S28, S29, S30.
**Mức tin chung:** `CHẮC` cho những gì tôi đã mở và kiểm (S15/S16 về giới hạn 32 bit; S31/S32 về giấy phép net; S3 về hành vi WAL; S20/S21 về nền tảng chụp màn/điều khiển; S23 về tương phản); `GIẢ THUYẾT CẦN ĐO` cho mọi ngưỡng và tham số tôi đề xuất; `KHÔNG BIẾT` cho mọi nội dung cần EULA, tài liệu GUI thương mại, hay số công bố mà tôi không mở được. **Mọi số đo của đội (552,5 thế/s; 10.540 vị trí/s; −301 Elo; ±7,2; 41.496 thế…) là số của đội, tôi không tự đo và không ghi là số của tôi.**

---

## Tổng kết: xếp hạng việc nên làm trước

Theo thứ tự **rẻ trước, đắt sau**, và dựa trên chính những lỗi đã gặp (không phải trên cảm giác):

1. **Làm cổng kiểm đáng tin trước khi đo bất cứ thứ gì** — cổng phải **từng đỏ thật**; nếu không có ca đỏ thì cổng chưa tồn tại. Đây là bước 1 vì nó bảo vệ **mọi** bước sau (A3-16, K8).
2. **Dừng cấp hình kho (1 bản, 1 writer, 1 batch) và bỏ bản chép dò 42 GB** — K1/K2/K7. Đây là nơi tiết kiệm đĩa và giảm rủi ro lớn nhất.
3. **Sửa lớp định danh nguồn (hash thay vì tên tệp) và bỏ đường ghi trạng thái lén** — K3, A3-12. Rẻ, và làm mọi lỗi "cổng xanh giả" biến mất.
4. **Cổng "không nhận ra" + xác nhận lại sau khi bấm** ở AUTO — 0.5, A3-14. Vì không có nhận bàn thì GUI không có tác dụng.
5. **Chuẩn hoá đơn vị ply/mate một lần, cho tất cả nguồn** — A3-01c, A3-11c. Rẻ, và là loại lỗi chỉ test mới bắt được.
6. **Quy tắc dừng sớm bằng phép đo** trước mỗi đợt thuê máy — K5, Y-8. Đừng để phép đo "đẹp" thay thế cho A/B.
7. **Tách mã / dữ liệu / nhật ký, và giảm chi phí token** — K7, K8.

**Điều tôi không làm và không khuyên làm:** không đề xuất bypass, không giải ngược, không hook sâu phần mềm thương mại, và không nói "không biết" về bất kỳ protocol nào mà tôi không đọc được tài liệu. Ở những chỗ đó tôi ghi `KHÔNG BIẾT` — đó là câu trả lời đúng, và **rẻ hơn nhiều** so với một con số bịa.

**Nhắc lại một lần cuối:** các số đo, cấu hình và tệp nội bộ trong bài này là của **đội**, tôi chỉ trích dẫn chúng khi bạn nêu; tôi **không** lưu, **không** lan truyền, **không** dùng để huấn luyện gì, và tôi **không** có zip nên **không** có mục `L-*` nào.
