# Day22 · AI Pricing, GTM & Evidence

**Đinh Trường An · MHV 2A202602393 · 08/10/2026**  
**Sản phẩm:** P-145 / BO-06 Onboarding Buddy Agent đang phát triển: hỗ trợ nhân viên mới tìm câu trả lời từ tài liệu nội bộ và theo dõi onboarding. Phạm vi định giá của bài là **FAQ onboarding**, không phải cam kết hoàn tất toàn bộ quá trình nhận việc.

## Bài làm

- [Monetization One-Pager — PDF, 1 trang](DinhTruongAn_Day22_onepager.pdf)
- [Monetization Model — Excel theo template lab](DinhTruongAn_Day22_model.xlsx)

```text
Track1_Day22_2A202602393_DinhTruongAn/
├── DinhTruongAn_Day22_model.xlsx
├── DinhTruongAn_Day22_onepager.pdf
└── README.md
```

Excel giữ 5 tab bài làm, tab hướng dẫn và tab tham chiếu của template. Giá trị được điền vào ô vàng; công thức được giữ nguyên. Template gốc có nhãn “Day 28” ở tab hướng dẫn; bài nộp và One-Pager là Day22. Các bảng giá lịch sử trong tab chỉ đọc không được dùng thay cho nguồn giá hiện hành bên dưới.

## 1. Định vị và ngân sách

| Phiên bản | Một câu mô tả | Người duyệt và ngân sách dự kiến |
|---|---|---|
| A — công cụ | P-145 là công cụ tra cứu tài liệu và hỗ trợ checklist onboarding cho HR và nhân viên mới. | HR đề xuất, IT/COO duyệt; ngân sách phần mềm HR. |
| B — công việc | P-145 giảm phần trả lời câu hỏi onboarding lặp lại của HR bằng FAQ có nguồn, với ca ngoài phạm vi vẫn do HR xử lý. | Trưởng HR/COO; ngân sách vận hành onboarding. |

**Chọn B:** chi phí thời gian trả lời FAQ có thể đối chiếu với đơn giá dịch vụ; không cần lập luận thay toàn bộ vị trí HR. Khách hàng ban đầu dự kiến là doanh nghiệp 50–200 nhân sự, có tài liệu onboarding được phê duyệt và đang dùng Microsoft Teams. Đây là ICP cần kiểm tra qua phỏng vấn, chưa phải danh sách khách đã mua.

**Một job hoàn tất:** một conversation FAQ onboarding được trả lời đúng theo tài liệu được duyệt, có nguồn kiểm tra được, người dùng xác nhận hữu ích, không cần HR tiếp quản và không mở lại cùng vấn đề trong 7 ngày. Chỉ tính một lần theo conversation ID; chào hỏi, từ chối, bỏ ngang, lỗi, chuyển HR và câu trả lời sai không tính phí usage. Ca mở lại bị hủy đơn vị đã ghi nhận. Dự kiến dùng log `conversation_id`, phiên bản tài liệu, nguồn, xác nhận và trạng thái mở lại; **tracking/billing này chưa được triển khai hoặc khách đồng ý**.

**Gói đề xuất:** $18/tenant/tháng cho workspace và quản trị tài liệu, cộng $0.23/FAQ đủ điều kiện. Phí nền là khoản cộng thêm, không phải credit trừ vào 18 đô đầu. Giới hạn pilot 1.000 lượt thử/tenant/tháng, yêu cầu đồng ý trước khi tăng hạn mức. Với 450 FAQ hoàn tất, doanh thu $121.50/tháng, tương đương $0.27/job. Không sử dụng mức $0.27 như đơn giá usage thật.

## 2. Cơ sở tính chi phí và giá

Tất cả input vận hành dưới đây là **giả định lập kế hoạch**, trừ giá nhà cung cấp và kết quả eval đã ghi nguồn. Đơn vị USD; tỷ giá 26.000 VND/USD chỉ là giả định quy đổi, không phải tỷ giá ngân hàng được xác nhận.

