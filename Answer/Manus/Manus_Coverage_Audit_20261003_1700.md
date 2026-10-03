# Manus — Audit coverage toàn bộ ASK01

**Ngày audit:** 2026-10-03 17:00  
**Repo:** `OngThan-PhatTo/DebateAI`  
**Mục tiêu:** kiểm tra phần nào đã có Markdown/DOCX và phần nào ASK01 đã bổ sung sau các lô trước.

---

## 1. Kết luận nhanh

Tài liệu hiện có **17 Markdown + 17 DOCX** trong `Answer/Manus` và đã push lên GitHub.

**Đã có báo cáo:**

- Phần A: `A1-01…A1-16`.
- Phần B: `T1…T12`.
- Phần C: `Q1…Q5`.
- Phần D: `D-01…D-07`, các ca `GT-B3`.
- Phần I trước đó: các lô `BPPC-03…DSDPC-04`, `BPCK…AGRV`.
- Phần I bổ sung: **151 dòng = 117 SAI + 34 BỊA**, thuộc 20 AI.
- Phần K–M: Meeting AI, SinhGame, AUTO TieuLongNu.
- Phần N–O: Download AI Local, port trainer NNUE Pikafish 2026.
- Phần P–Q kỹ thuật ban đầu: P-01…P-03, T34, W456, T36, T13-01…T13-03.

**Chưa có báo cáo riêng:**

1. **Phần R — D3-01…D3-20**: net/license, data.db, policy labels, datagen, deterministic A/B, px0data ODbL, UCI/UCCI, Hash, atomic replace, llama.cpp commit, TTS, Demucs, diarization, subtitle, Lightning, upload API, JJ AUTO, GPL Demo, Meeting WebView2.
2. **Q bổ sung sau 03/10:**
   - `NC-01…NC-04`: net chung/tàn cuộc, ensemble nhãn, lấy mẫu 3v3/4v4, Elo từ EGTB.
   - `FS-01…FS-02`: cỡ mẫu A/B engine tất định và nạp 174 triệu FEN vào SQLite.
   - `NC-05…NC-06`: search thí quân vào tàn cuộc và trợ lý ComfyUI offline.
   - `MT-01`: kết nối Manus vào Meeting AI.
   - `NC-07`: trợ lý đa ngôn ngữ 10 thứ tiếng.
   - `NC-08`: add dần nhãn tàn cuộc vào net hiện có không quên trung cuộc.
   - `SON-01…SON-03`: TT khi sinh nhãn, cỡ mẫu đo net tất định và giữ ổn định lưới YOLO.

## 2. Danh sách file đã tạo

| Nhóm | Markdown/DOCX |
|---|---|
| A1 | `Manus_A1_01-A1_05`, `Manus_A1_06-A1_10`, `Manus_A1_11-A1_15`, `Manus_A1_16` |
| B | `Manus_PhanB_T1-T5`, `Manus_PhanB_T6-T10`, `Manus_PhanB_T11-T12` |
| C | `Manus_PhanC_Q1-Q5` |
| D | `Manus_PhanD_D01-D05`, `Manus_PhanD_D06-D07`, `Manus_PhanD_GT-B3-01_GT-B3-03_T-B3-1` |
| I | `Manus_PhanI_BPPC03-DSDPC04`, `Manus_PhanI_BPCK-AGRV`, `Manus_PhanI_B2-B9` |
| K–M | `Manus_PhanK-M` |
| N–O | `Manus_PhanN-O` |
| P–Q | `Manus_PhanP-Q` |

Mỗi file trong bảng có cả `.md` và `.docx`.

## 3. Các placeholder E–H

ASK01 vẫn còn các heading E–H ghi “bổ sung khi lô B4…B8 xong”. Nội dung kỹ thuật tương ứng đã được đưa xuống các khối `D3`, `NC`, `FS`, `SON` ở cuối ASK01, nhưng **chưa có file trả lời riêng** cho các mã đó. Vì vậy không đánh dấu E–H là hoàn tất.

## 4. GitHub verification

Các commit trả lời đã push tuần tự:

- `e0d4062` — A1 và các lô đầu.
- `c77db7f` — Phần I BPCK–AGRV.
- `2a5c25b` — Phần K–M.
- `351e4f2` — Phần N–O.
- `aa07b27` — Phần P–Q kỹ thuật ban đầu.
- `86eae83` — Phần I bổ sung 151 mã.

Working tree audit tại thời điểm kiểm tra không có thay đổi chưa commit.

## 5. Coverage matrix

| Phần | Trạng thái | Ghi chú |
|---|---|---|
| A | Đã trả lời | A1-01…A1-16 |
| B | Đã trả lời | T1…T12 |
| C | Đã trả lời | Q1…Q5 |
| D | Đã trả lời một phần chính | D-01…D-07 và GT-B3; D3-01…D3-20 còn thiếu |
| E–H | Chưa đóng riêng | Nội dung mới nằm ở D3/NC/FS/SON |
| I | Đã trả lời | B1/B3 và 151 dòng bổ sung B2/B4–B9 |
| K–M | Đã trả lời | File PhanK-M |
| N–O | Đã trả lời | File PhanN-O |
| P | Đã trả lời | P-01…P-03 |
| Q ban đầu | Đã trả lời | T34, W456, T36, T13-01…03 |
| Q cập nhật sau | Chưa trả lời | NC-01…NC-08, FS-01…02, MT-01, SON-01…03 |
| R | Chưa trả lời | D3-01…D3-20 |

## 6. Kết luận audit

Không nên ghi “đã hoàn thiện toàn bộ ASK01” ở thời điểm này. Cách nói chính xác là:

> **Đã hoàn thành các lô A–D chính, Phần I 151 dòng bổ sung, K–P và Q kỹ thuật ban đầu. ASK01 còn các câu mới ở Phần R và các khối Q cập nhật sau 03/10; đây là phạm vi tiếp theo cần trả lời.**

Audit này được tạo để owner nhìn ngay phần đã làm và phần còn thiếu, tránh bỏ sót do ASK01 tiếp tục được bổ sung sau khi các file trước đã push.
