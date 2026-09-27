# Grok — trả lời Ask 1 (26/09/2026)

Đã đọc điều khoản bảo mật Ask04 / Ask03 / Ask02; không lưu, không lan truyền, không dùng để huấn luyện. Không có `Ask03_source_2026-09-25.zip` trong phiên này nên **không cite `file:dòng` trong zip**, không bịa đường dẫn. Phần ASK03 chỉ trả lập luận công khai + ghi rõ chỗ phải soi zip.

---

## 0.1 Tàn cuộc bậc 2

- Trả lời:
  1) **Không liệt kê hết ≤4v4.** 1,14·10²⁰ thế và ~42.687 ô mục lục là bài toán lưu trữ, không phải bài toán một máy. Bậc 1 (≤2 quân tấn công, 11.900.907 thế DTM+WDL) giữ nguyên làm **bảng tra cứu đầy đủ**. Bậc ≥2 chỉ **gán nhãn mẫu**, không EGTB đầy đủ.
  2) **Hạn ngạch theo ván thật, không theo kích thước ô.** Số đo đội: mẫu 400.000 thế ván thật có 0,023 % không sĩ/tượng và 91 % có ≥3 sĩ/tượng. Ô «không sĩ/tượng» gần rỗng trong thực chiến; vét hết ô đó chỉ làm net học thế gần như không gặp. Công thức đề xuất:
     `quota(ô) = Q_tổng × (f_ván(ô) + ε) / Σ(f_ván+ε)`, `ε` nhỏ (vd 0,002) để ô hiếm không = 0.
     Ô nhỏ (krk 8.748, kpk 5.841 theo ASK03) thì **vét hết**. Ô lớn: lấy mẫu có **tầng WDL** (thắng/thua nhiều hơn hoà) + giữ **đuôi DTM dài** (không chỉ mate 3–6 ply).
  3) **Một máy — pipeline thực tế:**
     - Retrograde nội bộ ~9.900 thế/s × 700 B/thế: dùng cho ô ≤ bậc 1 và ô bậc 2 **nhỏ** (một bên 3 quân tấn công, bên kia 0–1, sĩ/tượng hạn chế). 10⁷ thế ≈ 17 phút sinh + ~7 GB.
     - `chessdb-sst` offline 512 GB: tra nhãn sẵn, **không** gọi API khi train.
     - FelicityGen DTM: chỉ những cấu hình Felicity đã sinh được (MIT). Felicity README nói generator Xiangqi tập trung một bên 1–2 attacker / bên kia armless — **không** phủ 3v3–4v4 đầy đủ. https://github.com/nguyenpham/FelicityEgtb
     - Ô còn lại: chấm bằng teacher (OngThan + Doccocaubai), depth/nodes cố định, **tắt sách**.
  4) **Target NNUE tàn cuộc: WDL là chính, cp-DTM là phụ và phải TẮT bằng cờ.**
     Họ Stockfish/nnue-pytorch train trong **không gian WDL** (sigmoid(cp / scale)), `lambda` trộn score search với kết quả ván. Mate score 29xxx **bão hoà sigmoid** — net không học được «16 ply hơn 10 ply» nếu chỉ nhét cp thô. https://github.com/official-stockfish/nnue-pytorch/blob/master/docs/nnue.md
     Công thức nội bộ `cp = dấu × max(1500, 3000 − 10·dtm_ply)` trên dải 1500–2990:
     - Ưu: còn độ dốc DTM trong vùng chưa bão hoà hoàn toàn.
     - Nhược: đè lên thang eval trung cuộc (± vài trăm đến ~1500); BCE/WDL dùng cùng scale `S=1042,31` sẽ **lệch đơn vị**.
     Đề xuất đo (đội tự chạy): hai net cùng data, một `lambda=1` + kẹp mate thành WDL∈{0,1} không độ dốc DTM; một bật công thức 1500–2990. Cổng: Spearman(ρ) giữa output net và −DTM trên tập thắng-chỉ; A/B ≥300 ván NoBook, cấu hình thi đấu. **Không chọn công thức trước khi có ρ và A/B.**
  5) **«Nước đúng» bên thua: DTM thuần + lớp luật 80 ply riêng, không trộn một nhãn.**
     Syzygy có *cursed win / blessed loss* vì luật 50 nước. Stockfish **chơi đúng** lớp này sau khi vào TB; phân tích thế dựng tay thì mơ hồ hơn (issue #5175). https://github.com/official-stockfish/Stockfish/issues/5175
     Cờ tướng mốc 80 ply (đội đã chốt chỉ engine-đấu-engine): thế «thắng lý thuyết nhưng DTC > remain» phải là **nhãn hoà theo luật** nếu chủ chọn «luật thắng», hoặc **nhãn thắng bị giảm** nếu chủ chọn «DTM thuần». Hai lựa chọn cho **hai đầu ra / hai tập**, không một cột `bestmove` trộn. Bên thắng: DTM ngắn nhất **trong ràng buộc 80 ply**. Bên thua: kéo dài DTC rồi DTM.
  6) **Curriculum:** bậc 1 đầy đủ trước → mẫu bậc 2 theo f_ván → trộn vào kho trung cuộc với tỉ lệ nhỏ rồi tăng. Án lệ đội −301 Elo vì data: cổng dừng sớm bắt buộc (xem 0.8 / A2-08).

- Nguồn: số đo trong ASK 0.1 và A3-04; FelicityEgtb README (MIT, phạm vi generator); nnue-pytorch docs (WDL + lambda); Stockfish #5175; TalkChess Felicity vs noobpwnftw (đếm Legal / luật chiếu-đuổi mãi).
- Mức tin: khung lấy mẫu + không vét 4v4 = **CHẮC**. Công thức cp 1500–2990 và hạn ngạch số ε = **GIẢ THUYẾT CẦN ĐO**. Elo EGTB Xiangqi «≥50» chỉ là ước lượng tác giả Felicity trên TalkChess — **không phải số đo đội**.

---

## 0.2 Mô hình kho

