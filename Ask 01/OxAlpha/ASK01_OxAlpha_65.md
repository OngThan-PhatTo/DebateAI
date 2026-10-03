# ASK 01 — bộ hỏi cho OxAlpha — tệp 65/69 (5 câu)

**Bối cảnh ngắn:** đội cờ tướng của HaTrungTin (engine OngThan/Docco/Bodetosu/Nữ Oa/NhuLai, GUI TieuLongNu, BookTool, web Kỳ Viện, trainer NNUE, app Local AI có Meeting AI). Máy đích train: 192 lõi, 128–256 GB RAM, KHÔNG NVIDIA; máy sinh game: 44 lõi vật lý, 128 GB, không GPU; máy owner: AMD iGPU 8060S chạy ROCm trên Windows (torch 2.12.0a0+rocm7.13, cuda=True), 47,6 GB RAM. Bộ đầy đủ ở `ASK01_TONG_HOP_2026-10-01.md` — tệp này chỉ lấy 5 câu để OxAlpha không quên ngữ cảnh. Bản công khai (chỉ câu hỏi, không mã nguồn): https://github.com/OngThan-PhatTo/DebateAI — thư mục `Ask 01/` (tệp đầy đủ + `OxAlpha/`); đọc thẳng: https://raw.githubusercontent.com/OngThan-PhatTo/DebateAI/main/Ask%2001/ASK01_TONG_HOP_2026-10-01.md

**Luật trả lời:** trả theo đúng mã câu; dẫn nguồn thật (đường dẫn + dòng hoặc URL); không chắc thì nói «không chắc» + cách kiểm; 🔴 **CẤM BỊA** (đội kiểm bằng code, bịa làm mất thời gian); trả lời xong 5 câu thì đợi tệp kế; **lưu bài dưới tên `OxAlpha_<nội dung>_<YYYYMMDD_HHMM>.md`** (tránh trùng tên).

---

## Câu 321 (D3-10)

### D3-10 — `File.Replace` (`ReplaceFile`) lỗi 1175 (0x80070497) chập chờn khi tệp nằm trong cây làm việc: gốc là gì, ghi nguyên tử thế nào cho đúng trên Windows?
1. **Bối cảnh:** TieuLongNu ghi cấu hình kiểu `WriteAllLines(.tmp)` → `File.Replace(tmp, dich, null)` (`SourceCode/AppCo_TieuLongNu/TieuLongNu.App/KhungCongCu.cs:820-822`). Thăm dò 02/10 ~22:00 (sổ `_LANE_NOTE/lock/w_tln_post13_0210_2026-10-02.md` §5, mã `LaneScratch/w_tln_post13_0210/r4/do_replace.cs`, tệp mới trong thư mục GUID mới, lặp cố định): thư mục TẠM nằm trong cây repo ⇒ **7/2000** rồi **9/4000** lần `IOException 0x80070497 «Unable to remove the file to be replaced»`; `%LOCALAPPDATA%/Temp` **0/4000**; thư mục ngoài repo **0/4000**. Cùng họ: ComfyUI_Pro `KhoHoSoKenh` (`File.Replace` ném 1/3 lượt, sổ `_LANE_NOTE/lock/w_comfy_phude_0210_2026-10-02.md` §3), BookTool Engine Fight `EngineFightNapNet.cs:439,534`, `GhiManifest` (T-28).
2. **Đội đã chọn:** hàm chung `NhatKy.ThayTepNguyenTu(tmp, dich)` = Replace/Move + thử lại CÓ TRẦN (6 lần, chờ 10 → 160 ms, tổng ≤ ~0,3 s) CHỈ với mã 32 / 33 / 1175 / 1176; hết trần vẫn NÉM (bên gọi báo lỗi như cũ, tệp bị giữ HẲN vẫn là lỗi). Sau vá: cổng `--autonut-selftest` 30/30 lượt xanh (bản cũ cùng thư mục: 1/11 lượt đỏ).
3. **Phương án khác:** (a) `MoveFileEx(MOVEFILE_REPLACE_EXISTING | MOVEFILE_WRITE_THROUGH)` thay `ReplaceFile`; (b) `SetFileInformationByHandle(FileRenameInfoEx, FILE_RENAME_FLAG_POSIX_SEMANTICS)`; (c) loại thư mục khỏi Defender / Windows Search / IDE watcher (không kiểm soát được trên máy khách).
4. **Câu hỏi:** (i) Tiến trình nào thường giữ handle vài ms làm `ReplaceFile` trả 1175 (Defender real-time, Windows Search indexer, trình theo dõi tệp của IDE, Google Drive / OneDrive)? Process Monitor lọc thế nào để xác nhận? (ii) Cách ghi «nguyên tử + bền» được khuyến nghị trên NTFS hiện nay (ReplaceFile vs MoveFileEx vs rename POSIX), cách nào ít nhạy với handle mở có `FILE_SHARE_DELETE` của bên thứ ba? (iii) Số lần / khoảng thử lại như đội có hợp lý?
5. **Trả lời tốt:** tài liệu Microsoft Learn (URL) + nguồn uy tín (Raymond Chen, Windows Internals); số đo nếu có.

