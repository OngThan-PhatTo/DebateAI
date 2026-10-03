# ASK 01 — bộ hỏi cho OxAlpha — tệp 66/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 326 (D3-15)

### D3-15 — Phụ đề chuyên nghiệp: karaoke `\kf` cho BẢN DỊCH, dò sub cứng bằng OCR Windows, che bằng hộp đặc — có ổn không?
1. **Bối cảnh:** T-48 `SourceCode/ComfyUI_Pro/ComfyUIPro/Core/PhuDeChuyenNghiep.cs` (sổ `_LANE_NOTE/lock/w_comfy_phude_0210_2026-10-02.md` §1–§2): đốt sub qua `.ass`; nhịp từ lấy từ lượt whisper thứ 2 `-ml 1` (+ `-sow`, trừ zh / ja / ko / th…); OCR là OCR có sẵn của Windows (WinRT). Đo hộp che: OCR đọc 0 ký tự ở cả độ đục 0,85 / 0,95 / 1.
2. **Đội đã chọn:** luôn đốt qua `.ass` (chữ trắng, viền đen 0,28 % chiều cao, cỡ 5,2 %, lề dưới 6 %); karaoke `\kf`, ánh xạ mốc theo TỈ LỆ ký tự (áp cả cho bản dịch — mốc ƯỚC LƯỢNG); dò sub cứng: 12 khung đều, dòng có tâm ≥ 45 % chiều cao, cần ≥ 2 khung và ≥ 20 % số khung; che bằng hộp đặc độ đục 1 (lý do thẩm mỹ); kiểm lại 2 khung sau khi che, còn chữ ⇒ không đốt; gói OCR theo tiếng của video (zh ⇒ zh-Hans-CN, ja / ko, còn lại en-US).
3. **Phương án khác:** (a) karaoke bản dịch bằng căn chỉnh cưỡng bức (forced alignment) trên audio lồng tiếng; (b) xoá sub cứng bằng inpaint video (ProPainter, LaMa, `video-subtitle-remover`); (c) PaddleOCR thay OCR Windows cho chữ Trung.
4. **Câu hỏi:** (i) Karaoke cho câu DỊCH (không có audio của chính câu dịch) — thực hành của fansub / Aegisub là gì? (ii) Công cụ xoá sub cứng offline nào chất lượng tốt, giấy phép thương mại, chạy được Windows CPU hoặc ROCm? (iii) OCR Windows với chữ Trung giản thể trong video có số lỗi công bố so với PaddleOCR không?
5. **Trả lời tốt:** tên công cụ + URL + giấy phép + yêu cầu phần cứng.

---

## Câu 327 (D3-16)

### D3-16 — SDXL-Lightning / AnimateDiff-Lightning trong ComfyUI: cfg 1.0 · `euler` · `sgm_uniform` · đúng N bước — có đúng model card?
1. **Bối cảnh:** `SourceCode/ComfyUI_Pro/ComfyUIPro/Core/Templates.cs:843-861` (trước vá) không có nhánh Lightning ⇒ checkpoint `sdxl_lightning_4step` chạy cfg 6–7 + `dpmpp_2m/karras` + 25–30 bước ⇒ ảnh cháy; «Chế độ nháp» (`Core/CheDoNhap.cs:37-39`) hạ mọi KSampler `steps > 4` về ⌈s/3⌉ ⇒ bản 8 bước bị ép 4 (sổ `_LANE_NOTE/lock/w_t13_vua_0210_2026-10-02.md` §1h).
2. **Đội đã chọn:** tên chứa «lightning» (không phải FLUX) ⇒ cfg 1.0 · `euler` · `sgm_uniform` · không câu phủ định · steps = N đọc từ «Nstep» trong tên (không có ⇒ 4); chế độ nháp KHÔNG đổi workflow có loader tên Lightning (ckpt / LoRA / unet).
3. **Phương án khác:** sampler / scheduler riêng cho bản 1-bước, 2-bước (UNet vs LoRA); cho cfg > 1 với LoRA Lightning khi cần câu phủ định.
4. **Câu hỏi:** model card ByteDance `SDXL-Lightning` và `AnimateDiff-Lightning` khuyến nghị CHÍNH XÁC sampler / scheduler / cfg / steps nào cho ComfyUI (kể cả bản 1-bước có cần chế độ dự đoán đặc biệt không)? LoRA Lightning ghép vào checkpoint SDXL khác có cần đổi tham số gì?
5. **Trả lời tốt:** trích model card Hugging Face (URL), một câu nguyên văn cho mỗi ý.