- Trả lời:
  Giữ **3 vai trò tách file**, nối bằng `vkey` (INTEGER 64-bit, **không REAL**):
  | File | Vai trò | Ghi |
  |---|---|---|
  | `datanoscore.db` | hàng đợi `(vkey, vmove)` chưa chấm | chỉ DELETE khi đã vào `data.db` thành công |
  | `data.db` | đã có điểm | không UPDATE tại chỗ từ hàng đợi; INSERT mới |
  | `positions.fendb` | master train (FEN + nhãn) | gộp ra `positionsfinal` rồi **atomic rename** |

  Bất biến `datanoscore ∩ data.db = ∅` đúng. Rủi ro hiện tại:
  1) Gộp `positions → positionsfinal → đổi tên` mà không có **manifest + hash + đếm theo nguồn** ⇒ mất lô / nhân đôi lô / lô cấm lọt.
  2) Cùng một `vkey` hai nhãn (hai teacher, hai depth) nếu chỉ 1 hàng ⇒ mất xuất xứ.
  3) Giao dịch một phát trên 30 GB + `journal_mode=delete` + UTF-16LE vs staging UTF-8 WAL = đúng bệnh K2.

  **Kiểm hậu gộp (không cần bản chép thứ hai 42 GB):**
  - Trước: `COUNT(*)`, `MIN/MAX(vkey)`, `SUM(vkey)` (checksum thô), SHA256 tệp, sổ `nguon_lo`.
  - Trong: lô 50k–200k, bảng `tien_do(lo_id, vkey_from, vkey_to, n_ok, n_skip, sha_lo)`.
  - Sau: `PRAGMA quick_check;`, đếm theo `nguon`, mẫu 10⁴ `vkey` đối chiếu FEN/Zobrist, ca đỏ «0 dòng nguồn cấm».
  - Đổi tên: ghi `positionsfinal` cùng volume → `MOVE`/`REPLACE` atomic trên NTFS.

  **Có nên giữ 2 tệp nối vkey?** Có. Một tệp vừa hàng đợi vừa master khiến GUI đọc lúc worker ghi. `vkey` là khoá tự nhiên; FEN chỉ là payload.

- Nguồn: mô hình đội trong ASK 0.2 / K2; SQLite atomic rename + WAL docs (https://www.sqlite.org/wal.html , https://www.sqlite.org/atomiccommit.html).
- Mức tin: tách 3 vai trò + rename atomic = **CHẮC**. Cỡ lô 50k–200k = **GIẢ THUYẾT CẦN ĐO** trên máy 47,6 GB.

---

## 0.3 Sửa kho toàn bảng

- Trả lời:
  567.206.917 dòng, ~30 GB, **không sửa một giao dịch**. Rollback journal 5 GB khi bị cắt là đúng triệu chứng `DELETE` journal + txn khổng lồ.

  Quy trình an toàn:
  1) **Snapshot lạnh:** copy file khi không writer (`PRAGMA query_only` / dừng GUI). Không tin vào «đang mở mà copy».
  2) Tạo DB mới cùng schema, `vkey INTEGER PRIMARY KEY` (hoặc index unique), **không** để SQLite affinity biến INTEGER thành REAL.
  3) Đọc lô bằng cursor; ghi lô vào DB mới; mỗi lô `COMMIT`; ghi `tien_do`.
  4) Quy tắc dòng:
     - REAL trùng INT sẵn có (64.795.168): **bỏ REAL**.
     - REAL không trùng: `CAST` qua `CAST(vkey AS INTEGER)` chỉ khi giá trị là số nguyên đúng bit (kiểm `vkey == floor(vkey)` và nằm trong int64). REAL không biểu diễn hết int64 — đây đúng bug `bind_double`.
     - 7.830.204 book cấm: lọc theo sổ nguồn đã chuẩn hoá (K3), không theo tên mojibake.
     - INTEGER khoá sai ~1,3 M: loại `vkey IN (0, -9223372036854775808)` và binade âm quanh −2^55.
  5) Sau khi DB mới `quick_check` + đếm = 494.581.545 ± sai số đội công bố → đổi tên atomic. Giữ snapshot đến khi A/B train không hồi quy.

  **Nhận diện khoá sai còn sót:**
  ```sql
  -- minh hoạ, đội chạy trên máy mình
  SELECT typeof(vkey), COUNT(*) FROM the GROUP BY 1;
  SELECT COUNT(*) FROM the WHERE vkey = 0 OR vkey = -9223372036854775808;
  SELECT COUNT(*) FROM the WHERE vkey < 0 AND (vkey & 0x7fffffffffffff) = 0;
  SELECT COUNT(*) FROM the WHERE typeof(vkey) != 'integer';
  ```
  Thêm: FEN parse fail, Zobrist tự tính ≠ vkey, trùng FEN khác vkey.

  WAL cho kho 30 GB **đọc nhiều bởi GUI**: nên bật WAL + `busy_timeout` + **một writer** (mutex named trên Windows). Đừng để 2 tiến trình ghi. https://www.sqlite.org/wal.html

