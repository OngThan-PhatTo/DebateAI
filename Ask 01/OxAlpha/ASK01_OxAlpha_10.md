# ASK 01 — bộ hỏi cho OxAlpha — tệp 10/63 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 46 (DSPC-01)

**Mục DSPC-01 — DeepSeek (SAI):** AI DeepSeek nói: «official-pikafish/pxzero-training là repo chính thức huấn luyện mạng (NNUE) Pikafish». Bằng chứng của đội: pxzero-training là trainer TensorFlow policy/value cho Px0 (Codex dẫn tfprocess.py; SpaceBunny README «tensorflow»); trainer NNUE của Pikafish không công khai (OngThan/tools/ongthan/pikafish-nnue-pytorch/PROVENANCE.md:12-22). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 47 (DSDPC-01)

**Mục DSDPC-01 — DeepSeek (SAI):** AI DeepSeek nói: «Sinh dữ liệu: `./pikafish bench 16 1 13 default depth > positions.txt` rồi convert sang binpack». Bằng chứng của đội: SourceCode/6_Engine_SOURCE_D_20260806/Xiangqi/OngThan/pikafish-src/uci.cpp:141 + :225-288 `bench` chỉ chạy search trên thế mẫu (benchmark.cpp:32 Defaults) và in Nodes searched/Nodes/second; pikafish-src không có gensfen/generate_training_data (grep = 0). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 48 (DSDPC-02)

**Mục DSDPC-02 — DeepSeek (SAI):** AI DeepSeek nói: «Benchmark: 1×3090 ~85 it/s ⇒ 100 epoch ~30 ngày». Bằng chứng của đội: tự mâu thuẫn: 85 it/s × 16384 = 1,39 triệu mẫu/s; 100 epoch × 1e8 = 1e10 mẫu ⇒ ~2 giờ (epoch 20M của ta ⇒ ~24 phút). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 49 (DSDPC-03)

**Mục DSDPC-03 — DeepSeek (SAI):** AI DeepSeek nói: «evaluate.cpp Pikafish: `if (pos.checkers()) return VALUE_ZERO;` + hằng 485/11683/17720/3040/20120/267». Bằng chứng của đội: SourceCode/6_Engine_SOURCE_D_20260806/Xiangqi/OngThan/pikafish-src/evaluate.cpp:45 `assert(!pos.checkers())` (không trả 0); :53-60 hằng thật 465/11743/17380/3061/20582/253. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 50 (DSDPC-04)

**Mục DSDPC-04 — DeepSeek (SAI):** AI DeepSeek nói: «HalfKAv2_hm cờ tướng: NUM_PLANES 8, KING_BUCKETS 6, ATTACK_BUCKETS 6, LAYER_STACKS 16». Bằng chứng của đội: SourceCode/6_Engine_SOURCE_D_20260806/Xiangqi/OngThan/pikafish-src/nnue/features/half_ka_v2_hm.h:48-54 PS_NB 689, AttackBucketNB 4, Dimensions 6×4×689 = 16536; LayerStacks 16 đúng (nnue/nnue_architecture.h:49). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
