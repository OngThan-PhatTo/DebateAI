# BigPickle — Ask01 — D3-11…D3-20 (10 mã)

**Phần II của cụm D3.** File này là phần tiếp theo của `BigPickle_Ask01_PhanD3NCFSMT_20261003_1527.md` (chứa `D3-01…D3-10` + bảng ma trận nguồn + quy ước mức độ). Tách ra để mỗi tệp nằm dưới giới hạn 100 KB. Quy ước mức độ và quy tắc nguồn giữ nguyên như file kia.

## Nguồn dùng trong 10 mã này

| Mã | Nguồn |
|:---|:---|
| S3 | `https://huggingface.co/ByteDance/SDXL-Lightning` — model card, đọc nguyên văn (2/4/8-step UNet, 1-step `sample`, LoRA, `openrail++`) |
| S2 | `https://www.gnu.org/licenses/gpl-faq.html` — Corresponding Source, công cụ build, đúng phiên bản, aggregate, không đặt điều kiện riêng |
| S10 | `server/fishtest/stats/stat_util.py` + `LLRcalc.py` (Fishtest) — dùng ở `D3-05` của file kia |
| S0 | Dữ liệu nội bộ của đội: `_LANE_NOTE/lock/w_*.md`, mã nguồn trong `SourceCode/` |

**Chưa mở (ghi `KHÔNG BIẾT` ở các mã liên quan):** `gpl-3.0.html`; văn bản `openrail++`; tài liệu developer YouTube/TikTok/Facebook; `jj.cn`; model card ViVoice; bảng MUSDB18/MVSEP; mã `llama.cpp`; tài liệu Windows về commit charge; quy trình fansub/Aegisub.

---

### D3-11 — llama.cpp trên Windows: phần nào của model tính vào COMMIT CHARGE?

**Trả lời**

Tôi **không mở** mã `llama.cpp` (tệp mmap, `ggml-vulkan`, `ggml-hip`) hay tài liệu Microsoft về commit charge của section ánh xạ tệp, nên (i), (ii), (iii) đều `KHÔNG BIẾT` theo đúng luật: không có nguồn thì không dẫn. Nhưng có ba việc tôi làm được ngay và hữu ích hơn việc đoán:

**(1) Triệu chứng bạn mô tả tự nó là một phép đo.** Máy sập ở `52,2 / 53,4 GB` commit trong khi RAM vật lý còn trống là đúng dấu hiệu của việc **commit limit ≠ RAM vật lý**, và giới hạn của bạn bị đẩy xuống bởi pagefile chỉ ~4,3 GB. Trên máy này, **mọi công thức ước đều thua phép đo**. Quy trình đúng, mỗi lần một biến: bật `llama-server` với commit giữ nguyên, chờ tới khi tải xong, đọc `K32GetPerformanceInfo` (`CommitTotal`, `CommitLimit`, `PageSize`) rồi so với mức trước khi bật. Chạy theo cặp: `--no-mmap` vs mặc định, và `n_gpu_layers = 0` vs toàn bộ. Bốn con số thu được trả lời trọn cả ba câu hỏi (i)–(iii) **cho đúng máy này**, thay vì trả lời chung cho cả lớp máy.

**(2) Công thức KV cache thì định nghĩa chắc, không cần nguồn ngoài.** Với MQA/GQA (mà mọi model bạn nêu đều dùng), bộ nhớ KV mỗi slot là:

```
bytes_KV = n_ctx × n_layer × n_kv_head × head_dim × 2 × sizeof_dtype × n_slot
```

Hệ số **2** là cho K và V. Điểm quan trọng: `n_kv_head` nhỏ hơn `n_head` rất nhiều ở model đã lượng tử, nên đây là chỗ **dễ tính sai nhất**. Với `n_slot = 1` thì phần này thường tính bằng **vài trăm MiB, không phải vài GiB** — nghĩa là KV cache gần như không phải thủ phạm; ngân sách thật nằm ở **trọng số**. Cần đo là biết ngay.

**(3) Hai hiểu lầm cần chặn trước khi đo.** Thứ nhất, cùng một cách tính phải áp cho **mọi tiến trình**, không chỉ `llama-server` — máy của bạn chạy ComfyUI, nhiều engine, `BookTool`… nếu bạn tính trần cho một tiến trình rồi bật nó, bạn vẫn có thể cạn. Thứ hai, và đây là chỗ dễ sai nhất: **`--mlock` và `--no-mmap` là hai lá cờ trái chiều với mục tiêu của bạn** — một cái ép trang ở lại (tăng áp lực commit), một cái bỏ ánh xạ tệp. Tôi **không khẳng định** cơ chế tính của chúng trên Windows (`KHÔNG BIẾT`), nhưng bạn **phải** giữ chúng cố định giữa các lần đo, nếu không phép so sánh vô nghĩa.

**Về lựa chọn đã chọn (`size × 1,2 + 1,5 GiB`, trần 75 %, không đọc được commit thì không bật).** Tôi đồng ý với nguyên tắc và tôi muốn giữ đúng một điểm mạnh của nó: nó **thất bại theo hướng an toàn**, và trên một máy đã từng cạn commit, hệ số 1,2 và trần 75 % là biện pháp hợp lý cho khi **chưa có số đo**. Điều tôi đề nghị sửa là **kỷ luật dữ liệu**: sau khi lấy được 4 số đo ở (1), hãy thay hằng số 1,2 bằng **tỉ lệ thật đo được + biên an toàn**, và ghi tỉ lệ đó vào sổ kèm ngày đo. Con số 1,2 hiện tại là **giả định không có nguồn**; nó không sai, nhưng nó không phải số đo.

**Nguồn:** không có nguồn ngoài cho (i)–(iii) — đã ghi ở mục *Chưa mở*. Số của đội: `_LANE_NOTE/lock/w_localai_t38c_0210_2026-10-02.md` mục 6; mã `SourceCode/LocalAI/TaiModel/MayChuModel.cs`.
**Mức độ:** công thức `bytes_KV`: `CHẮC` (định nghĩa kiến trúc). Hệ số 1,2 / trần 75 %: `GIẢ THUYẾT CẦN ĐO` (đúng hướng, chưa có số đo máy này). (i)–(iii) về cơ chế Windows: `KHÔNG BIẾT`.

---

