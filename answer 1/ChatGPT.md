# CHATGPT — ANSWER 01

> Nguồn: owner dán vào phiên Fable 27/09/2026 07:2x (trả lời `ask 1/ASK_2026-09-26.md` trên DebateAI). Lưu nguyên văn, không sửa. Bản chép cùng nội dung tại DebateAI `answer 1/CHATGPT_2026-09-27.md`. Chấm: `answer 1/_CHAM.md`.

## Debate cờ tướng HaTrungTin — 26/09/2026

> **Phạm vi:** Tôi đã đọc `DEBATE.md` và `ask 1/ASK_2026-09-26.md`. Repo công khai không chứa source đầy đủ; vì vậy các nhận xét yêu cầu kiểm tra code nội bộ mà không có đoạn code trong Ask được đánh dấu **KHÔNG BIẾT** hoặc **GIẢ THUYẾT CẦN ĐO**, không tự tạo `file:dòng`.

---

# 0. KẾT LUẬN QUAN TRỌNG NHẤT

Nếu phải chọn những việc có giá trị nhất trước mốc train tháng 10, tôi xếp:

1. **Sửa pipeline nhãn DTM/DTZ/DTC và đơn vị ply/move trước khi train.**
2. **Không nhét DTM mate vào thang cp thông thường kiểu `30000-N` rồi sigmoid trực tiếp.**
3. **Tách WDL theo luật khỏi khoảng cách mate.**
4. **Tàn cuộc phải có sampling theo cấu hình + tần suất thực chiến; không được để vài ô nhỏ hoặc một kiểu quân áp đảo toàn bộ dataset.**
5. **EGTB nên là oracle/search accelerator và kiểm tra nhãn; không nên kỳ vọng EGTB tự nó mang lại một lượng Elo lớn trong middlegame.**
6. **AUTO GUI phải có một state machine duy nhất; mọi đường vào đều phải đi qua state transition, mọi đường gửi nước ra ngoài phải qua một `CanSendMove()` duy nhất.**
7. **SQLite lớn: một writer duy nhất + batch commit + progress/idempotency; WAL có lợi cho đọc đồng thời nhưng không phải thuốc chữa mọi vấn đề.**
8. **Dữ liệu phải có provenance ngay từ đầu: source file hash, engine, version, net hash, config, depth/nodes, timestamp, license/provenance.**
9. **Không train lô lớn chỉ vì metric nội bộ đẹp. Phải có negative control + bit-exact positive control + A/B engine.**

Stockfish hiện cũng mô tả WDL-space như một không gian riêng và chuyển cp→WDL bằng sigmoid; tài liệu NNUE cảnh báo việc tối ưu trực tiếp CP có thể gây gradient lớn và hạn chế loss. Vì vậy ý tưởng dùng một thang CP nhân tạo rất lớn cho mate cần đặc biệt thận trọng.

---

# A3-01 — NHÃN CHIẾU BÍ: CÓ NÊN ĐỔI MATE THÀNH CP?

## Trả lời

**Không nên dùng trực tiếp:**

`cp = sign × max(1500, 3000 - 10 × dtm_ply)`

làm target duy nhất.

Tôi đồng ý với việc giữ thông tin DTM, nhưng nên tách thành:

* `wdl_rule`
* `dtm`
* `dtc` nếu cần
* `mate_distance_target` riêng hoặc weight riêng

Thay vì bắt một scalar CP làm cả ba nhiệm vụ.

### Khuyến nghị

Đối với position thắng:

```text
WDL = WIN
DTM = số ply tới mate theo oracle
DTC = số ply tới capture/reset hoặc mate
```

Net value-only có thể vẫn xuất scalar value, nhưng target huấn luyện nên có dạng:

```text
L = λ_value * L_value
  + λ_wdl   * L_wdl
  + λ_dtm   * L_distance
```

Nếu net hiện tại chỉ cho một output thì có thể chưa cần sửa kiến trúc ngay. Có thể encode distance thành **sample weight** hoặc auxiliary target trong trainer.

## Quan trọng

`30000 - N` không nên được coi là CP thực.

Đây chỉ nên là **mate score encoding của search**, không phải giá trị vật lý của evaluation.

Stockfish NNUE dùng sigmoid để đưa evaluation vào WDL-space; scaling factor phụ thuộc engine/dataset.

### Về lỗi `mate N` = move hay ply

Đây là lỗi cần ưu tiên sửa.

Nếu một nguồn định nghĩa:

```text
mate 10 = 10 moves
```

và nguồn kia:

```text
DTM = 10 ply
```

thì ghép thẳng sẽ tạo sai x2.

**Mức tin: CHẮC** về nguyên tắc.

**Mức tin về đúng dòng `gen_thay_that.py:196`: KHÔNG BIẾT**, vì source đó không có trong repo công khai.

### Ca test

Tạo 1 position có:

```text
DTM = 2 ply
DTM = 4 ply
DTM = 8 ply
DTM = 16 ply
```

Sau đó assert:

```text
distance(2) < distance(4) < distance(8) < distance(16)
```

và kiểm tra không có conversion nào biến 2 ply thành 2 moves ở một pipeline nhưng thành 1 move ở pipeline khác.

Thêm:

```text
Spearman(net_output, -DTM)
```

trên tập **chỉ các position WIN đã oracle-certified**.

### Mức tin

**CHẮC** — kiến trúc target/loss.

**KHÔNG BIẾT** — dòng code nội bộ chưa công khai.