---

## Câu 322 (D3-11)

### D3-11 — llama.cpp trên Windows: phần nào của model tính vào COMMIT CHARGE (GGUF mmap, KV cache, offload Vulkan / ROCm trên iGPU UMA)?
1. **Bối cảnh:** sự cố 02/10 14:15:59: máy đội sập vì cạn commit (52,2 / 53,4 GB) trong khi RAM vật lý còn trống. Local AI bật server llama riêng (cổng 81xx) cho model GGUF người dùng tải. Máy owner: AMD Ryzen AI MAX+ 395, iGPU 8060S (UMA), 47,6 GB RAM, pagefile ~4,3 GB ⇒ commit limit ~51,9 GB. Commit đo bằng `K32GetPerformanceInfo` (CommitTotal / CommitLimit × PageSize).
2. **Đội đã chọn** (`SourceCode/LocalAI/TaiModel/MayChuModel.cs` bản chép T-38c; sổ `_LANE_NOTE/lock/w_localai_t38c_0210_2026-10-02.md` mục 6): tính TRỌN «RAM ước» = cỡ tệp × 1,2 + 1,5 GiB vào commit trước khi bật; trần 75 % commit limit; không đọc được commit ⇒ KHÔNG bật — bảo thủ.
3. **Phương án khác:** (a) chỉ tính KV cache + bộ đệm (vì GGUF mmap là trang ánh xạ tệp, có thể không tính commit); (b) tính cả trọng số khi `--no-mmap` / `--mlock` hoặc khi offload lên GPU UMA.
4. **Câu hỏi:** trên Windows 11 với llama.cpp (llama-server) bản mới: (i) trang GGUF ánh xạ chỉ-đọc (`CreateFileMapping` trên tệp) có tính vào commit charge không? (ii) Khi offload lên iGPU UMA qua Vulkan (hoặc HIP / ROCm), bộ nhớ «VRAM» chia sẻ được cấp phát thế nào — có tính commit / «Shared GPU memory» không? (iii) Có công thức ước commit theo (cỡ tệp, `n_ctx`, `n_gpu_layers`, kiểu KV) nào được công bố?
5. **Trả lời tốt:** dẫn mã llama.cpp (tệp mmap, backend ggml-vulkan / ggml-hip) + tài liệu Microsoft về commit charge của section / heap; số đo nếu có.

---

## Câu 323 (D3-12)

### D3-12 — Clone giọng tiếng Việt: F5-TTS ViVoice là CC-BY-NC-SA-4.0 — mô hình nào cho phép THƯƠNG MẠI; các thông số mặc định của đội có khớp model card không?
1. **Bối cảnh:** T-49 (sổ `_LANE_NOTE/lock/w_comfy_clonegiong_0210_2026-10-02.md` §1–§2): adapter `NguonF5Tts` trong `SourceCode/Shared/GiongPhuDe/F5TtsEngine.cs`; model `hynt/F5-TTS-Vietnamese-ViVoice` `model_last.pt` 5.394.362.124 B, vocab 2.566 dòng, giấy phép theo API HF **CC-BY-NC-SA-4.0** (`SourceCode/ComfyUI_Pro/ComfyUIPro/Assets/kho/goi_toi_thieu_F.json:291-299`) ⇒ giao diện gắn nhãn «THỬ NGHIỆM · CC-BY-NC-SA · không thương mại». Chưa chạy thật (môi trường python thiếu gói `f5_tts`; vocoder `charactr/vocos-mel-24khz` chưa có trên máy).
2. **Đội đã chọn:** kiến trúc `F5TTS_Base`; mẫu người dùng 10–30 s, đoạn LƯU ≤ 12 s cắt đúng biên câu whisper (F5 tự cắt mẫu > 12 s mà giữ nguyên chữ ⇒ lệch lời); một giọng clone cho cả lượt, chỉ áp tiếng Việt; KHÔNG chuẩn hoá chữ tiếng Việt (số, viết tắt) trước khi đọc.
3. **Phương án khác:** (a) xin giấy phép thương mại từ tác giả ViVoice / chủ bộ dữ liệu gốc; (b) mô hình khác (VieNeu-TTS, XTTS-v2, F5-TTS gốc, Piper tiếng Việt không clone…) — giấy phép từng cái đội chưa kiểm; (c) app không kèm model, người dùng tự tải và tự chịu giấy phép.
4. **Câu hỏi:** (i) Mô hình TTS / clone giọng tiếng Việt nào hiện có TRỌNG SỐ giấy phép cho phép thương mại (Apache / MIT / CC-BY), chất lượng (MOS / WER) có nguồn? (ii) Phương án (c) có thật sự tách trách nhiệm khi app bán có nút «tải model» không? (iii) Kiến trúc, độ dài mẫu tham chiếu, chuẩn hoá chữ của đội có khớp model card ViVoice không?
5. **Trả lời tốt:** URL model card + dòng giấy phép nguyên văn; số MOS / WER có nguồn; ô nào không chắc ghi «không biết».

