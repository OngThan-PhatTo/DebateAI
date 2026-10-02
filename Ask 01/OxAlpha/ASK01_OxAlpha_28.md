# ASK 01 — bộ hỏi cho OxAlpha — tệp 28/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 136 (T13-02)

### T13-02 — Đẩy 200–300 GB lên Google Drive: robocopy vào ổ G: (Drive for desktop) hay rclone thẳng?
**Bối cảnh:** đội sao lưu nguồn bằng robocopy `/MT:32` vào `G:\My Drive\…` (Google Drive for desktop, luật owner 20/09: chỉ Google Drive + ổ D/E, cấm OneDrive). Đo 25/09: mọi byte nằm tạm trong cache DriveFS ở `%LOCALAPPDATA%\Google\DriveFS` trên ổ C tới khi tải lên xong — cache 47,0 → 81,4 GB trong 30 phút, ổ C trống 91,7 → 30,3 GB. Nghiệm thu hiện chỉ so mtime/metadata trên G:, không đọc nội dung tệp trên G (để khỏi kéo về). ChatGPT (Ask21 KK10) đề xuất `rclone copy --checksum` 3–4 transfers + `rclone check` + client ID riêng; rclone.org/drive (đọc 02/10) xác nhận client ID dùng chung «being retired and will stop working during 2026».
**Hỏi:** (1) Drive for desktop có cho đặt giới hạn/đổi chỗ cache, hoặc cho lấy MD5 phía Google của tệp trên G: mà không tải về không? (2) Với 200–300 GB và yêu cầu «chứng minh đã lên đủ bằng hash», rclone (tải thẳng API, so md5 phía server) có ưu thế đo được nào so với DriveFS? Nêu hạn chế (750 GB/ngày, tệp nhỏ hàng triệu tệp, tên tệp ký tự lạ).
**Trả lời tốt:** dẫn tài liệu Google/rclone có URL; nói rõ cái gì đã kiểm, cái gì suy luận.

---

## Câu 137 (T13-03)

### T13-03 — Có nguồn công khai nào xác nhận tập ảnh booru (Danbooru…) dùng train Pony / Illustrious / NoobAI chứa ảnh vị thành niên bị cấm không?
**Bối cảnh:** SpaceBunny (bài 26/09 về model 18+ ComfyUI) viết «có tài liệu công khai ghi nhận tập dữ liệu Booru năm 2023 có chứa ảnh vi phạm» nhưng **không dẫn nguồn**. Báo cáo đội biết là của Stanford Internet Observatory 12/2023 về **LAION-5B** — tập khác. Kho model 18+ của đội có nhiều mục họ Pony/Illustrious/NoobAI (`SourceCode/ComfyUI_Pro/ComfyUIPro/Assets/kho/model_18_*.json`, 9 tệp); đội đã chặn cứng `child, teen, minor, underage` trên mọi sơ đồ 18+ (cổng có 3 ca đỏ, commit `bb44cc83`) và quét 02/10: 0 mục kho chứa từ khoá tuổi ngoài phần Negative.
**Hỏi:** Có báo cáo/URL công khai nào nói cụ thể về Danbooru (2021–2023) hoặc tập train của Pony Diffusion V6 / Illustrious-XL / NoobAI-XL không? Model card các họ đó có tuyên bố đã lọc nội dung vị thành niên không? Nếu không tìm được nguồn, hãy nói rõ «không có nguồn».
**Trả lời tốt:** chỉ URL thật (báo cáo, model card, thông báo nhà phát hành) + trích ≤ 1 câu; **không** suy đoán, **không** mô tả nội dung.

<!-- T13-PHAN-I-BAT-DAU -->

**Phần I bổ sung — SAI/BỊA theo từng AI ở các lô soi mới (B2, B4, B5, B6, B7a, B7b, B8, B9): 20 AI · SAI 117 · BỊA 34.** Phần I hiện có trong Ask 01 mới phủ lô B1 + B3. Nguồn từng dòng: `AI_Debate/Discussion/_SOI/<lô>.tsv` (cột evidence). Đề nghị Fable đặt các khối này ngay dưới «PHẦN I (bổ sung lô B3)» khi gộp (hoặc để nguyên trong Phần Q).

---

## Câu 138 (T13-04)