- Nguồn: số đo ASK 0.3; SQLite typeof/affinity (https://www.sqlite.org/datatype3.html); IEEE-754 không chứa mọi int64.
- Mức tin: cấm một txn 30 GB + lọc typeof = **CHẮC**. Bitmask binade 2^55 = **GIẢ THUYẾT CẦN ĐO** (đội đã có danh sách ~1,3 M — đối chiếu).

---

## 0.4 CBL

- Trả lời:
  2,92 M ván CCBridgeLibrary là **policy / sách nước**, không phải nhãn cp tin được. Dùng:
  - **Policy / ranking nước:** nước được chơi + chú giải nếu giữ được.
  - **Value:** chỉ sau khi teacher chấm lại thế yên; không tin điểm ghi trong sách.
  Lọc: hash ván (chuỗi ICCS/FEN chuẩn) + sổ 37 book cấm + loại ván < N nước / bất hợp lệ / mojibake nguồn.
  Định dạng giữ chú giải: **không** nhét chú giải vào `positions.fendb`. Giữ sidecar (PGN/WXF/DhtmlXQ) keyed theo hash ván. `.cbl` là định dạng đóng của CCBridge — **không tìm được đặc tả chính thức công khai**. Importer đội (`DocCcBridge.cs` nêu trong bản đồ nguồn ASK03) là nguồn nội bộ; AI ngoài không có zip thì không soi được lệch zlib v3.

- Nguồn: số đo ASK 0.4; không có spec `.cbl` công khai khi tìm.
- Mức tin: tách policy/value = **CHẮC**. Spec `.cbl` = **KHÔNG BIẾT** (không có tài liệu chính thức).

---

## 0.5 AUTO giả lập + đăng nhập nền tảng (kể cả H1–H4)

- Trả lời:
  **(a) Nhận bàn bền — làm được, ưu tiên học mẫu tại chỗ hơn mạng lớn.**

  Pipeline 4 tầng đội đã có là đúng hướng. T3 (15 lớp) nên bắt đầu bằng **template matching trên ô đã cắt**, học từ thế khai cuộc (32 quân vị trí biết trước) + mẫu ô trống. Quân cờ tướng = chữ Hán trong vòng tròn: đặc trưng màu vòng + ink chữ ổn định hơn CNN khi khách đổi skin liên tục.

  Bẫy đã biết (không cần zip): quân đang chọn/tô sáng, mũi tên nước vừa đi, hoạt ảnh bay, bàn lật 180°, DPI ≠ 100 %, ô không vuông, giả lập (LDPlayer/BlueStacks/MuMu/AVD) scale cửa sổ. Khi tin < ngưỡng → **cấm đi**, báo «không nhận ra».

  Mã mở nhận **bàn cờ tướng từ ảnh chụp màn** có công bố độ chính xác + kho sẵn: **không biết link đạt chuẩn production**. Có thư viện luật/FEN (xiangqi.js) nhưng đó không phải nhận ảnh. https://github.com/lengyanyu258/xiangqi.js

  **(b) Đăng nhập từng nền tảng — ranh giới rõ.**

  Tôi **không** mô tả dịch ngược Unity IL2CPP, bundle mã hoá, protobuf TCP+UDP, vượt PlayIntegrity/DexGuard, hay cách qua mặt «không cho đăng nhập qua proxy». Đó là can thiệp giao thức/chống đảo của phần mềm bên thứ ba.

  Cái biết ở mức sản phẩm:
  | Nền tảng | Cách hợp pháp, bền | Nội bộ giao thức |
  |---|---|---|
  | clubxiangqi.com, zigavn (web) | WebView2 nhúng trang chính thức, user tự đăng nhập | không cần tự viết giao thức |
  | vndynapp | ASK nói REST — nếu đội đã có endpoint chính thức thì dùng REST + token user | chi tiết API = không đủ dữ liệu công khai |
  | JJ / MoveSky / Ziga / ZingPlay / Kỳ Vương | user đăng nhập **trong app gốc** (giả lập hoặc cửa sổ thật); GUI chỉ nhận cửa sổ + bàn | **KHÔNG BIẾT** nội bộ; không đoán |

  Relay `127.0.0.1` bị chặn «không cho đăng nhập qua proxy»: đây là chống man-in-the-middle phía sàn. **Không** đề xuất cách né.

  **(c) H1–H4 MoveSky:** cần **một phiên thật do chủ sở hữu ghi trên tài khoản của mình** (cửa sổ, nước đi, danh sách đối thủ hiện trên UI) hơn là «đọc mã máy». Tôi không xác định được «các bạn hiện tại» là 对阵表 hay friend-list, cũng không xác nhận nick `qqmovesky` / `ccmsexe` / `cmllh`. Web nhúng **thay** giao thức native: chỉ khi sàn có web play ổn định; nếu chỉ có client thì WebView không đủ.

  Thiết kế AUTO nên coi **hồ sơ kết nối là dữ liệu** (A2-03): nhận diện HWND/title, vùng bàn, cách đọc nước, cách gửi nước (SendInput/PostMessage vào cửa sổ app **đã mở sẵn**), cách biết lượt, cách hết ván. Thêm sàn = thêm hồ sơ, không sửa mã lõi.

- Nguồn: đặc tả đội 0.5 / A2-01…A2-05; xiangqi.js (luật, không phải CV); nguyên tắc không dịch ngược phần mềm bên thứ ba (DEBATE.md luật 3).
- Mức tin: T3 template + fail-closed = **GIẢ THUYẾT CẦN ĐO**. Giao thức JJ/MoveSky/Kỳ Vương = **KHÔNG BIẾT**. Né proxy/PlayIntegrity = **không trả**.

---

## 0.6 Giao diện TieuLongNu

- Trả lời:
  Đã có (theo ASK): đăng nhập dropdown Chiến đấu+Nghiên cứu, panel online, mời/nhận, đánh trực tiếp, AUTO Analyze/Full/Stop, học mẫu, skin, KyPho, ảnh→FEN, 3 ngôn ngữ, engine 64-bit.

  So với GUI thương mại kiểu BHGui/SharkGui, phần **thường thấy trên màn đánh** mà ASK không liệt kê — suy luận, không phải tài liệu nội bộ họ:
  - hồ sơ kết nối theo sàn (lưu/nạp, tự nhận cửa sổ)
  - Full ↔ One (một nước rồi trả quyền)
  - đổi bên / ra nước ngay / ponder
  - đồng hồ đối thủ + cờ hết giờ
  - MultiPV / bảng nước + eval bar
  - sách bật/tắt lúc đánh
  - tự hồi phục khi mất bàn (đổi cỡ, overlay QC)
  - nhật ký nước ICCS + lưu PGN một phím

  BHGui: đội tự ghi là **nối client / hồ sơ cửa sổ, không bóc ảnh**; tác giả BH nói giao thức đổi thì tính năng chết. Tôi **không** có `bhhelp.pdf` trong phiên này nên không trích trang.

  **Thanh AUTO 8–10 nút (Thực chiến):**
  1 Analyze · 2 Full · 3 Stop · 4 One/Full · 5 Đổi bên · 6 Ra nước ngay · 7 Chọn engine · 8 Sách on/off · 9 Lưu/nạp hồ sơ nối · 10 (tuỳ) Học mẫu.
  Chế độ = **bảng dữ liệu**, ẩn chứ không xoá (A2-05). Tooltip 3 thứ tiếng.

  Mã WPF C# 5 mẫu cho phần thiếu — chỉ khung visibility theo mode, không phải copy GUI người khác:

```csharp
// C# 5, csc /langversion:5 — giả định
public sealed class NutMode {
    public string Id;      // "Analyze"
    public bool ThucChien;
    public bool NghienCuu;
}
// Một mảng NutMode[] quyết định Visibility. Collapsed nếu mode hiện tại = false.
// Cổng: duyệt LogicalTree, mọi Button.Tag nằm ngoài danh sách mode => FAIL.
```

- Nguồn: danh sách chức năng ASK 0.6 / A2-05 / A2-35 (ghi chép đội về BHGui/Shark); không có URL help chính thức Shark/BH mở được trong phiên này cho từng nút.
- Mức tin: tách 2 mode + thanh 8–10 nút = **GIẢ THUYẾT CẦN ĐO** (UX). Chi tiết nội bộ BH/Shark = **KHÔNG BIẾT** chỗ không có tài liệu.

---

## 0.7 Encrypt book + update khách

- Trả lời:
  Mục tiêu chính đáng: sách `.obk` là tài sản, update không đè 5 file config, 3 loại key (hết hạn / ngưng vĩnh viễn / hạn cập nhật).

  Thiết kế **đủ dùng, không phải chống-dump tuyệt đối** (máy khách do khách kiểm soát — dump RAM luôn làm được nếu quyết tâm):
  1) Sách trên đĩa: AES-256-GCM, mỗi bản build một `key_wrap`.
  2) Key mở sách = KDF(machine-id ổn định + secret trong license file). Machine-id: lấy vài nguồn (Volume serial + MachineGuid) hash; chấp nhận false-deny khi đổi mainboard.
  3) License file: payload `{exp, revoked, update_until, machine_hash}` + chữ ký Ed25519 server. 3 loại key = 3 cờ trong payload, **không** 3 thuật toán mã.
  4) Engine/GUI đọc sách qua API «cho FEN → vài nước», không map cả file plaintext suốt đời. Vẫn bị dump nếu ai gắn debugger — ghi rõ vậy, đừng hứa «chống dump RAM».
  5) Update: installer chỉ ghi file trong allowlist. 5 config nằm **deny-list**. Ca đỏ: hash 5 file trước/sau update phải giống; hash sách sau update phải đổi nếu có sách mới.

  Không đưa kỹ thuật chống-debug/anti-dump cấp malware (hide pages, detect Cheat Engine, v.v.).