---

# A3-02 — DOC CO: SIGMOID BÃO HÒA + "NƯỚC TỐT NHẤT LÀ ĂN QUÂN"

## (a) Có nên lọc nước ăn quân ở tàn cuộc?

**Không.**

Đặc biệt trong endgame, capture/exchange thường chính là nước quyết định.

Nếu pipeline loại:

```text
best move = capture
```

thì nó có thể loại chính xác những transition quan trọng nhất của tablebase.

Đúng hơn là phân loại:

```text
quiet position
tactical position
capture/reset position
mate position
```

chứ không phải:

```text
capture = bad sample
```

## (b) `in_scaling=1000` nhưng mate ~29900

Không đưa 29900 trực tiếp vào sigmoid CP-space.

Nếu:

```text
sigmoid(29900 / 1000)
```

thì gần như saturation hoàn toàn.

Do đó:

* WDL target: `0 / 0.5 / 1`
* distance target: riêng
* evaluation target: clip/transform hợp lý

Đây cũng phù hợp với cách Stockfish tách evaluation-space và WDL-space.

### Mức tin

**CHẮC.**

---

# A3-03 — VALUE-ONLY NET VÀ NHÃN TỪNG NƯỚC

## (a)

Với value-only network:

**hãy lưu nhãn cho các thế con**, không cần biến network thành policy network ngay.

Ví dụ:

```text
Position P
  move A -> P1 -> value V1
  move B -> P2 -> value V2
  move C -> P3 -> value V3
```

Search dùng:

```text
choose argmax(V(child))
```

Đối với bên thắng.

Có thể bổ sung ranking loss:

```text
L_rank = max(0, margin - (V_good - V_bad))
```

nhưng tôi **không khuyên làm ngay**.

Trước tiên hãy có dataset:

```text
parent
move
child
oracle_wdl
oracle_dtm
oracle_dtc
rule_result
```

sau đó mới thử ranking.

## (b) WDL và "cursed win/blessed loss"

Nguyên tắc rất quan trọng:

**DTM result và game-rule result không phải cùng một nhãn.**

Ví dụ:

```text
theoretically WIN
nhưng không thể chuyển hóa trước 80 ply
=> DRAW theo luật của dataset
```

Phải giữ cả hai:

```text
theoretical_wdl
rule_wdl
```

Không nên ghi đè một cái lên cái kia.

Tôi đề xuất:

```text
TB_WDL
RULE_WDL_60
RULE_WDL_80
DTM
DTC
```

Sau đó trainer chọn target theo cấu hình.

## (c) Dùng DTZ/DTC làm weight?

Có tiền lệ trong các hệ thống tablebase/chess-engine, nhưng tôi **không có bằng chứng công khai đủ chắc rằng trainer NNUE của Stockfish hiện dùng chính DTC cờ tướng theo cách đội đang định nghĩa**.

Vì vậy:

**KHÔNG BIẾT** đối với câu "trainer NNUE công khai nào dùng đúng DTC này".

Không nên ghi "Stockfish làm như vậy" nếu chưa có source cụ thể.

---

# A3-04 — SAMPLING 0v0 → 4v4

Đây là một trong những câu quan trọng nhất.

## Không dùng một tiêu chí duy nhất.

Không nên:

```text
quota = đều tuyệt đối
```

và cũng không nên:

```text
quota = đúng tần suất ván thật
```

vì hai cực đều gây bias.

Tôi đề xuất:

### Tầng 1 — exhaustive

Các ô nhỏ:

```text
krk
kpk
...
```

nếu đủ nhỏ thì lấy toàn bộ.

### Tầng 2 — stratified

Các ô lớn chia theo:

```text
material configuration
side to move
WDL
DTM bucket
DTC bucket
symmetry
```

### Tầng 3 — real-game replay

Bổ sung position thực chiến từ kho ván.

### Tầng 4 — hard positions

Oversample:

```text
near-draw
near-conversion
DTM close
rule-boundary
```

## Quota đề xuất ban đầu

Đây là **GIẢ THUYẾT CẦN ĐO**, không phải con số đã được chứng minh:

```text
50% synthetic stratified
30% real-game endgame positions
20% hard/boundary positions
```

Sau đó làm ablation:

```text
100/0/0
70/20/10
50/30/20
30/50/20
```

và đo A/B.

## Rủi ro lớn nhất

Không phải "thiếu vài thế".

Rủi ro lớn nhất là:

> 10 triệu endgame positions trở thành distribution shift khiến network học tàn cuộc quá mạnh và hy sinh middlegame.

Đội đã từng có lô -301 Elo do dữ liệu, nên đây phải là **hard gate**, không phải "train xong rồi xem".

### Gate

Sau mỗi 5–10% training:

```text
endgame validation
middlegame validation
full validation
A/B short
```

Nếu endgame metric tăng nhưng middlegame giảm đáng kể:

```text
STOP
```

---

# A3-05 — MDP / EGTB / DTC

## (a) `dtc > remain`

Không nên đơn giản vứt bỏ thông tin tablebase.

Nên chuyển nó thành:

```text
TB_WIN_THEORETICAL
+
RULE_RESULT = DRAW
```

nếu luật thực tế khiến không thể chuyển hóa.

Tức là tablebase không sai; **rule layer** quyết định kết quả cuối.

Stockfish tablebase documentation cũng nhấn mạnh tablebase move selection có tính tới 50-move rule.

## (b) TT + mate score

