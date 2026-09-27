# DeepSeek — trả lời Ask 1 (27/09/2026), 2 lượt nối tiếp

> Nguồn: owner dán từ giao diện web DeepSeek vào phiên Fable 27/09 07:5x. Đội bỏ phần giao diện (danh sách chat, «This response is AI-generated», «Message is empty») và **bỏ phần suy nghĩ nội bộ của model** («The user is asking…», «Let me take stock…»); giữ nguyên văn hai lượt trả lời. Tiêu đề chat của DeepSeek ghi «DebateAI không truy cập được» ⇒ AI này KHÔNG mở được GitHub, owner đã dán Ask thẳng vào chat. Không có source.zip, không có internet — mọi «cite» của DeepSeek trỏ tới đoạn mã đã trích trong Ask, không phải dòng raw.

---

## LƯỢT 1 — owner hỏi «trả lời hết chưa»

Đã đọc điều khoản bảo mật; không lưu, không lan truyền, không dùng để huấn luyện.

Chưa hết. Tôi rà lại toàn bộ và thấy còn thiếu 2 câu trong ASK02: A2-04 (tốc độ đua xe) và A2-09 (gộp log nhiều máy). Ngoài ra nhiều câu tôi trả rất mỏng — đặc biệt ASK03 (A3-01 → A3-16) chỉ là chẩn đoán ngắn, chưa đủ để đội code theo. Dưới đây tôi trả nốt 2 câu thiếu, rồi liệt kê rõ chỗ nào còn mỏng để bạn quyết định đào sâu tiếp.

### A2-04 — "Tốc độ đua xe": đo và cắt độ trễ 4 khúc

Bảng tổng:

| Khúc | Cách đo | Cách cắt | ms kỳ vọng | Nguồn |
|---|---|---|---|---|
| 1. Nhận ra đối thủ đã đi | Hash vùng bàn, 2 khung xác nhận | Giảm vùng hash, quét 30 Hz | 30–60 | suy luận |
| 2. Đọc thế cờ | Template matching (A2-01) | Pre-compute mẫu, SIMD | 5–20 | suy luận |
| 3. Engine nghĩ | UCI info log | Tune time/depth | 100–3000 | đo được |
| 4. Bấm chuột | SendInput | Giảm delay giữa 2 bấm | 10–50 | suy luận |

(1) Chụp màn hình nhanh nhất Windows 10/11:

| Cách | ms/khung (vùng ~600×700) | Ghi chú |
|---|---|---|
| DXGI Desktop Duplication | 1–3 | Nhanh nhất, Windows 8+. Hỗ trợ vùng. |
| Windows.Graphics.Capture | 2–5 | Hiện đại, Win10 1803+. API sạch. |
| GDI BitBlt | 5–15 | Tương thích rộng, chậm hơn. |
| PrintWindow | 10–50 | Cho cửa sổ cụ thể, app có thể chặn. |

Bẫy DPI: gọi SetProcessDpiAwareness(PROCESS_PER_MONITOR_DPI_AWARE) trước khi chụp, nếu không tọa độ bị scale. Đa màn hình: mỗi màn hình là một DXGI output riêng — phải chọn đúng output chứa cửa sổ đích.

Khuyến nghị: DXGI Desktop Duplication cho vùng, fallback GDI BitBlt nếu DXGI fail.

(2) Phát hiện "đối thủ đã đi" rẻ nhất:

| Cách | Chi phí | Độ tin |
|---|---|---|
| Hash vùng bàn | Trung bình | Cao (nhưng nhạy hoạt ảnh) |
| So vài điểm ảnh | Rẻ nhất | Thấp (dễ miss) |
| Đợi sự kiện cửa sổ | Rẻ | Không đáng tin cho app game |

Khuyến nghị: hash vùng bàn (ví dụ 32×32 mẫu) + xác nhận 2 khung liên tiếp (cách nhau 30–50 ms). Tần số quét 30 Hz (33 ms) là đủ — nhanh hơn không cần vì mắt người + engine nghĩ đã chiếm phần lớn.

Bẫy: hoạt ảnh quân bay làm hash đổi liên tục ⇒ phải đợi hash ổn định ≥ 2 khung mới coi là "đã đi xong".

