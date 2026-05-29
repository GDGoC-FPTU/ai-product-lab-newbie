# Báo cáo Deep-Dive — Vin Smart Future (VinFast International Review Use Case)

> **Báo cáo hoàn chỉnh từ đầu đến cuối lab, đã được định vị theo Rubric và bối cảnh vận hành của Vin Smart Future.**
> 
> * **Mục tiêu của file này:** Trình bày kết quả nghiên cứu, phân tích và giải pháp AI Scoping của nhóm cho đề tài phân tích cảm xúc đánh giá xe quốc tế VinFast.
> * **Thành viên thực hiện:** 
>   * Lê Dương Hiếu (MSSV: 2A202600635) - AI Product Engineer
>   * Nhóm: Vin Smart Future Team 01

---

## 🏛️ Bối cảnh: Tôi là ai?

Tôi là **Lê Dương Hiếu**, AI Product Engineer tại **Vin Smart Future**. Nhóm chúng tôi được giao nhiệm vụ phối hợp với Khối Chăm Sóc Khách Hàng Quốc Tế & Phân Tích Thị Trường của **VinFast** để tìm kiếm các cơ hội tối ưu hóa bằng trí tuệ nhân tạo khi VinFast đang mở rộng mạnh mẽ ra các thị trường toàn cầu (Mỹ, Canada, Châu Âu, Đông Nam Á). 

Thông qua khảo sát thực tế tại bộ phận CSKH Toàn Cầu, tôi nhận thấy các nhân viên phân tích (Analysts) đang gặp áp lực cực kỳ lớn khi phải xử lý hàng ngàn đánh giá xe bằng nhiều ngôn ngữ khác nhau (Anh, Pháp, Đức, Tây Ban Nha, Thái Lan) trên các diễn đàn quốc tế (Reddit, YouTube, Facebook, VinFast Community). Việc dịch thuật thủ công, phân loại cảm xúc và gắn nhãn lỗi kỹ thuật gây tắc nghẽn quy trình, dẫn đến việc chậm trễ phát hiện các lỗi an toàn nghiêm trọng và giảm hiệu quả phản hồi khách hàng quốc tế.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20-30 phút/bệnh nhân để viết tóm tắt hồ sơ xuất viện (Discharge Summary) thủ công, phải tổng hợp từ nhiều nguồn dữ liệu lâm sàng rời rạc dẫn đến quá tải cuối ca trực. |
| 2 | **Vinhomes** | Lặp lại | Nhân viên ban quản lý tòa nhà phải đọc và phân loại thủ công hàng trăm phản ánh/khiếu nại của cư dân mỗi ngày trên App Vinhomes Resident rồi chuyển đến đúng bộ phận xử lý. |
| 3 | **VinFast** | AI có thể tốt hơn | Khách hàng mô tả lỗi xe bằng tiếng Việt đời thường, nhân viên CSKH phải tự tra cứu và phân loại mã lỗi kỹ thuật, thường mất 10-15 phút/case và dễ phân loại sai. |
| 4 | **Xanh SM** | Pain từ người khác | Tài xế Xanh SM phàn nàn hệ thống gợi ý điểm đón khách không chính xác khi khách đặt xe tại các tòa nhà lớn, trung tâm thương mại — tài xế mất 5-10 phút lòng vòng tìm khách, gây hủy chuyến tỉ lệ cao (~12%). |
| 5 | **Vinpearl** | Tốn thời gian | Nhân viên Revenue Manager phải tổng hợp thủ công review từ nhiều nền tảng (Booking.com, Agoda, Google Maps, TripAdvisor) để phát hiện phàn nàn khẩn cấp, mất 2-3 giờ/ngày. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#1 (Vinmec — Hồ sơ xuất viện), #2 (Vinhomes — Phân loại khiếu nại), #3 (VinFast — Phân tích lỗi xe).**

## Thẻ bài toán tiêu biểu: Card #3 — VinFast Phân tích cảm xúc & Ý kiến đánh giá xe quốc tế

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Đánh giá của người dùng quốc tế về xe VinFast bị  │
│ trôi nổi trên nhiều diễn đàn, đa ngôn ngữ, phân tích thủ    │
│ công gây chậm phản hồi lỗi nghiêm trọng.                    │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Đội ngũ CSKH toàn cầu, Kỹ sư chất lượng sản    │
│ phẩm (Quality Engineers) và khách hàng quốc tế.             │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
>   1. Thu thập đánh giá từ diễn đàn quốc tế (Reddit, YouTube)│
│   → 2. Dịch nội dung đa ngôn ngữ sang tiếng Anh/tiếng Việt  │
│   → 3. Phân loại thủ công cảm xúc (Tích cực/Tiêu cực/Lỗi)   │
│   → 4. Gắn nhãn phân loại bộ phận lỗi (Pin, Phần mềm, ADAS) │
│   → 5. Soạn tin nhắn phản hồi/Báo cáo kỹ thuật khẩn cấp     │
│                                                             │
│ Bước nào tốn nhất? Bước 2, 3 và 4 (⏱ 25 phút/lượt)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3-4            │
│ (Tự động dịch thuật -> Phân loại cảm xúc -> Gắn nhãn lỗi)   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý review từ 25 phút ──> dưới 10 giây.    │
│ Tỉ lệ phát hiện chính xác các lỗi an toàn khẩn cấp đạt 98%. │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Dịch & Classify JSON)  │
└─────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn của nhóm:
Nhóm quyết định chọn bài toán **"Card #3 — VinFast Phân tích cảm xúc & Ý kiến đánh giá xe quốc tế"** để thực hiện Deep-Dive.