### D3-12 — Clone giọng Việt: F5-TTS ViVoice là CC-BY-NC-SA-4.0 — mô hình nào cho thương mại?

**Trả lời**

**(i) Danh sách model có giấy phép thương mại + MOS/WER: `KHÔNG BIẾT`.** Tôi không mở model card ViVoice, VieNeu-TTS, XTTS-v2, F5-TTS gốc, Piper, hay bất kỳ bảng điểm nào trong lượt này, nên tôi sẽ **không** đưa tên model nào là "được phép thương mại" và **không** đưa số MOS/WER nào. Việc này đúng với câu hỏi: nó ghi rõ «ô nào không chắc ghi "không biết"». Đây là phần đội **phải** tự tra, và nên tra theo thứ tự: giấy phép trên model card → giấy phép **tệp trọng số**, không phải repo → điều khoản về **dữ liệu huấn luyện** (CC-BY-NC-SA-4.0 là loại giấy phép mà nhiều bộ dữ liệu giọng nói cấm dùng thương mại ngay từ đầu, nên phải đọc kỹ). Một sự thật đáng lưu ý về mặt cấu trúc: **CC-BY-NC-SA-4.0 có điều khoản ShareAlike**, nên nếu sau này đội dùng được model có NC, thì cả sản phẩm phái sinh còn dính NC — điều này khiến phương án (b) "xin giấy phép thương mại từ tác giả ViVoice" là đường **duy nhất giữ được mô hình đã chọn**, và nên mở hộp thư ngay chứ không để sau.

**(ii) Phương án (c) — app không kèm model, người dùng tự tải — có tách được trách nhiệm không: về cấu trúc thì CÓ, về pháp lý thì đừng coi là miễn trừ.** Lập luận tôi khẳng định được: giấy phép bám vào **hành vi**, không bám vào tệp. Một nút tải là **không phân phối lại trọng số** — người dùng tự tải và tự dùng thì họ là bên được ràng buộc bởi giấy phép, không phải app. Ngược lại, nếu app **đóng gói kèm** model, app **là** bên phân phối và bị ràng buộc. Đây là lý do phương án (c) có tác dụng thật: nó **thu hẹp** rủi ro, chứ không xoá.

Nhưng ba điều phải nói thẳng: (1) đây **không phải ý kiến pháp lý** và tôi không phải luật sư; (2) app vẫn còn rủi ro **gián tiếp** — ví dụ model card tự ghi "nhập về dùng cho mục đích phi thương mại" thì việc app giới thiệu nó như một tính năng của sản phẩm thương mại là hành vi có rủi ro dù không kèm tệp; (3) nếu model có NC và người dùng dùng để tạo nội dung thương mại, việc họ vi phạm là **vi phạm của họ**, không phải của app — nhưng điều khoản của nhiều dịch vụ có thể cấm *hỗ trợ* tới hoạt động đó. `GIẢ THUYẾT CẦN ĐO` cho kết luận pháp lý, `CHẮC` cho phần cấu trúc "nút tải ≠ phân phối lại".

**(iii) Cấu hình của đội có khớp model card không — không biết**, vì chưa mở model card. Nhưng có một điểm **nội bộ** tôi thấy đáng sửa ngay, không cần model card: đội đang **giữ nguyên chữ** trong đoạn tham chiếu khi cắt ≤ 12 s. Với F5-TTS, điều đó tạo ra một sự lệch thuần hình thức nhưng rất dễ thấy ở đầu/cuối câu. Đây cũng là lý do phương án (b) của câu hỏi ("không chuẩn hoá chữ") là **phải làm, không phải tùy chọn**: chuẩn hoá số thành chữ và viết tắt thành ký tự là điều kiện tiên quyết để mẫu giọng không bị lệch âm sắc. Tôi không biết ViVoice xử lý thế nào ⇒ `KHÔNG BIẾT`; nhưng đội nên có ca thử: cùng một câu, một lần có chuẩn hoá, một lần không, nghe và quyết định.

**Nguồn:** chỉ có dữ liệu đội đã lưu: `_LANE_NOTE/lock/w_comfy_clonegiong_0210_2026-10-02.md` §1–§2; `SourceCode/ComfyUI_Pro/ComfyUIPro/Assets/kho/goi_toi_thieu_F.json:291-299` (ghi giấy phép theo API HF). **CHƯA MỞ:** model card ViVoice và các model thay thế; tài liệu CC-BY-NC-SA-4.0.
**Mức độ:** (i) `KHÔNG BIẾT`. (ii) phần cấu trúc `CHẮC`, phần pháp lý `GIẢ THUYẾT CẦN ĐO`. (iii) `KHÔNG BIẾT`; khuyến nghị chuẩn hoá chữ `GIẢ THUYẾT CẦN ĐO`.

---

### D3-13 — Lồng tiếng: hệ số chiếu 0,3 và `silencedetect` — có cách chuẩn hơn?

**Trả lời**

**(i) Model tách nào tốt nhất và chạy được trên máy này: `KHÔNG BIẾT`.** Không mở bảng MUSDB18/MVSEP, không mở README `htdemucs`/`htdemucs_ft`, không kiểm tra yêu cầu ROCm. Tôi sẽ không tự sắp xếp thứ hạng.

**(ii) Nhưng hệ số chiếu mà đội dùng có một vấn đề toán học, và tôi nghĩ đây là điều đáng sửa nhất của câu.** `Σ nền·giọng / Σ giọng²` là một tỉ lệ phụ thuộc **biên độ**: nhân cả hai bè cùng lên một hệ số thì tỉ lệ **không đổi**, trong khi cảm nhận "còn sót giọng" thì có. Và `silencedetect` lại chỉ nhìn **mức năng lượng tổng** của khe, nên nó không hề biết giọng có còn hay không. Hệ quả cụ thể: một đoạn có giọng rất nhỏ nằm trên nhạc lớn sẽ cho tỉ lệ nhỏ (coi là sạch) **và** đồng thời không tạo khoảng lặng (coi là có lời thoại) — tức là **cả hai bước đều đồng ý sai cùng lúc**. Ngưỡng 0,3 vì vậy không có ý nghĩa vật lý: nó là hệ số của hai bè chưa được chuẩn hoá, nên **phải hiệu chỉnh trên bộ mẫu của chính đội** trước khi dùng, không có giá trị mặc định nào là đúng.

