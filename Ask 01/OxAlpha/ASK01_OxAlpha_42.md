# ASK 01 — bộ hỏi cho OxAlpha — tệp 42/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 206 (T13-11)

### T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark — SAI 8 · BỊA 2
Hỏi lại **đúng MuseSpark**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | MuseSpark nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| A22-02 | **BỊA** | Link tải 4x-UltraSharp: huggingface.co/Kim2091/4x-UltraSharp | HF API 02/10: /api/models/Kim2091/4x-UltraSharp → 401 (repo không tồn tại; đối chứng zz-khong-co/xx-khong-co-123 cũng 401, sd-turbo 200); repo thật Kim2091/Ult… | ASK22_1909 |
| A22-03 | **BỊA** | Link HunyuanVideo-I2V: huggingface.co/Tencent-Hunyuan/HunyuanVideo-I2V | HF API 02/10 → 401; repo thật tencent/HunyuanVideo-I2V → 200; kho đội có 1 tệp tham chiếu hunyuan_video_i2v (git grep SourceCode/ComfyUI_Pro/ComfyUIPro) | ASK22_1909 |
| MS-0.1bnthua | **SAI** | 0.1 bên thua: DTM thuần + `dtc` + cờ cursed/blessed kiểu Syzygy; SF damp `v -= v*rule50/199`, TT guard `< 96`, `is_draw… | SF master fetch 27/09: evaluate.cpp **`/ 189`** (không phải 199 — hằng đổi theo phiên bản), search.cpp `rule50_count() < 96` ✓, position.cpp `rule50 > 99` ✓; p… | ASK01_2609 |
| MS-0.1ngunCPW | **SAI** | 0.1 nguồn CPW: chessprogramming Felicity: «EGTB giúp < 13 Elo 6-men Syzygy» | trang tồn tại nhưng **không có số Elo nào** (fetch 27/09); số 13 Elo từ nơi khác, không ghi | ASK01_2609 |
| MS-0.1ngunFelicity | **SAI** | 0.1 nguồn Felicity: trích README «provides some popular metrics such as depth to mate and depth to convert» + «code and… | README GitHub (fetch 27/09) **và** bản đội `BookTool_Xiangqi/_docs_and_submodules_2026-07-21/FelicityEgtb/README.md` đều **không có** câu metrics; câu MIT `:85… | ASK01_2609 |
| MS-0.1quym | **SAI** | 0.1 quy mô: «retrograde 9.900/s, 700 B/thế hợp lý cho bậc 2–3: vài chục triệu thế = vài chục giờ + vài chục GB» | 5·10⁷ thế / 9.900 = **1,4 giờ**, 35 GB ✓; nhưng retrograde phải **vét cả ô**: 2v2 không sĩ/tượng 1,35·10¹¹ (Ask A3-04) ⇒ ~158 ngày. Cái đắt là vét ô, không phả… | ASK01_2609 |
| MS-MA-HOMO | **SAI** | Mã kèm zip: `grid_homography.py`, `OneBtn_CSharp5.cs` | A1 §4: `cv.` chưa import ⇒ NameError, `reproj_ok` luôn True; mọi stub C# trả rỗng — khung tự ghi TODO, không chạy được | ASK02_A2_2309 |
| ES-03 | **SAI** | Docco fairy_src position.cpp:2997/3087/3052 là chỗ trừ drawScore | git show c88a430b (bản 17/09) SourceCode/OngThan_variants/Docco/fairy_src/src/position.cpp: :2995-2999 `gStaticChase`, :3050-3054 + :3085-3089 thân `chased()`;… | ENGINE_STYLE_1709 |
| ES-04 | **SAI** | Bodetosu gom eval_mat_floor ở config.cpp:375 | git show 86bed1c8^:SourceCode/OngThan_variants/Bodetosu/src/config.cpp: eval_mat_floor ở :414-415 và :590; :370-380 là khối khác (sau KM-1 thành khối killer_*) | ENGINE_STYLE_1709 |
| B8-MSA-02 | **SAI** | «Nút duy nhất nhận mọi nguồn» (ảnh/SVG/FEN) | OneBtn_CSharp5.cs:6-11: Img.ChupVungVaNhan/NhanTuAnh trả "" conf 0; Vec.ParseSvg trả "" với conf = 1.0 ⇒ khung giả, nhánh SVG tự tin 100 % với kết quả rỗng (PA… | ARCHIVE_2709 |

---

## Câu 207 (A22-02)

**Mục A22-02 — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (BỊA):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «Link tải 4x-UltraSharp: huggingface.co/Kim2091/4x-UltraSharp». Bằng chứng của đội: HF API 02/10: /api/models/Kim2091/4x-UltraSharp → 401 (repo không tồn tại; đối chứng zz-khong-co/xx-khong-co-123 cũng 401, sd-turbo 200); repo thật Kim2091/Ult…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 208 (A22-03)

**Mục A22-03 — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (BỊA):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «Link HunyuanVideo-I2V: huggingface.co/Tencent-Hunyuan/HunyuanVideo-I2V». Bằng chứng của đội: HF API 02/10 → 401; repo thật tencent/HunyuanVideo-I2V → 200; kho đội có 1 tệp tham chiếu hunyuan_video_i2v (git grep SourceCode/ComfyUI_Pro/ComfyUIPro). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 209 (MS-0.1bnthua)

**Mục MS-0.1bnthua — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «0.1 bên thua: DTM thuần + `dtc` + cờ cursed/blessed kiểu Syzygy; SF damp `v -= v*rule50/199`, TT guard `< 96`, `is_draw…». Bằng chứng của đội: SF master fetch 27/09: evaluate.cpp **`/ 189`** (không phải 199 — hằng đổi theo phiên bản), search.cpp `rule50_count() < 96` ✓, position.cpp `rule50 > 99` ✓; p…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 210 (MS-0.1ngunCPW)

**Mục MS-0.1ngunCPW — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «0.1 nguồn CPW: chessprogramming Felicity: «EGTB giúp < 13 Elo 6-men Syzygy»». Bằng chứng của đội: trang tồn tại nhưng **không có số Elo nào** (fetch 27/09); số 13 Elo từ nơi khác, không ghi. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