| Input | Giá trị và căn cứ | Ô |
|---|---|---|
| Lượt thử / thành công | 1.000/tháng; r=45%, xấp xỉ thấp hơn 19/42 đúng đủ trong eval offline; cần đo lại xác nhận người dùng và mở lại | `1_Cost_Job!B9:B10` |
| Model và token | GPT-4o-mini; 2 lượt/conversation, mỗi lượt 2.048 token chung + 1.000 token riêng và 400 output; dự toán, chưa có token telemetry | `B15:B22` |
| Cache | Input/write hiệu dụng $0.15, read hiệu dụng $0.15/triệu vì chưa đo hit; giá cache thật $0.075. Không ghi nhận tiết kiệm trong baseline | `B17:B18` |
| Hạ tầng + embedding | $0.008/lượt: dự toán $5/tháng app/vector/storage, $1 log/egress, $1 eval và $1 ingestion/embedding dự phòng cho 1.000 lượt | `B41` |
| Retry | 8% chi phí LLM; ước tính chưa đo, nằm trong dải 5–10% của lab | `B46` |
| QA nội bộ | 10% lượt thử × 2 phút × $6/giờ = $20/tháng; không phải lao động miễn phí | `B50:B54` |
| HITL A | HR khách hàng xử lý 550 ca chưa hoàn tất; nhà cung cấp không cam kết giao kết quả thay HR. QA nhà cung cấp vẫn nằm trong COGS | `B6/B55` |
| Overhead | $10/tenant/tháng: $5 hỗ trợ/R&D phân bổ và $5 chi phí thiết lập $60 phân bổ 12 tháng; CAC bán hàng được xét riêng, không cộng lại vào COGS | `B59` |
| Speech | 0: sản phẩm ở phạm vi chat văn bản; không STT, TTS hoặc cuộc gọi | `B34:B38/B42` |
| Giá trị tiết kiệm | 450 FAQ × 8 phút HR × $9/giờ = $540/tháng; phải xác minh thời gian thực trước/sau pilot | `2_Pricing!B10:B11` |
| Lương vị trí | $600/tháng chỉ là neo tham khảo; không chọn vì FAQ không thay cả vị trí | `2_Pricing!B14` |
| CPO / win / quota | $60/cơ hội = 6 giờ × $6 + $24 tiếp cận; win 25%; quota doanh thu $24.000/năm, 250 ngày. Dự toán founder địa phương; chưa phải dữ liệu CAC thực tế | `4_Channel_Fit!B13:B14/B20:B21` |

**Token math:** 2×3.048×0.15/1M + 2×400×0.60/1M = **$0.0013944/lượt thử**. Retry $0.000111552/lượt. Tổng COGS tháng = $1.3944 API + $0.111552 retry + $8 infra/embedding + $20 QA = **$29.505952**. Cộng overhead là **$39.505952**.

Chia cho **450 hoàn tất**, không phải 1.000 lượt thử: COGS/job **$0.06556878**; đủ overhead **$0.08779100**. Chia sai cho lượt thử sẽ ra $0.02950595 và đánh giá thấp cost/job **55%** (cost đúng cao hơn **122.22%**).

Sàn template = 3×COGS = **$0.19670635**; sàn đủ overhead = 3×$0.08779100 = **$0.26337301**. Giá hiệu dụng **$0.27** vượt cả hai. GM COGS **75.72%**; sau overhead, biên trước chi phí acquisition **67.48%**. Khoảng neo 10–25% tiết kiệm là **$0.12–$0.30/job**, giá đề xuất nằm trong khoảng này. Các mức này chưa phải willingness-to-pay được khách xác nhận.

### Cache, batch và điểm gãy

