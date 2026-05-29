# 📝 AI Log & Reflection — Nhật ký chiêm nghiệm AI

> **Họ và tên:** Lê Dương Hiếu  
> **MSSV:** 2A202600635  
> **Ngày thực hiện:** 29/05/2026

---

## 1. AI đã giúp gì trong buổi Lab?

Trong suốt buổi Lab hôm nay, tôi đã sử dụng AI (Gemini, Claude) làm trợ lý đồng hành (thought-partner) cho nhiều tác vụ khác nhau:

### 🧠 Brainstorm ý tưởng bài toán (Phase 1 — SCAN)
- Tôi đã dùng prompt: *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng Vinmec và Vinhomes. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*
- AI giúp tôi nhanh chóng có được danh sách các pain point thực tế với số liệu ước lượng, tiết kiệm thời gian nghiên cứu đáng kể.
- AI cũng giúp tôi phân loại các bài toán theo 4 Lenses (Lặp lại, Tốn thời gian, AI-upgrade, Stakeholder Pain) một cách có hệ thống.

### 🛡️ Viết System Prompt & Thiết lập ranh giới an toàn (Phase 4)
- Tôi nhờ AI hỗ trợ cấu trúc system prompt theo best practices: chia thành các phần rõ ràng (Vai trò, Quy tắc, Ranh giới cấm).
- AI gợi ý thêm các edge case cần xử lý mà tôi chưa nghĩ đến, ví dụ: trường hợp tài xế giả danh quản lý để yêu cầu bỏ quy tắc an toàn (social engineering).

### 🔧 Hỗ trợ debug code Python
- Khi gặp lỗi kết nối Gemini API, tôi dán error message vào AI và nhận được hướng dẫn sửa lỗi chính xác (cách cài đặt đúng SDK `google-genai`, cách truyền `system_instruction` qua `GenerateContentConfig`).

---

## 2. AI đã sai gì? (Hallucination & Limitations)

### ❌ Hallucination về số liệu thống kê
- Khi tôi hỏi AI về số liệu vận hành cụ thể của Xanh SM (số lượng sự cố pin trung bình mỗi ngày tại Hà Nội), AI tự tin đưa ra con số "~120 sự cố/ngày" kèm nguồn trích dẫn. Tuy nhiên, khi tôi kiểm tra lại, nguồn trích dẫn đó không tồn tại — đây là hallucination điển hình. Con số thực tế không có dữ liệu công khai để xác minh.

### ❌ Đề xuất giải pháp quá phức tạp
- Ban đầu khi tôi hỏi AI về kiến trúc cho bài toán phân loại khiếu nại cư dân Vinhomes, AI đề xuất xây dựng hệ thống Multi-Agent với 5 agents chuyên biệt (Classification Agent, Routing Agent, Response Agent, Escalation Agent, Feedback Agent). Đây là giải pháp over-engineering — một LLM Feature đơn giản với prompt phân loại và rule-based routing hoàn toàn đủ cho scope ban đầu.

### ❌ System prompt bị bypass trong lần thử đầu
- Phiên bản system prompt đầu tiên tôi viết (theo gợi ý của AI) khá chung chung, chỉ nói "hãy tuân thủ quy tắc an toàn". Khi chạy test adversarial, mô hình Gemini đã bỏ qua thẻ `[DRAFT_ONLY]` khi bị người dùng yêu cầu "gửi thẳng đi". Điều này cho thấy prompt cần phải cực kỳ rõ ràng và lặp lại nhiều lần các ranh giới cấm.

---

## 3. Tôi đã sửa đổi ra sao?

### ✅ Xác minh chéo mọi số liệu
- Sau khi phát hiện AI hallucinate số liệu, tôi áp dụng nguyên tắc: **không bao giờ trích dẫn số liệu từ AI mà không có nguồn xác minh độc lập**. Thay vào đó, tôi sử dụng các ước lượng hợp lý dựa trên logic vận hành (VD: "~80 sự cố/ngày" dựa trên quy mô đội xe Xanh SM tại Hà Nội khoảng 3,000 xe).

### ✅ Đơn giản hóa kiến trúc
- Tôi phản biện lại gợi ý Multi-Agent của AI bằng câu hỏi: *"Giải pháp rule-based đơn giản có thể giải quyết bài toán này ở mức nào? Tại sao tôi cần đến Multi-Agent thay vì một LLM Feature?"*. AI sau đó thừa nhận rằng LLM Feature kết hợp rule-based routing là đủ cho MVP, và Multi-Agent chỉ cần thiết khi scale lên hàng nghìn danh mục phân loại.

### ✅ Viết lại system prompt chi tiết và cứng rắn hơn
- Tôi sửa system prompt theo nguyên tắc:
  1. **Lặp lại quy tắc nhiều lần** ở các phần khác nhau (Vai trò, Quy tắc, Ranh giới cấm) để tăng khả năng mô hình tuân thủ.
  2. **Dùng từ ngữ tuyệt đối**: "TUYỆT ĐỐI KHÔNG ĐƯỢC", "BẮT BUỘC", "MỌI output PHẢI" thay vì "nên", "hãy cố gắng".
  3. **Liệt kê cụ thể các kịch bản tấn công**: nêu rõ nếu người dùng yêu cầu bỏ thẻ thì phải từ chối, nếu giả danh cấp trên thì vẫn phải tuân thủ.
- Kết quả: Sau khi sửa, cả 3 adversarial test cases đều pass — mô hình giữ vững `[DRAFT_ONLY]`, từ chối đề xuất trạm sạc xa khi pin thấp, và không bị social engineering bypass.

---

## 4. Bài học rút ra

> **AI là công cụ mạnh mẽ nhưng cần được kiểm soát chặt chẽ.** Trong suốt buổi lab, tôi nhận ra rằng:
> 
> 1. **AI giỏi brainstorm nhưng kém ở fact-checking** — luôn cần xác minh chéo mọi số liệu và nguồn trích dẫn.
> 2. **AI có xu hướng over-engineering** — cần phản biện và đặt câu hỏi "Giải pháp đơn giản nhất là gì?" trước khi chấp nhận đề xuất phức tạp.
> 3. **Prompt engineering là kỹ năng then chốt** — sự khác biệt giữa một system prompt "vừa đủ" và "thực sự an toàn" nằm ở chi tiết, sự lặp lại, và ngôn ngữ tuyệt đối.
> 4. **Adversarial testing là bắt buộc** — không thể tin tưởng AI sẽ tuân thủ ranh giới nếu không chủ động tấn công thử nghiệm.