Đây là vùng có rủi ro cao.

Các nguyên tắc:

```text
mate scores phải normalize theo ply khi lưu TT
```

Nếu không:

```text
same position at different ply
```

có thể được hiểu thành cùng mate distance dù khoảng cách thực tế khác.

Đồng thời rule-history-dependent states không thể chỉ dựa vào board hash.

Nếu kết quả phụ thuộc:

```text
repetition history
rule counter
```

thì state key phải đại diện đủ cho những dữ kiện ảnh hưởng legality/result.

### Mức tin

**CHẮC về nguyên tắc.**

**KHÔNG BIẾT** việc implementation hiện tại có sai hay không vì đoạn code không công khai.

## (c) Felicity có xử lý đầy đủ perpetual check/chase?

Tôi **KHÔNG BIẾT** nếu chưa đọc source Felicity cụ thể.

Không nên tuyên bố "đủ".

---

# A3-06 — GẮN EGTB CHO DOC/ONGTHAN

Tôi sẽ **không khuyên sửa trực tiếp search trước**.

Thiết kế an toàn:

```text
Search
  |
  +-- TB probe OFF -> current behavior
  |
  +-- TB probe ON
          |
          +-- TB miss -> current search
          |
          +-- TB hit -> TB result
```

Điểm hook tốt nhất về mặt kiến trúc là **trước khi search mở rộng node**, sau khi position đã hợp lệ và rule-state cần thiết đã sẵn sàng.

Không đưa TB lookup vào evaluator.

### Bắt buộc

`TB OFF` phải:

```text
bench == old bench
```

hoặc ít nhất bit-exact trên cùng binary/config nếu thay đổi chỉ nằm sau feature gate.

## EGTB có +Elo lớn không?

Không nên kỳ vọng.

Stockfish documentation nói rõ tablebase mang lại **limited increase in general playing strength**, dù trong các position nằm trong tablebase nó có thể chọn chính xác các nước giữ win/draw theo rule.

Vì vậy:

```text
EGTB = correctness + endgame strength
```

hơn là:

```text
EGTB = guaranteed huge Elo
```

---

# A3-07 — 80 PLY + HASH KEY

## (a)

Không nên suy luận rằng:

```text
120 -> 80
```

mà giữ nguyên mọi bucket/hash compression là an toàn.

Nếu hash chỉ mã hóa board + coarse rule counter thì hai position:

```text
remain = 2
remain = 70
```

không được để TT coi như cùng state nếu rule outcome phụ thuộc counter.

Đề xuất:

```text
PositionKey
+
RuleCounterBucket
```

hoặc tốt hơn:

```text
full rule state
```

nếu chi phí hash chấp nhận được.

## (b) Thắng phải chuyển hóa; thua phải kéo dài

Không nên chỉ trừ eval tuyến tính theo counter.

Nên để **search terminal/rule logic** quyết định legality/result, còn eval bias chỉ là preference.

Tức:

```text
rule correctness
    >
search preference
    >
evaluation bias
```

Không đảo ngược.

Về câu hỏi còn hằng số 120 ở những dòng khác: **KHÔNG BIẾT** vì tôi không có source đầy đủ.

Ca grep nên tự động tìm:

```text
120
```

nhưng không chỉ literal `120`; phải tìm cả:

```text
MAX_RULE
RULE_LIMIT
PLY_LIMIT
/8
/10
```

và các conversion tương đương.

---

# A3-08 — CỜ ÚP: PATCH ĐÚNG LUẬT NHƯNG -33 ELO

Đây là trường hợp điển hình:

> **correctness patch ≠ strength patch**

Không được rollback một patch luật chỉ vì A/B Elo âm.

Phải tách:

```text
correctness
performance
evaluation calibration
```

## Thứ tôi nghi nhất

### 1. `materialKey` thay đổi

Nếu:

```text
materialKey
```

trước đây vô tình đại diện cho một distribution mà NNUE/eval đã tune, sau patch key đổi:

```text
same real position
=> different material bucket
```

thì eval có thể đổi mạnh dù code đúng.

### 2. PSQ cập nhật khi flip

Nếu PSQ của hidden/revealed piece được cập nhật hai lần:

```text
remove hidden
add revealed
```

nhưng một path đã làm một trong hai bước rồi, eval bị lệch.

### 3. TT key

Nếu key thay đổi làm cache hit pattern khác:

```text
same nodes
different TT behavior
```

Elo có thể thay đổi mà không có bug legality.

## Test ≤30 phút

Chạy 4 build:

```text
A = old
B = B2b only
C = B1m only
D = B2b+B1m
```

Cùng:

```text
positions
nodes
threads
seed
```

So:

```text
root score
bestmove
PV
eval components
TT hit %
```

Nếu:

```text
B != A
```

thì B2b gây thay đổi.

Nếu:

```text
C == A
```

thì B1m không ảnh hưởng search, đúng như mô tả.

**Đây quan trọng hơn chạy thêm 1.000 ván.**

---

# A3-09 — FELICITY LEGAL POSITIONS KHÔNG KHỚP

Tôi **không có source FelicityEgtb đủ để kết luận cách họ định nghĩa `Legal positions`**.

Do đó:

**KHÔNG BIẾT** câu (a).

Nhưng có thể kết luận một việc:

Nếu:

```text
Max-DTM 40/40 khớp
```

thì engine/listing không thể bị coi là "toàn bộ sai".

Sai khác nằm trong:

```text
legal-state definition
draw/repetition/perpetual rules
index universe
side-to-move convention
symmetry convention
```

Đây là nơi cần làm một **position-by-position diff**.

Đừng đo tỷ lệ aggregate trước.

Lấy:

```text
Felicity legal = true
internal legal = false
```

và ngược lại.

Sau đó phân loại nguyên nhân.

---

# A3-10 — `W-M-nnnn` CHESSDB

Về chính xác:

```text
M-nnnn
rank 2/1/0
```

tôi **KHÔNG BIẾT** nếu không có tài liệu chính thức của chessdb xác nhận.

Không nên dùng lời truyền miệng trên forum làm specification.

Cũng không xác nhận được limit:

```text
100,000 requests/IP/day
```

nếu trang terms hiện không còn.

**Kết luận: KHÔNG BIẾT.**

Nếu hệ thống phụ thuộc vào quota thì phải code:

```text
rate limit configurable
retry-after
backoff
local cache
daily counter
```

và coi quota bên ngoài là configurable data, không hard-code như một sự thật vĩnh viễn.

---

# A3-11 — HỌC TỪ ENGINE BẢN QUYỀN

Kiến trúc trọng tài của đội là hợp lý nhưng có một lỗ hổng:

> "engine báo mate ⇒ tin" vẫn chưa đủ.

Cần phân biệt:

```text
engine says mate
```

với:

```text
oracle independently proves mate
```

Tôi đề xuất provenance:

```text
label_source = commercial_engine_X
label_type = MATE
engine_version
engine_config
depth
nodes
score
mate_distance
oracle_status
license_class
```

## Dè dặt

Không nên biến uncertainty thành một CP tùy tiện.

Tôi chọn:

```text
oracle-certified mate:
    weight = 1.0

one-engine mate:
    weight = 0.5

two-engine agreement:
    weight = 0.75

engine disagreement:
    weight = 0.1 hoặc reject

non-mate commercial evaluation:
    soft label / low weight
```

Các con số trên là:

**GIẢ THUYẾT CẦN ĐO.**

Cách tốt hơn là học weight từ validation.

## UCCI mate N = move hay ply?

**Không được giả định đồng nhất giữa mọi engine.**

Phải có adapter:

```text
EngineProtocolAdapter
    -> NormalizeMateDistanceToPly()
```

và mỗi engine có test vector riêng.

---

# A3-12 — AUTO: MỘT NGUỒN TRẠNG THÁI

Đây là phần tôi đánh giá **thiết kế hiện tại đã đi đúng hướng**.

Đội đã phát hiện N1–N4 và đưa chúng về đường chung.

Nhưng thiết kế mạnh hơn nữa là:

```text
AutoState
{
    Stopped,
    Analyze,
    Full
}
```

và tuyệt đối không cho code khác tự sửa:

```text
ChiXem
AutoDo
AutoDen
_chay
```

## Chỉ được:

```text
RequestStartAnalyze()
RequestStartFull()
RequestStop()
```

Sau đó state machine quyết định.

Đặc biệt:

```text
SendMoveOutside()
```

phải assert:

```text
state == Full
```

### Nếu state != Full

```text
DO NOT SEND
```

Đây là defense-in-depth.

## Persistent default

Tôi khuyên:

```text
default = Analyze / ChiXem
```

không phải Full.

Người dùng cũ có config Full vẫn được giữ, nhưng migration version phải rõ:

```text
configVersion < X
=> migrate
```

Không âm thầm thay hành vi người dùng.

## Cổng kiểm

Không chỉ grep.

Dùng hai tầng:

### Static

Search:

```text
ChiXem =
AutoDo =
AutoDen =
_chay =
SendMove
PostMessage
```

### Dynamic

Test matrix:

```text
entry × state × expected side effect
```

Ví dụ:

```text
F9 Analyze -> no external move
F9 Full    -> external move allowed
F10        -> same
menu       -> same
1-click    -> same
2-click    -> same
STOP       -> no external move
```

---

# A3-13 — STOP + DISPATCHER RACE

Tôi **không thể xác nhận còn race cụ thể ở `TuChoi.cs` vì source không công khai**.

Nhưng có một lỗi kiến trúc rất đáng kiểm:

```text
Dispatcher.BeginInvoke(action)
```

không tự biến action thành "vô hại" sau STOP.

Ví dụ:

```text
t1 worker:
    BeginInvoke(UpdateBoard)

t2 UI:
    STOP
    _phien++

t3 dispatcher:
    UpdateBoard()
```

Nếu callback không kiểm generation/session:

```text
old callback
=> modifies new session
```

## Cách sửa

Mọi queued callback phải mang:

```text
sessionId
```

và đầu callback:

```text
if (sessionId != currentSessionId)
    return;
```

Tốt hơn nữa:

```text
CancellationToken
+
session generation
```

`CancellationToken` để dừng công việc; generation để chống callback đã xếp hàng nhưng chưa chạy.

---

# A3-14 — NHẬN BÀN + BẤM NƯỚC

Đây là nơi tôi khuyên đội thay đổi quan trọng:

**Không coi "đã click" = "nước đã được nhận".**

Pipeline nên là:

```text
READ BOARD A
    ↓
ENGINE MOVE
    ↓
CLICK
    ↓
WAIT
    ↓
READ BOARD B
    ↓
VERIFY expected transition
    ↓
ONLY THEN commit move
```

Ví dụ nếu engine định:

```text
e2 -> e4
```

thì sau click phải xác nhận:

```text
old[e2] = piece
new[e2] = empty
new[e4] = expected piece
```

hoặc transition tương đương.

Nếu fail:

```text
retry / re-read / stop
```

không được gửi nước tiếp theo.

## 30 ms

Không có cơ sở để nói:

```text
30 ms universally safe
```

Nó phụ thuộc client/animation/network.

Ngân sách 150 ms/nước cũng không nên là correctness criterion.

Correctness phải là:

```text
ACK by observed board transition
```

latency chỉ là performance metric.

## Android

`AccessibilityService` có API chụp màn hình từ API 30, và Android yêu cầu service khai báo capability tương ứng. API cũng hỗ trợ `takeScreenshotOfWindow`, hữu ích khi overlay có thể che màn hình.

Do đó Android mới không đồng nghĩa "không thể screenshot", nhưng phải thiết kế theo capability/version thay vì giả định mọi thiết bị giống nhau.

---

# A3-15 — HAI BỘ ĐỌC KÝ PHÁP CÙNG SAI

Đây là một trong những rủi ro nghiêm trọng nhất.

Hai importer độc lập **không chứng minh độc lập** nếu cả hai cùng dựa trên một giả định sai.

Các ca cần đặc biệt:

```text
前/后/中
一/二/三...
進/退/平
車/车
馬/马
砲/炮/包
帥/帅
兵/卒
full-width digits
```

Đặc biệt:

```text
进 5
退 3
平 4
```

không được hiểu như "đi 5 bước".

Đây là notation semantic, không phải coordinate notation.

## Bộ 50 test

Tôi chia:

```text
10 basic
10 ambiguity
10 same-file multiple pieces
10 Chinese/Arabic/full-width variants
10 capture/check/mate/endgame
```

Mỗi test phải có:

```text
source notation
expected ICCS move
expected board hash after move
expected final board hash
```

Hai parser cùng pass string không đủ.

Phải pass:

```text
position_before
move
position_after
```

---

# K1 — ĐĨA PHÌNH 500 GB

Giải pháp chính:

**không tạo bản copy thứ hai chỉ để kiểm.**

Pipeline:

```text
SOURCE
  ↓
stream reader
  ↓
normalize
  ↓
hash + count + validate
  ↓
batch INSERT
  ↓
commit
```

Mỗi record có:

```text
source_file_hash
source_record_no
```

Như vậy có thể audit mà không cần giữ thêm bản sao.

Backup SQLite lớn nên dùng SQLite Online Backup API hoặc cơ chế snapshot phù hợp thay vì copy file sống bằng thao tác file thông thường; SQLite mô tả Online Backup API chính xác cho việc này.

---

# K2 — MERGE SQLITE 30 GB

Tôi khuyên:

```text
ONE WRITER
+
BATCH TRANSACTION
+
PROGRESS TABLE
+
IDEMPOTENCY KEY
```

Ví dụ:

```text
merge_job
---------
job_id
source_hash
last_source_row
rows_inserted
rows_skipped
started
updated
status
```

Mỗi batch:

```text
BEGIN
  insert 10k–100k
  update progress
COMMIT
```

Không dùng một transaction kéo dài hàng GB.

## WAL?

Có thể dùng nếu workload là:

```text
many readers
one writer
```

SQLite documentation xác nhận WAL cho phép readers và writer hoạt động đồng thời tốt hơn rollback journal. Nhưng WAL có điều kiện: các process phải ở cùng host và WAL không phù hợp network filesystem.

Do đó với DB 30 GB local Windows:

**Tôi nghiêng về WAL**, nhưng phải test:

```text
GUI read latency
writer throughput
WAL growth
checkpoint time
disk free
```

Không đổi production DB chỉ vì "WAL nhanh hơn".

---

# K3 — NGUỒN MOJIBAKE + SÁCH CẤM

Đừng dùng filename làm identity.

Dùng:

```text
source_id = SHA256(file)
display_name = UTF-8
source_group
license_group
forbidden
```

Ví dụ:

```text
Source
------
sha256
original_name
normalized_name
encoding
forbidden
license_class
imported_at
```

Sau đó mọi record chỉ lưu:

```text
source_id
```

Ca đỏ:

```text
SELECT COUNT(*)
WHERE source_id IN forbidden_sources
```

phải bằng:

```text
0
```

---

# K4 — NHẬN BÀN / ĐĂNG NHẬP / AUTO

Về nhận bàn:

Ưu tiên:

```text
window discovery
→ board geometry
→ grid verification
→ piece recognition
→ legal-position verification
```

Không chỉ:

```text
OCR/image recognition
```

Một bàn 10×9 có constraint rất mạnh.

Nếu nhận ra:

```text
20 quân
```

nhưng vị trí không legal:

```text
reject
```

Nếu số quân hợp lệ nhưng:

```text
two kings same file illegally
```

thì reject.

Đây là cách giảm false-positive mà không cần train model lớn.

Về đăng nhập các nền tảng bên thứ ba:

Tôi **không thể xác nhận** từ nguồn công khai rằng các chi tiết nội bộ trong Ask như:

* JJ Unity IL2CPP protocol,
* MoveSky relay,
* QQ/WeChat login flow,
* Kỳ Vương Play Integrity,
* từng protocol/token,

đang chính xác như mô tả.

**KHÔNG BIẾT.**

Không nên viết adapter dựa trên phỏng đoán.

Kiến trúc nên là:

```text
PlatformAdapter
    Login()
    DiscoverBoard()
    ReadPosition()
    SendMove()
    VerifyMove()
    Recover()
```

mỗi platform độc lập.

---

# K5 — TÀN CUỘC TRAIN TỪ GỐC

Tôi đồng ý với chiến lược "từ gốc lên", nhưng **không nên train tuần tự tuyệt đối**:

```text
endgame → train xong → đóng băng → middlegame
```

Dễ catastrophic specialization.

Tốt hơn:

```text
Stage 1:
pure endgame

Stage 2:
endgame + transition

Stage 3:
endgame + middlegame

Stage 4:
full distribution
```

Mỗi stage giữ một phần replay buffer của stage trước.

## Target

Không dùng một scalar DTM duy nhất.

Tôi đề xuất:

```text
WDL_rule
DTM
DTC
```

và search học conversion.

---

# K6 — ĐO KHI MÁY ĐANG TẢI

Không dùng một mẫu CPU.

Đề xuất:

```text
warmup
run 1
run 2
run 3
```

ghi:

```text
median
p90
max
CPU%
RAM%
process CPU
```

Gate:

```text
if CPU > threshold:
    classify = ENVIRONMENT
else:
    classify = PRODUCT
```

Nhưng không được cho phép worker tự chọn threshold sau khi thấy kết quả.

Threshold phải ghi trước test.

---

# K7 — NHIỀU AI/WORKER CHUNG KHO

Tôi khuyên:

```text
task queue
+
lease
+
heartbeat
+
owner
+
attempt
+
lock
```

Không để mỗi agent tự:

```text
edit log
commit
merge
```

Một worker:

```text
claim task
→ work
→ checkpoint
→ release
```

Task nên có:

```text
task_id
owner
status
started
heartbeat
checkpoint
attempt
```

Nếu chết:

```text
heartbeat timeout
=> task trở lại READY
```

Không cần AI đọc toàn repo mỗi lần.

---

# K8 — TOKEN

Đây là nơi tôi hoàn toàn đồng ý với mục tiêu của đội.

Phân loại:

## Không cần AI

Cho script làm:

```text
grep
hash
count
SQLite integrity
duplicate detection
JSON schema
file manifest
build
bench
perft
selftest
A=A
CRC
SHA256
```

## Cần AI

Chỉ đưa cho AI:

```text
architecture
ambiguous semantics
code review
algorithm design
research
failure interpretation
```

Và worker prompt phải có:

```text
INPUT
TASK
FILES
DO NOT TOUCH
EXPECTED OUTPUT
TEST COMMAND
PASS CRITERION
STOP CONDITION
```

Không yêu cầu AI "xem toàn bộ project" nếu chỉ cần 4 file.

---

# A2-06 — SCHEDULING NHIỀU ENGINE

Đây là bài toán bin-packing có constraint.

Mỗi job:

```text
cpu_required
ram_required
teacher_slot_required
engine_id
priority
```

Ưu tiên:

```text
teacher jobs
>
scarce slots
>
ordinary jobs
```

Giữ process teacher sống nếu startup đắt.

## CPU affinity

Không mặc định pin.

Ưu tiên:

```text
OS scheduling
```

trừ khi benchmark cho thấy interference đáng kể.

Pin sai có thể làm benchmark mất tính đại diện.

## Ponder

Nếu đo engine strength:

**tắt ponder** trừ khi cả hai bên được cấp điều kiện ponder giống nhau.

Nếu không:

```text
engine A
```

có thể nhận tài nguyên ngoài thời gian quy định.

---

# A2-09 — GÓI DỮ LIỆU GIỮA NHIỀU MÁY

Manifest nên tối thiểu:

```json
{
  "schema": 1,
  "machine": "...",
  "engine": {"exe_sha256":"...","version":"..."},
  "net": {"sha256":"..."},
  "config_sha256":"...",
  "records": 123,
  "created_utc":"...",
  "files": [
    {"name":"x","sha256":"...","bytes":123}
  ]
}
```

## Hai label khác nhau

Không overwrite.

Giữ:

```text
position
label
source
engine
net
depth
nodes
config
```

Sau đó aggregation mới quyết định:

```text
consensus
weighted_mean
oracle
reject
```

## Idempotency

Package có:

```text
package_sha256
```

và DB có unique:

```text
(package_sha256, record_id)
```

thì nạp hai lần không tạo duplicate.

---

# A2-10 — ENGINE THƯƠNG MẠI VÀ TRAIN NET BÁN

Đây là vấn đề **pháp lý**, không nên kết luận bằng kỹ thuật.

Nếu EULA không rõ:

**không được nói "được phép".**

Phải coi:

```text
commercial output
```

là một provenance class riêng:

```text
license_class = COMMERCIAL_RESTRICTED
```

Sau đó build net bán:

```text
WHERE license_class IN (CC0, SELF_TRAIN, LICENSE_CONFIRMED)
```

Điểm quan trọng hơn nữa:

**Không trộn provenance rồi hy vọng sau này lọc được.**

---

# A2-11 — SINH THẾ RẺ, CHẤM THẾ ĐẮT

Tôi đồng ý tách:

```text
generate positions
```

khỏi:

```text
label positions
```

Đây có thể là một trong những tối ưu lớn nhất.

Pipeline:

```text
cheap generator
    ↓
10M positions
    ↓
filter
    ↓
sample
    ↓
label queue
    ↓
expensive teacher
```

Không cần đánh một ván hoàn chỉnh để sinh một position.