(3) Bấm chuột:

| Cách | Nhanh | Tương thích | Ghi chú |
|---|---|---|---|
| SendInput | Nhanh | Rộng | Cần cửa sổ focus |
| PostMessage | Rất nhanh | Hẹp | Không cần focus, nhưng app chống auto có thể bỏ qua |
| Kéo-thả | Chậm hơn 2 bấm | Tùy app | Một số app chỉ nhận kéo-thả |

Khuyến nghị: SendInput mặc định, PostMessage fallback. Cờ tướng thường dùng 2 cú bấm (chọn quân → chọn đích), không cần kéo-thả.

(4) Độ trễ tổng GUI thương mại:

Không có số công bố cho cờ tướng — tôi không biết. Ước từ kiến trúc: 100–300 ms từ đối thủ đi xong đến mình đi xong (không tính engine nghĩ). Engine nghĩ thêm 0,1–3 s tùy cấu hình.

Ca thử ≤ 30 phút: chạy 50 nước auto, ghi ms từng khúc vào CSV. Ngưỡng ĐÚNG: khúc 1+2+4 ≤ 150 ms (p90). Ngưỡng SAI: p90 > 300 ms.

### A2-09 — Gộp log + dữ liệu + net từ nhiều máy

Manifest JSON (≤ 20 dòng):

```json
{
  "id_goi": "may44loi_20260927_1000",
  "may": "may44loi",
  "thoi_gian": "2026-09-27T10:00:00Z",
  "engine": {"ten": "OngThan", "hash_exe": "sha256:abc..."},
  "net": {"ten": "c07e94a5", "hash": "sha256:def..."},
  "cau_hinh": {"threads": 12, "hash_mb": 2048, "depth": 20},
  "so_ban_ghi": 41496,
  "giay_phep": "cc0|thuong_mai|tu_train",
  "tep": [
    {"ten": "data.bin", "sha256": "...", "so_dong": 41496},
    {"ten": "log.txt", "sha256": "..."}
  ]
}
```

(1) Gói bàn giao: như trên. Bắt buộc có id_goi, hash_exe, hash net, so_ban_ghi, SHA256 từng tệp.

(2) Khử trùng lặp khi cùng thế 2 nhãn khác nhau:

| Trường hợp | Quy tắc |
|---|---|
| Cùng engine, khác depth | Giữ depth cao hơn |
| Cùng engine, khác máy | Giữ cả hai, trọng số theo số ván |
| Khác engine, cùng độ mạnh | Giữ cả hai, trọng số theo Elo |
| Khác engine, khác độ mạnh | Giữ engine mạnh hơn, ghi engine yếu vào phu |
| Nhãn mâu thuẫn (một thắng một hoà) | Giữ cả hai, đánh dấu mau_thuan=1 để lọc sau |

Khuyến nghị: giữ cả hai có trọng số thay vì chọn một — tránh mất thông tin. Lọc sau nếu cần.

(3) Chống nạp trùng + phát hiện gói cắt dở: Idempotent — trước khi nạp, kiểm id_goi đã có trong bảng goi_da_nap chưa; nếu có ⇒ bỏ qua, báo "đã nạp". Gói cắt dở: kiểm SHA256 từng tệp + tổng số bản ghi khớp manifest; không khớp ⇒ từ chối, không nạp một phần.

(4) Giấy phép: trường giay_phep trong nguon_lo: cc0 / thuong_mai / tu_train. Tách thay_thuong_mai vs thay_cc0 thành hai cột riêng để sau lọc ra được. Có thể lọc bỏ toàn bộ nhãn của một Thầy nếu phải — điều kiện tiên quyết.

Ca đỏ: nạp cùng gói 2 lần ⇒ lần 2 báo "đã có, bỏ qua" (rc ≠ 0 để cổng biết). Gói thiếu 1 tệp ⇒ báo lỗi. Gói có so_ban_ghi sai ⇒ báo lỗi.

### Những chỗ tôi trả MỎNG — đội cần đào sâu

