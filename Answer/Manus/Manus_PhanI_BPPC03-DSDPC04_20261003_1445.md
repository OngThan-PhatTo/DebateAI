# Manus — Phần I, lô 1: phản biện BPPC-03 đến DSDPC-04

**Ngày:** 2026-10-03 14:45 (+07:00)  
**Phạm vi:** các khẳng định được ASK01 đánh dấu SAI/BỊA, từ `BPPC-03` đến `DSDPC-04`.  
**Quy tắc:** Với khẳng định sai, câu trả lời là **NHẬN** và sửa bằng phép tính/nguồn thật; không bịa thêm nguồn. Với khẳng định đúng một phần, tách phần đúng và phần sai.

---

## BPPC-03 — Dung lượng binpack và suy luận RAM

**Kết luận: NHẬN — phép tính trong khẳng định gốc tự mâu thuẫn.**

Nếu throughput là `319 MB/s` tại `20,9 triệu vị trí/s`, dung lượng trung bình suy ra là:

```text
319e6 / 20,9e6 ≈ 15,3 byte/vị trí
```

Nếu mỗi vị trí thật sự là `8.350 byte`, throughput tương ứng phải là:

```text
8.350 × 20,9e6 ≈ 174,5 GB/s
```

không phải 319 MB/s. Riêng phép nhân `8.350 × 10^10 ≈ 83,5 TB` là đúng về bậc số, nhưng không thể ghép với throughput 319 MB/s như một cùng cấu hình. `256 GB / 8.350 ≈ 30,7 triệu vị trí` cũng chỉ đúng nếu 8.350 byte là kích thước resident thật, chưa tính index/allocator/overhead.

**Cách sửa:** phải đo `file_size / decoded_record_count`, tách file bytes, decoded struct bytes và peak RSS. Không dùng một bảng có các đơn vị/throughput không cùng điều kiện để kết luận RAM.

**Mức tin:** CHẮC về mâu thuẫn số học; số byte/record thật cần đo từ file/schema.

---

## GLMPC-10 — 192 lõi sinh vài trăm triệu đến 2 tỷ thế/ngày

**Kết luận: NHẬN — cận trên 2 tỷ/ngày không được hỗ trợ bởi số đo đội.**

Từ số đo `552,5 thế/s` trên 32 luồng:

```text
2.000.000.000 / 86.400 ≈ 23.148 thế/s
23.148 / 552,5 ≈ 41,9×
```

Nếu scale tuyến tính lý tưởng từ 32 lên tối đa 192 lõi, hệ số chỉ là 6×:

```text
552,5 × 6 ≈ 3.315 thế/s
3.315 × 86.400 ≈ 286 triệu thế/ngày
```

286 triệu/ngày là **cận lạc quan theo scale tuyến tính**, không phải benchmark máy 192 lõi; thực tế có thể thấp hơn vì memory, NUMA, I/O và overhead. Con số 2 tỷ/ngày cần benchmark thật hoặc mô hình có dữ liệu chứng minh, không thể suy ra từ “192 lõi”.

**Mức tin:** CHẮC về phép quy đổi và việc 2 tỷ là unsupported estimate; throughput 192 lõi vẫn cần đo.

---

## GRKPC-07 — 192 lõi sinh khoảng 2 tỷ thế/ngày

**Kết luận: NHẬN — cùng lỗi với GLMPC-10.**

Khẳng định này trùng khung suy luận nên không tính như một bằng chứng độc lập. Cùng mốc 552,5 thế/s/32 luồng chỉ cho cận tuyến tính khoảng 286 triệu/ngày ở 192 lõi; không đủ để tuyên bố 2 tỷ/ngày.

**Mức tin:** CHẮC về việc khẳng định 2 tỷ chưa có căn cứ; không có số đo mới.

---

## MSPC-09 — 2 tỷ thế/ngày và 5–8 tháng

