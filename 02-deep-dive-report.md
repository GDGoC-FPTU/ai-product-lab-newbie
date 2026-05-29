# Deep-Dive Report — Vin Smart Future (VinFast)

## Thông tin nhóm

| | |
|---|---|
| **Tên nhóm** | Newbie |
| **Thành viên 1** | Nguyễn Trường Phúc — MSSV: 2A202600767 |
| **Thành viên 2** | Vũ Đăng Khiêm — MSSV: 2A202600727 |
| **Thành viên 3** | Lê Dương Hiếu — MSSV: 2A202600635 |
| **Thành viên 4** | Hoàng Hải Đăng — MSSV: 2A202600916 |

---

## Quyết định lựa chọn đề tài

Nhóm chọn bài toán: **"Phân tích cảm xúc đánh giá xe VinFast trên thị trường quốc tế"**

### Lý do chọn:
- Đây là bài toán **có dữ liệu sẵn có** (review công khai trên Reddit, YouTube, Edmunds, Cars.com) — không cần thu thập dữ liệu nội bộ.
- **Business impact rõ ràng và đo được:** VinFast đang mở rộng sang Mỹ, Canada, Châu Âu — insight từ thị trường quốc tế có giá trị chiến lược cao.
- **AI Fit tốt:** LLM xử lý ngôn ngữ tự nhiên, sarcasm, tiếng lóng tiếng Anh tốt hơn hẳn rule-based keyword matching.

### Lý do loại bỏ các đề tài khác:
- **Phân loại yêu cầu bảo hành:** Có thể giải bằng rule-based keyword matching đơn giản, chưa đủ justification cho LLM.
- **Tự động soạn báo cáo chẩn đoán OBD:** Cần tích hợp sâu với hệ thống phần mềm xưởng dịch vụ nội bộ — dependency cao, khó prototype nhanh.

---

# Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow (Quy trình hiện tại)

Quy trình tổng hợp sentiment review quốc tế thủ công của đội Marketing VinFast:

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Thu thập     │     │ Đọc & gắn    │     │ Phân loại    │     │ Tổng hợp     │
│ review thủ   │ ──→ │ nhãn cảm xúc │ ──→ │ chủ đề:      │ ──→ │ viết báo cáo │
│ công từ      │     │ từng review  │     │ pin, giá,    │     │ insight      │
│ Reddit/YT/   │     │ Pos/Neg/Neu  │     │ thiết kế,    │     │ tuần         │
│ Edmunds      │     │              │     │ dịch vụ...   │     │              │
│ Ai: Marketing│     │ Ai: Marketing│     │ Ai: Marketing│     │ Ai: Marketing│
│ ⏱ 3 giờ     │     │ ⏱ 8 giờ 🔴  │     │ ⏱ 6 giờ 🔴  │     │ ⏱ 3 giờ     │
│ Out: Raw text│     │ Out: Labels  │     │ Out: Tags    │     │ Out: Report  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Trình bày    │
                                                               │ cho Product/ │
                                                               │ Marketing    │
                                                               │ ⏱ 2 giờ     │
                                                               └──────────────┘