- ASK03 (A3-01 → A3-16): chỉ chẩn đoán ngắn, không có trích tệp:dòng cụ thể (không có source.zip), không diff unified, không ca thử ≤ 30 phút với ngưỡng ghi trước, không độ chắc (ĐÃ CHẠY / ĐỌC MÃ / SUY LUẬN). Muốn trả đủ khuôn cần source.zip hoặc ít nhất các tệp được trích (train_nnue_gpu_v3.py, gen_thay_that.py, tan_cuoc_liet_ke.py, search.cpp, egtb.cpp, evaluate.cpp, position.h/cpp, TuChoi.cs, HopAuto.cs, CuaSoChinh.cs, AutoBamBanCo.cs, BocCoManHinh.cs, ChuotTuDong.cs, kiem_import_backupdata.py, NapVanHan.cs).
- ASK12: tóm tắt, chưa đủ cho 9 chủ đề (chưa có công thức chia hạn ngạch; schema da_cham; script SQL sửa kho; parser .cbl; bước reverse JJ; danh sách chức năng BHGui/SharkGui; code mẫu encrypt; checklist kiểm toán; kiến trúc Kỳ Viện).
- ASK04-B: chẩn đoán + đề xuất ngắn, chưa có script K1, schema tien_do K2, code mẫu K3, kiến trúc K4, công thức K5, mã K6, mô hình khoá K7, đề bài worker K8.
- ASK02: đủ 35 câu nhưng nhiều câu mỏng (A2-06/07 chưa có lệnh cutechess/BayesElo; A2-10 chưa trích EULA; A2-14/15 chưa số đo; A2-19 chưa Elo HKA; A2-23 chưa commit cụ thể; A2-25 chưa nguồn sản phẩm; A2-29 chưa công thức bộ nhớ; A2-30 chưa URL điều khoản; A2-33/34 chưa URL/issue).

| Phần | Đã trả | Còn mỏng |
|---|---|---|
| ASK02 (35 câu) | Đủ 35 | Nhiều câu chưa có URL/số đo thật |
| ASK03 (16 câu) | Đủ 16 | Chỉ chẩn đoán ngắn, thiếu cite + diff + ca thử |
| ASK12 (9 chủ đề) | Đủ 9 | Tóm tắt, chưa đủ để code |
| ASK04-B (8 mục) | Đủ 8 | Chẩn đoán + đề xuất, chưa có script |

---

## LƯỢT 2 — owner: «cứ trả lời hết rồi đào sâu»

Đã đọc điều khoản bảo mật; không lưu, không lan truyền, không dùng để huấn luyện.

Nhắc lại giới hạn: không có source.zip trong phiên này, không có internet. Vì vậy mọi "cite" dưới đây trỏ tới đoạn mã đã trích sẵn trong chính file ASK03 (tôi đánh số theo đoạn đó), không phải dòng raw trong zip. Chỗ nào cần đọc thêm mã, tôi ghi rõ "cần zip". Đào sâu theo thứ tự ưu tiên §1.5 của Ask03: training → engine → auto trước, rồi ASK12, rồi ASK04-B.

### PHẦN A — ĐÀO SÂU ASK03 (training / engine / auto)

#### A3-01 — Trainer vứt nhãn chiếu bí: sửa target thế nào

(a) Hình dạng cp = dấu × max(1500, 3000 − 10·dtm_ply) có hợp NNUE họ Stockfish? NNUE họ Stockfish (và Pikafish) dùng một đầu ra cp với thang tuyến tính ~centipawn. Vùng hoạt động thực tế của eval là ±1500 cp; ngoài vùng đó hiếm khi search tin. Nhồi DTM vào vùng ±1500…3000 có hai rủi ro: (1) Nén thang — với max(1500, 3000 − 10·dtm_ply), dtm_ply từ 0 đến 150 nằm trong [1500, 3000], nhưng phần lớn thế tàn cuộc có dtm_ply > 150 ⇒ tất cả bị kẹp về 1500, net mất độ dốc ở đúng vùng phổ biến. (2) Xung đột nhánh WDL — WDL = 1 và cp = 1500 nhưng BCE kéo pred/1042 về 1, smooth_l1 kéo pred/200 về 7.5 — hai lực ngược nhau ở vùng biên.