- Baseline cache tiết kiệm **0%** vì dùng no-hit. Nếu lượt thứ hai hit đủ prefix 2.048 token ở $0.075, LLM còn **$0.0012408**, tiết kiệm **11.02% riêng LLM**; tổng cost/job chỉ giảm khoảng **0.56%**. Không lấy hệ số cache 10% trong bảng mẫu áp cho GPT-4o-mini.
- Batch giả định giảm 50% giá LLM: LLM **$0.0006972/lượt**; gồm retry, cost/job khoảng **$0.06389550**. Không dùng cho FAQ cần phản hồi ngay vì Batch có thời gian hoàn tất đến 24 giờ; chỉ cân nhắc eval/ingestion async.
- Ở giá hiệu dụng cố định P=$0.27, `v+q=0.029505952`, HITL A nên e=0. Điều kiện GM≥60% là `r >= (v+q)/(0.27×0.4) = 27.3203%` (`2_Pricing!B33`). Đây là ngưỡng GM mục tiêu, không phải hòa vốn tuyệt đối.
- Với **hybrid thật**, N=1.000, doanh thu `18 + 0.23×1.000×r`. Điều kiện GM≥60%: `18+230r >= 29.505952/0.4`, nên **r≥24.2456%**. GM<50% khi r<17.8313%; lỗ COGS khi r<5.0026%. Nếu xét cả overhead, r cần **≥35.1152%** cho biên 60%; baseline 45% vẫn đạt.
- **Biến gãy ưu tiên: QA.** Tăng tỷ lệ QA từ 10% lên 20% (hoặc thời gian từ 2 lên 4 phút) làm COGS/job **$0.11001323**, GM **59.25%**. Khi pilot vượt ngưỡng QA, dừng mở rộng và kiểm tra chất lượng/cách xử lý; không bỏ QA để làm đẹp GM.

| r | Cost/hoàn tất | GM với P=$0.27 (template) | GM gói hybrid thật |
|---:|---:|---:|---:|
| 50% | $0.059012 | 78.14% | 77.82% |
| 60% | $0.049177 | 81.79% | 81.09% |
| 70% | $0.042151 | 84.39% | 83.52% |
| 80% | $0.036882 | 86.34% | 85.39% |
| 90% | $0.032784 | 87.86% | 86.89% |

Sensitivity của template giữ giá hiệu dụng cố định; với Hybrid, hiệu dụng thay đổi theo số hoàn tất. Vì vậy chỉ trùng gói thật tại baseline 450 job; bảng cuối đối chiếu đúng doanh thu Hybrid. GM trên 85% ở kịch bản r cao là kết quả giả định chi phí theo lượt thử cố định, không phải hiệu quả đã đo; cần đo QA, ingestion và support lại trước khi dùng kịch bản đó để bán.

## 3. Value Metric và kênh

**Attribution 5/10:** log mới đủ mức eval (1); có eval số liệu (2); chưa khách đồng ý tính thành công (0); tách được đúng/sai trong test nhưng chưa ảnh hưởng kinh doanh (1); có định nghĩa billing trên giấy, chưa kiểm thử (1). **Autonomy 1/10:** RAG trả lời một phần ca (1); chưa chứng minh SLA ngoài giờ (0); chưa chứng minh tự handoff đúng HR (0); tỷ lệ ca chưa hoàn tất vượt 20% (0); chưa vận hành chỉ QA mẫu (0). Hybrid khớp gợi ý, không cần market override; không cam kết outcome hoàn thành onboarding.

**Hai benchmark cùng tác vụ tìm/trả lời kiến thức nội bộ:** Microsoft 365 Copilot Business và Atlassian Rovo Search/Chat. Phạm vi rộng hơn FAQ onboarding nên không so đơn giá trực tiếp. Giá và đơn vị đã kiểm tra 08/10/2026, ghi ở `3_Value_Metric!A26:D27`; không quy đổi credit Rovo thành một FAQ khi chưa biết mức tiêu thụ.

**Một kênh 90 ngày: Sales-Led do founder thực hiện.** Đội phải phối hợp HR/IT để duyệt dữ liệu, thử giá và tích hợp, nên PLG chưa phù hợp hiện tại. Không có quan hệ partner đã xác nhận. Điểm PLG/Sales/Partner = **15/22/11**, là đánh giá hiện trạng có giả định cần kiểm chứng.