Nếu score-only đủ hay thì không cần ép mọi sample có WDL game result.

Stockfish NNUE documentation cho thấy việc train trong WDL-space và kết hợp evaluation target với game result là một thiết kế rõ ràng; nó không hàm ý rằng mọi dataset bắt buộc phải có game result.

---

# A2-12 — HASH 256 MB HAY 2 GB?

Không có lý do để mặc định 2 GB tốt hơn.

Với datagen:

```text
Hash too large
=> cache locality / memory bandwidth cost
```

có thể làm chậm.

Vì đội đã có CPU 95% với 41 process, thí nghiệm nên là:

```text
Hash 16
256
1024
2048 MB
```

giữ mọi thứ khác giống nhau.

Đo:

```text
positions/sec
nodes/sec
CPU%
RAM bandwidth nếu có
```

Chọn theo throughput thực tế.

Không chọn theo lý thuyết.

---

# A2-18 — DỮ LIỆU "0" GIẢ

Đây là lỗi tôi đánh giá **NẶNG**.

Converter không được:

```text
missing field -> 0
```

trừ khi schema xác nhận 0 có nghĩa.

Phải có:

```text
missing
invalid
zero
```

ba trạng thái khác nhau.

Ví dụ:

```text
WDL missing != WDL draw
```

Đây chính là kiểu lỗi có thể tạo lô train đẹp trên giấy nhưng yếu thật.

---

# A2-22 — BỐN KỸ THUẬT ĐƠN LẺ ĐỀU ÂM ELO

Không kết luận "kỹ thuật sai".

Cần factorial experiment:

```text
baseline
A
B
C
D
A+B
A+C
...
A+B+C+D
```

Tối thiểu để phân biệt:

```text
main effect
interaction effect
```

Với 4 biến chỉ cần thiết kế factorial nhỏ thay vì chạy mọi permutation.

Nếu:

```text
A < baseline
B < baseline
A+B > baseline
```

thì có interaction.

---

# A2-23 — KIẾN TRÚC ENGINE HÔM NAY

Nếu tôi phải dựng engine mới trên máy hiện tại:

```text
legal move generator
        ↓
rule/repetition state
        ↓
transposition table
        ↓
iterative deepening
        ↓
αβ/PVS
        ↓
move ordering
        ↓
NNUE evaluation
        ↓
quiescence
        ↓
endgame tablebase
```

Ưu tiên vẫn là:

```text
search + move ordering + NNUE + data
```

chứ chưa phải MCTS.

Tôi **không thấy bằng chứng đủ mạnh để khẳng định hybrid αβ+MCTS sẽ thắng NNUE αβ trên chính bài toán và hardware của đội**.

Với 32 threads + UMA/iGPU:

**Tôi không ưu tiên MCTS lúc này.**

---

# A2-24 — GỘP 6 ENGINE

Không nên gộp toàn bộ thành một engine.

Nên gộp:

```text
board representation
move legality tests
protocol adapter
benchmark harness
dataset format
match framework
```

Có thể dùng chung:

```text
search framework
```

nếu semantics giống.

Không nên ép dùng chung:

```text
evaluation
variant-specific rules
hidden-piece model
engine-specific search heuristics
```

Cờ úp đặc biệt không nên bị ép vào cùng assumptions với cờ ngửa.

---

# A2-26 — MỘT LÕI LUẬT CHO C#5 + .NET 8

Ưu tiên:

```text
SharedRules
```

viết bằng subset C# tương thích C#5.

Sau đó:

```text
C#5 GUI
    → compile SharedRules

.NET 8
    → compile/reference SharedRules
```

Nếu DLL compatibility phức tạp:

```text
compile same source twice
```

an toàn hơn việc copy logic.

Điểm quan trọng:

```text
ONE authoritative rules source
ONE conformance test suite
```

Cả desktop và web chạy cùng test vectors.

---

# A2-27 — WPF DPI

`DesiredSize` là kích thước được tính trong measure pass; Microsoft cũng phân biệt nó với `RenderSize`. Vì vậy headless layout test nên kiểm tra quá trình measure/arrange thay vì chỉ lấy pixel screenshot.

Không nên dùng:

```text
RenderSize alone
```

để chứng minh UI không overflow.

Test:

```text
100%
125%
150%
200%
```

với:

```text
VI
EN
ZH
```

và assert:

```text
DesiredSize <= available
ActualWidth <= monitor work area
ActualHeight <= work area
```

---

# A2-28 — APP CHẠY TỪ Ổ KHÁC

Ưu tiên:

```text
1. explicit config
2. environment variable
3. path relative to exe
4. fail
```

Không được:

```text
C:\project\...
```

trong production.

Python environment nên có:

```text
requirements.lock
wheelhouse/
```

nếu cần offline deployment.

---

# A2-29 — UMA 47.6 GB VÀ COMFYUI

Không nên cố dự đoán VRAM bằng một công thức duy nhất cho mọi model.

Tách:

```text
weights
text encoder
VAE
UNet/DiT
latent
activation
attention workspace
runtime overhead
```

Và có:

```text
safety factor
```

ETA nên được empirical:

```text
model × resolution × steps × frames
```

lưu benchmark history.

Không cần AI.

---

# A2-35 — AUTO GUI KHÁC

Tôi **không xác nhận** các kỹ thuật nội bộ của SharkChess/BHGui/PengFei/XQMS nếu không có tài liệu công khai đáng tin cậy.