Đề xuất: tách nhánh mate khỏi nhánh cp thường; giữ mate chỉ trong đầu WDL (win=1) và để search lo khoảng cách (search đã có mate-distance pruning + EGTB probe). Nếu vẫn muốn độ dốc DTM trong cp: thang phi tuyến `cp = dấu × 1500 × (1 − exp(−dtm_ply / τ))` với τ ≈ 30 — dốc mạnh gần chiếu bí, bão hoà mềm ở 1500, không chạm 3000.

(b) Nhãn mate chỉ vào WDL hay cần độ dốc DTM ở cp? Chỉ vào WDL là đủ cho học, độ dốc DTM ở cp là tùy chọn. Net value-only không quyết định nước đi — search quyết định (MDP + EGTB + rule60). Kết luận: WDL = 1 cho mate, cp = kẹp ±1500 (không theo DTM).

(c) Lệch đơn vị mate N (nước) vs 30000 − ply: đây là bug thật. UCI score mate N tính bằng nước (full move), 30000 − ply tính bằng nửa nước; trộn hai nguồn ⇒ DTM lệch ~×2. Chỗ khác có thể sai (suy luận từ đoạn trích): gen_thay_that.py:196; train_nnue_gpu_v3.py nếu đọc score mate từ UCI; tan_cuoc_liet_ke.py nếu ghi/đọc DTM từ chessdb. Cách sửa: chọn một quy ước duy nhất (đề xuất ply), ghi rõ docstring.

(d) Công thức: nếu nguồn báo mate ⇒ wdl = 1/0, cp = dấu × min(1500, 1500 × (1 − exp(−dtm_ply / 30))) (tuỳ chọn); nếu cp thường ⇒ wdl = sigmoid(cp / S) với S ≈ 300–400, cp kẹp ±3000. Ca thử ≤ 30 phút: 500 thế thắng có DTM thật, đo Spearman giữa −DTM và cp đầu ra; ĐÚNG ρ ≥ 0.5, SAI ρ < 0.2.

#### A3-02 — Sigmoid bão hoà + bộ lọc "nước tốt nhất là ăn quân"

(a) Bộ lọc pieceAt(to) != None làm mất phần quan trọng của tàn cuộc — có, rất nặng: tàn cuộc thường "đổi quân để vào thế thắng", nước ăn quân đưa về bảng EGTB nhỏ hơn; bỏ hết ⇒ net học sai phân phối. Đề xuất: tắt bộ lọc cho kho tàn cuộc (cờ --khong-loc-an-quan); nếu muốn lọc "thế không yên" đúng nghĩa thì dùng SEE — cần movegen.

(b) in_scaling = 1000 với mate 29.9xx: sigmoid(29.9) ≈ 1.0 — mọi mate cùng giá trị. Ánh xạ đề xuất: |score| ≥ MATE_THRESHOLD (29000) ⇒ p = 1.0/0.0, cp đầu ra kẹp ±1500 (không dùng 29.9xx); ngược lại p = sigmoid(score / 1000). Hoặc giảm in_scaling xuống 300–400 nhưng phải retrain.

#### A3-03 — Nhãn từng nước cho net value-only

(a) Ba lựa chọn: nhãn thế con (mỗi nước → thế con có WDL/cp; đơn giản, tương thích) · ranking loss (cặp tốt/xấu, margin; học thứ tự nhưng phức tạp) · học value thế con rồi search chọn. Đề xuất: nhãn thế con + search chọn; không cần ranking loss.

(b) Syzygy 5 lớp (win / cursed win / draw / blessed loss / loss): trainer thường map cursed win → draw, blessed loss → draw. Áp sang cờ tướng mốc 80 ply: thế thắng lý thuyết nhưng dtc > 80 ⇒ theo luật hoà. Gắn HOÀ ⇒ an toàn nhưng mất thông tin; gắn THẮNG ⇒ search cố thắng, gặp đối thủ biết luật bị cầm hoà. Đề xuất: gắn HOÀ + lưu thêm trường thang_ly_thuyet=1. Quyết định của chủ sở hữu.

(c) Trainer công khai dùng dtz/dtc? Không biết. Stockfish NNUE dùng score + WDL từ search, không dùng dtz/dtc trực tiếp trong train.

