# ASK 01 — bộ hỏi cho OxAlpha — tệp 6/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 26 (T10)

**Ca thử T10 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T10 | Bộ mã Manus tiny_xq_nnue biên dịch + test đạt; mapping feature ≠ Nữ Oa | g++ trong LaneScratch, `build_and_test_xiangqi.sh`; so `featureIndex` với `nnue_accum.h:14-17` | perft 44/1920 | khớp mapping ⇒ có thể làm CPU trainer C++ tham khảo | 15 phút |

---

## Câu 27 (T11)

**Ca thử T11 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T11 | SPSA margin RFP/razor/LMR +10…+40 Elo (Grok) | lộ margin thành UCI option Nữ Oa → SPSA 20k ván 2k nodes → SPRT | A=A + dương | SPRT pass [0, +10] | nhiều ngày |

---

## Câu 28 (T12)

**Ca thử T12 (Phần B — góp ý cách đo/ngưỡng/đối chứng):**

| Mã | Giả thuyết | Chạy gì | Đối chứng | Ngưỡng (đặt TRƯỚC) | Chi phí |
|---|---|---|---|---|---|
| T12 | DirectML venv riêng (torch 2.4.1) chạy `--device dml` 1 batch; tốc độ vs ROCm | venv nháp | — | ra loss hữu hạn; nếu < ROCm ⇒ bỏ hẳn DML | 30 phút |

---

## Câu 29 (D-01)

### D-01 — Bao nhiêu thế có nhãn (độ sâu/nodes bao nhiêu) thì một NNUE HalfKAv2 10.530→512 tới gần Pikafish?
**Các AI nói:** 0,5 tháng (gravity) · 3–10 ngày (SpaceBunny, train trên px0data) · 5–8/5–9 tháng (GLM 5.2 = Grok47 = MuseSpark, cùng khung) · 6–18 tháng có thầy, 24–60+ tháng tự học (Codex) · 12–18 tháng (TAI) · 18–48 tháng (unknown docx). Không bài nào có số đo.
**Bằng chứng đội:** 552 thế/s/32 luồng; tuyến tính 6× ⇒ ~3.300 thế/s ≈ 285 triệu thế/ngày trên 192 lõi (cận trên «~2 tỷ thế/ngày» của GLM/Grok47/MuseSpark là ~42× số đo — đội chấm SAI).
**Hỏi:** (1) Có nguồn công khai nào cho **số thế + độ sâu nhãn** mà Pikafish/Stockfish đã dùng cho một net (không phải số ván Px0)? (2) Công thức nối «số thế × chất lượng nhãn» → Elo mà bạn tin, kèm cách kiểm bằng một lô nhỏ (vd 50M vs 200M thế cùng độ sâu). (3) Nếu chỉ được tự sinh (NoBook, thầy CC0), độ sâu/nodes nhãn nào là điểm cân bằng tốc độ–chất lượng trên 153 luồng?

---

## Câu 30 (D-02)

### D-02 — Tiêu chuẩn «ngang Pikafish»: biên tương đương và số ván
**Codex:** KTC95 chênh Elo nằm trọn trong ±20, thống kê pentanomial theo **cặp** đổi màu, quy tắc dừng định trước; SPRT «không phát hiện thua» chưa chứng minh tương đương.
**Hỏi:** Với tỉ lệ hoà engine–engine cờ tướng cao (bạn ước bao nhiêu?), cần bao nhiêu **cặp ván** để KTC95 rộng ≤ ±20? SPRT với cận Elo nào là đủ cho mục đích «không kém hơn 20 Elo»? Dẫn công thức (vd từ fishtest) nếu chắc.