ARPU $121.50, GM 75.72%, SMB 12 tháng → budget CAC **$1.103,93**. ACV **$1.458**, quota $24.000 → **16.46 deal/năm**, **0.06584 deal/ngày**. CPO $60/win 25% → CAC **$240**, **0.2174×** ngân sách, payback **2.61 tháng**. Các phép tính không chứng minh khả năng tuyển AE: đây là thử nghiệm founder với chi phí lao động có tính tiền. Benchmark CPO thấp nhất của brief $6.300/win25% → CAC $25.200, **22.83×** budget, không phù hợp mô hình AE quốc tế. Nếu CPO lên $120 và win xuống 15%, CAC $800 vẫn dưới budget baseline nhưng payback tăng **8.70 tháng**; cần dừng nếu GM/ARPU cũng giảm. Ở GM60%, ARPU100, budget720 → CAC800 vượt **1.11×**; phải kiểm tra ICP/gói giá trước tăng chi phí bán hàng.

Pain Moment, 3 giai đoạn và người chịu trách nhiệm đã điền `5_90Day_Plan`. Teams là **bề mặt tích hợp dự kiến**, không phải partner/kênh bán hàng thứ hai. Tháng 4+ giữ cùng ICP và kênh; chỉ sau khi có bằng chứng thắng ngách mới đánh giá ngách kế tiếp là doanh nghiệp dịch vụ 200–500 nhân sự.

## 4. Evidence Pack / Procurement Q&A

**Eval đã có:** run `rag-tudu-2026-10-07-b/holdout_3000`, 58 câu: 42 answerable, 8 ca ngoài phạm vi, 8 ca còn lại. Recall@3 **38/42=90.48%**; đúng đủ **19/42=45.24%**; đúng đủ trong ca trả lời **19/26=73.08%**; có citation **25/26=96.15%**; false refusal **16/42=38.10%**, false acceptance ngoài phạm vi **0/8**. Chấm theo required facts, không có LLM judge độc lập. Đây là eval RAG offline trên tài liệu, không phải pilot/containment/ROI sản phẩm. Không chuyển Recall@3 hoặc citation coverage thành tỷ lệ xử lý hoàn tất.

| Câu hỏi Procurement | Câu trả lời trong phạm vi thực tế | Nghiệm thu trước pilot |
|---|---|---|
| AI nói sai thì sao? | Vẫn có nguy cơ trả lời sai hoặc từ chối nhầm; số đúng đủ mới 19/42. Nguồn giúp kiểm tra, không bảo đảm đúng. Không cho AI phê duyệt chính sách/ngoại lệ hay quyền hệ thống; HR duyệt ca nhạy cảm. | 100 FAQ giữ kín + 20 ca ngoài phạm vi; kiểm tra nguồn, quy trình khiếu nại/mở lại và handoff. Trường An, 22/10; HR duyệt 26/10. |
| Dữ liệu có bị train không? | Theo chính sách OpenAI API, không dùng train mặc định trừ khi chủ tài khoản chủ động opt in; vẫn có lưu trữ phục vụ giám sát lạm dụng, thường đến 30 ngày. P-145 cấu hình được provider, nên phải xác minh backend và setting thật của pilot; không tuyên bố zero retention. | Xác nhận quyền dùng tài liệu, quyền theo tenant, danh mục dữ liệu, opt-in, endpoint, retention và hợp đồng; không dùng hồ sơ/định danh cá nhân trong bộ FAQ. Trường An + HR/IT, 26/10. |
| Dừng dịch vụ thì dữ liệu ở đâu? | Dự án có tài liệu/code vận hành nhưng chưa có bằng chứng nghiệm thu quy trình bàn giao/xóa dữ liệu khách. Không hứa xuất/xóa tức thì hoặc SLA chưa được xây dựng. | Thỏa thuận xuất tài liệu và log cần thiết thành định dạng đọc được, bàn giao cho người được chỉ định, xóa bản vận hành/backup theo lịch xác nhận. Thực hành export/restore/delete trước pilot; Trường An, 26/10. |

**Pilot Report chưa có:** dự kiến tuyển 2 doanh nghiệp trong ICP, chưa chọn tên/chưa có xác nhận; bắt đầu 01/11/2026 sau các gate, chạy 30 ngày. Đo tổng lượt, hoàn tất xác nhận, reopen7d, ca HR, token/retry/QA, phút HR trước–sau, chi phí triển khai và acquisition. Báo cáo ngày 08/12/2026, Đinh Trường An phụ trách. Nếu chưa tuyển được, lùi lịch thay vì ghi đã chạy.