---

## Câu 328 (D3-17)

### D3-17 — Đăng video đa nền tảng: API chính thức (YouTube Data v3, TikTok Content Posting, Facebook Graph) hay tự lái trang creator trong WebView2?
1. **Bối cảnh:** T-44 / T-50 ComfyUI_Pro (sổ `_LANE_NOTE/lock/w_comfy_dang_tai_0210_2026-10-02.md` §1–§2): owner tự đăng nhập nền tảng trong WebView2 riêng (app không gõ / lưu mật khẩu, gặp kiểm tra người ⇒ dừng êm báo owner). Tải bằng yt-dlp — extractor có sẵn: TikTok, Douyin (chỉ từng video), Bilibili (kênh + `bilisearch`), YouTube `ytsearch`, Facebook reel, Instagram user, Xiaohongshu (từng mục); KHÔNG có Kuaishou.
2. **Đội đã chọn:** YouTube / Facebook: owner dán token (lưu DPAPI) ⇒ đi API (resumable upload / Graph `/videos`), chưa có ⇒ tự lái creator studio trong WebView2 đã đăng nhập; TikTok Content Posting API cần app audit ⇒ ghi «API chưa được duyệt — dùng tự lái»; nguồn riêng tư ⇒ xuất cookie của hồ sơ WebView2 ra tệp Netscape TẠM cho yt-dlp, xoá ngay sau lượt; lịch đăng lặp ngày / tuần, lỡ giờ khi app tắt ⇒ chạy bù đúng một lần.
3. **Phương án khác:** chỉ API (bỏ tự lái); dịch vụ trung gian có API (Buffer, Hootsuite…); không tự đăng, chỉ chuẩn bị gói để owner bấm.
4. **Câu hỏi:** (i) Hạn mức + điều kiện duyệt hiện hành: YouTube Data API v3 upload (quota mỗi video; video từ dự án chưa kiểm toán có bị khoá «private» không), TikTok Content Posting API (audit, giới hạn khi chưa audit), Facebook Graph đăng video (quyền cần)? (ii) Điều khoản các nền tảng nói gì về tự động hoá giao diện web khi CHÍNH CHỦ tài khoản đã đăng nhập — rủi ro khoá tài khoản thực tế? (iii) Douyin / Kuaishou có API đăng chính thức cho tài khoản ngoài Trung Quốc không?
5. **Trả lời tốt:** URL tài liệu developer chính thức + câu điều khoản; số quota kèm ngày của tài liệu.

---

## Câu 329 (D3-18)