#### A3-05 — Mate-distance pruning + EGTB

(a) THUA mà dtc > remain: trả cận hoà thay vì bỏ. Hiện dungDuoc trả false khi dtc > remain ⇒ bỏ thông tin EGTB. Đề xuất:

```cpp
bool dungDuoc(const EgtbHit& h, int remain) {
    if (h.wdl == 0) return true;   // hoà thật
    return h.dtc <= remain;        // thắng/thua trong hạn
}
// Ở search: nếu egHit.wdl != 0 && egHit.dtc > remain ⇒ return VALUE_DRAW (bound EXACT)
```

Phân biệt "hoà thật" (wdl=0) và "hoà do luật" (wdl≠0, dtc>remain) — cùng score nhưng có thể thêm is_rule_draw.

(b) Ba lỗi kinh điển cần kiểm quanh search.cpp :1393-1426: mate score qua TT (scoreToTT phải giữ dạng mate); graph-history interaction với luật lặp/chiếu mãi (không dùng TT cho thế đang lặp); bound sai cho EGTB (wdl=0 ⇒ EXACT, wdl>0 ⇒ LOWER, wdl<0 ⇒ UPPER).

(c) Felicity sót chiếu mãi/đuổi mãi? Có thể — nếu retrograde không xử lý luật chiếu/đuổi mãi. Đề xuất: bọc probe bằng kiểm luật lặp — thế đang lặp thì bỏ EGTB.

#### A3-07 — Luật không ăn quân 80 ply trong eval + hash

(a) adjust_key60 nấc /8 từ 14 − AfterMove: mốc 120 có ~14 nấc, mốc 80 có ~9 nấc; thế rule60 = 6 và 14 cùng hash; với mốc 80 khoảng 0–14 chiếm 17,5 % thay vì 11,7 % ⇒ va chạm TT nhiều hơn. Đề xuất: dịch nấc đầu xuống (10 − AfterMove hoặc 6) hoặc dùng /4 cho mốc 80; đo 100 ván A/B.

(b) Cách chuẩn ưu tiên chuyển hoá trước mốc (Stockfish rule50): giảm eval theo bộ đếm; move ordering ưu tiên ăn quân/chiếu gần mốc; không dùng TT cho thế gần mốc. Chỗ khác giả định 120 trong pikafish-src: cần grep 120, rule60, noCaptureDraw — cần zip.

#### A3-08 — Cờ úp −33 Elo

(a) Ba giả thuyết theo khả năng: (1) bảng material/imbalance chưa tune cho quân úp — B1m đổi materialKey ⇒ tra bảng theo key mới, bảng tune cho cờ ngửa; kiểm: 100 ván B1m bật nhưng tắt tra bảng material. (2) PSQ cập nhật hai lần — B2b cập nhật psq khi lật trong cây search, nếu do_move cũng cập nhật ⇒ eval lệch; kiểm: log đếm số lần cập nhật psq mỗi nước lật. (3) TT key đổi — nếu B1m đổi materialKey nhưng không đổi TT key; kiểm: so TT hit rate.

(b) position.cpp quanh do_move/undo_move: kiểm darkSquare = SQ_NONE reset đúng; materialKey cập nhật một lần; psq cập nhật một lần.

(c) Phép đo ≤ 30′: 100 ván chỉ B2b (10′), 100 ván chỉ B1m (10′), so Elo với baseline (5′), khoanh (5′).

#### A3-12 — Đường ghi lén trạng thái

(a) Cần zip: grep ChiXem, AutoDo, AutoDen, _chay, BatDau, Dung, DungGiuBan, MoAuto — đếm chỗ gán.

(b) Thiết kế không thể lọt: State Machine một điểm vào duy nhất — enum {DUNG, ANALYZE, FULL}; lớp AutoState có lock; chỉ ChuyenSangAnalyze() (từ DUNG), ChuyenSangFull() (từ ANALYZE), Dung(); DuocPheDiNuoc() = (_tt == FULL). Hàm gửi nước ra ngoài (BamChuot, GuiNuoc) bắt buộc gọi DuocPheDiNuoc() trước — cổng cuối, không lách được dù UI có bug.