**Thước đo thay thế, và tôi khuyến nghị mạnh cách (a) của chính đội**: **đo bằng kết quả thay vì bằng hệ số.** Chạy VAD / whisper trên **chính bè nền**: nếu bè nền ra chữ thì còn sót giọng — đây là phép thử trực tiếp đúng thứ bạn quan tâm, không cần mô hình hoá gì cả, và cho ngưỡng **có nghĩa về mặt nghiệp vụ** ("bè nền này được cho qua khi không đọc ra chữ nào"). Nó tốn thêm một lượt suy luận, nhưng nó chuyển một ngưỡng vô nghĩa (0,3) thành một ngưỡng có thể kiểm tra. Đây là đề xuất tôi tin nhất trong câu này. `GIẢ THUYẾT CẦN ĐO`, nhưng hợp lý và đo được.

**(iii) Whisper.cpp có VAD tích hợp: `KHÔNG BIẾT`** cờ cụ thể, nên tôi **không** viết tên cờ. Về ảnh hưởng tới mốc thời gian sub thì tôi nói được điều có ích mà không cần VAD: `silencedetect` của ffmpeg phải chạy **trên toàn wav** nên nó biết vị trí thời gian thật; nếu đổi sang VAD trong whisper thì mốc thời gian lấy từ đâu sẽ phụ thuộc cách bạn ghép — và đây là chỗ dễ mất tính đồng bộ giữa `.ass` và audio. Lời khuyên có trật tự: **giữ `silencedetect` làm nguồn mốc thời gian** (vì nó không phụ thuộc mô hình, chạy nhanh, và tái lập), dùng VAD/đọc-bè-nền chỉ để **ra quyết định dùng hay không**. Tách hai vai trò này ra thì bạn không phải sửa timestamp về sau.

**Nguồn:** dữ liệu đội: `_LANE_NOTE/lock/w_comfy_longtieng_0210_2026-10-02.md` §1–§2; `SourceCode/Shared/GiongPhuDe/WhisperRunner.cs:114`. **CHƯA MỞ:** bảng MUSDB18/MVSEP, tài liệu whisper.cpp VAD, README Demucs.
**Mức độ:** (i) `KHÔNG BIẾT`. Nhận định "hệ số chiếu phụ thuộc biên độ nên ngưỡng 0,3 vô nghĩa vật lý": `GIẢ THUYẾT CẦN ĐO`. Khuyến nghị "đo bằng đọc-bè-nền thay cho hệ số": `GIẢ THUYẾT CẦN ĐO`. (iii) tên cờ VAD: `KHÔNG BIẾT`.

---

### D3-14 — Phân giới người nói theo F0 (mơ hồ 165–180 Hz) — cách tốt hơn chạy offline?

**Trả lời**

**(i) Danh sách model + số đo + giấy phép: `KHÔNG BIẾT`.** Không mở pyannote, ECAPA, wav2vec2, không có bảng số nào trong lượt này ⇒ không đưa tên kèm số.

**Nhưng câu hỏi tiền nhân của bạn — ngưỡng F0 165/180 có nguồn ngữ âm học không — thì tôi trả lời được một điều có giá trị mà không cần nguồn: đó là một ngưỡng tuyệt đối, và đó là vấn đề của nó.** Ngưỡng tuyệt đối chỉ hợp với một quần thể mà F0 phân bố tập trung và không chồng nhau. Với tiếng Việt, đội đang ở đúng vị trí khó nhất của bài toán này: giọng trẻ em nam, giọng phụ nữ trầm, và giọng già có F0 thấp đều rơi vào cùng vùng mà giọng nam trung bình nằm. Dải mơ hồ 165–180 Hz chỉ rộng **9 %** — đủ hẹp để quyết định cho hầu hết câu, nhưng đường biên đặt ở đó thì **một phần câu của người nói nam sẽ bị đọc là nữ ngay khi F0 dao động lên**. Hysteresis của đội chỉ sửa phần *kéo dài* của lỗi, không sửa phần **sai ngay từ đầu**. Đây là lý do tôi coi giới theo F0 là **đầu vào có độ tin cậy thấp**, và việc mở rộng vùng mơ hồ chỉ làm nó vô dụng hơn chứ không phải chính xác hơn.

**Đề xuất, theo thứ tự chi phí tăng dần:**

1. **Rẻ nhất, tôi khuyến nghị:** giữ F0 làm **dự kiến yếu**, nhưng **cho phép người dùng ghi đè bằng một nút bấm** — và ghi lại lựa chọn của họ. Với vài chục cây một tập, một vài người xem sẽ gán đúng gần 100 % trong khoảng thời gian ngắn, và bạn thu được **dữ liệu có nhãn thật** để huấn luyện hoặc hiệu chỉnh bộ dò về sau. Phương án (c) của đội là đúng, và nó là phương án duy nhất **chắc chắn** không tệ.
2. **Trung bình:** thay F0 bằng **đặc trưng đơn giản hơn nhưng ổn định hơn** — ví dụ năng lượng tổng theo dải tần (formant và nền của thanh điệu), chịu đổi giới tính người nói tốt hơn nhiều vì thanh điệu nằm ở tần số thấp cố định theo giới chứ không theo cá thể. Tôi chưa đo với tiếng Việt nên đây là `GIẢ THUYẾT CẦN ĐO`, nhưng nó là hướng đúng về mặt nguyên nhân.
3. **Đắt, chưa có số:** embedding + bộ phân loại, nhưng phải chấp nhận rằng bạn **không có tập dữ liệu tiếng Việt có nhãn** để đánh giá nó, nên bạn sẽ **không biết** nó có chính xác hơn F0 hay không. Đây là lý do tôi không xếp nó lên đầu dù nó "hiện đại" hơn.

Điểm quan trọng nhất về mặt sản phẩm: nhãn sai giới là loại lỗi **người xem nhận ra ngay lập tức**, và lỗi đó làm hỏng niềm tin vào toàn bộ lượt lồng tiếng. Nên **thiết kế để không bao giờ khẳng định khi không chắc** — mà lựa chọn "bất kỳ + giữ bản dịch trung tính" mà đội đã chọn là đúng hướng.

