# ASK 01 — bộ hỏi cho OxAlpha — tệp 11/27 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 51 (TAIPC-01)

**Mục TAIPC-01 — TAI (SAI):** AI TAI nói: «Ngang Pikafish cần net NNUE 768→768→1 (hoặc 512→512→1), ~100M+ thế». Bằng chứng của đội: SourceCode/6_Engine_SOURCE_D_20260806/Xiangqi/OngThan/pikafish-src/nnue/nnue_architecture.h:44-49 L1 1024, L2 31, L3 32, LayerStacks 16; input HalfKAv2_hm 16536 + threats 45547 (OngThan/tools/ongthan/pikafish_nnue/featgen.cpp:40-41); 768 là input cờ vua. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 52 (TAIPC-02)

**Mục TAIPC-02 — TAI (SAI):** AI TAI nói: «Băng thông 5080 ~768 GB/s». Bằng chứng của đội: 3 bài cùng lô dẫn spec NVIDIA/PNY 960 GB/s; 256-bit × 30 Gbps / 8 = 960. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 53 (TAIPC-03)

**Mục TAIPC-03 — TAI (BỊA):** AI TAI nói: «Stockfish có `src/sprt.h`; self-play mode trong `src/search.cpp`/`timeman.h`». Bằng chứng của đội: Glob sprt.h trong SourceCode + SourceThamKhao = 0; grep self_play\|selfplay trong *.cpp/*.h của SourceCode/6_Engine_SOURCE_D_20260806/Xiangqi (Pikafish + Fairy) chỉ ra 1 chú thích (pikafish-src/benchmark.cpp:92), không có chế độ self-play; SPRT nằm ở fishtest (máy chủ). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 54 (TAIPC-04)

**Mục TAIPC-04 — TAI (SAI):** AI TAI nói: «Tách 2 card train 2 model khác nhau thì «cần synchronize gradients»». Bằng chứng của đội: hai model độc lập không đồng bộ gradient; đồng bộ gradient là DDP (một model) — Codex §5 cùng lô; PyTorch DDP API. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 55 (TAIPC-06)

**Mục TAIPC-06 — TAI (SAI):** AI TAI nói: «Dual GPU bằng DataParallel / init_process_group(backend='nccl')». Bằng chứng của đội: trên Windows: .venv đo 02/10 `torch.distributed.is_available()` = False; NCCL không có trên PyTorch Windows (CDXPC-08). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
