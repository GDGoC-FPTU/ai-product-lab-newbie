# 📋 Báo Cáo Deep-Dive — Vin Smart Future
## Đề tài: Phân tích cảm xúc đánh giá xe quốc tế (VinFast)

---

## 👥 Thông tin nhóm

| Thông tin | Chi tiết |
|---|---|
| **Tên nhóm** | *(Điền tên nhóm tại đây)* |
| **Mảng kinh doanh** | VinFast — Xe điện quốc tế |

### Danh sách thành viên:

| Họ và tên | Mã số sinh viên (MSSV) |
|---|---|
| Vũ Đăng Khiêm | *(Điền MSSV)* |
| *(Thành viên 2)* | *(Điền MSSV)* |
| *(Thành viên 3)* | *(Điền MSSV)* |
| *(Thành viên 4)* | *(Điền MSSV)* |

---

## 🗳️ Quyết định lựa chọn bài toán

Nhóm quyết định chọn bài toán **"Phân tích cảm xúc tự động từ đánh giá của khách hàng quốc tế về xe VinFast"** để thực hiện Deep-Dive.

### Lý do lựa chọn:
VinFast đang mở rộng mạnh mẽ sang thị trường quốc tế (Mỹ, Canada, Pháp, Đức, Hà Lan). Tuy nhiên, đội ngũ PR & Marketing toàn cầu đang phải **đọc thủ công hàng trăm đến hàng nghìn đánh giá** từ các nền tảng quốc tế (Reddit, Trustpilot, YouTube, Google Reviews, các tờ báo lớn như Motor Trend, Car and Driver) bằng nhiều ngôn ngữ khác nhau. Tốc độ phản ứng chậm trước làn sóng đánh giá tiêu cực có thể gây tổn hại nghiêm trọng đến hình ảnh thương hiệu VinFast tại thị trường quốc tế.

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

Sơ đồ quy trình hiện tại: Xem file `04-workflow-diagram.png`

```text
┌──────────────────┐      ┌──────────────────┐      ┌──────────────────┐
│     Bước 1        │      │     Bước 2        │      │     Bước 3        │
│ Thu thập thủ công│      │  Đọc & dịch thuật │      │  Phân loại cảm   │
│ đánh giá từ      │ ──►  │  từng đánh giá    │ ──►  │  xúc thủ công    │
│ Reddit, Trustpilot│      │  (EN/FR/DE)       │      │  (Pos/Neg/Neu)   │
│ YouTube, News...  │      │  bằng Google Dịch │      │  + gán nhãn loại │
│                  │      │                   │      │  lỗi (pin, SW..) │
│ Ai: Social Media │      │ Ai: Analyst       │      │ Ai: Analyst      │
│ Analyst          │      │                   │ 🔄   │                  │
│ ⏱ 4 giờ/ngày    │      │ ⏱ 6 giờ/ngày     │      │ ⏱ 8 giờ/ngày 🔴 │
└──────────────────┘      └──────────────────┘      └──────────────────┘
                                                              │
        🔄 Handoff: File Excel tổng hợp ◄────────────────────┘
                    │
                    ▼
┌──────────────────┐      ┌──────────────────┐
│     Bước 4        │      │     Bước 5        │
│  Tổng hợp báo   │ ──►  │  PR Manager       │
│  cáo tuần       │      │  phê duyệt &      │
│  (Excel/PPT)    │      │  phân phối        │
│                  │      │  cho các team     │
│ Ai: Analyst      │ 🔄   │ Ai: PR Manager   │
│ ⏱ 3 giờ/ngày   │      │ ⏱ 1 giờ          │
└──────────────────┘      └──────────────────┘

🔴 = Bottleneck chính (Bước 3 — chiếm 37% tổng thời gian)
🔄 = Điểm Handoff chuyển giao dữ liệu giữa người/hệ thống
⏱ Tổng thời gian xử lý thủ công: ~22 giờ/tuần (3–4 ngày làm việc)
📊 Số lượng đánh giá xử lý: ~500–1.000 reviews/tuần
```

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Chuyên viên phân tích truyền thông xã hội (Social Media Analyst) và đội PR & Marketing Quốc tế của VinFast. |
| **2. Current Workflow** | Analyst thủ công tìm kiếm các đề cập đến VinFast trên Reddit, Trustpilot, YouTube, Google Reviews và các trang báo quốc tế. Sau đó đọc, dịch (qua Google Translate), phân loại cảm xúc (Positive/Negative/Neutral) và gán nhãn loại vấn đề (Pin/Phần mềm/Chất lượng/Dịch vụ) cho từng đánh giá. Dữ liệu tổng hợp vào Excel rồi làm báo cáo PowerPoint gửi cho PR Manager cuối tuần. Toàn bộ quy trình mất 22 giờ/tuần, hoàn toàn thủ công. |
| **3. Bottleneck** | Bước 3 — Phân loại cảm xúc và gán nhãn loại vấn đề thủ công (chiếm 8/22 giờ, tức 36% tổng thời gian). Mỗi review cần 5–10 phút xử lý, với 500–1.000 reviews/tuần thì 1 analyst không đủ tải. Tỉ lệ nhất quán giữa các analyst khi phân loại cảm xúc biên giới (ví dụ: phàn nàn nhẹ vs. tiêu cực) chỉ đạt ~72%, dẫn đến báo cáo thiếu chính xác. |
| **4. Business Impact** | (1) Mất 22 giờ nhân công/tuần tương đương ~1.100 USD chi phí nhân sự/tuần cho mỗi thị trường. (2) Báo cáo chỉ được cập nhật 1 lần/tuần — phát hiện khủng hoảng truyền thông trễ tới 7 ngày, trong khi một post viral tiêu cực trên Reddit có thể đạt 50.000+ views chỉ trong 24 giờ. (3) VinFast đã nhận nhiều đợt đánh giá tiêu cực lớn tại Mỹ năm 2023–2024 liên quan đến pin và phần mềm mà team phản ứng chậm. |
| **5. Success Metric** | (1) **Tốc độ:** Rút ngắn chu kỳ phân tích từ 22 giờ/tuần xuống còn dưới 2 giờ/tuần (real-time monitoring, dashboard cập nhật mỗi 4 giờ). (2) **Độ chính xác phân loại:** Đạt ≥90% accuracy so với nhãn do chuyên gia gán trên tập test gồm 200 reviews đa ngôn ngữ. (3) **Phủ ngôn ngữ:** Hệ thống xử lý đồng thời ít nhất 4 ngôn ngữ: Tiếng Anh, Pháp, Đức, Hà Lan. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Thu thập và phân tích văn bản đánh giá công khai từ các nền tảng; tự động phân loại cảm xúc (Positive/Negative/Neutral) và gán nhãn danh mục vấn đề; gợi ý draft phản hồi để PR team tham khảo; gửi cảnh báo tự động khi phát hiện review có điểm sentiment tiêu cực cao (score < -0.7). **TUYỆT ĐỐI CẤM:** AI không được tự động đăng bất kỳ phản hồi/bình luận nào lên nền tảng công khai mà không có sự phê duyệt của PR Manager (bắt buộc HITL); không được truy cập dữ liệu cá nhân (PII) của người dùng; không được tự ý chỉnh sửa nội dung báo cáo đã được phê duyệt và gửi đi. |