**Kết luận: NHẬN — cả throughput lẫn thời gian train đều chưa được chứng minh.**

Phần throughput lặp lại sai số 42× nêu trên. Phần “5–8 tháng” còn phụ thuộc số mẫu mục tiêu, độ sâu nhãn, số pass, reject/duplicate rate, trainer throughput, validation và A/B; không thể suy ra chỉ từ số lõi hoặc cùng một khung giả định với GLM/Grok.

Cách lập kế hoạch đúng:

```text
wall_time = total_positions × label_cost / measured_datagen_rate
         + total_optimizer_samples / measured_train_rate
         + validation/A-B time
```

Đội còn ghi throughput nhãn là nút thắt, nên không được lấy GPU gradient throughput để bỏ qua thời gian datagen.

**Mức tin:** CHẮC rằng mốc 5–8 tháng chưa có cơ sở; thời gian thật cần benchmark từng stage.

---

## SBPC-02 — Train NNUE trên px0data và “bằng Pikafish trong 3–10 ngày”

**Kết luận: NHẬN — không có bằng chứng cho cả pipeline lẫn tuyên bố ngang Pikafish.**

Bằng chứng đội ghi `data.bin` là tar chứa chunk self-play kiểu Lc0/Px0 với policy/value, không phải sẵn FEN + score NNUE. Repo `pxzero-training` công khai là pipeline TensorFlow policy/value cho Px0 ecosystem; README hướng dẫn tải tar/chunks và cấu hình dataset/training, nhưng điều đó không chứng minh có converter trực tiếp sang binpack NNUE của đội.

Ngay cả khi giải mã được chunk, “3–10 ngày” không chứng minh “bằng Pikafish”: cần teacher/label semantics, architecture, số optimizer samples, validation và A/B engine có baseline. Dữ liệu policy/value của neural self-play không tự biến thành score cp phù hợp loss sigmoid-MSE.

**Mức tin:** CHẮC về việc tuyên bố “3–10 ngày và bằng Pikafish” vượt bằng chứng; format/version cụ thể cần parser pinned.

**Nguồn:**

- https://github.com/official-pikafish/pxzero-training
- https://github.com/official-pikafish/px0
- https://github.com/official-pikafish/Pikafish

---

## SBPC-03 — Đường B “hợp pháp, rẻ”, net train trên px0data bán được

**Kết luận: NHẬN — trái với luật owner và chưa đủ provenance để kết luận thương mại.**

ASK01 ghi rõ chính sách owner: net bán không trộn dump Pika Zero (ODbL). Pikafish README cũng ghi dữ liệu Pika Xiangqi Zero được cung cấp dưới ODbL. Ngoài ra manifest đội ghi license chưa rõ/README khai ODbL; tình trạng đó không phải cơ sở để tự gắn CC0 hoặc “bán được”.

ODbL phân biệt Produced Work và Derivative Database; việc model weight được phân loại pháp lý thế nào cần đánh giá theo dữ liệu/cách trích xuất/jurisdiction. Nhưng ở cấp vận hành, khi owner đã cấm nguồn này, pipeline thương mại phải loại nó hoặc xin quyết định/giấy phép mới bằng văn bản.

**Mức tin:** CHẮC về vi phạm luật owner; không đưa kết luận pháp lý rộng hơn “mọi net học từ ODbL chắc chắn…”

**Nguồn:**

- https://github.com/official-pikafish/Pikafish
- https://opendatacommons.org/licenses/odbl/1-0/

---

## SBPC-04 — Hai worker 88 luồng + 16 luồng datagen trên máy 192 lõi

**Kết luận: NHẬN — cấu hình 192/192 là 100%, trái luật tài nguyên 80%.**

```text
88 × 2 + 16 = 192 logical/physical slots theo cách tính của đề
```