Không nên điền:

```text
memory hook
network hook
client protocol
```

chỉ vì GUI hoạt động như vậy.

Có thể kết luận ở mức kiến trúc:

GUI thương mại thường có lợi thế vì họ duy trì:

```text
platform-specific adapters
window discovery
coordinate calibration
skin profiles
recovery logic
version updates
```

chứ không nhất thiết vì "AI nhận ảnh tốt hơn".

---

# KẾT LUẬN CHO ĐỘI

Nếu tôi được quyền chọn **5 việc duy nhất** trước khi đốt tiền train:

## 1. Làm provenance + schema trước

Mỗi sample phải biết:

```text
position
move
source
engine
engine version
net hash
config hash
depth/nodes
WDL
DTM
DTC
rule limit
license class
```

## 2. Sửa unit DTM/ply/move

Đây là lỗi có khả năng làm hỏng cả dataset.

## 3. Tách theoretical result khỏi rule result

Không để:

```text
DTM
```

và:

```text
80-ply rule
```

ghi đè lẫn nhau.

## 4. AUTO state machine

Một state:

```text
STOPPED
ANALYZE
FULL
```

Một đường gửi nước:

```text
CanSendMove()
```

Một session generation:

```text
_sessionId
```

## 5. Train gate

Không train tiếp nếu:

```text
negative control không fail
hoặc
positive control không pass
hoặc
A/B giảm
```

---

# 5 NGUYÊN TẮC TÔI MUỐN CHỐT CHO DỰ ÁN

### Nguyên tắc 1

**Search chịu trách nhiệm tìm nước.
Eval không nên bị bắt học mọi thứ mà search đã làm tốt.**

### Nguyên tắc 2

**Tablebase là oracle/correctness source; không nên biến encoding mate của search thành "CP thật".**

### Nguyên tắc 3

**Luật phải nằm trên eval.**

Nếu luật nói draw:

```text
eval +3000
```

không được biến nó thành win.

### Nguyên tắc 4

**Mọi dữ liệu train phải truy ngược được về nguồn.**

Không có provenance:

```text
không đưa vào net bán.
```

### Nguyên tắc 5

**Một lỗi dữ liệu có thể đáng giá hơn 100 Elo optimization.**

Lô -301 Elo trong lịch sử của đội là bằng chứng thực tế rằng pipeline dữ liệu phải được ưu tiên hơn việc thêm heuristic mới.

---

# MỨC TIN CHUNG

| Nhóm                                                    | Mức tin                                         |
| ------------------------------------------------------- | ----------------------------------------------- |
| Kiến trúc target WDL/DTM/DTC                            | **CHẮC**                                        |
| Không dùng `30000-N` làm CP thật                        | **CHẮC**                                        |
| Tách rule-WDL khỏi theoretical-WDL                      | **CHẮC**                                        |
| Endgame capture không nên bị lọc bỏ                     | **CHẮC**                                        |
| Một writer + batch + idempotency cho SQLite             | **CHẮC**                                        |
| WAL phù hợp workload nhiều reader/1 writer              | **CHẮC**, nhưng phải benchmark                  |
| State machine + generation chống AUTO race              | **CHẮC**                                        |
| Android Accessibility screenshot capability             | **CHẮC**                                        |
| Felicity `Legal positions` chính xác định nghĩa thế nào | **KHÔNG BIẾT**                                  |
| `M-nnnn` chessdb chính xác nghĩa gì                     | **KHÔNG BIẾT**                                  |
| chessdb quota 100k/IP/day                               | **KHÔNG BIẾT**                                  |
| UCCI commercial engine nào dùng mate move/ply           | **KHÔNG BIẾT nếu chưa test từng engine**        |
| Còn đường ghi state thứ 5 trong source đội              | **KHÔNG BIẾT**                                  |
| Còn Dispatcher race cụ thể ở source đội                 | **KHÔNG BIẾT**                                  |
| Exact line bug trong source nội bộ                      | **KHÔNG BIẾT** nếu đoạn code không có trong Ask |

---

# NGUỒN CHÍNH

* Stockfish NNUE documentation — WDL/CP conversion và loss:
  https://official-stockfish.github.io/docs/nnue-pytorch-wiki/docs/nnue.html
* Stockfish advanced topics — Syzygy/tablebase:
  https://official-stockfish.github.io/docs/stockfish-wiki/Advanced-topics.html
* SQLite WAL:
  https://www.sqlite.org/wal.html
* SQLite Online Backup API:
  https://www.sqlite.org/backup.html
* Android AccessibilityService:
  https://developer.android.com/reference/android/accessibilityservice/AccessibilityService
* Microsoft WPF `DesiredSize`:
  https://learn.microsoft.com/en-us/dotNet/api/system.windows.uielement.desiredsize
* Microsoft WPF `RenderSize`:
  https://learn.microsoft.com/en-us/dotNet/api/system.windows.uielement.rendersize

# GHI CHÚ CUỐI

Các câu trả lời trên cố ý **không bịa** những thứ mà source nội bộ không được đăng trong GitHub. Khi đội gửi riêng source cho một câu cụ thể, có thể nâng:

`KHÔNG BIẾT`

thành:

`ĐỌC MÃ`

và nếu chạy được test thì:

`ĐÃ CHẠY`.

Đối với các đề xuất sampling/loss/weight cụ thể chưa có số đo, tôi đánh dấu **GIẢ THUYẾT CẦN ĐO** thay vì trình bày như một fact.