- Nguồn: mô hình owner 21/09 trong ASK; AES-GCM / Ed25519 là chuẩn NIST/RFC.
- Mức tin: deny-list config + license ký = **CHẮC** về quy trình. «Chống dump RAM» hoàn toàn = **không khả thi**, nói thẳng.

---

## 0.8 Kiểm toán engine trước train (mốc 01/10)

- Trả lời:
  Còn 20 ô CHƯA ĐO / 13 KHÔNG QUA (13/09). Cắt rig 60–100 h bằng **thứ tự fail-fast**, không bằng «chạy hết cho có».

  Thứ tự soi (mỗi cổng phải từng đỏ):
  1) **Luật / movegen** — `go perft` vị trí chuẩn + ca chiếu-đuổi; sai thì mọi Elo vô nghĩa.
  2) **Đơn vị mate** — UCI `mate N` = nước, EGTB/chessdb thường ply. ASK A3-01c đã chỉ lệch ×2. Một test: KRK mate đã biết DTM ply ↔ output UCI.
  3) **Khoá băm + rule50/80** — đổi mốc 120→80, nấc `/8` từ 14: rủi ro TT confuse «gần mốc» vs «xa mốc». Cổng: cùng thế hai giá trị halfmove khác nhau phải khác hash (hoặc có kế hoạch rõ).
  4) **Đồng hồ / ponder / threads** — bench Threads=1 bit-exact trước; Threads=12 là cổng riêng, không chặn train nếu Threads=1 sạch.
  5) **Eval-net nạp** — Fairy `setoption EvalFile` sai thì **tụt HCE im lặng** (đội đã biết, BookTool stage riêng). Cổng: eval startpos phải ≠ HCE và ổn định giữa 2 lần nạp.
  6) **Search không đi nước bất hợp lệ / ván 0 nước** — harness phải FAIL khi 0 move (A2-20 án lệ 86 ván).
  7) **Cổng ưu thế ≥400** và thang Pikafish 5 bậc: chỉ chạy khi 1–6 xanh. Cắt thời gian: 60 ván nodes-cố-định để loại net/engine gãy; 300 ván pentanomial chỉ cho ứng viên sống.

  Teacher = OngThan + Doccocaubai, net CC0 / tự train. Không dùng `pikafish.nnue` official cho net bán.

- Nguồn: ASK 0.8 / P.1; Pikafish Networks README (official cấm thương mại, net Fairy CC0). https://github.com/official-pikafish/Networks
- Mức tin: thứ tự fail-fast = **CHẮC** về phương pháp. Số giờ từng cổng = **GIẢ THUYẾT CẦN ĐO**.

---

## 0.9 App điện thoại Kỳ Viện

- Trả lời:
  Học **cấu trúc sản phẩm** JJ (MVVM, 3 lối MỜI / PHÒNG MÃ / XEM, login tách kênh, module 实名/chống nghiện riêng) — **không** học Unity IL2CPP + hot-update.

  Tối thiểu phát hành:
  - MAUI/Android: bàn + luật local, 3 lối vào ván, đăng nhập (REST đội / WebView sàn web).
  - Chơi ngay: guest hoặc token; không chặn ở splash vì thiếu 实名 nếu luật địa phương chưa bắt buộc với bản thử.
  - Engine: **trên máy ARM64** cho phân tích offline; server nhà chỉ cho phòng online / phân tích nặng. Phụ thuộc server cho nước đi = app chết khi mất mạng.
  - AUTO trên điện thoại: AccessibilityService + chụp màn hình bị Android hạn chế dần (media projection consent, hạn overlay). Đừng đặt AUTO giả lập sàn khác làm điều kiện phát hành. AUTO nên là «phân tích bàn trong app mình».
  - Chống nghiện / 实名: module tách, bật theo store Trung Quốc khi thật sự lên store đó.

- Nguồn: ASK 0.9 / Y-6; chính sách Android về media projection / accessibility là hướng chung — chi tiết API từng version = cần đọc developer.android.com lúc triển khai.
- Mức tin: engine on-device + 3 lối vào = **GIẢ THUYẾT CẦN ĐO**. Nội bộ JJ = **KHÔNG BIẾT**.

---

## ASK04-B / K1–K8

Đã đọc điều khoản bảo mật Ask04; không lưu, không lan truyền, không dùng để huấn luyện.

### K1. Đĩa phình 500 GB
- Chẩn đoán: pipeline giữ 3–4 bản cùng bytes (D 485 + C 243 + staging 30 + dò 42 + Drive cache 88).
- 1–3 ngày: **streaming attach** — không copy cả cục về C. USB: đọc trực tiếp hoặc `robocopy /J` từng lô vào staging rồi xoá lô nguồn trên C ngay. Backup Drive: `rclone`/`gdrive` từ USB, cờ tắt chunk-cache trên C (rclone `cache` off; không mount ổ ảo full).
- Kiểm không bản 2: SHA256 theo file nguồn + COUNT theo `nguon_lo`. Ca đỏ: hash lệch ⇒ dừng, không gộp.
- Tiêu chí: peak trống C ≥ 80 GB suốt một đêm nhập; số bản cùng dataset ≤ 2 (USB gốc + 1 staging).
- Rủi ro: USB rớt giữa lô → bảng `tien_do` phải nối lại được.

### K2. Gộp staging 30 GB khi bị cắt
- Chẩn đoán: 1 txn + journal delete + 2 writer.
- 1–3 ngày: WAL trên kho thật; named mutex Windows (`Global\HaTrungTinDb`); lô + `tien_do`; `busy_timeout=5000`; một writer.
- Cổng sau: count trước/sau, mẫu vkey, `quick_check`.
- WAL + GUI đọc: được, miễn writer=1. https://www.sqlite.org/wal.html
- Rủi ro: đổi UTF-16LE → UTF-8 phải migrate một lần, không vừa gộp vừa đổi encoding.

### K3. Sách cấm + mojibake
- Nguồn chuẩn = `sha256_file + ten_utf8 + nhom_cam`. Importer đọc sổ cấm **trước** INSERT. Cột `source` ghi UTF-8; tên đĩa ANSI chỉ là alias.
- Ca đỏ: đưa 1 file tên nằm trong 37 book cấm ⇒ 0 dòng mới, rc≠0.

### K4. Nhận bàn 1 chạm
- Xem 0.5 / A2-01. Tiêu chí «đã bắt»: 32 quân khai cuộc khớp mẫu; giữa ván: số quân ∈ [2,32], hai tướng trong cung, không 2 quân một ô. Fail-closed.