### D3-18 — AUTO cho JJ象棋 khi bundle ảnh bị mã hoá và owner KHÔNG có tài khoản: lấy mẫu bàn / quân hợp pháp ở đâu, hay nhận dạng không cần mẫu skin?
1. **Bối cảnh** (`AI_Debate/onGoing/PHIEU_JJCHESS_LOGIN_2026-09-26.md` §5–§10): ảnh bàn / quân JJ nằm trong 1.568 bundle UnityFS có trường version là hằng `0x3FCB9E08`, blockInfo không giải được LZ4 (quét XOR 1 byte: 0 khoá) ⇒ mã hoá riêng; đội KHÔNG bẻ (biện pháp bảo vệ kỹ thuật + bản quyền). Bóc được 95 ảnh RawRes + 189 PNG giao diện — 0 bàn / quân. Màn đăng nhập JJ: SĐT / mật khẩu / 微信 / QQ / 抖音 / 支付宝 — KHÔNG có «Khách». Điều khoản `www.jj.cn/agreement/regService.html` cấm client trái phép và «外挂 / 机器人». TieuLongNu AUTO đi bằng nhận dạng màn hình (chụp + `SendInput`) và đã có đường nhận bàn từ ảnh; trang JJ (đăng nhập do khách tự làm) đã ghép vào TieuLongNu (ghép 13).
2. **Đội đã chọn:** TieuLongNu chỉ làm trang đăng nhập + kênh, KHÁCH tự đăng nhập tài khoản của họ; AUTO dùng nhận dạng màn hình trên máy khách; bộ mẫu skin JJ chỉ lấy bằng chụp màn hình một ván thật khi có tài khoản — hiện CHƯA có.
3. **Phương án khác:** (a) bộ nhận dạng tổng quát nhiều skin (YOLO train trên nhiều GUI, vd mô hình của dự án GPLv3 VinXiangQi) không cần mẫu JJ; (b) ảnh từ video / ảnh cửa hàng ứng dụng công khai của JJ (vướng bản quyền nếu đưa vào sản phẩm); (c) người dùng tự «dạy mẫu» lần đầu (chọn 2 Xe — câu M-02).
4. **Câu hỏi:** (i) JJ象棋 có chế độ xem ván (观战) hoặc bản web nào xem được bàn KHÔNG cần đăng nhập không? (ii) Có bộ dữ liệu / model nhận dạng bàn-quân cờ tướng công khai, giấy phép rõ, tổng quát qua nhiều skin (kể cả 3D), độ chính xác công bố? (iii) Dùng ảnh chụp màn hình JJ CHỈ làm dữ liệu train nội bộ cho bộ nhận dạng (không phân phối ảnh) có rủi ro bản quyền gì?
5. **Trả lời tốt:** URL hoặc ảnh chứng minh cho (i); tên repo + giấy phép + số đo cho (ii); nguồn pháp lý cho (iii) hoặc «không chắc».

---

## Câu 330 (D3-19)

### D3-19 — Gói Demo dùng Docco (fork Fairy-Stockfish, GPLv3) + GUI đóng nguồn có khoá hết hạn: thế nào là đủ tuân thủ GPLv3?
1. **Bối cảnh** (sổ `_LANE_NOTE/lock/w_demo_cc0_0210_2026-10-02.md` §1–§2): gói `TieuLongNu_Demo` bỏ Pikafish (net official đã ném), thay bằng Docco exe (bản không có cổng key) đổi tên `docco.exe` + net CC0 `xiangqi-c07e94a5c7cb.nnue` (giữ tên gốc upstream) + `doccocaubai_config.ini` (tên bắt buộc theo mã). Kèm `SOURCE_GPL.zip` sinh TÁI LẬP từ commit nguồn (116 mục, 823.003 B, sinh 2 lần cùng byte, 0 tệp nguồn lệch), `Copying.txt`, `NET_CC0_LICENSE.txt`, README 3 ngôn ngữ. GUI TieuLongNu (đóng nguồn, tiến trình riêng, nói UCI qua stdin / stdout) có khoá dùng thử: hết hạn xoá **net CC0 + tệp dùng thử** (KHÔNG xoá exe GPL) rồi GUI tự thoát.
2. **Đội đã chọn:** như trên — coi GUI + engine là hai chương trình riêng gộp chung («aggregate»); kèm sẵn source GPL trong gói (không chỉ offer bằng văn bản).
3. **Phương án khác:** (a) offer bằng văn bản 3 năm thay vì kèm zip; (b) không đổi tên exe; (c) không cho khoá dùng thử đụng bất kỳ tệp nào trong thư mục engine.
4. **Câu hỏi:** (i) Đổi tên binary GPL + kèm zip source đúng commit có đủ «Corresponding Source» (GPLv3 §1 — gồm script build / CMake) không, có phải kèm toolchain / cờ build không? (ii) Khoá hết hạn của GUI xoá tệp net (CC0, không phải GPL) có bị coi là «hạn chế thêm» (§10) với engine GPL không? (iii) README có phải ghi rõ engine là bản sửa đổi kèm ngày sửa (§5a) không?
5. **Trả lời tốt:** trích điều GPLv3 có số mục + GPL FAQ của FSF (URL: «aggregate», «Corresponding Source»); «không chắc» nếu không có nguồn.