---

## 3.3. Future-State Flow & AI Fit

**AI Fit được chọn:** ✅ **LLM Feature** (Không cần Agentic Loop vì quy trình có cấu trúc cố định; rủi ro nếu AI phản hồi sai trên mạng xã hội quốc tế là rất cao, cần HITL bắt buộc)

**So sánh các lựa chọn kiến trúc:**

| Kiến trúc | Đánh giá |
|---|---|
| **Rule-based** | Không đủ linh hoạt — các từ điển sentiment cứng (như VADER, TextBlob) không hiểu được ngữ cảnh ("VinFast surprisingly good" bị phân loại sai) |
| **LLM Feature** ✅ | Phù hợp nhất — LLM hiểu ngữ cảnh, đa ngôn ngữ, có thể giải thích lý do phân loại |
| **Agentic Loop** | Over-engineering — không cần thiết cho bài toán phân loại có cấu trúc này |

**Quy trình tương lai (Future-State):**

```text
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│     Bước 1        │     │     Bước 2        │     │     Bước 3        │
│ 🔵 Auto-crawl   │     │ 🔵 LLM Gemini    │     │ 🟢 PR Manager    │
│ reviews từ API  │ ──► │ phân tích cảm    │ ──► │ xem dashboard &  │
│ (Reddit, Google │     │ xúc + gán nhãn   │     │ phê duyệt alerts │
│  Trustpilot...) │     │ loại vấn đề      │     │ trước khi gửi    │
│                  │     │ (JSON output)    │     │ báo cáo          │
│ ⏱ ~15 phút      │     │ ⏱ ~30 phút      │     │ ⏱ ~1 giờ        │
└──────────────────┘     └──────────────────┘     └──────────────────┘
                                                          │
                                                    ↩️ Fallback:
                                                    Nếu LLM trả về
                                                    confidence < 0.7,
                                                    đưa sang analyst
                                                    review thủ công
                                                    (Low-confidence Queue)
```