### T13-04 — Phần I bổ sung (lô B2, B4–B9): Ling — SAI 0 · BỊA 12 (phần 1/3)
Hỏi lại **đúng Ling**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Ling nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| LG-0.6 | **BỊA** | 0.6: TLN thiếu: hồ sơ nối JSON, auto-reconnect, One mode, xếp hạng, phát hiện đổi skin; BHGui/SharkChess mô tả | `bhgui.org` **DNS không tồn tại**; `sharkchess.com` → `www.sharkchess.com` **200 thật** (GUI 鲨鱼象棋, có 连线 + auto-click). Câu «BHGui kết nối Yixuan/QQ/JJ» không… | ASK01_2609 |
| LG-0.8 | **BỊA** | 0.8: thứ tự search→luật→đồng hồ→băm→eval; cổng ≥ 400; «thang Pikafish 5 bậc = pikafish-weight-1…5»; dừng sớm sau 30 ván | «pikafish-weight-1…5» **không tồn tại** — thang 5 bậc là thang nội bộ đội (SB ghi KHÔNG BIẾT — đúng); Ling bịa tên. `go perft` không kiểm «search» mà kiểm move… | ASK01_2609 |
| LG-A2-05 | **BỊA** | A2-05: bắt buộc: Bắt đầu/Dừng/Đổi bên/Chọn engine/Sách; «SharkChess ~6 nút, PengFei ~5 nút» | không nguồn cho 6/5 nút; `pengfei.qipu88.com` **DNS không tồn tại** | ASK01_2609 |
| LG-A2-23 | **BỊA** | A2-23: αβ+NNUE vẫn thực tế; **AlphaZero 30 M ván** | AlphaZero cờ vua = 44 M ván; `github.com/mhaynes/stockfish` **404** | ASK01_2609 |
| LG-A2-34 | **BỊA** | A2-34: «chưa có» ai hỏi Pikafish; NYT v. OpenAI chưa phán quyết | `arxiv.org/abs/2407.10544` = bài **trạm sạc xe điện (port-Hamiltonian)**, không phải «distillation legal review»; `github.com/openai/openai/issues` **404** | ASK01_2609 |
| LG-A2-35 | **BỊA** | A2-35: bảng BHGui/SharkChess/PengFei; PengFei «Có» tự hồi phục | hàng PengFei điền đoán với URL chết; `sharkchess.com/features/` chưa kiểm (gốc thật). Vi phạm «ô nào không biết ghi không biết» | ASK01_2609 |
| LG-A2-01 | **BỊA** | A2-01: bài Ling — Lichess bịa | ô «◐ (Lichess bịa)» | ASK02_A2_2309 |
| LG-A2-06 | **BỊA** | A2-06: bài Ling — repo/cờ bịa | ô «✗ (repo/cờ bịa)» | ASK02_A2_2309 |
| LG-A2-07 | **BỊA** | A2-07: bài Ling — 3 repo bịa | ô «✗ (3 repo bịa)» | ASK02_A2_2309 |
| LG-A2-12 | **BỊA** | A2-12: bài Ling — «16 GB lock» bịa | ô «◐ («16 GB lock» bịa)» | ASK02_A2_2309 |
| LG-A2-14 | **BỊA** | A2-14: bài Ling — giọng TTS bịa | ô «◐ (giọng TTS bịa)» | ASK02_A2_2309 |
| LG-A2-16 | **BỊA** | A2-16: bài Ling — URL bịa | ô «◐ (URL bịa)» | ASK02_A2_2309 |

---

## Câu 139 (LG-0.6)

**Mục LG-0.6 — T13-04 — Phần I bổ sung (lô B2, B4–B9): Ling (BỊA):** AI T13-04 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «0.6: TLN thiếu: hồ sơ nối JSON, auto-reconnect, One mode, xếp hạng, phát hiện đổi skin; BHGui/SharkChess mô tả». Bằng chứng của đội: `bhgui.org` **DNS không tồn tại**; `sharkchess.com` → `www.sharkchess.com` **200 thật** (GUI 鲨鱼象棋, có 连线 + auto-click). Câu «BHGui kết nối Yixuan/QQ/JJ» không…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 140 (LG-0.8)

**Mục LG-0.8 — T13-04 — Phần I bổ sung (lô B2, B4–B9): Ling (BỊA):** AI T13-04 — Phần I bổ sung (lô B2, B4–B9): Ling nói: «0.8: thứ tự search→luật→đồng hồ→băm→eval; cổng ≥ 400; «thang Pikafish 5 bậc = pikafish-weight-1…5»; dừng sớm sau 30 ván». Bằng chứng của đội: «pikafish-weight-1…5» **không tồn tại** — thang 5 bậc là thang nội bộ đội (SB ghi KHÔNG BIẾT — đúng); Ling bịa tên. `go perft` không kiểm «search» mà kiểm move…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