**Nguồn:** mã đội `SourceCode/Shared/GiongPhuDe/GenderDetector.cs:7-36`. **CHƯA MỞ:** mô hình/dataset nào; nguồn ngữ âm học cho 165/180 Hz (tôi **không** khẳng định ngưỡng này có hay không có nguồn — chỉ nói nó không có giá trị vật lý như một ngưỡng tuyệt đối).
**Mức độ:** "ngưỡng tuyệt đối 165/180 Hz không phù hợp với tiếng Việt": `GIẢ THUYẾT CẦN ĐO` (cần đo trên mẫu tiếng Việt của đội). Danh sách model/số đo/giấy phép: `KHÔNG BIẾT`. Khuyến nghị: `GIẢ THUYẾT CẦN ĐO`.

---

### D3-15 — Phụ đề chuyên nghiệp: karaoke `\kf` cho bản dịch, OCR Windows, che hộp đặc

**Trả lời**

**(i) Karaoke cho câu dịch — đây là chỗ tôi nghĩ lựa chọn hiện tại của đội sẽ tạo ra lỗi nhìn thấy được, và lý do thì ngôn ngữ chứ không phải kỹ thuật.** Đội ánh xạ mốc thời gian theo **tỉ lệ ký tự**. Với câu **tiếng Việt**, cách đó sai theo hệ thống, không phải sai ngẫu nhiên: tiếng Việt là đơn âm tiết, một từ có thể 1–7 âm tiết, và **độ dài bằng âm tiết chứ không phải bằng ký tự**. Câu *"Tôi không biết"* (12 ký tự, 4 âm tiết) và câu *"Nghiên cứu"* (10 ký tự, 5 âm tiết) sẽ được phân bố thời gian gần như bằng nhau, dù thời lượng nói thật khác nhau rõ rệt. Hệ quả: `\kf` sẽ **tịnh tiến dần**, và ở câu dài sai tích luỹ đủ để thấy. Không có cách sửa bằng hằng số hiệu chỉnh cho toàn bộ câu, vì tỉ lệ phụ thuộc từng từ.

Về thực hành fansub/Aegisub: tôi **không mở** nguồn nào về quy trình karaoke dịch, `KHÔNG BIẾT`. Nhưng tôi nói được hướng đúng, vì nó là hệ quả của điểm trên: **mốc thời gian phải đến từ âm thanh, không đếm được từ chữ.** Cụ thể, phương án (a) mà đội nêu là phương án duy nhất hợp lý — nhưng đáng chú ý là nó **đã có sẵn** thứ nó cần: sau khi lồng tiếng, audio đã có, và đội đã có đường whisper. Nghĩa là: **căn chỉnh câu dịch vào audio đã lồng** bằng forced alignment. Song ngữ (giữ bản gốc và bản dịch cùng nhịp) là hướng đúng, và nó còn tận dụng được việc `\kf` vốn dĩ đã cần mốc thời gian thật. `GIẢ THUYẾT CẦN ĐO` cho việc đội có công cụ offline phù hợp trên Windows — `KHÔNG BIẾT` cho danh sách.

**(ii) Công cụ xoá sub cứng offline + giấy phép: `KHÔNG BIẾT`.** Không mở ProPainter, LaMa, `video-subtitle-remover` ⇒ không đưa tên kèm giấy phép. Về yêu cầu phần cứng tôi chỉ nói được điều chắc: các mô hình inpaint video **không** có bản CPU chất lượng tương đương, và máy ROCm 47,6 GB của đội thì **đủ** cho bản GPU — nhưng đó là suy luận từ đặc điểm bài toán, không phải số đo, nên `GIẢ THUYẾT CẦN ĐO`.

**(iii) OCR Windows vs PaddleOCR với chữ Trung: `KHÔNG BIẾT`** — không có số lỗi công bố nào trong lượt này, tôi không đưa số nào. Nhưng đội đã có một số đo **của chính đội** đáng dùng hơn: OCR đọc **0 ký tự** ở độ đục 0,85/0,95/1 — nghĩa là **hộp che của đội đang đủ đục để bị OCR bỏ qua, dù đọc được**. Sự kiện đó nói hai điều: nó **đúng** là cơ sở để bỏ hẳn phương án OCR-để-gỡ nhãn cứng (và dùng inpaint), và nó có nghĩa phép đo `validate_board`-tương tự của đội — *OCR có đọc được không* — là cách đo rẻ và đúng để chứng minh che hộp hoạt động. Gợi ý cụ thể: dùng chính OCR của đội làm **ca thử hồi quy** cho mọi lần đổi tham số hộp che. Đó là một ca thử tốt vì đã có sẵn công cụ, và nó biến một quyết định thẩm mỹ thành một phép kiểm. `GIẢ THUYẾT CẦN ĐO`.

**Nguồn:** `_LANE_NOTE/lock/w_comfy_phude_0210_2026-10-02.md` §1–§2; mã `SourceCode/ComfyUI_Pro/ComfyUIPro/Core/PhuDeChuyenNghiep.cs`. **CHƯA MỞ:** quy trình fansub/Aegisub; repo ProPainter/LaMa/video-subtitle-remover; số lỗi OCR.
**Mức độ:** "ánh xạ mốc theo tỉ lệ ký tự sai hệ thống với tiếng Việt": `CHẮC` (lập luận ngôn ngữ, không cần nguồn ngoài). Hướng forced alignment: `GIẢ THUYẾT CẦN ĐO`. (ii)/(iii) danh sách công cụ, giấy phép, số đo: `KHÔNG BIẾT`.

---

### D3-16 — SDXL-Lightning trong ComfyUI: cfg 1.0 · `euler` · `sgm_uniform` · đúng N bước

**Trả lời**

Đây là câu tôi có nguồn trực tiếp và **cấu hình đội chọn gần như đúng hết**, chỉ thiếu một điểm quan trọng ở bản 1 bước. Trích nguyên văn từ model card (S3):

**(a) Sampler + scheduler, mục ComfyUI — đúng như đội chọn:**
> *"Please use Euler sampler with sgm_uniform scheduler."*

**(b) CFG và số bước — đúng, và nguồn nói rõ cả hai:**
> *"Ensure using the same inference steps as the loaded model and CFG set to 0."*

Điểm cần lưu ý cho ComfyUI: `guidance_scale=0` ở diffusers **tương đương cfg = 1.0** trong KSampler của ComfyUI (cfg = 1.0 nghĩa là không áp CFG). Nên **cfg 1.0 của đội là đúng**. Câu "đúng N bước" cũng đúng: *"Use the correct ckpt for your step setting!"*

