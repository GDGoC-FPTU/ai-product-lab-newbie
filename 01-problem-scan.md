# 🔍 Phase 1 — SCAN

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | VinFast | Lặp lại (Repetitive) | Phân loại và gán nhãn hàng ngàn phản hồi của khách hàng (feedback) từ các nguồn khác nhau về xe điện để chuyển đến đúng bộ phận. |
| 2 | Xanh SM | Tốn thời gian (Time-consuming) | Tổng đài viên phải thao tác thủ công trên nhiều phần mềm để điều xe cho khách ở khu vực chưa có xe trống gần đó. |
| 3 | Vinhomes | AI-upgrade | BQL tòa nhà phải đọc thủ công các yêu cầu sửa chữa của cư dân (điện, nước) để phân loại và gọi kỹ thuật viên phù hợp. |
| 4 | Vinmec | Tốn thời gian (Time-consuming) | Bác sĩ mất nhiều thời gian nhập liệu, gõ tay tóm tắt hồ sơ bệnh án vào phần mềm HIS sau mỗi ca khám. |
| 5 | Vinpearl | Stakeholder Pain | Khách hàng phải chờ đợi lâu ở quầy lễ tân để check-in và nhận tư vấn lịch trình do nhân viên quá tải xử lý giấy tờ định danh. |

---

# 🃏 Phase 2 — QUICK-ASSESS

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và định tuyến phản hồi/lỗi xe điện của khách hàng tới đúng bộ phận kỹ thuật. │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH & Kỹ sư tiếp nhận lỗi   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách gửi lỗi ──> 2. CSKH đọc nội dung ──> 3. CSKH phân loại lỗi (phần mềm/cơ khí) ──> 4. Chuyển ticket cho bộ phận │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (⏱ 5 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 - Đọc nội dung text/hình ảnh lỗi và tự động phân loại + gán nhãn ưu tiên. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian phân loại ticket từ 5 min ──> under 1 min, độ chính xác >90%" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động trích xuất thông tin và điều phối kỹ thuật viên xử lý sự cố căn hộ cho cư dân. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Ban quản lý tòa nhà          │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân báo lỗi qua app ──> 2. BQL đọc, hiểu vấn đề ──> 3. BQL tra cứu lịch thợ ──> 4. BQL tạo phiếu điều việc │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 10 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 - Đọc mô tả của cư dân, xác định loại thợ và gợi ý thợ đang trống lịch. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian xử lý yêu cầu/điều thợ từ 10 min ──> under 2 min" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Hỗ trợ bác sĩ tự động tóm tắt và điền hồ sơ bệnh án (Clinical Notes) từ giọng nói lúc khám. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ khám bệnh                       │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khám bệnh ──> 2. Bác sĩ tự nhớ/ghi nháp ──> 3. Mở phần mềm HIS ──> 4. Gõ tay tóm tắt bệnh án vào hệ thống │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 4 (⏱ 15 phút/ca)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4 - Lắng nghe (Voice-to-Text) hội thoại lúc khám và bóc tách thành bệnh án chuẩn. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian làm hồ sơ bệnh án từ 15 min/ca ──> under 3 min/ca" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
