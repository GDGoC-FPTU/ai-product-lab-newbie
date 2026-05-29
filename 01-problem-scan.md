# Phase 1 & 2 — Báo cáo cá nhân: Scan & Quick Problem Cards

> **Họ và tên:** Lê Dương Hiếu  
> **MSSV:** 2A202600635  
> **Ngày thực hiện:** 29/05/2026

---

## 🔍 Phase 1 — SCAN: Quét cơ hội AI cho các công ty thành viên Vingroup

Sử dụng **4 Lenses** để quét qua hoạt động vận hành của các công ty thành viên Vingroup.

### 📝 List bài toán của tôi:

| # | Subsidiary (Công ty) | Lens | Mô tả ngắn bài toán |
|---|----------------------|------|---------------------|
| 1 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20-30 phút/bệnh nhân để viết tóm tắt hồ sơ xuất viện (Discharge Summary) thủ công, phải tổng hợp từ nhiều nguồn dữ liệu lâm sàng rời rạc (kết quả xét nghiệm, ghi chú khám, đơn thuốc) dẫn đến quá tải cuối ca trực. |
| 2 | **Vinhomes** | Lặp lại | Nhân viên ban quản lý tòa nhà phải đọc và phân loại thủ công hàng trăm phản ánh/khiếu nại của cư dân mỗi ngày trên App Vinhomes Resident (mất nước, hỏng thang máy, ồn ào, vi phạm nội quy...) rồi chuyển đến đúng bộ phận xử lý. |
| 3 | **VinFast** | AI có thể tốt hơn | Khách hàng mô tả lỗi xe bằng tiếng Việt đời thường (VD: "xe đi qua gờ giảm tốc kêu lọc cọc ở gầm bên phải"), nhân viên CSKH phải tự tra cứu và phân loại mã lỗi kỹ thuật, thường mất 10-15 phút/case và dễ phân loại sai. |
| 4 | **Xanh SM** | Pain từ người khác | Tài xế Xanh SM phàn nàn hệ thống gợi ý điểm đón khách không chính xác khi khách đặt xe tại các tòa nhà lớn, trung tâm thương mại — tài xế mất 5-10 phút lòng vòng tìm khách, gây hủy chuyến tỉ lệ cao (~12%). |
| 5 | **Vinpearl** | Tốn thời gian | Nhân viên Revenue Manager phải tổng hợp thủ công review từ nhiều nền tảng (Booking.com, Agoda, Google Maps, TripAdvisor) để phát hiện phàn nàn khẩn cấp (phòng bẩn, dịch vụ kém), mất 2-3 giờ/ngày và thường bỏ sót review tiêu cực. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn **top 3 bài toán** tiềm năng nhất: **#1 (Vinmec — Hồ sơ xuất viện), #2 (Vinhomes — Phân loại khiếu nại), #3 (VinFast — Chẩn đoán lỗi xe).**

---

### 📋 Card #1 — Vinmec: Tự động soạn thảo tóm tắt hồ sơ xuất viện

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Bác sĩ Vinmec mất quá nhiều thời gian    │
│ viết tóm tắt hồ sơ xuất viện (Discharge Summary) thủ công,  │
│ cần AI hỗ trợ soạn nháp tự động từ dữ liệu bệnh án.        │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị (quá tải cuối ca)     │
│   và bệnh nhân (chờ xuất viện lâu).                         │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Bác sĩ mở hệ thống HIS tra cứu bệnh án điện tử       │
│   → 2. Đọc lại toàn bộ kết quả xét nghiệm + ghi chú khám  │
│   → 3. Tổng hợp thông tin và viết tay bản tóm tắt          │
│   → 4. Kiểm tra lại đơn thuốc và lịch tái khám             │
│   → 5. In ấn và ký xác nhận giao cho bệnh nhân             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 20 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3             │
│ (Auto-extract dữ liệu từ HIS → Draft bản tóm tắt)          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian soạn tóm tắt xuất viện từ 25 phút            │
│ ──> dưới 5 phút/bệnh nhân. Tỉ lệ bản nháp được bác sĩ     │
│ duyệt không cần sửa lớn đạt ≥ 90%.                          │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (LLM đọc dữ liệu HIS → Draft tóm tắt → Bác sĩ duyệt)     │
└─────────────────────────────────────────────────────────────┘
```

---

### 📋 Card #2 — Vinhomes: Phân loại & Điều hướng phản ánh cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại tự động hàng trăm phản ánh cư  │
│ dân gửi qua App Vinhomes Resident mỗi ngày và điều hướng    │
│ đến đúng bộ phận quản lý xử lý, thay vì phân loại thủ công.│
│ Công ty thành viên: [x] Vinhomes                             │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH Ban Quản Lý tòa nhà    │
│   (xử lý quá tải), cư dân (chờ phản hồi > 12 giờ).         │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhận phản ánh của cư dân qua App/hotline               │
│   → 2. Nhân viên đọc nội dung, phân loại thủ công           │
│     (điện, nước, thang máy, an ninh, tiếng ồn, v.v.)       │
│   → 3. Chuyển ticket đến đúng bộ phận/tầng/tòa nhà         │
│   → 4. Soạn phản hồi xác nhận gửi lại cư dân               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 8 phút/lượt)  │
│   Phân loại sai bộ phận xảy ra ~15% → delay thêm 6-24h.    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3             │
│ (Auto-classify → Auto-route → Draft phản hồi)               │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ 95% phản ánh được phân loại chính xác dưới 30 giây.          │
│ Thời gian phản hồi cư dân giảm từ 12h ──> dưới 2h.          │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (LLM classify phản ánh + route + draft phản hồi tự động)    │
└─────────────────────────────────────────────────────────────┘
```

---

### 📋 Card #3 — VinFast: Chẩn đoán mã lỗi xe từ mô tả tiếng Việt

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Khách hàng VinFast mô tả lỗi xe bằng    │
│ ngôn ngữ đời thường (tiếng Việt), cần AI phân loại mã lỗi  │
│ kỹ thuật ban đầu để tăng tốc quy trình tiếp nhận sửa chữa. │
│ Công ty thành viên: [x] VinFast                              │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận dịch vụ (Service   │
│   Advisor) tại hãng và khách hàng (chờ đợi lâu).            │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gọi hotline/đến xưởng mô tả triệu chứng         │
│   → 2. Nhân viên SA nghe và ghi chú thủ công                │
│   → 3. Tra cứu bảng mã lỗi kỹ thuật (hàng nghìn mã)       │
│   → 4. Phân loại sơ bộ và tạo phiếu sửa chữa               │
│   → 5. Chuyển xe đến kỹ thuật viên chuyên trách             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 12 phút/lượt) │
│   Phân loại sai mã lỗi xảy ra ~20% → xe phải chẩn đoán lại.│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3             │
│ (NLP → Mapping mô tả → Top 3 mã lỗi gợi ý)                 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian phân loại từ 12 phút ──> dưới 2 phút.        │
│ Tỉ lệ phân loại đúng mã lỗi ban đầu đạt ≥ 85%.             │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (LLM phân tích mô tả tiếng Việt → Gợi ý top-3 mã lỗi      │
│  → SA xác nhận → Tạo phiếu tự động)                         │
└─────────────────────────────────────────────────────────────┘
```