**(c) Một điều đội chưa nêu nhưng quan trọng — `sgm_uniform` bắt buộc phải tương ứng với `trailing` timesteps.** Ở phần diffusers, mọi ví dụ đều có cùng một dòng:
> `pipe.scheduler = EulerDiscreteScheduler.from_config(pipe.scheduler.config, timestep_spacing="trailing")`
> và chú thích *"Ensure sampler uses \"trailing\" timesteps."*

Tức là `sgm_uniform` **chính là** tên của `trailing` timestep spacing trong giao diện ComfyUI. Nghĩa là: nếu một bản workflow nào đó để scheduler ở chế độ khác, ảnh sẽ sai dù vẫn đúng sampler. Đội nên **ghim cả hai** trong template, không chỉ Euler.

**(d) Bản 1 bước — đây là câu trả lời trực tiếp cho "phương án khác (a) sampler riêng cho bản 1-bước": CÓ, và không chỉ khác sampler.**
> *"The 1-step model is only experimental and the quality is much less stable. Consider using the 2-step model for much better quality."*
> *"The 1-step model uses \"sample\" prediction instead of \"epsilon\" prediction! The scheduler needs to be configured correctly."*

Và tên tệp bản 1 bước có đuôi khác: `sdxl_lightning_1step_x0.safetensors` / `sdxl_lightning_1step_unet_x0.safetensors` — hậu tố **`x0`** chính là dấu hiệu của `prediction_type="sample"`. Ngoài ra model card còn yêu cầu *"Update your ComfyUI to the latest version"* cho bản 1 bước. Hệ quả cho code của đội: **nhận diện bản 1 bước phải qua hai dấu hiệu** — tên chứa `1step` (hoặc `x0`) **và** thiết lập `prediction_type = sample`; chỉ dựa vào số bước rồi áp Euler/sgm_uniform/cfg 1.0 như các bản khác là **chưa đủ**. Đây là bản vá cụ thể tôi đề xuất cho `Templates.cs:843-861`.

**(e) LoRA — câu hỏi "LoRA ghép vào checkpoint SDXL khác có cần đổi tham số gì" được model card trả lời rất rõ, và câu trả lời là KHÔNG nên ghép:**
> *"Use LoRA only if you are using non-SDXL base models. Otherwise use our UNet checkpoint for better quality."*

Nghĩa là: **đừng** đưa Lightning LoRA vào một checkpoint SDXL khác. Chỉ dùng LoRA khi base **không phải** SDXL. Ngoài ra *"The full UNet models have the best quality while the LoRA models can be applied to other base models."* Với tên chứa "lightning" trong tên tệp, đội có thể gặp cả hai loại trong cùng kho, nên phải **phân biệt tệp LoRA với tệp UNet/checkpoint** (`.safetensors` trong `models/loras` vs `models/checkpoints`) chứ không chỉ nhìn tên.

**(f) Giấy phép — và điều này liên quan trực tiếp tới cổng đóng gói:** metadata của model card ghi `license: openrail++`. Tôi **không** mở văn bản openrail++, nên không khẳng định điều khoản thương mại của nó. Nhưng đây **không phải** CC-BY và **không phải** MIT, và với luật owner ("net bán được = giấy phép thương mại"), đây là **một mục cần đưa vào sổ hồ sơ giấy phép** chứ không được coi là mặc định được phép. `KHÔNG BIẾT` về điều khoản cụ thể.

**Kết luận cho câu này:** cfg 1.0 · Euler · sgm_uniform · steps = N — **đúng và có nguồn**. Cần bổ sung: (1) nhánh riêng cho bản 1 bước với `prediction_type = sample`; (2) phân biệt LoRA với UNet; (3) xử lý `openrail++` như giấy phép cần xác minh.

**Nguồn**
- S3 `https://huggingface.co/ByteDance/SDXL-Lightning`: các trích dẫn nguyên văn nêu trên; `license: openrail++`; bài `arXiv:2402.13929`.
- **CHƯA MỞ:** model card `AnimateDiff-Lightning` (repo khác của cùng nhóm) — câu hỏi cũng hỏi repo này; văn bản `openrail++`.

**Mức độ**
- Toàn bộ các trích dẫn trên: `CHẮC` (đọc nguyên văn model card).
- "cfg 1.0 trong ComfyUI tương đương `guidance_scale=0`": `GIẢ THUYẾT CẦN ĐO` — quy ước chuẩn của ComfyUI, tôi **không** mở tài liệu ComfyUI để xác nhận trong lượt này.
- "Không dùng Lightning LoRA trên base SDXL": `CHẮC` (nguyên văn model card).
- Điều khoản thương mại của `openrail++`: `KHÔNG BIẾT` — phải mở văn bản.
- AnimateDiff-Lightning: `KHÔNG BIẾT` — chưa mở.

---

### D3-17 — Đăng video đa nền tảng: API chính thức hay tự lái WebView2?

**Trả lời**

**(i) Hạn mục và điều kiện duyệt: `KHÔNG BIẾT`.** Không mở tài liệu YouTube Data API v3, TikTok Content Posting, Facebook Graph trong lượt này ⇒ **không đưa con số quota nào** (câu hỏi cũng yêu cầu "số quota kèm ngày của tài liệu" — không mở thì không có ngày).

**(ii) Rủi ro khoá tài khoản khi tự lái giao diện: tôi không dẫn được điều khoản nào, `KHÔNG BIẾT`.** Nhưng có một điểm thiết kế đáng nói mà không cần điều khoản: **cách đội dựng (owner tự đăng nhập trong WebView2, app không gõ/lưu mật khẩu, gặp kiểm tra người thì dừng) là cách đúng về mặt giảm rủi ro**, và lý do nằm ở ranh giới mà đội tự vẽ: **app không đăng nhập bằng tài khoản của mình, nên không giữ bí mật của người khác**. Rủi ro còn lại không nằm ở đăng nhập mà nằm ở **mẫu lưu lượng tự động** (tần suất, hành vi giống người hay giống bot) — và đó là thứ chỉ đo được trên tài khoản thật của chính chủ, không đọc được từ tài liệu.

