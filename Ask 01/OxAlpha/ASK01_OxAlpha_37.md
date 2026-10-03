# ASK 01 — bộ hỏi cho OxAlpha — tệp 37/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 181 (NM-MA-C5)

**Mục NM-MA-C5 — T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron (SAI):** AI T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «Mã kèm tự xưng C# 5». Bằng chứng của đội: A1 §4: dùng `$""`, `?.`, `out var`, tuple, `=>`, `catch…when` — không biên dịch được bằng csc C# 5 của đội. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 182 (NM-MA-PASSGIA)

**Mục NM-MA-PASSGIA — T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron (SAI):** AI T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «Mã kèm: `integration_test.py:36-80` + `build.proj:55-57` cổng kiểm». Bằng chứng của đội: A1 §4: accuracy = conf > 0,9 không nhãn (PASS giả); Verify grep `obj\**` sau csc ⇒ luôn xanh — cổng chưa từng đỏ. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 183 (NM-MA-UPDATER)

**Mục NM-MA-UPDATER — T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron (SAI):** AI T13-08 — Phần I bổ sung (lô B2, B4–B9): Nemotron nói: «Mã kèm: `Src/App.xaml.cs:69-96` AutoUpdater tải bản phát hành GitHub rồi chạy quyền admin lúc khởi động». Bằng chứng của đội: A1 §4: placeholder `yourorg/xiangqi-autoplay`, không hash/chữ ký, `catch {}` nuốt — trái luật không đưa mã/tải mạng; KHÔNG chạy. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 184 (T13-09)

### T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity — SAI 6 · BỊA 3
Hỏi lại **đúng Supergravity**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Supergravity nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| SG-A309 | **BỊA** | A3-09: `tan_cuoc_liet_ke.py:182-209` tính `legal = bi_chieu and not (rule60 >= noCaptureDrawPly)`; Felicity bỏ thế rule… | 02/10 sed 182-209: không có biểu thức `legal = bi_chieu …` nào; nguyên nhân thật = không gian chỉ số (A01-9, krk 4.806 tái lập tuyệt đối) | ASK01_2609 |
| SG-A312 | **BỊA** | A3-12: diff mẫu sửa `EngineState.cs` (enum AutoMode + singleton) | 02/10 `ls`: `TieuLongNu.App/EngineState.cs` không tồn tại; diff có dòng «−» như sửa tệp có sẵn ⇒ dựng tệp ảo; trạng thái thật nằm ở `TuChoi.cs` (ChiXem/DangCha… | ASK01_2609 |
| SG-K3-CS | **BỊA** | K3: đoạn C# «trích từ `Shared/DataFen/tan_cuoc_liet_ke.cs`» | 02/10 `ls`: `SourceCode/Shared/DataFen/tan_cuoc_liet_ke.cs` KHÔNG tồn tại (chỉ có `.py`); đoạn mã băm TÊN tệp qua cp1252 không có ở đâu trong kho | ASK01_2609 |
| SG-A301 | **SAI** | A3-01: định nghĩa lại MATE_SENTINEL 30000; `mate_ply = 30000 − /cp/; target = sign·(30000 − mate_ply)` | đoạn mã là phép đồng nhất (target = cp) ⇒ không đổi gì; trainer thật `train_nnue_gpu_v3.py:397` MATE_SENTINEL = 20000, `:737` loại | ASK01_2609 |
| SG-A302 | **SAI** | A3-02: thay `return e.pos.pieceAt(to) != Piece::None;` bằng `return e.pos.isLegalMove(move);` để giữ mọi nước | lambda `do_filter` trả TRUE = BỎ mẫu (`training_data_loader.cpp:621-624`) ⇒ trả isLegalMove sẽ bỏ gần HẾT mẫu; chỗ cắt thật ở bộ sinh `training_data_generator.… | ASK01_2609 |
| SG-K3 | **SAI** | K3: băm SHA-256 TÊN tệp (UTF-8) để chặn sách cấm; sửa mojibake CJK bằng cp1252→UTF-8 | đội chặn theo SHA256 NỘI DUNG tệp (`Shared/DataFen/nap_cbl_staging.py:15-28`) — băm tên sai đúng chỗ K3 hỏng (hai tên khác cùng nội dung); mojibake GBK/936 khô… | ASK01_2609 |
| SG-K5 | **SAI** | K5: loss log1p chèn vào `train_nnue_gpu_v3.py` | 02/10 grep `log1p` trong `train_nnue_gpu_v3.py` = 0 — đoạn được ghi như trích tệp thật nhưng không có; log-scale cp làm phẳng độ dốc DTM (ngược mục tiêu A3-01) | ASK01_2609 |
| SG-K6 | **SAI** | K6: đo chập chờn bằng 60 mẫu `\Processor(_Total)` mỗi 500 ms + `Get-HighResolutionTimer` | CLAUDE.md 21/09 04:1x §5: mẫu `Get-Counter` đơn lẻ là rác (44,8/31,0/97,4 %), phải cộng CPU-time theo PID cửa sổ ≥ 20 s; `Get-HighResolutionTimer` không phải c… | ASK01_2609 |
| SG-K7 | **SAI** | K7: mỗi AI `git lfs lock` + `git push` nhật ký 30 s/lần; pull --rebase khi lỗi | trái luật cứng: cấm push/remote ra ngoài (memory khong-day-code-len-ngoai) + mỗi lane một tệp `_LANE_NOTE/lock` + commit `-- <path>` (CLAUDE.md §4.0, §4.7) | ASK01_2609 |

---

## Câu 185 (SG-A309)

**Mục SG-A309 — T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity (BỊA):** AI T13-09 — Phần I bổ sung (lô B2, B4–B9): Supergravity nói: «A3-09: `tan_cuoc_liet_ke.py:182-209` tính `legal = bi_chieu and not (rule60 >= noCaptureDrawPly)`; Felicity bỏ thế rule…». Bằng chứng của đội: 02/10 sed 182-209: không có biểu thức `legal = bi_chieu …` nào; nguyên nhân thật = không gian chỉ số (A01-9, krk 4.806 tái lập tuyệt đối). **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