(c) Giá trị lưu bền mặc định: ANALYZE (an toàn hơn).

(d) Cổng tự kiểm phủ mọi lối vào: reflection liệt kê mọi method có attribute [LoiVaoAuto], với mỗi method chạy ma trận trạng thái DUNG/ANALYZE/FULL × kỳ vọng đi nước; bất kỳ method fail ⇒ cổng đỏ.

#### A3-13 — Dispatcher race

Kịch bản: t1 luồng nền gọi Dispatcher.BeginInvoke(BomThe) · t2 người dùng STOP ⇒ DungGiuBan() ⇒ _phien++ · t3 UI chạy BomThe từ t1 ⇒ đè bàn. Sửa: mỗi lần bơm thế mang số phiên chụp lúc bắt đầu tính; đầu callback `if (phien != _phien) return;`. Kiểm: log _phien mỗi BomThe, test STOP-rồi-chạy-lại 100 lần.

#### A3-15 — Trọng tài vs importer có cùng điểm mù?

| Ca | Mô tả | Rủi ro |
|---|---|---|
| 前/后/中 cho 2–3 quân cùng cột | Phải đếm quân cùng cột | Cao |
| Nhiều tốt cùng cột (一二三四五 / 前中后) | Quy ước khác nhau giữa các nguồn | Cao |
| 进/退/平 cho mã–tượng–sĩ | Số đích là CỘT, không phải số bước | Rất cao |
| Số Hán cho Đỏ, Ả Rập cho Đen | Kể cả toàn-khổ １２３ | Trung bình |
| Biến thể chữ 車/车, 馬/马, 砲/炮/包, 帥/帅, 兵/卒 | Phải chuẩn hoá | Trung bình |
| 平 cho tướng | Tướng đi ngang trong cung | Thấp |

Cả hai bộ đọc cùng sai nếu cùng dùng một quy ước sai. Bộ 50 ván thử đối kháng: 10 ván mỗi nhóm ca trên (前/后 · nhiều tốt cùng cột · 进/退/平 cho mã · số Hán vs Ả Rập · biến thể chữ); chạy cả hai bộ đọc, khác ⇒ một trong hai sai.

### PHẦN B — ĐÀO SÂU ASK12

#### ASK12-1 — Kho tàn cuộc bậc 2+

Tính toán: 9.900 thế/s ⇒ 35,6 M thế/giờ ⇒ 855 M thế/ngày; 1,14·10²⁰ thế ⇒ không vét cạn. Chiến lược: chỉ làm ô nhỏ trước (≤ 1 tỷ thế ⇒ vét cạn); ô lớn lấy mẫu theo tần suất ván thật (91 % thế thật có ≥ 3 sĩ/tượng). Chia hạn ngạch: ô tần suất > 1 % ⇒ vét cạn nếu ≤ 1 tỷ, mẫu 10 M nếu lớn hơn; 0,1–1 % ⇒ mẫu 1 M; < 0,1 % ⇒ 100 k hoặc bỏ. Target NNUE: WDL 3 lớp + cp phi tuyến theo DTM (A3-01(a)). "Nước đúng" bên thua: DTM thuần cho train (kéo dài nhất), luật 80 ply cho thi đấu; lưu cả hai nhãn.

#### ASK12-2 — Mô hình kho

Rủi ro: bất biến datanoscore ∩ data.db = ∅ khó giữ khi race; mất dữ liệu nếu analyze crash giữa chừng; không biết cái nào đã analyze xong. Đề xuất thêm bảng da_cham (vkey PRIMARY KEY, thoi_gian, phien_ban, ket_qua). Khi analyze xong: ghi data.db → ghi da_cham → xoá datanoscore, cả 3 trong một giao dịch (cùng DB) hoặc 2-phase commit. Kiểm hậu gộp: count trước/sau; mẫu 1.000 vkey ngẫu nhiên kiểm có trong data.db; PRAGMA quick_check. Giữ 2 tệp nối bằng vkey: có — datanoscore (hàng đợi) + data.db (đã chấm) + da_cham (sổ).

#### ASK12-7 — Encrypt book