---

## Câu 324 (D3-13)

### D3-13 — Lồng tiếng: Demucs 2 bè giữ nhạc nền, ngưỡng «còn sót giọng» 0,3, chia whisper theo khoảng lặng ≤ 60 s — có cách chuẩn hơn?
1. **Bối cảnh:** T-45 / T-46 (sổ `_LANE_NOTE/lock/w_comfy_longtieng_0210_2026-10-02.md` §1–§2): trước vá trộn `[0:a]volume=0.15` ⇒ giọng gốc còn nằm dưới giọng lồng (`SourceCode/ComfyUI_Pro/ComfyUIPro/Core/PhimSubVoice.cs:363-364` bản cũ); whisper có hạn cố định 120 s ⇒ video dài ra 0 sub (`SourceCode/Shared/GiongPhuDe/WhisperRunner.cs:114`). Demucs dùng model `htdemucs` (`SourceCode/ComfyUI_Pro/ComfyUIPro/Core/LamNhac.cs:112-157`).
2. **Đội đã chọn:** Demucs `--two-stems vocals` (htdemucs), BẬT mặc định khi lồng tiếng; kiểm bè nền bằng hệ số chiếu `Σ nền·giọng / Σ giọng²` trong khe thoại: < 0,3 ⇒ dùng, ≥ 0,3 ⇒ lùi khử kênh ffmpeg `pan c0-c1` + báo; chia wav bằng ffmpeg `silencedetect=noise=-35dB:d=0.35`, đoạn 15–60 s, không có khoảng lặng ⇒ cắt cứng 60 s, chỉ chia khi > 75 s; hạn whisper mỗi đoạn = 60 + 4 × số giây (kẹp 120…1800 s).
3. **Phương án khác:** (a) `htdemucs_ft` / MDX-Net (UVR) cho bè sạch hơn; (b) VAD có sẵn của whisper.cpp (Silero) thay `silencedetect`; (c) đo rò giọng bằng cách cho whisper đọc bè nền (ra chữ ⇒ còn giọng) hoặc SI-SDR ước lượng.
4. **Câu hỏi:** (i) Model tách nào cho bè «nhạc + hiệu ứng, không giọng» tốt nhất hiện nay (bảng MUSDB18 / MVSEP có nguồn), chạy được CPU hoặc ROCm trên Windows? (ii) Thước đo rò giọng nào đáng tin hơn hệ số chiếu, ngưỡng gợi ý? (iii) whisper.cpp bản mới có VAD tích hợp — có nên thay chia theo `silencedetect`, ảnh hưởng gì tới mốc thời gian sub?
5. **Trả lời tốt:** URL bảng xếp hạng / README chính thức + phiên bản; cờ dòng lệnh chính xác.

---

## Câu 325 (D3-14)

### D3-14 — Phân giới người nói theo F0 (vùng mơ hồ 165–180 Hz) để chọn giọng lồng và xưng hô tiếng Việt — có cách tốt hơn chạy offline?
1. **Bối cảnh:** `SourceCode/Shared/GiongPhuDe/GenderDetector.cs:7-36`: F0 < 165 Hz ⇒ nam, > 180 Hz ⇒ nữ, khoảng giữa ⇒ giữ kết quả trước (hysteresis). T-47 đo cửa sổ 0,5 s trong khe mỗi câu, bỏ phiếu, lưu `<srt>.gioi.json`. Trước vá, dịch sang tiếng Việt ép xưng hô giọng nữ cho MỌI câu (`SourceCode/Shared/GiongPhuDe/TranslationService.cs:80-84,287-288`, `NguoiNoiLaNu = true`).
2. **Đội đã chọn:** câu không đo được giới (mơ hồ / im lặng) ⇒ giọng «bất kỳ» + KHÔNG đổi xưng hô (giữ bản dịch trung tính); mỗi tiếng đích chọn «theo người nói (F0) / luôn Nữ / luôn Nam / bất kỳ».
3. **Phương án khác:** (a) diarization (pyannote) + phân loại giới trên embedding (ECAPA / wav2vec2); (b) LLM đoán giới từ ngữ cảnh lời thoại; (c) người dùng gán giới cho từng người nói.
4. **Câu hỏi:** model phân loại giới / diarization nào chạy offline trên Windows CPU (ONNX), độ chính xác công bố trên giọng châu Á, giấy phép cho thương mại? Ngưỡng F0 165 / 180 Hz có nguồn ngữ âm học không (giọng trẻ em, người già)?
5. **Trả lời tốt:** tên model + URL + giấy phép + số đo; nguồn cho ngưỡng F0.