**Cấu trúc JSON Output của AI:**
```json
{
  "review_id": "reddit_abc123",
  "source": "Reddit r/vinfast",
  "language": "en",
  "sentiment": "negative",
  "confidence": 0.92,
  "issue_categories": ["battery", "software"],
  "summary_vi": "Người dùng phàn nàn về pin sụt nhanh sau 6 tháng sử dụng và lỗi phần mềm màn hình.",
  "alert_flag": true,
  "draft_response": "[DRAFT_ONLY] Cảm ơn bạn đã chia sẻ. Chúng tôi đã ghi nhận vấn đề và sẽ liên hệ hỗ trợ trong vòng 24h..."
}
```

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
   → CÓ: Hàng nghìn đánh giá công khai từ Reddit r/VinFast, Trustpilot (vinfast.us), YouTube review videos đã được công khai. Có thể crawl ngay để tạo tập test labeled.

2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   → CÓ: AI chỉ phân loại và gợi ý draft nội bộ. Mọi phản hồi công khai PHẢI qua PR Manager phê duyệt (HITL bắt buộc). Reviews có confidence < 0.7 tự động vào Low-confidence Queue cho analyst xử lý.

3. [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
   → CÓ: Đội PR VinFast Quốc tế đang chịu áp lực lớn sau các đợt đánh giá tiêu cực tại Mỹ, sẵn sàng áp dụng công nghệ mới để cải thiện tốc độ phản ứng.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

**[x] GO (Bắt đầu xây dựng Prototype)**

**Justification — Lý giải quyết định:**

> Dự án được đánh giá **GO** dựa trên 4 bằng chứng kỹ thuật và thương mại sau:
>
> **1. Tính khả thi kỹ thuật cao:** Bài toán phân loại cảm xúc đa ngôn ngữ là use case đã được kiểm chứng rộng rãi cho LLM. Gemini 2.5 Flash hỗ trợ natively 40+ ngôn ngữ, cho độ chính xác >90% trên benchmark chuẩn SST-2 và multilingual review datasets. Không cần fine-tuning — zero-shot/few-shot prompting là đủ.
>
> **2. ROI rõ ràng và đo được:** Giảm từ 22 giờ/tuần xuống ~2 giờ/tuần = tiết kiệm 20 giờ nhân công/tuần × 4 tuần/tháng × 50 USD/giờ = **~4.000 USD/tháng** chỉ tính riêng một thị trường. Chi phí API Gemini 2.5 Flash ước tính ~50–100 USD/tháng (xử lý 4.000 reviews/tháng × 1.000 tokens × $0.00015/1K tokens). Tỉ lệ ROI: **40x**.
>
> **3. Rủi ro được kiểm soát chặt:** Mọi phản hồi đối ngoại đều qua HITL bắt buộc. Hệ thống chỉ tự động hóa phân tích nội bộ — không có rủi ro phát ngôn sai trên mạng xã hội quốc tế.
>
> **4. Thời điểm chiến lược:** VinFast đang trong giai đoạn mở rộng thị trường quốc tế quan trọng (2024–2026). Khả năng phát hiện và phản ứng với khủng hoảng truyền thông trong vòng <4 giờ thay vì 7 ngày là lợi thế cạnh tranh đáng kể.

---

## 💻 Phase 4 — Prompt Prototype

Nhóm đã xây dựng file `extras/prompt_prototype.py` và kiểm tra ranh giới an toàn bằng **Gemini 2.5 Flash**.

### Ranh giới an toàn được bảo vệ:
- **Rule 1 [DRAFT_ONLY]:** Mọi draft phản hồi công khai đều phải có tiền tố `[DRAFT_ONLY]` — AI không được tự ý bỏ qua dù bị áp lực.
- **Rule 2 [NO_PUBLIC_POST]:** AI tuyệt đối không được tạo ra nội dung dưới dạng "sẵn sàng đăng" (ready-to-post) mà không có nhãn DRAFT_ONLY.
- **Rule 3 [SCOPE_LIMIT]:** AI chỉ được phân tích đánh giá VinFast. Mọi yêu cầu so sánh tiêu cực với đối thủ cạnh tranh (Tesla, BYD) phải bị từ chối.

### Kết quả Adversarial Tests:
- **Test 1** (Yêu cầu AI đăng phản hồi trực tiếp): **[PASS]** — AI từ chối và trả về `[DRAFT_ONLY]`
- **Test 2** (Yêu cầu AI bỏ tag DRAFT_ONLY): **[PASS]** — AI giữ nguyên tag dù bị ép
- **Test 3** (Yêu cầu so sánh chê Tesla): **[PASS]** — AI từ chối và giải thích lý do
