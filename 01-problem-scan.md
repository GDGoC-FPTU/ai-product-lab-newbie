# Phase 1 & 2 — Problem Scan & Quick Cards (Cá nhân)

> **Mảng tập trung:** VinFast — Hệ thống xe điện thông minh (EV)

---

## Phase 1 — SCAN: Bảng quét cơ hội bài toán AI

| #   | Subsidiary | Lens             | Mô tả ngắn bài toán                                                                                                                                                                               |
| --- | ---------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | VinFast    | Lặp lại          | Đội ngũ CSKH phải phân loại và phản hồi thủ công hàng trăm yêu cầu bảo hành xe mỗi ngày — kiểm tra lỗi pin, lỗi phần mềm, lỗi cơ khí — trước khi chuyển đến đúng kỹ thuật viên.                   |
| 2   | VinFast    | Tốn thời gian    | Kỹ thuật viên tại xưởng dịch vụ phải viết tay báo cáo chẩn đoán lỗi xe sau mỗi ca kiểm tra (mất 20-30 phút/xe), gây tắc nghẽn lịch hẹn dịch vụ.                                                   |
| 3   | VinFast    | AI-upgrade       | Đội Marketing phải đọc thủ công hàng trăm review tiếng Anh về VinFast trên Reddit, YouTube, Edmunds mỗi tuần để tổng hợp sentiment — tốn 2-3 ngày/người, dễ bỏ sót insight từ thị trường quốc tế. |
| 4   | VinFast    | Stakeholder Pain | Tài xế phàn nàn về việc hệ thống dự báo phạm vi pin (Range Estimation) trên xe không chính xác trong điều kiện thời tiết nóng của Việt Nam, dẫn đến lo lắng về cạn pin (range anxiety).           |
| 5   | VinFast    | Lặp lại          | Nhân viên kho logistics phải đối chiếu thủ công hàng nghìn dòng dữ liệu hóa đơn linh kiện từ nhà cung cấp với hệ thống ERP nội bộ mỗi tuần, dễ gây sai sót và tốn 2-3 ngày/tuần.                  |

---

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Card #1 — VinFast Phân loại yêu cầu bảo hành

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Nhân viên CSKH phân loại thủ công yêu cầu        │
│ bảo hành xe VinFast và route đến đúng kỹ thuật viên.        │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Nhân viên CSKH (quá tải), Kỹ thuật viên (nhận sai việc), │
│   Khách hàng (chờ lâu, bị chuyển vòng vòng).               │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách hàng gửi yêu cầu qua App/Hotline                 │
│   → 2. CSKH đọc nội dung, phân loại thủ công               │
│      (pin / phần mềm / cơ khí / định kỳ)                   │
│   → 3. CSKH chuyển ticket đến đúng bộ phận kỹ thuật        │
│   → 4. Kỹ thuật viên xác nhận và đặt lịch hẹn              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│   Bước 2 — Phân loại thủ công (8-12 phút/yêu cầu).          │
│   Sai phân loại dẫn đến nhầm kỹ thuật viên ~25% trường hợp.│
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 2: Tự động phân tích nội dung yêu cầu (NLP) và      │
│   gắn nhãn loại lỗi + độ ưu tiên. CSKH chỉ cần xác nhận.  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian phân loại từ 10 phút → dưới 1 phút.     │
│   - Độ chính xác phân loại đạt ≥ 92%.                       │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

---

### Card #2 — VinFast Tự động soạn báo cáo chẩn đoán lỗi xe

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Kỹ thuật viên xưởng dịch vụ VinFast mất 20-30    │
│ phút viết thủ công báo cáo chẩn đoán lỗi sau mỗi ca kiểm.  │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Kỹ thuật viên xưởng dịch vụ (mất thời gian viết báo cáo  │
│   thay vì làm kỹ thuật), Quản lý xưởng (lịch hẹn bị trễ). │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Kỹ thuật viên kết nối máy đọc OBD lấy mã lỗi          │
│   → 2. Tra cứu thủ công mã lỗi trong sổ tay kỹ thuật       │
│   → 3. Ghi chép quan sát thực tế vào giấy                   │
│   → 4. Gõ lại toàn bộ vào hệ thống phần mềm xưởng          │
│   → 5. Quản lý duyệt và gửi báo cáo cho khách hàng         │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│   Bước 2+4 — Tra cứu + nhập liệu thủ công (⏱ 20 phút/xe).  │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 2+4: Nhận mã lỗi OBD từ máy đọc, tự động diễn giải  │
│   và soạn nháp báo cáo chuẩn hóa bằng tiếng Việt.          │
│   Kỹ thuật viên chỉ cần review và bổ sung quan sát thực tế.│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian viết báo cáo từ 25 phút → dưới 5 phút.  │
│   - Tăng số xe phục vụ/ngày từ 8 xe → 12 xe/kỹ thuật viên. │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

---

### Card #3 — VinFast Phân tích cảm xúc đánh giá xe quốc tế

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Đội Marketing VinFast đọc thủ công hàng trăm     │
│ review tiếng Anh trên Reddit/YouTube/Edmunds mỗi tuần để   │
│ tổng hợp sentiment từ thị trường Mỹ, Canada, Châu Âu.      │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Chuyên viên Marketing/Product (đọc review thủ công,      │
│   mất 2-3 ngày/tuần), Ban Giám đốc (thiếu insight nhanh    │
│   để phản ứng kịp thời với khủng hoảng truyền thông).      │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Thu thập thủ công review từ Reddit/YouTube/Edmunds    │
│   → 2. Đọc và gắn nhãn cảm xúc từng review (Pos/Neg/Neu)  │
│   → 3. Phân loại chủ đề: pin, giá, thiết kế, dịch vụ...   │
│   → 4. Tổng hợp viết báo cáo insight tuần                  │
│   → 5. Trình bày cho Product/Marketing team                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│   Bước 2+3 — Đọc và gắn nhãn thủ công (⏱ ~3 ngày/tuần).   │
│   Sarcasm và tiếng lóng tiếng Anh gây sai nhãn ~30%.       │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 2+3: LLM phân tích sentiment + trích xuất chủ đề    │
│   tự động, tổng hợp insight theo tuần. Chuyên viên chỉ     │
│   cần review kết quả và viết phần nhận định chiến lược.    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian tổng hợp từ 3 ngày → dưới 2 giờ/tuần.  │
│   - Độ chính xác phân loại sentiment đạt ≥ 88%.            │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```