AES-256-GCM; key = HKDF(master_key, salt=hardware_id, info="book_v1"), hardware_id = SHA256(CPU_ID || DISK_SERIAL || MAC). Chống dump RAM: giải mã từng phần (streaming), xoá key sau dùng (SecureString / RtlSecureZeroMemory); không chống được 100 % với admin + debugger. 3 loại key: token JWT-like ký bằng private key owner {loai: het_han | ngung_vinh_vien | han_cap_nhat, hardware_id, het_han, ky}; client kiểm bằng public key nhúng. Update giữ config: 5 tệp cấm đè liệt kê trong manifest; kiểm hash trước ghi; khác ⇒ không ghi. Ca đỏ: cố ý sửa 1 trong 5 tệp, chạy update, kiểm không bị đè.

### PHẦN C — ĐÀO SÂU ASK04-B

#### K1 — Đĩa phình 500 GB

ATTACH DATABASE 'D:/nguon.db' AS nguon; INSERT INTO main.positions SELECT * FROM nguon.positions WHERE …; DETACH — không chép về C. Kiểm không cần bản chép thứ hai: hash từng lô SHA256 + đếm dòng trước/sau. Backup Google Drive: `rclone copy D:/nguon.db gdrive:backup/ --progress` (stream, không đệm C). Tiêu chí: C trống ≥ 100 GB sau mỗi lô.

#### K2 — Gộp staging an toàn

Bảng tien_do (lo PRIMARY KEY, so_dong, trang_thai 'dang_chay'|'xong'|'loi', thoi_gian); mỗi lô 100 k dòng trong BEGIN…COMMIT kèm ghi tien_do; resume từ lô cuối 'xong'. Khoá một writer: CreateFile("lock.txt", GENERIC_WRITE, FILE_SHARE_NONE…) — handle INVALID ⇒ có writer khác. WAL cho kho thật: có (đọc nhiều ghi ít); backup trước khi chuyển.

#### K5 — Kho tàn cuộc để train

Lấy mẫu 3–4 quân: 70 % theo tần suất thực chiến + 30 % theo đối xứng (cẩn thận, cờ tướng không đối xứng hoàn toàn). Định dạng nhãn: vkey, fen, dtm, dtc, wdl, thang_luat_60, thang_luat_80. Loss: cp smooth L1, WDL BCE, trọng số 0,7 WDL + 0,3 cp. Cổng "đã học": Spearman(net, −DTM) trên tập thế thắng ≥ 0.5.

#### K7 — Nhiều AI trên git

Mỗi lane một nhánh; merge qua PR; nhật ký append-only với lock. Bảng task (id, lane, trang_thai 'cho'|'dang_lam'|'xong'|'loi', thời gian bắt đầu/kết thúc); worker claim bằng UPDATE … WHERE trang_thai='cho' AND id=?.

#### K8 — Ngân sách token

| Việc | Script? | AI? |
|---|---|---|
| Count, hash, so sánh số | ✅ | ❌ |
| Chạy bench, perft | ✅ | ❌ |
| So eval tĩnh | ✅ | ❌ |
| Đọc log, tìm lỗi | ❌ | ✅ |
| Viết code mới | ❌ | ✅ |
| Review code | ❌ | ✅ |
| Quyết định thiết kế | ❌ | ✅ |

Đề bài worker: MỤC TIÊU (1 câu) · NGƯỠNG (số cụ thể) · CA ĐỎ · OUTPUT (JSON). Ngắn gọn.

### TỔNG KẾT

Đã trả lời hết: ASK02 35/35 · ASK03 16/16 (đào sâu A3-01, 02, 03, 05, 07, 08, 12, 13, 15) · ASK12 9/9 (đào sâu 1, 2, 7) · ASK04-B 8/8 (đào sâu K1, 2, 5, 7, 8). Giới hạn: không có source.zip ⇒ không cite tệp:dòng raw; không có internet ⇒ không verify URL/EULA; nhiều chỗ ghi "không biết" — thật, không bịa. Để đào sâu hơn (đủ khuôn Ask03 với diff + ca thử) cần source.zip hoặc các tệp được trích; nếu dán đoạn mã cụ thể, sẽ trả đủ khuôn cho đoạn đó.
