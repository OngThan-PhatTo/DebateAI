# ASK 01 — bộ hỏi cho OxAlpha — tệp 60/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 296 (G47A2-D7)

**Mục G47A2-D7 — T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 (SAI):** AI T13-21 — Phần I bổ sung (lô B2, B4–B9): Grok47 nói: «D-7: Legal = cộng hai bên, isLegal chỉ lọc tướng đối mặt; 4.806/3.294 «vẫn không tái lập»». Bằng chứng của đội: A01-9 `de4e28df` 27/09 12:1x (TRƯỚC bài 28/09): quy ước «felicity» = không gian chỉ số ⇒ krk 4.806 · kpk 3.294 đúng tuyệt đối (`Shared/DataFen/tan_cuoc_liet_ke…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 297 (T13-22)

### T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus — SAI 3 · BỊA 0
Hỏi lại **đúng Manus**: mỗi dòng trả «NHẬN» (sai thật) hoặc đưa nguồn thật (đường dẫn+dòng / URL). Không bịa thêm.

| Mã | Loại | Manus nói | Bằng chứng của đội | Vòng |
|---|---|---|---|---|
| MN-0.3 | **SAI** | 0.3: Rebuild theo chunk + audit; REAL chỉ nhận khi `isfinite ∧ floor==x ∧ | ≤2^53`; quarantine; `input = output + dup + cấm + quarantine` / CHẮC (audit) · **SAI ngữ cảnh** (2^53 — xem B.2) · KHÔNG BIẾT miền mã hoá (trung thực) / Số :9… | ASK01_2609 |
| MN-A3-05 | **SAI** | A3-05: Không bỏ EGTB khi dtc>remain → kết quả theo luật + provenance `tb_result/rule_result`; TT key phải gồm bộ đếm | đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl… | ASK01_2609 |
| B8-MA-04 | **SAI** | build_and_test_xiangqi.sh in «ALL BUILD/TEST STEPS PASSED» | xiangqi_search_upgrades.exe (rc 0) in «nnue_check=skipped (pass XQNN1 path)»; tiny_xq_nnue_engine.exe được build nhưng script không chạy (cần NET, :568-573) ⇒… | ASK05_NOCUDA_0110 |

---

## Câu 298 (MN-0.3)

**Mục MN-0.3 — T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus (SAI):** AI T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus nói: «0.3: Rebuild theo chunk + audit; REAL chỉ nhận khi `isfinite ∧ floor==x ∧». Bằng chứng của đội: ≤2^53`; quarantine; `input = output + dup + cấm + quarantine` / CHẮC (audit) · **SAI ngữ cảnh** (2^53 — xem B.2) · KHÔNG BIẾT miền mã hoá (trung thực) / Số :9…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 299 (MN-A3-05)

**Mục MN-A3-05 — T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus (SAI):** AI T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus nói: «A3-05: Không bỏ EGTB khi dtc>remain → kết quả theo luật + provenance `tb_result/rule_result`; TT key phải gồm bộ đếm». Bằng chứng của đội: đo 27/09 23:0x (A01-7 ĐÓNG — KHÔNG ỦNG HỘ, `_LANE_NOTE/lock/w_phieu_thu_t1_t3_2709_2026-09-27.md`): S1 20 thế `dtc > remain` ⇒ 13/20 vẫn THẮNG trong luật 80 pl…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».

---

## Câu 300 (B8-MA-04)

**Mục B8-MA-04 — T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus (SAI):** AI T13-22 — Phần I bổ sung (lô B2, B4–B9): Manus nói: «build_and_test_xiangqi.sh in «ALL BUILD/TEST STEPS PASSED»». Bằng chứng của đội: xiangqi_search_upgrades.exe (rc 0) in «nnue_check=skipped (pass XQNN1 path)»; tiny_xq_nnue_engine.exe được build nhưng script không chạy (cần NET, :568-573) ⇒…. **Câu hỏi:** đây có phải bịa/sai không? Nếu không, dẫn nguồn thật (đường dẫn + dòng hoặc URL) hoặc ghi «NHẬN».