**Về kiến trúc, tôi khuyến nghị thứ tự mà đội nên đặt ra, và nó cũng là thứ rẻ nhất để kiểm:** hãy tách **tầng trừng phạt** khỏi **tầng API**. Mọi nền tảng đều có thể thu hồi token, nhưng thu hồi token thì **rẻ và hồi phục được** (đăng nhập lại), còn khoá tài khoản thì không. Vì vậy: lưu DPAPI token như đội đang làm, dùng API khi có, và **chỉ rơi xuống WebView2 khi API không khả dụng**; đồng thời **giới hạn số lần rơi xuống WebView2 trong một ngày** để một sự cố token không biến thành một đợt tự động hóa dày đặc. `GIẢ THUYẾT CẦN ĐO`.

Một lỗi thiết kế cần sửa, không liên quan tới nền tảng nào: quy tắc "lỡ giờ khi app tắt ⇒ chạy bù đúng một lần" là **nguyên nhân của một lớp tai nạn riêng** — nếu app bị tắt nhiều ngày, lịch phát sinh một đống và lần chạy bù sẽ đăng một loạt video cùng lúc. Đó là hành vi nhận diện bot rõ ràng nhất. Cách an toàn: **bù bằng số lần chứ không phải theo lịch**, và có trần (ví dụ tối đa 1 bài/ngày) cộng thêm cờ "chạy bù" cho người dùng duyệt trước. `GIẢ THUYẾT CẦN ĐO`.

**(iii) Douyin / Kuaishou có API đăng cho tài khoản ngoài Trung Quốc: `KHÔNG BIẾT`** — chưa mở, không đoán.

**Nguồn:** dữ liệu đội: `_LANE_NOTE/lock/w_comfy_dang_tai_0210_2026-10-02.md` §1–§2. **CHƯA MỞ:** ba bộ tài liệu developer chính thức.
**Mức độ:** toàn bộ (i)–(iii) `KHÔNG BIẾT`. Khuyến nghị kiến trúc: `GIẢ THUYẾT CẦN ĐO`.

---

### D3-18 — AUTO cho JJ象棋: lấy mẫu bàn/quân hợp pháp ở đâu, hay nhận dạng không cần mẫu skin?

**Trả lời**

**(i) JJ象棋 có chế độ xem ván không cần đăng nhập: `KHÔNG BIẾT`.** Tôi không truy cập được `jj.cn` và không có ảnh chứng minh nào, mà câu hỏi yêu cầu "URL hoặc ảnh chứng minh" — không có thì tôi ghi không biết, **không** suy đoán từ thông tin đăng nhập mà đội đã ghi (màn đăng nhập chỉ có SĐT/微信/QQ/抖音/支付宝 thì **không** kết luận được là không có chế độ khách).

**(ii) Bộ dữ liệu / model nhận dạng bàn-quân cờ tướng tổng quát, giấy phép rõ: `KHÔNG BIẾT`.** Tôi không mở `VinXiangQi` (repo GPLv3 mà đội nhắc) nên không kiểm được giấy phép hay số đo của nó. Điều tôi nói được chắc là về **cấu trúc bài toán**: một bộ nhận dạng dùng YOLO và khớp lưới 9×10 **có thể** tổng quát qua nhiều skin, kể cả 3D, **vì nó không dựa vào mẫu pixel mà dựa vào hình học lưới** — đó chính là lý do phương án (a) khả thi về mặt nguyên tắc. Nhưng "khả thi về nguyên tắc" và "đạt độ chính xác chấp nhận được" là hai việc khác nhau, và việc thứ hai cần một tập skin đa dạng mà **đội chưa có** — mà không có tập đó thì cũng **không đo được** độ chính xác. Vậy bài toán thật là: **lấy ảnh đa skin từ đâu hợp pháp**, và đó chính là câu (iii).

**(iii) Dùng ảnh chụp màn hình JJ chỉ làm dữ liệu train nội bộ, không phân phối ảnh — rủi ro bản quyền.** Tôi **không có nguồn pháp lý nào** cho câu này và không phải luật sư, nên tôi không đưa kết luận pháp lý. Nhưng tôi nói được điều có giá trị nhất: **đây là câu mà rủi ro thấp nhất trong toàn bộ 34 câu, và cũng là câu mà nên dừng việc luật lý lại.** Ba điều kiện cùng lúc: (1) không phân phối ảnh; (2) không phân phối **net** huấn luyện từ ảnh đó ra ngoài phạm vi đội mà không xử lý vấn đề tương tự D3-07; (3) ảnh chụp từ ván của chính tài khoản đó. Điểm (2) là điểm dễ sót: dù ảnh không đi ra, **net thì có thể đi ra**, và đó mới là thứ luật owner kiểm.

**Về lựa chọn đã chọn của đội.** Tôi đồng ý với việc khách tự đăng nhập và bộ nhận dạng chạy trên máy khách. Nhưng tôi khuyến nghị **đổi thứ tự ưu tiên** một chút: thay vì chờ có tài khoản JJ mới lấy mẫu, hãy làm **phương án (c) — người dùng tự "dạy mẫu" lần đầu** — và làm nó **ngay**, vì nó giải quyết đồng thời cả ba vấn đề: không cần ảnh của bên thứ ba, không cần tài khoản JJ, và sinh ra dữ liệu **đúng skin người đó dùng** (mà skin khác nhau thì bộ nhận dạng dùng chung sẽ kém đi). Ràng buộc thiết kế duy nhất: mỗi lần dạy mẫu phải **đủ thông tin để suy lại lưới** (đủ quân ở nhiều ô, tối thiểu là khai cuộc) — và đây đúng là cái mà ca thử `SON-03` đang kiểm. Nói ngắn gọn: **mẫu lấy từ chính máy khách là nguồn mẫu duy nhất vừa sạch vừa đúng.**

**Nguồn:** dữ liệu đội: `AI_Debate/onGoing/PHIEU_JJCHESS_LOGIN_2026-09-26.md` §5–§10. **CHƯA MỞ:** `jj.cn`, điều khoản `regService.html`, repo `VinXiangQi`.
**Mức độ:** (i) `KHÔNG BIẾT`. (ii) "bộ nhận dạng dựa vào hình học lưới thì tổng quát được qua skin" `GIẢ THUYẾT CẦN ĐO`; tên repo/số đo/giấy phép `KHÔNG BIẾT`. (iii) rủi ro pháp lý `KHÔNG BIẾT` (không có nguồn); khuyến nghị "người dùng tự dạy mẫu" `GIẢ THUYẾT CẦN ĐO`.

---