### K5. Kho tàn cuộc train từ gốc
- Xem 0.1. Cổng rẻ «đã học»: ρ(output, −DTM) trên hold-out bậc 1; % thế hoà net cho |cp|<ngưỡng; **không** dùng r trên tập train làm cổng (án lệ −301 Elo).

### K6. Đo chập chờn
- Ngưỡng theo tải: lưu `(p50,p90) × 3 lượt` kèm `%Processor Information(_Total)` (P.1 đã chốt). Đỏ chỉ khi p90 vượt ngưỡng **và** tải < trần (vd 40%). PID cửa sổ 20 s giữ làm nguồn tin. `Get-Counter` 1 mẫu = bỏ.

### K7. Nhiều AI một git
- Một **hàng đợi việc** (file JSON + lock); mỗi lane một worktree hoặc một thư mục `lane_*` không đụng DB. Hook: cấm commit `*.db` / `*.fendb`. Nhật ký append-only theo `lane_id`. Khi đổi tài khoản: đọc hàng đợi, không đọc lại toàn repo.

### K8. Token
- Script tự chạy: hash, count, quick_check, perft, bench, A=A nodes, grep ca đỏ, import diff.
- AI chỉ đọc: diff nhỏ, ca FAIL, quyết định kiến trúc.
- Đề bài worker: 1 mục tiêu + 1 cổng đỏ/xanh + cấm đụng path ngoài allowlist.

---

## ASK03 — câu còn mở (không có zip)

Đã đọc điều khoản bảo mật Ask03; không lưu, không lan truyền, không dùng để huấn luyện; không có zip để xoá.

Không cite đường dẫn zip. Trả theo mã, lập luận công khai.

### A3-01 — target mate
- (a) `cp = sign * max(1500, 3000-10*dtm_ply)` **không** phải hình dạng chuẩn Stockfish. Chuẩn là đưa mọi thứ qua sigmoid/WDL rồi λ. Nhét mate vào dải 1500–2990 có rủi ro nén trung cuộc và xung đột BCE nếu cùng S=1042,31.
- (b) Net value-only: mate → WDL=1 (hoặc 0) **đủ để search học khoảng cách qua MDP**. Độ dốc DTM ở cp chỉ đáng nếu cổng ρ cải thiện **và** A/B không âm.
- (c) Lệch `mate N` (nước) vs `30000-ply`: **CHẮC là bug đơn vị** nếu hai nguồn trộn. Chỗ khác trong trainer: không soi được without zip. Quy ước đội đã đo khớp EGTB `±(30000 − DTM ply)` — lấy ply làm chuẩn nội bộ, đổi UCI lúc I/O.
- (d) Công thức đề xuất mặc định TẮT: WDL ∈ {0, 0.5, 1} từ EGTB; cp teacher kẹp ±3000 cho thế không mate. Ca: Spearman trên 5k thế thắng bậc 1.

Mức tin: đơn vị nước/ply = **CHẮC**. Công thức 10*dtm = **GIẢ THUYẾT CẦN ĐO**.

### A3-02 — lọc «nước tốt nhất là ăn»
- (a) Ở tàn cuộc, nước đúng **thường** là ăn/đổi. Lọc này cắt đúng phần EGTB dạy. **Tắt trên kho tàn cuộc.** Thay «thế không yên» cần movegen — comment `:621` nói thư viện không có movegen Xiangqi thì **đừng giả lọc**.
- (b) `in_scaling=1000` + nhãn 29900 ⇒ sigmoid ≈ 1. Map: `score_train = clip(score, -4000, 4000)` với mate thay bằng WDL; hoặc scale riêng `score_tb = 4000 - k*dtm` chỉ khi cờ `use_dtm_cp` và cờ này **nằm trong cache key**.

### A3-03 — nhãn từng nước + 80 ply
- (a) Value-only: nhãn **thế con** (sau nước) rồi search chọn max. Ranking loss giữa các con là phụ, tốn movegen lúc train.
- (b) Cursed/blessed: Stockfish search chơi đúng DTZ; trainer NNUE phổ biến **không** đưa DTZ vào feature. Hệ quả: nếu gắn THẮNG cho thế quá mốc → net khích lệ kéo ván đến hoà luật; nếu gắn HOÀ → net chịu hoà sớm, search phải MDP/50-rule lo chuyển hoá. Chủ chọn; hai hệ quả đó là kỹ thuật, không phải đạo đức.
- (c) Trainer công khai dùng dtz làm feature: **không biết** tên trainer NNUE chính thức làm vậy. Syzygy WDL 5 lớp dùng lúc **probe**, không lúc train net.

### A3-04 — hạn ngạch
- (a) Theo **f_ván**, không đều, không thuần log|ô|. Hoà trọng số thấp hơn thắng/thua.
- (b) Rút đều trong ô ≠ phân bố ván thật (0,023 % vs ô không sĩ/tượng khổng lồ). Trộn thế tàn từ ván là bắt buộc.
- (c) Rủi ro lớn nhất = đúng án lệ −301 Elo: lô tàn cuộc áp đảo trung cuộc. Đề xuất bắt đầu 5–10 % mẫu tàn cuộc, cổng A/B âm thì rollback. Số 5–10 % = **GIẢ THUYẾT CẦN ĐO**.

### A3-05 / A3-06 — EGTB trong search
- Felicity MIT, `.fexq`. Hook probe trước Q-search, **tắt = bench bit-exact**.
- (a) `THUA && dtc > remain` → trả **cận hoà** (hoặc 0 theo luật đội), đừng bỏ bảng im lặng nếu chủ đã chốt luật 80 ply.
- Elo EGTB cờ vua ~vài Elo (Felicity/Stockfish 6-men). Xiangqi: ước «≥50» trên TalkChess **chưa phải số đo mở**. P.1 đội đã chốt «EGTB chưa đáng đưa vào search trong» — ưu tiên **nhãn train** hơn probe trong search trước 01/10.
- https://github.com/nguyenpham/FelicityEgtb  
  https://talkchess.com/viewtopic.php?t=83676

### A3-07 — 80 ply vs nấc băm /8
- Đổi mốc 120→80 mà giữ nấc từ 14: **có thể** va TT. Phải đo: hai vị trí khác halfmove, cùng nước, hash có khác; A=A sau đổi mốc.
- Stockfish rule50 giảm eval khi gần 50; muốn bên thắng chuyển hoá: phạt eval khi `remain` nhỏ **và** WDL_tb=win. Hằng 120 còn sót: **không biết** without grep zip.

### A3-08 — Jieqi −33 Elo
- Không có diff 8 tệp trong phiên. Khả năng hợp lý (suy luận, không cite hunk):
  - `materialKey` mới đi vào bảng imbalance **chưa tune** cho quân úp.
  - PSQT cập nhật hai lần / quên undo.
  - TT key đổi làm mất hit nhưng eval mới kém hơn eval sai-cũ (đúng luật, yếu hơn).