**Test người lạ:** chưa có nhóm khác đọc thử; dự kiến 09/10/2026, người phụ trách Đinh Trường An, ghi đủ 3 câu trả lời và số câu hỏi lại. Không ghi “đạt” hay “0 câu hỏi” khi chưa thực hiện.

## 5. Hai lượt phản biện theo §4.7

Phản biện dưới đây áp dụng các nhiệm vụ của §4.7.1 và §4.7.3 vào bản mô hình. `Accept/Reject/Partial` mô tả cách bản sửa xử lý góp ý, không phải bằng chứng người học đã tự phỏng vấn, pilot hoặc xác nhận quyết định cá nhân.

### Prompt 1 — Cost/Job Stress Test (English)

```text
Review P-145, an onboarding knowledge assistant, as a CFO and an
infrastructure reviewer. Audit the assumptions; do not replace inputs.
At 1,000 monthly conversations, use 45% successful completion. Each
conversation has two model turns with 3,048 input tokens and 400 output
tokens per turn. GPT-4o-mini costs $0.15 input and $0.60 output per
million tokens. No cache benefit is assumed; cached input list price is
$0.075. Infrastructure plus embedding is $0.008 per attempt. Retry adds
8% of inference cost. Internal QA covers 10% of attempts at two minutes
and $6/hour. Customer HR pays escalation. Allocated overhead is $10.
Pricing is $18 per tenant plus $0.23 per accepted FAQ; 450 completions
imply $0.27 effective revenue per completion.
Check omitted costs and the completion denominator. Rebuild token math,
cache and batch alternatives. Flag price expiry. Derive the 60% margin
threshold for both fixed effective price and actual hybrid revenue.
Show margins at 50, 60, 70, 80 and 90 percent completion. Identify the
input whose doubling breaks 60% margin. Distinguish measured evidence
from estimates and expose the weakest assumption.
```

| Điểm phản biện | Xử lý trong bản sửa |
|---|---|
| 45% chưa đo xác nhận/mở lại; đúng offline không đồng nghĩa containment | **Accept:** ghi proxy rõ ở Excel/One-Pager; thêm eval và pilot gate. |
| Dùng 1.000 làm mẫu số sẽ thiếu 55% cost | **Accept:** chia 450; đối chiếu bằng phép tính ở mục 2. |
| Cache chung trong template có thể làm lợi nhuận quá cao | **Accept:** baseline no-hit; chỉ tính tiết kiệm giả định 11.02% LLM riêng. |
| Phải tính cả escalation HR vào COGS vendor | **Reject:** biến thể A, khách chịu ca HR; giữ QA vendor $20. Nếu chuyển managed service phải đổi sang B và tính lại. |
| Tính sàn chỉ bằng COGS chưa đủ safety cho overhead | **Accept:** giá hiệu dụng $0.27 cũng vượt sàn đủ overhead $0.26337. |
| Sensitivity P cố định không đúng toàn bộ gói Hybrid | **Accept:** tính thêm doanh thu 18+230r, ngưỡng 24.25%; giữ nguyên công thức template và giải thích phạm vi. |
| Infra/QA thấp, có thể tăng khi dữ liệu và hỗ trợ phức tạp | **Partial:** giữ dự toán có breakdown, chỉ coi là baseline; QA gấp đôi làm GM59.25%, phải đo trước scale. |

### Prompt 2 — Channel Reality Check (English)

```text
Challenge a single 90-day sales-led plan for P-145, an onboarding FAQ
assistant for Vietnamese firms with 50 to 200 employees. Monthly ARPU
is $121.50, gross margin 75.7153%, and the SMB payback ceiling is twelve
months. Annual quota is $24,000 across 250 working days. Founder-led
cost per opportunity is estimated at $60 and win rate at 25%; neither
has been measured. The pain moment is 08:00 to 09:00 in the first work
week, while a new employee asks HR questions in Microsoft Teams.
Recompute ACV, annual and daily deals, allowable CAC, acquisition CAC,
the budget multiple and payback. Include the founder's paid labour.
Compare with the lab's $6,300 opportunity benchmark for a rep-driven
motion and explain applicability. Stress $120 opportunity cost and
15% win rate. Attack the missing customer evidence and proposed Teams
integration. State what must be validated before hiring sales staff
or expanding. Give the strongest objection to this channel.
```