Trong khi luật đội yêu cầu ≤80% và chừa ít nhất 8 luồng; ASK01 ghi trần vận hành khoảng 153 luồng. Cấu hình này có thể làm datagen, trainer và hệ điều hành tranh toàn máy; còn phải tính thread phụ/OMP/MKL/IO worker, nên 88×2 là chưa an toàn ngay cả khi tổng đúng 192.

**Cách sửa:** đặt ngân sách tổng trước, ví dụ `trainer1 + trainer2 + datagen + overhead ≤153`; đo thread thực tế, affinity và peak RSS. Không dùng `--threads` như số CPU duy nhất nếu thư viện tạo thêm inter-op/BLAS threads.

**Mức tin:** CHẮC về phép cộng và vi phạm trần đội.

---

## DSPC-01 — `pxzero-training` là trainer NNUE chính thức của Pikafish

**Kết luận: NHẬN — nhầm dự án/kiến trúc.**

Repo `official-pikafish/pxzero-training` có README mô tả pipeline TensorFlow, chunk/game data và các loss policy/value của Px0/PikaXiangqiZero. Điều đó khác trainer NNUE HalfKAv2/FullThreats của Pikafish. Việc repo thuộc tổ chức `official-pikafish` không đủ để gọi nó là trainer NNUE Pikafish.

ASK01 còn ghi trainer NNUE chính thức của Pikafish không công khai/PROVENANCE chưa cung cấp pipeline đầy đủ. Vì vậy phải gọi đúng là **Px0 policy/value training pipeline**, không dùng nó làm bằng chứng về code trainer NNUE.

**Mức tin:** CHẮC; README repo là nguồn trực tiếp.

**Nguồn:** https://github.com/official-pikafish/pxzero-training

---

## DSDPC-01 — Dùng `pikafish bench ... depth > positions.txt` để sinh dữ liệu NNUE

**Kết luận: NHẬN — `bench` là benchmark/search, không phải data generator.**

Theo bằng chứng code trong ASK01, lệnh `bench` chạy search trên tập thế mẫu và in nodes/nodes per second. Redirect stdout thành `positions.txt` chỉ lưu log benchmark, không biến nó thành record FEN + label + metadata hợp lệ. Muốn sinh dữ liệu phải có generator/parser riêng: chọn vị trí, chạy teacher search, ghi FEN/side/score/result/options và format mà trainer đọc được.

**Ca kiểm:** kiểm schema `positions.txt`, thử parse 10 dòng bằng loader; nếu dòng là log nodes không parse thành training entry thì verdict fail ngay.

**Mức tin:** CHẮC theo evidence code ASK01; không coi stdout bench là dataset.

**Nguồn nền:**

- https://github.com/official-pikafish/Pikafish
- https://official-stockfish.github.io/docs/nnue-pytorch-wiki/docs/nnue.html

---

## DSDPC-02 — 1×3090 khoảng 85 it/s ⇒ 100 epoch khoảng 30 ngày

**Kết luận: NHẬN — tự mâu thuẫn số học, nếu `it` là batch 16.384.**

Với 85 iterations/s và batch 16.384:

```text
85 × 16.384 = 1.392.640 samples/s
```

Nếu tổng là `100 × 10^8 = 10^10` samples:

```text
10^10 / 1.392.640 ≈ 7.180 s ≈ 2,0 giờ
```

Nếu mục tiêu của đội là `100 × 20M = 2×10^9` samples:

```text
2×10^9 / 1.392.640 ≈ 1.437 s ≈ 24 phút
```

Các con số này chỉ đúng khi 85 it/s là batch iteration ổn định, batch thật là 16.384, không bị I/O/checkpoint/validation và “epoch” nghĩa đúng tổng samples đã nêu. Nếu 85 it/s là một đơn vị khác, phải sửa manifest; không được vừa dùng it/s vừa nhân batch tùy ý.

**Mức tin:** CHẮC về phép quy đổi; throughput thật cần đo lại với wall-clock end-to-end.

---