- Phép ≤30′: bật riêng B2b, riêng B1m, 100 ván nodes 200k Threads 1 — đúng đề đội.

### A3-09 — Legal Felicity ≠ bộ đội
- Felicity in thống kê `Total positions` / `Legal positions` theo không gian chỉ số generator (ví dụ krkcn trên TalkChess). Số `krk Legal 4806` vs đội 8748: khác lọc chiếu / đối mặt / gương / bên đi — đội đã thử 16 tổ hợp không khớp.
- Lệch hoà 1–1,5 % Đen đi: khả năng **luật chiếu-đuổi mãi**. noobpwnftw trên cùng thread nói adjudication double-perpetual và cách đếm perpetual vào DTM khác nhau. Max-DTM khớp 40/40 ⇒ generator cùng độ sâu, khác luật hoà.
- Lỗi trong `tan_cuoc_liet_ke.py`: không đọc được file. Không khẳng định bug dòng :182/:209/:275.
- https://talkchess.com/viewtopic.php?t=83676&start=80

### A3-10 — chessdb `W-M-nnnn`
- API docs chính thức: https://www.chessdb.cn/cloudbook_api_en.html (mở được trang API). `queryall` trả `move, score, rank, winrate, note`. Docs **không** định nghĩa `M-nnnn` đếm từ thế hiện tại hay sau nước, cũng không nói `rank` 2/1/0 nghĩa formal ngoài «sắp xếp».
- Hạn mức 100.000/IP/24h: **không thấy** trên trang API khi đọc. Điều khoản 404 theo đội. → **không biết** hạn mức chính thức.
- Thế «thắng số chẵn» rc 7: không đủ dữ liệu (không có FEN).

### A3-11 — thầy bản quyền
- (a) Lỗ hổng trọng tài: hết 4e6 nút = KHÔNG BIẾT nhưng thầy báo mate nông ⇒ vẫn có thể mate thật sâu hơn ngân sách. Cần EGTB trước search.
- (b) Dè dặt = trọng số mẫu × min(1, depth/d0) × đồng thuận; kẹp |cp|; không đưa mate giả vào WDL=1.
- (c) UCCI/UCI `mate N`: UCI = nước (moves). Engine thương mại Xiangqi **không biết** từng hãng (không có tài liệu mở). Liên quan A3-01c: chuẩn hoá về ply nội bộ.

### A3-12 … A3-16
- Cần grep zip. **Không biết** đường thứ 5, cuộc đua Dispatcher cụ thể, hay `_cache_key` thiếu cờ — không bịa `tệp:dòng`.
- Thiết kế A3-12 không lọt (lập luận): một `AutoState` setter duy nhất; `DuocPhepGuiNuocRaNgoai` chỉ true khi `(Analyze && Full && !ChiXem)`; mọi SendInput/PostMessage gọi predicate đó. Cổng: bảng lối vào × trạng thái, mỗi ô một ca đỏ.
- A3-14 (c) AccessibilityService + capture trên Android mới: cần consent; Play policy hạn chế accessibility không vì người khuyết tật. Cite lúc triển khai từ developer.android.com — không bịa số API.

### ASK11 Q-A…Q-H
- Q-A: trộn trọng số + tăng dần tàn cuộc, không curriculum cứng «xong hết bậc 1 mới đụng trung cuộc» nếu kho trung cuộc đã là phần lớn Elo.
- Q-B: xem A3-01 / A3-05.
- Q-F self-play song song: được, tắt sách, 4–8 nước ngẫu nhiên (đã chốt).
- Q-G: cổng từng giai đoạn = ρ DTM + A=A + A/B 300 + đối chứng nhãn xáo âm.

---

## ASK02 — ưu tiên (P.3)

Đã đọc điều khoản bảo mật; không lưu, không lan truyền, không dùng để huấn luyện.

### A2-01 — T3 phân loại ô
| Phương án | Độ chính xác kỳ vọng | ms/khung | Dữ liệu | Bẫy |
|---|---|---|---|---|
| Template trên ô cắt + học khai cuộc | cao nếu skin tĩnh | vài–vài chục ms CPU | 14 mẫu + trống, 1 bàn | highlight, animation |
| Màu + ink chữ Hán | vừa | rất rẻ | ít | skin lạ, quân đỏ/đen gần nhau |
| CNN/ONNX nhỏ | cao nếu train đúng skin | tuỳ runtime | hàng nghìn ô | đổi skin ⇒ chết |

Chọn **template + tự học lúc bấm khai cuộc** trước. Sản phẩm làm vậy: đội tự mô tả BH/Shark khác đường (nối client). Mã mở CV Xiangqi screenshot có số công bố: **không biết link**. Ngưỡng: max-score template < τ ⇒ không nhận ra; τ đo trên 50 skin/cửa sổ.

### A2-02 — 2 cú bấm xe
- 2 tâm xe góc cho lưới **affine trực giao** 9×10 nếu ô gần vuông. Ô không vuông / phối cảnh → cần điểm 3 hoặc giả sử hàng//cột theo Hough.
- Bàn lật: màu quân góc 2 + chữ (車 vs 车 cùng nghĩa nhưng màu vòng). Cung tướng 3×3 là neo.
- Lượt đi từ 1 khung: **không chắc**. Đồng hồ nhấp, viền nước vừa đi — GUI thương mại dùng gì: **không biết** (cấm đoán từ tên nút).
- Ca đỏ: bấm nhầm quân, lệch ≥ 0,4 ô, cửa sổ đổi cỡ sau setup → phải hỏi setup lại.

### A2-03 — full/one + hồ sơ dữ liệu
- Full = máy đánh cả ván; One = một nước trả quyền. Đội ghi BHGui có Autoplay vs Human-machine. Tài liệu gốc BH không có trong phiên → không trích nguyên văn.
- 3 đường: ảnh (phủ rộng, yếu khi UI đổi); UIA/Win32 (bền hơn nếu control thật); hook web (sâu, chết khi site đổi). Thương mại «nối nhiều sàn» thường = **thư viện hồ sơ phía server**, không phải CV giỏi hơn — suy luận từ chính câu tác giả BH («giao thức đổi là chết»).
- JSON hồ sơ (ý): `id, title_regex, class, dpi, board_rect_or_2click, read=screen|uia|js, send=sendinput|postmessage|js, turn=diffhash|clock, end=result_text`.
- Hỏng: 3 frame liên tiếp fail-closed → disable auto, không đi bừa.

### A2-04 — 4 khúc trễ
| Khúc | Đo | Cắt | ms kỳ vọng | Nguồn |
|---|---|---|---|---|
| Đối thủ đã đi | hash vùng bàn | DXGI duplication nếu được | vài–16 ms/khung | DXGI là API Microsoft; số ms **ước** |
| Đọc thế | T3 template | chỉ so ô đổi | phụ thuộc T3 | đo đội |
| Engine | `go movetime` / nodes | tách tiến trình | theo TC | — |
| Bấm | SendInput vs PostMessage | nghỉ ≥ thời gian animation app kia | 30–50 ms giữa 2 bấm là **hằng chép, chưa đo** (ASK) | không có số GUI thương mại công bố |