| Điểm phản biện | Xử lý trong bản sửa |
|---|---|
| CPO $60 không phải evidence khách mua; quota thấp có thể che chi phí founder | **Accept:** tính 6 giờ×$6 và $24/cơ hội, gắn mục tiêu ghi thực chi/thời gian trong pilot; không tuyên bố CAC đã đạt. |
| ACV $1.458 không đủ mặc nhiên nuôi đội AE tiêu chuẩn | **Accept:** chỉ chạy founder-led 90 ngày; đối chiếu CAC quốc tế $25.200, vượt budget22.83×. |
| CPO120/win15% tự nó đã vượt budget | **Reject:** CAC800 vẫn dưới budget1.103,93; chỉ vượt nếu ARPU/GM cũng giảm (ví dụ budget720). Sửa kết luận ở Excel. |
| Teams chưa triển khai; app/giờ đau chưa được hỏi khách | **Accept:** ghi giả thuyết/điểm nhúng dự kiến; 8 phỏng vấn và consent trước pilot. |
| Nên chạy thêm Partner-Led ngay để hạ CAC | **Reject:** chưa có partner xác nhận và lab yêu cầu một kênh; Teams integration không phải thỏa thuận phân phối. |
| Sales-led không thể scale với 3 khách thử | **Partial:** đây là giai đoạn học; chỉ lặp lại trong cùng ICP khi GM≥60%, payback≤12 tháng và chất lượng đạt gate. |

## Nguồn

Kiểm tra ngày **08/10/2026**:

- [Brief Day22 trên VLearn — 6 trạm và bộ prompt](https://vlearn.dev/course/k04-l34-p2-t1/reader?day=D12&part=lab-ff5b3a59-s04-doc), [rubric và đầu ra](https://vlearn.dev/course/k04-l34-p2-t1/reader?day=D12&part=lab-ff5b3a59-s06-doc).
- [Template Excel và One-Pager của lab](https://drive.google.com/drive/folders/1lAN_fxwtTr1fOHm1SoPR7OH3F0Z8D5Ya).
- P-145, nhánh `truongan`: [`docs/brief/01-prd.md`](https://github.com/AI20K-Build-Phase-Cohort-4/P-145/blob/truongan/docs/brief/01-prd.md), [`src/config.py`](https://github.com/AI20K-Build-Phase-Cohort-4/P-145/blob/truongan/src/config.py), [eval summary](https://github.com/AI20K-Build-Phase-Cohort-4/P-145/blob/truongan/eval/results/rag-tudu-2026-10-07-b/summary.json). Liên kết nguồn dự án phụ thuộc quyền repo; bài chỉ trích các chỉ số tổng hợp, không kèm dữ liệu cá nhân.
- [OpenAI GPT-4o-mini](https://developers.openai.com/api/docs/models/gpt-4o-mini): input $0.15, cached $0.075, output $0.60/triệu token; không dùng giá khuyến mại trong model.
- [text-embedding-3-small](https://developers.openai.com/api/docs/models/text-embedding-3-small): $0.02/triệu token, được dự phòng trong $0.008 infra/embedding, không tính thêm lần hai.
- [Batch API](https://developers.openai.com/api/docs/guides/batch): giảm 50%, xử lý async đến 24 giờ; không dùng cho FAQ trực tiếp.
- [Microsoft 365 Copilot Business](https://www.microsoft.com/en-us/copilot/pricing/business): $18/user/tháng thanh toán năm, giá gốc $21; ưu đãi 01/07–31/12/2026, năm đầu, cần Microsoft 365 đủ điều kiện riêng. Benchmark, không phải chi phí P-145; hết ưu đãi không đổi COGS model.
- [Atlassian Rovo licensing](https://www.atlassian.com/licensing/rovo): gộp trong plan đủ điều kiện; usage vượt allowance $0.01/credit; không suy ra giá một FAQ.
- [OpenAI API data controls](https://developers.openai.com/api/docs/guides/your-data): training mặc định tắt, ngoại lệ opt-in; retention theo endpoint/cấu hình.
