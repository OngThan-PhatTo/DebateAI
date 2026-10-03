# ASK 01 — bộ hỏi cho OxAlpha — tệp 43/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 211 (MS-0.1ngunFelicity)

**Mục MS-0.1ngunFelicity — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «0.1 nguồn Felicity: trích README «provides some popular metrics such as depth to mate and depth to convert» + «code and…». Bằng chứng của đội: README GitHub (fetch 27/09) **và** bản đội `BookTool_Xiangqi/_docs_and_submodules_2026-07-21/FelicityEgtb/README.md` đều **không có** câu metrics; câu MIT `:85…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 212 (MS-0.1quym)

**Mục MS-0.1quym — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «0.1 quy mô: «retrograde 9.900/s, 700 B/thế hợp lý cho bậc 2–3: vài chục triệu thế = vài chục giờ + vài chục GB»». Bằng chứng của đội: 5·10⁷ thế / 9.900 = **1,4 giờ**, 35 GB ✓; nhưng retrograde phải **vét cả ô**: 2v2 không sĩ/tượng 1,35·10¹¹ (Ask A3-04) ⇒ ~158 ngày. Cái đắt là vét ô, không phả…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 213 (MS-MA-HOMO)

**Mục MS-MA-HOMO — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «Mã kèm zip: `grid_homography.py`, `OneBtn_CSharp5.cs`». Bằng chứng của đội: A1 §4: `cv.` chưa import ⇒ NameError, `reproj_ok` luôn True; mọi stub C# trả rỗng — khung tự ghi TODO, không chạy được. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 214 (ES-03)

**Mục ES-03 — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «Docco fairy_src position.cpp:2997/3087/3052 là chỗ trừ drawScore». Bằng chứng của đội: git show c88a430b (bản 17/09) SourceCode/OngThan_variants/Docco/fairy_src/src/position.cpp: :2995-2999 `gStaticChase`, :3050-3054 + :3085-3089 thân `chased()`;…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 215 (ES-04)

**Mục ES-04 — T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark (SAI):** AI T13-11 — Phần I bổ sung (lô B2, B4–B9): MuseSpark nói: «Bodetosu gom eval_mat_floor ở config.cpp:375». Bằng chứng của đội: git show 86bed1c8^:SourceCode/OngThan_variants/Bodetosu/src/config.cpp: eval_mat_floor ở :414-415 và :590; :370-380 là khối khác (sau KM-1 thành khối killer_*). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