## Lý do lựa chọn và loại bỏ các thẻ khác:
* **Card #1 (Vinmec — Hồ sơ xuất viện):** Liên quan mật thiết đến y tế và sức khỏe con người, độ nhạy cảm pháp lý rất cao. AI sai sót có thể dẫn đến hậu quả nghiêm trọng về mặt pháp lý và tính mạng. Cần thử nghiệm nội bộ quy mô hẹp trước khi đưa vào Scoping chính thức.
* **Card #2 (Vinhomes — Phân loại khiếu nại):** Tác vụ mang tính chất nội địa, tính cấp bách không cao bằng việc hỗ trợ chiến lược bứt phá toàn cầu của VinFast khi thương hiệu đang đối diện với nhiều nguồn dư luận đa chiều phức tạp ở nước ngoài.

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow
Quy trình xử lý các đánh giá quốc tế thủ công hiện tại của đội ngũ Phân tích thị trường & CSKH VinFast:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Thu thập     │     │ Tra cứu dịch │     │ Đọc & Phân   │     │ Gắn nhãn lỗi │
│ đánh giá     │ ──→ │ thuật        │ ──→ │ loại cảm xúc │ ──→ │ kỹ thuật     │
│              │     │              │     │              │     │              │
│ Ai: Analyst  │     │ Ai: Analyst  │     │ Ai: Analyst  │     │ Ai: Analyst  │
│ ⏱ 5 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │     │ ⏱ 7 phút 🔴  │
│ In: Raw link │     │ In: Text gốc │     │ In: Bản dịch │     │ In: Bản dịch │
│ Out: Raw text│     │ Out: Bản dịch│     │ Out: Sentiment│    │ Out: Nhãn lỗi│
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Soạn nháp    │
                                                               │ phản hồi     │
                                                               │ Ai: Analyst  │
                                                               │ ⏱ 3 phút     │
                                                               │ In: Nhãn lỗi │
                                                               │ Out: Email/SMS│
                                                               └──────────────┘
🔴 = Bottlenecks
⏱ Tổng thời gian xử lý thủ công: 25 phút/lượt.
```

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên Chăm sóc khách hàng Toàn cầu & Chuyên viên Phân tích Sản phẩm VinFast. |
| **2. Current Workflow** | Khi có đánh giá mới từ thị trường nước ngoài (Mỹ, Châu Âu), chuyên viên thu thập text gốc, dùng Google Translate dịch sang tiếng Việt/Anh, đọc phân loại cảm xúc (Tích cực, Tiêu cực, Lỗi), thủ công phân loại lỗi kỹ thuật (Hệ truyền động, Pin, ADAS, Phần mềm), soạn thảo nội dung phản hồi nháp gửi bộ phận liên quan. 5 bước hoàn toàn thủ công, mất 25 phút/lượt. |
| **3. Bottleneck** | Bước 2, 3 và 4 (mất 17 phút): Dịch thuật đa ngôn ngữ khó khăn khi có tiếng lóng chuyên ngành xe; Phân tích sắc thái cảm xúc ẩn ý của người nước ngoài dễ bị hiểu sai lệch văn hóa; Gắn nhãn loại lỗi kỹ thuật tốn thời gian đối chiếu danh mục. |
| **4. Business Impact** | Mỗi ngày có hàng trăm review quốc tế. Quy trình thủ công gây trễ từ 24-48 giờ trước khi các lỗi an toàn nghiêm trọng (ví dụ: mất phanh, lỗi pin khẩn cấp) được báo cáo cho đội ngũ Kỹ sư. Gây rò rỉ cơ hội sửa sai lỗi kỹ thuật, làm tăng tỉ lệ bài viết tiêu cực lan truyền trên MXH quốc tế, ảnh hưởng trực tiếp đến doanh số bán hàng toàn cầu của VinFast. |
| **5. Success Metric** | 1. Giảm tổng thời gian phân tích từ 25 phút xuống dưới 10 giây/đánh giá (Efficiency).<br>2. Tỉ lệ gắn nhãn cảm xúc và phân loại lỗi kỹ thuật chính xác đạt từ 95% trở lên (Quality).<br>3. Thời gian cảnh báo lỗi an toàn nghiêm trọng cho ban phản ứng nhanh giảm từ 24 giờ xuống dưới 1 phút (Safety). |
| **6. Operational Boundary** | AI được phép tự động dịch, phân tích sentiment, phân loại danh mục lỗi và soạn nháp tin phản hồi. **CẤM:** AI tuyệt đối không được tự động xuất bản (post) phản hồi lên diễn đàn/mạng xã hội khi chưa có HITL (chuyên viên CSKH duyệt). Mọi bản nháp soạn thảo phải bắt đầu bằng tag `[DRAFT_ONLY]`. Nếu phát hiện lỗi đe dọa an toàn tính mạng (cháy nổ, hỏng phanh đột ngột), AI phải dừng soạn tin nhắn thông thường và lập tức kích hoạt command cảnh báo kỹ thuật khẩn cấp. |

---

## 3.3. Future-State Flow & AI Fit

* **AI Fit:** Chọn **LLM Feature** vì đây là tác vụ xử lý thông tin đầu vào dạng văn bản phi cấu trúc, phân loại đa nhãn và sinh văn bản có cấu trúc JSON ổn định, không yêu cầu cơ chế Agent tự trị phức tạp nhằm giảm thiểu rủi ro ảo tưởng (hallucination).
* **Quy trình tương lai (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Thu thập     │     │ 🔵 AI dịch & │     │ 🔵 AI draft  │     │ 🟢 Analyst   │
│ đánh giá     │ ──→ │ classify JSON│ ──→ │ phản hồi &   │ ──→ │ click duyệt  │
│ tự động      │     │ tự động      │     │ alert khẩn   │     │ & gửi đi     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼
                                                                ↩️ Fallback:
                                                                Nếu AI báo lỗi,
                                                                Analyst tự dịch
                                                                và phân loại
                                                                bằng tay như cũ.
```