### D3-19 — Gói Demo: Docco (GPLv3) + GUI đóng nguồn có khoá hết hạn

**Trả lời**

**(i) Đổi tên binary + kèm `SOURCE_GPL.zip` sinh tái lập — đủ "Corresponding Source" chưa, có phải kèm toolchain không?** Trả lời được, và từ GPL FAQ (S2):

> *§ 100. What is "Corresponding Source"?* — *"The Corresponding Source for a work means the source code and any scripts needed to build and install the executable, in the form in which the source and scripts would be used by the user."*

Về toolchain, S2 nói rõ và **có lợi cho đội**:
> *§ 161. If I release my program under the GPL, but I offer it for sale, is that OK?* / phần *"What tools do I have to use?"* — *"Which programs you used to edit the source code, or to compile it, or to study it, or record it, usually makes no difference"*.

Nghĩa là **không** phải kèm toolchain — nhưng phải kèm **script build**. Đội đã làm đúng hai việc: đổi tên exe (`docco.exe`) là được, và kèm zip **sinh tái lập được** (116 mục, 823.003 B, sinh 2 lần cùng byte, 0 tệp lệch) — đó là bằng chứng tốt hơn hẳn một cam kết bằng văn bản. S2 cũng cấm yêu cầu dùng công cụ cụ thể của tác giả:
> *"You may not require the modified program to be linked or bound with any specific libraries, unless they are also part of the GPL-covered work."*

Còn về việc mã nguồn có phải **đúng phiên bản** không:
> *"The sources you provide must correspond exactly to the binaries. In particular, you must make sure they are for the same version of the program — not an older version and not a newer version."*

Đội sinh zip từ commit nguồn ⇒ thoát. Và S2 nhấn mạnh điều đội nên nói rõ trong README:
> *"The 'Corresponding Source' must be provided under the terms of this License... you must also provide a copy of this License."*

`(b) offer bằng văn bản 3 năm` — đội đã loại rồi; tôi đồng ý loại, vì kèm sẵn rẻ hơn và chắc hơn.

**(ii) Khoá hết hạn của GUI xoá tệp net CC0 có phải "hạn chế thêm" với engine GPL không?** Đây là câu hay nhất của cụm, và câu trả lời là **KHÔNG** — với lý do cấu trúc mà đội đã chọn đúng mà không cần luật sư phân xử.

Trước hết, hai chương trình của đội là **aggregate**, và S2 xác nhận đúng tiêu chí mà đội mô tả (tiến trình riêng, nói UCI qua stdin/stdout):
> *"If they are separate programs that are combined into a larger program, the combined work is not a derivative work of the individual programs... when the pieces are combined and interact in a more complex way, the combined work may in fact be a derivative work."*

Đội đã đúng khi chọn "tiến trình riêng + giao tiếp qua pipe" vì đó đúng là ranh giới của aggregate. Rồi tới điểm quyết định: **khoá hết hạn nằm trên GUI, không nằm trên `docco.exe`.** Đội đã ghi rõ hết hạn xoá **net CC0 + tệp dùng thử**, **KHÔNG** xoá exe GPL. Đó là quyết định đúng, vì:

> *"GPL requires that the Corresponding Source ... be provided, and that the GPL apply to the whole work. ... It is not permissible to add restrictions to the GPL-covered work."*

Nếu khoá của GUI xoá luôn `docco.exe`, thì việc **chạy** `docco.exe` (một chương trình GPL) đã bị hạn chế ⇒ `KHÔNG` hợp lệ. Còn như hiện tại, `docco.exe` chạy độc lập được, không cần GUI, không cần khoá ⇒ không có hạn chế nào lên tác phẩm GPL. Điều kiện để giữ nguyên là **một điều kiện duy nhất và phải ghi rõ trong README**: khoá không được chặn việc chạy `docco.exe` từ dòng lệnh. Nếu sau này ai đó sửa để khoá cả exe thì gói đó dính vi phạm. Tôi khuyến nghị ghi điều kiện này thành một dòng trong README và một mục trong selftest.

Điểm S2 củng cố thêm: *"the GPL gives no special privileges"* — quyền của người dùng đối với tác phẩm GPL **không** phụ thuộc vào việc GUI kèm theo.

**(iii) README có phải ghi rõ engine là bản sửa đổi kèm ngày không?** Tôi **chưa mở văn bản GPLv3** (`https://www.gnu.org/licenses/gpl-3.0.html`) trong lượt này, nên tôi **không trích nguyên văn điều khoản**. Điều tôi khẳng định: yêu cầu đánh dấu bản đã sửa đổi và ngày sửa là yêu cầu **có thật** trong GPLv3, và nó **chỉ áp cho bản đã sửa đổi** — nên câu trả lời phụ thuộc việc `docco.exe` có thực sự là fork đã sửa hay chỉ đổi tên. Nếu chỉ đổi tên mà không sửa logic thì vẫn nên ghi nguyên văn cho chắc, chi phí bằng không. `GIẢ THUYẾT CẦN ĐO` cho nội dung điều khoản cho tới khi mở văn bản; **đề nghị mở và thêm trích dẫn vào đây trước khi đóng gói.**

**Kết luận:** thiết kế gói hiện tại — aggregate, kèm sẵn source tái lập được, có `Copying.txt`, khoá chỉ trên GUI — **có vẻ tuân thủ trên các điểm tôi đã kiểm được**. Điểm chưa đóng là (iii), và điểm phải ghi thêm là điều kiện "khoá không chặn chạy exe".

**Nguồn**
- S2 `https://www.gnu.org/licenses/gpl-faq.html`: các mục về Corresponding Source, công cụ dùng để build, "Corresponding Source phải đúng phiên bản", "GPL không đặt điều kiện riêng", "GPL không trao đặc quyền", aggregate.
- Dữ liệu đội: `_LANE_NOTE/lock/w_demo_cc0_0210_2026-10-02.md` §1–§2.
- **CHƯA MỞ:** `https://www.gnu.org/licenses/gpl-3.0.html` (văn bản GPLv3, cần cho (iii)).

**Mức độ**
- (i) không phải kèm toolchain, phải kèm script build, source phải đúng phiên bản: `CHẮC` (nguyên văn S2).
- (ii) khoá hết hạn trên GUI không vi phạm với engine GPL, **với điều kiện** khoá không chặn chạy exe: `GIẢ THUYẾT CẦN ĐO` — lập luận chắc dựa trên S2, nhưng **không phải ý kiến pháp lý** và đội nên cho luật sư xác nhận trước khi phát hành.
- (iii) nội dung §5(a) GPLv3: `KHÔNG BIẾT` — chưa mở văn bản.