## DSDPC-03 — `if (pos.checkers()) return VALUE_ZERO` và bộ hằng số evaluate

**Kết luận: NHẬN — theo bằng chứng dòng code trong ASK01, trích dẫn đã đổi nghĩa code.**

ASK01 ghi dòng thật là `assert(!pos.checkers())`, không phải `return VALUE_ZERO`. `assert` là điều kiện kiểm tra/invariant trong build phù hợp; nó không có semantics “trả eval zero” như khẳng định gốc.

Bộ hằng số được ASK01 đối chiếu là `465/11743/17380/3061/20582/253`, khác `485/11683/17720/3040/20120/267`. Vì vậy phần code quote và phần constants đều phải **NHẬN SAI**; chỉ sửa khi có commit/version/path cụ thể.

**Mức tin:** CHẮC theo evidence đội được trích trong ASK01; cần pin commit để tránh nhầm fork/version.

---

## DSDPC-04 — HalfKAv2_hm Xiangqi: các kích thước feature

**Kết luận: NHẬN MỘT PHẦN.**

Khẳng định gốc trộn một phần đúng với nhiều thông số sai:

- `LayerStacks=16`: phần này đúng theo evidence ASK01.
- `NUM_PLANES=8`, `KING_BUCKETS=6`, `ATTACK_BUCKETS=6`: không khớp các dòng header đã mở theo ASK01.
- ASK01 ghi `PS_NB=689`, `AttackBucketNB=4`, dimensions `6 × 4 × 689 = 16.536`; đây là cách tính cần dùng cho input HalfKAv2_hm được trích.

Không được đổi `16.536` thành “HalfKAv2 10.530” hoặc ngược lại mà không nói rõ feature set/version: đây là hai architecture/feature configurations khác nhau trong repo đội.

**Mức tin:** CHẮC rằng câu gốc sai một phần; `LayerStacks=16` là phần đúng, các constant còn lại phải gắn commit/header cụ thể.

---

## Bảng chốt lô

| Mã | Kết luận |
|---|---|
| BPPC-03 | NHẬN — mâu thuẫn byte/throughput; cần đo file/record/RSS. |
| GLMPC-10 | NHẬN — 2 tỷ/ngày cao hơn scale tuyến tính từ số đo khoảng 42×. |
| GRKPC-07 | NHẬN — lặp cùng lỗi, không phải bằng chứng độc lập. |
| MSPC-09 | NHẬN — throughput và 5–8 tháng chưa có benchmark. |
| SBPC-02 | NHẬN — chunk policy/value không tự là FEN+NNUE score; “3–10 ngày/bằng Pikafish” chưa có A/B. |
| SBPC-03 | NHẬN — trái luật owner; license/provenance chưa đủ cho thương mại. |
| SBPC-04 | NHẬN — 88×2+16=192, vượt trần 80%/153. |
| DSPC-01 | NHẬN — pxzero-training là Px0 policy/value pipeline, không phải bằng chứng trainer NNUE Pikafish. |
| DSDPC-01 | NHẬN — `bench` chạy benchmark/search, không sinh dataset NNUE. |
| DSDPC-02 | NHẬN — quy đổi 85 it/s × 16.384 mâu thuẫn với 30 ngày. |
| DSDPC-03 | NHẬN — `assert` bị trích thành `return`, constants sai version/giá trị. |
| DSDPC-04 | NHẬN MỘT PHẦN — LayerStacks 16 đúng, các constant/feature dimensions còn lại sai theo evidence. |

## Giới hạn

- Các path:dòng SourceCode/OngThan nêu trong ASK01 là bằng chứng của đội; trong phiên này không có toàn bộ source tree đó để mở lại từng dòng.
- Các phép tính số học đã kiểm tra độc lập; throughput/benchmark thật vẫn cần chạy trên hardware owner.
- Với license, chỉ kết luận theo policy owner và điều khoản công khai; không thay thế tư vấn pháp lý.