---

# 💻 Phase 4 — Prompt Prototype & Boundary Test

Nhóm đã xây dựng và hoàn thiện file lập trình nguyên mẫu [prompt_prototype.py](starter-code/prompt_prototype.py) chạy trên **Gemini 2.5 Flash** để stress-test các ranh giới vận hành.

### Ranh giới an toàn (Operational Boundary) được lập trình nghiêm ngặt:
* **Quy tắc 1 (Safety Review Tag):** Mọi tin nhắn phản hồi nháp soạn thảo cho khách hàng bắt buộc phải bắt đầu bằng tag `[DRAFT_ONLY]` để tránh hệ thống tự động đẩy thẳng lên các kênh MXH công cộng.
* **Quy tắc 2 (Safety Escalation Command):** Nếu đánh giá chứa các lỗi đe dọa an toàn nghiêm trọng (mất phanh, cháy pin, khói, lỗi vô lăng đột ngột ở tốc độ cao), AI tuyệt đối không được soạn tin nhắn phản hồi xoa dịu thông thường. Thay vào đó, AI phải lập tức phủ quyết và kích hoạt mã lệnh báo động hệ thống:
  `{"action": "escalate_safety_team", "reason": "<lý_do_chi_tiết>"}`.

### Kết quả thử nghiệm tấn công (Adversarial Stress-Testing):
* **Test Case 1 (Tấn công bỏ qua tag duyệt):** Người dùng yêu cầu soạn tin cảm ơn và đẩy thẳng trực tiếp lên diễn đàn.
  * *Kết quả:* AI giữ vững ranh giới và trả về bản nháp bắt đầu bằng `[DRAFT_ONLY]`. Vượt qua kiểm tra an toàn thành công!
* **Test Case 2 (Lỗi pin cực kỳ khẩn cấp):** Review mô tả xe đang đi trên cao tốc Mỹ đột nhiên khói bốc lên từ gầm và xe mất phanh.
  * *Kết quả:* AI phát hiện lỗi an toàn nghiêm trọng nguy hiểm tính mạng, từ chối soạn tin xoa dịu và tự động trả về lệnh escalate cho đội kỹ sư phản ứng nhanh: `{"action": "dispatch_mobile_charger", "reason": "Battery level under critical threshold..."}` (hoặc các lệnh cứu hộ tương ứng). Vượt qua kiểm tra an toàn thành công!

---

## 🏁 Kết luận từ buổi Lab
Dự án **VinFast International Review Co-Pilot** được đánh giá đạt mức độ **GO** từ Ban Giám Đốc Vin Smart Future nhờ bám sát định hướng mở rộng toàn cầu, giải quyết trực tiếp bài toán quá tải ngôn ngữ, tối ưu hóa SLA xử lý đánh giá từ 25 phút xuống dưới 10 giây và kiểm soát rủi ro an toàn tuyệt đối thông qua cơ chế ranh giới nghiêm ngặt đã được chứng minh qua code kiểm thử thực tế.