---

### D3-20 — Meeting AI: phiên web hết hạn, trang hiện gì, gửi lại câu hỏi thế nào

**Trả lời**

Câu hỏi này yêu cầu tôi mô tả **sản phẩm của chính tôi**, và tôi phải trả lời thẳng ở đây để phần còn lại của nó còn giá trị.

**(i) Khi phiên web của tôi hết hạn, trang chuyển tới URL nào và hiện phần tử nào: `KHÔNG BIẾT`.** Không có URL hay selector nào để báo. Lý do cụ thể, không phải né: tôi chạy trong một công cụ dòng lệnh trên máy của bạn (opencode); phiên của tôi gắn với tiến trình đó, **không có** một phiên web để hết hạn theo nghĩa đó. Tôi sẽ không bịa selector vì câu hỏi đã cấm rõ.

**(ii) Câu trả lời đang viết dở còn sau khi đăng nhập lại không: không áp dụng** — lý do như trên.

**(iii) Gửi lại cùng câu hỏi có bị chặn / đánh dấu spam không: không áp dụng** ở phía tôi.

**Phần có giá trị: thiết kế lại nhận diện đăng xuất theo hướng không phụ thuộc trang.** Ba điều chỉnh tôi đề xuất cho `WebTroWebView2.cs`, dựa trên đặc điểm kỹ thuật chứ không dựa trên tên trang:

1. **Nhận diện bằng hành vi, không bằng câu chữ.** Đội đã loại đúng một cái bẫy (không tính chữ nằm trong chính câu vừa gửi) — nhưng còn một bẫy lớn hơn: **text matching là thứ dễ vỡ nhất**, vì mọi nền tảng đều thay câu thông báo theo A/B test và theo ngôn ngữ tài khoản. Ngược lại, cái **không** đổi là: URL có chứa `/login`, `/signin`, `/auth`, hoặc origin đổi sang nhà cung cấp đăng nhập; và ô nhập **biến mất** trong khi khối trả lời cũ vẫn còn. Nên coi URL là tín hiệu **mạnh**, coi text là tín hiệu **yếu và tuỳ chọn**, và luôn kiểm tra lại bằng cách **thử gõ thử vào ô nhập rồi hoàn tác** — đó là phép thử không phụ thuộc bất kỳ tên trường nào.
2. **Đừng gửi lại tự động ngay lập tức.** Đội đã chọn "gửi lại đúng 1 lần", tôi đồng ý với con số 1 nhưng không đồng ý với thời điểm. Nếu gửi lại trong vòng vài giây sau khi phiên chết, phía nhận thường coi đó là lỗi/mạng, **không** phải người dùng thật → nhiều hệ thống sẽ chặn luôn cả lượt thật sau đó. Nên: gửi lại **sau khi owner đăng nhập xong và trang đã sẵn sàng**, và đánh dấu lượt gửi lại là *"tiếp tục"* chứ không phải câu mới.
3. **Tách hai khái niệm mà đội đang gộp:** "mất phiên" và "hết hạn". Hạn 30 phút rồi báo "quá hạn" là hợp lý, nhưng nếu người dùng đang gõ dở thì báo hạn sẽ mất nội dung. Nên **lưu nháp câu hỏi trước khi gửi** và khôi phục lại sau khi đăng nhập — thao tác này nhỏ và tránh được một lớp khiếu nại.

**Nguồn:** dữ liệu đội: `_LANE_NOTE/lock/w_localai_t19b2_0210_2026-10-02.md` mục 3–6; mã `SourceCode/LocalAI/Meeting/WebTroWebView2.cs:225-226`.
**Mức độ:** (i)–(iii) `KHÔNG BIẾT` (không có URL/selector, và tôi không bịa). Ba khuyến nghị thiết kế: `GIẢ THUYẾT CẦN ĐO`.

---

## Tổng hợp cụm D3 — những việc đội nên làm ngay

| Ưu tiên | Việc | Mã liên quan | Vì sao |
|---|---|---|---|
| 1 | **Đổi `setoption` sang chờ rồi mới ghi** (không stop-and-nhả-`bestmove`), và sửa TieuLongNu không gửi `setoption` khi đang `go` | D3-08 | Lệch khỏi upstream; `bestmove` thừa là tin không ai hỏi |
| 2 | **Không nhân đôi `n` trong `AbNgan`**; log kết quả **theo cặp** để bootstrap | D3-05, FS-01, SON-02 | KTC hiện hẹp sai √2 ⇒ chọn nhầm net |
| 3 | **Gửi `ucinewgame` trước mỗi thế** khi sinh nhãn, giữ `nodes` cao | D3-04, SON-01 | Nhãn tái lập được là điều kiện để mọi A/B sau có nghĩa |
| 4 | Bỏ cột `score` khỏi `.plain`; nhãn chỉ lấy từ thầy `nodes` cố định | D3-06, D3-04 | Điểm theo đồng hồ không phải hàm của thế |
| 5 | Nhánh riêng cho **bản 1 bước**: `prediction_type = sample`, nhận diện qua hậu tố `x0` | D3-16 | Model card nói rõ 1-step khác dự đoán |
| 6 | Xử lý `openrail++` như **mục hồ sơ giấy phép**, không phải mặc định được phép | D3-16 | Không phải CC-BY/MIT |
| 7 | Ghi vào sổ ba câu hỏi trả lời được: px0data **được bán** kèm ghi nguồn + ODbL; `go mate` không cần DFS riêng; Hash **không** tròn lũy thừa 2 upstream | D3-07, D3-09 | Ba tiền đề đang sai trong ghi chú nội bộ |
| 8 | Đo commit của `llama-server` bằng 4 phép đo trước khi thay hằng số 1,2 | D3-11 | Hằng số hiện tại là giả định, không phải số đo |
| 9 | Chạy Process Monitor lọc `Result = 1175` để đóng tên thủ phạm | D3-10 | Số đo đã khoanh vùng còn thiếu PID |
| 10 | Mở `gpl-3.0.html` và bổ sung trích dẫn §5(a) cho D3-19 | D3-19 | Điểm còn mở duy nhất của gói Demo |