Số trễ tổng GUI thương mại: **không có số**.

### A2-05 / A2-35
- Bộ nút tối thiểu: xem 0.6. Nguồn từng GUI cho từng nút: **không biết** nếu không mở help/video.
- A2-35 bảng:

| GUI | Nhận bàn bằng | Sàn | Tự hồi phục | Nguồn |
|---|---|---|---|---|
| BHGui | nối client + hồ sơ cửa sổ, **không bóc ảnh** (đội ghi) | một sàn nói chuyện thẳng server (đội ghi) | không biết | ghi chép đội; không có pdf trong phiên |
| SharkChess | không biết (đóng nguồn) | đội ghi Yixuan/QQ/JJ/TianTian — **không kiểm được từ đây** | không biết | ghi chép đội |
| PengFei / 兵河 / 象棋名手 / 旋风 GUI | **không biết** | không biết | không biết | — |
| Mã mở 连线 | **không biết kho** | — | — | không bịa repo |

3 việc copy trước: (1) hồ sơ kết nối = dữ liệu; (2) fail-closed khi mất bàn; (3) Full/One + Stop giữ bàn.

### A2-06
- Bin-pack cặp có Thầy trước; giữ process Thầy sống (cấm respawn <60 s — đã chốt).
- >64 thread: processor group — P.1 chốt đo `\Processor Information(_Total)`. Ghim CPU Sets: có thể giảm nhiễu; lệch Elo nếu một bên bị group xấu — **đo A=A**.
- Ponder nhiều cặp: **tắt** khi chia lõi chặt (đội đã vấp ước luồng ×2).
- 6 vs 20 lõi cùng TC đồng hồ: không so tài.search; dùng **nodes** để đo chất lượng nước, đồng hồ để đo sản phẩm.
- cutechess-cli / fastchess xếp lịch lõi không đều + max instance thương mại: không khẳng định có option sẵn — **không biết option cụ thể**, tự viết scheduler.

### A2-07
- Ít ván, không cân: **Ordo** hoặc BayesElo; neo Thầy cố định làm méo CI của Trò (phương sai dồn vào Trò).
- Trộn có sách / không sách cùng bảng: **không hợp lệ** nếu mục đích đo engine.
- Phân biệt 30 Elo chỉ đấu Thầy: rough n ≈ (σ/δ)²; σ ván cờ ~80–120 Elo-equivalent; hàng trăm ván mỗi Trò — đội đã đo 300 ván ±7,2. Công thức chính xác phụ thuộc pentanomial; dùng chính harness đội.

### A2-08
- **Không cập nhật net sau từng ván.** Gom lô. Quên thảm + nhiễu + án lệ −301 / r đẹp nhưng −338…−800 Elo.
- Lc0 train liên tục từ self-play có cửa sổ dữ liệu lớn (hàng triệu vị trí), không vài trăm thế/ván. Không lấy số cửa sổ cụ thể nếu không mở paper lúc viết.
- λ: nnue-pytorch `lambda_=1` = chỉ score, `0` = chỉ WDL. https://github.com/official-stockfish/nnue-pytorch/blob/master/docs/nnue.md
- Ưu tiên ván Thầy: trọng số 2–3×, **không** 10× (lệch phân phối). Cổng: A/B KTC95>0 + đối chứng nhãn xáo âm + bit-exact replay.
- CPU vs GPU: cố định seed, thuật toán ổn định, cùng quantize cuối. Không giả «giống hệt» nếu kernel khác.

### A2-09
```json
{
  "machine_id": "host44",
  "engine_exe_sha256": "",
  "net_sha256": "",
  "config": {"nodes": 200000, "threads": 1, "book": false},
  "t0": "ISO", "t1": "ISO",
  "n_records": 41496,
  "files": [{"path": "a.bin", "sha256": "", "bytes": 0}]
}
```
Trùng vkey khác nhãn: giữ **cả hai** + cột `teacher_id, depth, license`. Idempotent: `goi_id` unique. Gói cắt: bytes ≠ manifest ⇒ từ chối. Nhãn thầy thương mại: cột `license=proprietary_teacher` để lọc khỏi net bán.

### A2-10
*Ý kiến kỹ thuật, không phải tư vấn pháp lý.*

| Engine | EULA công khai? | Điều khoản train từ output | URL/ngày | Rủi ro |
|---|---|---|---|---|
| 旋风 / XQMS / BugChess / 鲨鱼 | **không tìm được EULA công khai** trong phiên này | — | — | không biết chữ ký khách đã click |
| Pikafish official net | có | cấm thương mại, cấm trọng số phái sinh | https://github.com/official-pikafish/Networks | đã chốt đội |
| Pikafish engine GPL | có | GPL mã nguồn, **tách** với license net | https://github.com/official-pikafish/Pikafish | net ≠ engine |

Tiền lệ ChessBase↔Stockfish nói net «included or dynamically loaded» — đội đã ghi, không mở rộng. LLM ToS cấm train từ output là **hợp đồng dịch vụ**, không phải án. Kết luận: giữ luật đội (net bán = CC0/tự train); cột xuất xứ để lọc.

### A2-11
1. Lấy mẫu thế từ kho + playout ngẫu nhiên hợp lệ (không sách).
2. Lọc thế yên (không chiếu, SEE≤0) trước khi chấm đắt — **GIẢ THUYẾT CẦN ĐO**.
3. Chấm teacher depth/nodes cố định.
4. WDL: logistic(score / S) nếu không có ván; S Xiangqi **chưa có số công bố chuẩn** tương đương pawn-advantage chess — đo S từ dữ liệu đội.
5. λ xem A2-08.
6. Cổng 10× thế/s so với 0,75 thế/s hiện tại.

NNUE λ=1 (chỉ score) là chế độ nodchip/Stockfish cổ điển, **không mặc nhiên kém** nếu score teacher tốt. Số «kém rõ» công bố: không gắn một Elo.

### A2-12
- Depth 20 mỗi nước: Hash lớn giúp ít hơn TC dài. Số hashfull/Elo-theo-Hash công bố Pikafish: không có bảng trong phiên. **Chạy phiếu Hash 16/256/1024/2048 đội đã soạn** — đó là câu trả lời đúng.
- Large pages: `SeLockMemoryPrivilege`; 41 tiến trình × 2 GB large page có thể thất bại / phân mảnh.
- Datagen: nhiều process 1 thread thường dễ scale; 1 process nhiều thread chung Hash tốt khi RAM là nút. Đội đã 95 % CPU → Hash không phải nút hiện tại.
- EGTB trong RAM lúc datagen: lợi **nhãn đúng tàn cuộc**, không phải thế/s.