🔴 = Bottlenecks (Bước 2 + Bước 3)
🔄 Handoff: Bước 4 → Bước 5 (Marketing → Product/Ban Giám đốc)
⏱ Tổng thời gian xử lý thủ công: ~22 giờ/tuần (~3 ngày làm việc).
```

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Chuyên viên Marketing và Product của VinFast tại bộ phận phân tích thị trường quốc tế. |
| **2. Current Workflow** | Mỗi tuần, chuyên viên Marketing thu thập thủ công 200-400 review tiếng Anh từ Reddit (r/vinfast, r/electricvehicles), YouTube (video test drive, review), Edmunds, Cars.com. Sau đó đọc từng review, gắn nhãn cảm xúc (Positive/Negative/Neutral), phân loại theo chủ đề (pin, phạm vi, giá, thiết kế, chất lượng, dịch vụ hậu mãi), tổng hợp viết báo cáo insight trình bày cho team Product và Ban Giám đốc. Toàn bộ quy trình thủ công, mất ~22 giờ/tuần/người. |
| **3. Bottleneck** | Bước 2 & 3 (mất ~14 giờ): Đọc và gắn nhãn cảm xúc thủ công từng review. Sarcasm và tiếng lóng tiếng Anh (ví dụ: *"Oh great, another VinFast quirk"*) gây sai nhãn ~30% trường hợp. Khi có sự kiện ra mắt xe mới hoặc khủng hoảng truyền thông, lượng review tăng đột biến 3-5x khiến quy trình không kịp đáp ứng. |
| **4. Business Impact** | ~22 giờ nhân công/tuần bị lãng phí vào tác vụ lặp lại. Phản hồi chậm với khủng hoảng truyền thông (mất 3-5 ngày mới có insight) dẫn đến thiệt hại hình ảnh thương hiệu khó đo lường. Ước tính chi phí nhân công lãng phí: ~$800-1,200 USD/tuần (1 FTE tại thị trường Mỹ). |
| **5. Success Metric** | 1. Giảm thời gian tổng hợp sentiment từ ~22 giờ/tuần → dưới 2 giờ/tuần (Efficiency: giảm 90%). 2. Độ chính xác phân loại sentiment đạt ≥ 88% (so với ground truth do chuyên viên review). 3. Thời gian phát hiện khủng hoảng truyền thông từ 3-5 ngày → dưới 4 giờ (Responsiveness). |
| **6. Operational Boundary** | **AI được phép:** Phân loại sentiment (Positive/Negative/Neutral), trích xuất chủ đề, tóm tắt insight theo tuần, gắn flag "khủng hoảng" khi tỉ lệ Negative > 60% trong 24 giờ. **TUYỆT ĐỐI CẤM:** AI không được tự động publish bất kỳ phản hồi nào ra mạng xã hội; không được đưa ra cam kết thay đổi sản phẩm; không được tự động gửi báo cáo đến đối tác/báo chí mà không có phê duyệt của Marketing Manager (bắt buộc HITL). |

---

## 3.3. Future-State Flow & AI Fit

**AI Fit:** Chọn **LLM Feature** — không cần Agentic Loop vì quy trình một chiều (input: text review → output: JSON phân tích), không cần AI tự trị ra quyết định. Rule-based keyword matching không đủ xử lý sarcasm và ngôn ngữ tự nhiên phức tạp của reviewer quốc tế.

**Quy trình tương lai (Future-State):**

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ 🔵 Auto-     │     │ 🔵 AI phân   │     │ 🔵 AI tổng   │     │ 🟢 Marketing │
│ crawl review │ ──→ │ tích sentiment│ ──→ │ hợp insight  │ ──→ │ Manager      │
│ Reddit/YT/   │     │ + chủ đề     │     │ + flag khủng │     │ review &     │
│ Edmunds      │     │ từng review  │     │ hoảng        │     │ approve báo  │
│              │     │              │     │              │     │ cáo          │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI confidence
                                                               < 70%, flag sang
                                                               chuyên viên đọc
                                                               thủ công.
```

**Chú thích:**
- 🔵 **AI Step:** LLM xử lý tự động
- 🟢 **Human Step (HITL):** Marketing Manager phê duyệt trước khi báo cáo được gửi đi
- ↩️ **Fallback:** Review có confidence < 70% được đẩy sang hàng chờ để chuyên viên xử lý thủ công

---

# Phase 5 — EVALUATE

## AI Readiness Checklist

| # | Câu hỏi | Đánh giá |
|---|---|---|
| 1 | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | **Có** — Review công khai trên Reddit/YouTube/Edmunds, không cần dữ liệu nội bộ |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát? | **Có** — AI chỉ tạo báo cáo nháp, Marketing Manager phê duyệt trước khi gửi (HITL). Sai nhãn sentiment chỉ ảnh hưởng nội bộ, không public |
| 3 | Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? | **Có** — Đội Marketing đang chịu áp lực lớn từ khối lượng review tăng nhanh khi VinFast mở rộng thị trường |

## Quyết định cuối cùng

**[x] GO — Bắt đầu xây dựng Prototype với scope hẹp**

**Justification:**

> Bài toán đủ điều kiện GO vì: **(1) Dữ liệu sẵn có** — review công khai không cần xin phép hay tích hợp hệ thống nội bộ phức tạp; **(2) Rủi ro thấp** — AI chỉ tạo báo cáo nháp, mọi quyết định đều qua HITL của Marketing Manager, sai nhãn không gây hậu quả trực tiếp với khách hàng hay pháp lý; **(3) ROI rõ ràng** — tiết kiệm ~20 giờ nhân công/tuần (~$1,000 USD/tuần), payback period dưới 1 tháng với chi phí API Gemini ước tính ~$50-100 USD/tuần cho 300-400 review; **(4) LLM vượt trội rule-based** — sarcasm và tiếng lóng tiếng Anh của reviewer quốc tế không thể xử lý bằng keyword matching đơn giản; **(5) Prototype kỹ thuật** đã được stress-test ranh giới an toàn thành công với Gemini 2.5 Flash.
>
> **Scope prototype đề xuất:** Bắt đầu với 50 review/tuần từ Reddit r/vinfast, đo accuracy so với ground truth của chuyên viên, sau 4 tuần mở rộng lên toàn bộ pipeline nếu accuracy ≥ 88%.