### A2-13 … A2-17, A2-18…A2-32
Trả ngắn chỗ có nguồn; còn lại không đốt token đoán.

- **A2-13:** `DynamicResource` + thay `MergedDictionaries` lúc chạy là đúng WPF. App csc không App.xaml: load ResourceDictionary từ pack/file rồi clear/add merged. Màu cứng: regex `Color.FromRgb|#|Brushes\.|SolidColorBrush`. DynamicResource dày = tra cứu lại; bàn cờ vẽ mỗi frame nên brush lấy một lần.
- **A2-17:** `GeneratedInternalTypeHelper` sinh khi XAML reference kiểu không public cùng cách markup compile expect. Cổng «file > 10 B ⇒ rc≠0» có thể false-positive nếu bản hợp lệ vẫn sinh file. `Handled=true` nuốt lỗi — log + đừng đánh dấu handled trừ đúng loại đã biết. Issue dotnet/wpf cụ thể: **không chốt số issue trong phiên này** → không bịa.
- **A2-18:** Gương trái-phải Xiangqi **không** luôn tương đương (cung, tốt qua sông, chiếu-đuổi theo hướng). Nhân đôi gương chỉ sau khi đối chiếu luật + perft thế gương. PackedSfen Xiangqi: tự viết. Cổng lỗi câm: mọi field thiếu ⇒ rc≠0 (đã học từ −301 Elo).
- **A2-19:** Nâng 1260×256 → HKA khi A/B thắng sau khi chấp nhận NPS 0,7–0,8×. Chưng cất hỏng: val loss đẹp + A/B âm (đội đã có −407). Dấu hiệu sớm: ρ với teacher giảm trên hold-out **khác** tập train.
- **A2-20:** Ván 0 nước = FAIL cứng. Tag PGN: `Term`, `PlyCount`, `TimeLoss`, `Illegal`.
- **A2-21:** Phát hiện −20 Elo SMP: hàng nghìn ván nếu nhiễu ponder; đo ponder OFF trước. Công thức SPRT/pentanomial dùng harness đội.
- **A2-22:** Tách cụm: bật từng kỹ thuật trên nền **đã có phụ thuộc** (QS+TT chỉ sau QS ổn). Âm khi nhập lẻ là chuyện thường ở Stockfish fishtest.
- **A2-23:** Hôm nay: αβ + NNUE (Pikafish/Fairy). Sau NNUE: chưa có gì thay ở sản phẩm cờ đỉnh không GPU rời. Lai αβ+MCTS sản phẩm thật cờ tướng: **không biết**.
- **A2-24:** Gộp còn **2 cây ship** (OngThan + Docco) + 1 Jieqi nếu còn bán. Tầng luật lặp/chiếu-đuổi phải **một cửa**. 105 thư mục trùng tên là nợ.
- **A2-26:** `netstandard2.0` DLL: `csc` tham chiếu DLL được (`/r:`). C# 5 lõi: tránh `?.` `$""` tuple async — viết tay null-check.
- **A2-31:** Dồn **engine + GUI cờ + kho nhãn**. Dừng hoặc đóng băng: thị trường token, CinPlay, Comfy nhiều tập, app thứ 7. Bài toán đang hiểu lệch: thêm bề mặt (7 hệ thống) trong khi Elo còn −230…−320 và lô −301. Đường ngắn tới sản phẩm bán: GUI nhận bàn được + engine CC0 đủ mạnh + update không đè config. Đo bằng: auto chạy được N sàn tự host / giả lập **tài khoản mình**, và A/B không âm so teacher CC0.
- **A2-32:** −318 Elo cùng net ⇒ search/time/law, không phải net. Ưu tiên: illegal/0-move, mate unit, null-move/LMR parameter so với Pikafish cùng net, rồi time mgmt. Mốc «đừng tự viết»: khi cùng net vẫn −200+ sau 1–2 tháng vá có A/B — fork Pikafish search và chỉ giữ luật/eval hook.

### A2-33 — Git cắt blob mod 2³²
- Kết luận: Git trên Windows (LLP64, `unsigned long` = 32-bit) dùng `unsigned long` cho kích thước object/filter ⇒ **nội dung** bị cắt `size mod 2³²`, không chỉ field index. Đây đúng hiện tượng đội đo (15.157.196.800 → 2.272.294.912, 1 MB đầu trùng SHA256).
- Nguồn: thảo luận Git LFS/Windows files >4GB; `unsigned long` vs `size_t`; git-for-windows theo dõi lâu (vd #1063 / PR #2179 historical); series 2026 vẫn sửa `size_t` cho object >4GB. https://github.com/git-for-windows/git/issues/1063 (issue tracker cổ điển) · mail «Creates unreadable pack files… sizeof(unsigned long)» · commit giải thích LLP64 `read_blob_entry`.
- Git 2.55: **không khẳng định đã hết mọi đường cắt loose object**. Coi **chưa an toàn** với blob >4 GiB trên Windows.
- Phòng: không commit file ≥ 4 GiB; Git LFS **cũng từng** bị cắt trước khi vào LFS; đội cấm rewrite history ⇒ file lớn để ngoài git (USB + SHA256 sidecar). Cấu hình «báo lỗi thay vì cắt»: không có switch ổn định trên bản đang dùng — **không biết** knob chắc chắn trên 2.55.

### A2-34 — distillation net official
1) Hỏi thẳng nhóm Pikafish về train từ **nhãn điểm** official: **chưa có** (không thấy issue/Discord/forum trả lời đúng câu này trong tìm kiếm phiên này; đội đã kiểm FAQ = không có).
2) Án toà / cơ quan về «distillation từ output» kiểu NNUE cờ: **chưa có** trong phạm vi đã tìm (không đếm ToS nhà LLM).
3) **chưa có** / **chưa có**.

Giữ luật đội: không dùng net official làm thầy cho net bán. CC0 Fairy `xiangqi-c07e94a5c7cb.nnue` mới là thầy được phép. https://github.com/official-pikafish/Networks

---

## Việc đội có thể đem về code ngay (không cần zip)

1. Sửa `vkey` REAL + cấm txn 30 GB; cổng `typeof(vkey)`.
2. Cache key trainer phải chứa **mọi cờ đổi nhãn** (A3-01/A3-02).
3. Tắt lọc «best move là ăn» trên kho tàn cuộc.
4. Chuẩn hoá mate về **ply** nội bộ.
5. AUTO: một setter trạng thái + fail-closed + hồ sơ sàn = JSON.
6. Update khách: deny-list 5 config + ca đỏ hash.
7. Không commit blob >4 GiB trên Git for Windows.
8. Net bán: chỉ CC0 / tự train; cột `license` trên mọi nhãn thầy.

Hết bài. Câu nào trên đây không có URL hoặc không có zip đều đã gắn **KHÔNG BIẾT** hoặc **GIẢ THUYẾT CẦN ĐO**.
