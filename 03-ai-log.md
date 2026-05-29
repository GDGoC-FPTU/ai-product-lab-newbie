# 📝 Nhật ký chiêm nghiệm tương tác AI (AI Log)

### 1. AI đã giúp tôi những gì?
Tôi đã sử dụng AI (như ChatGPT, Gemini) làm "thought-partner" (trợ lý tư duy) để brainstorm các ý tưởng quy trình vận hành của các công ty thuộc Vingroup (như VinFast, Vinhomes, Vinmec). AI giúp tôi nhanh chóng liệt kê ra các nút thắt cổ chai (bottleneck) phổ biến trong vận hành (ví dụ: nhân viên BQL tòa nhà mất nhiều thời gian phân loại ticket). 
Ngoài ra, tôi còn dùng prompt yêu cầu AI đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe để phản biện lại các "Quick Problem Cards" mà tôi vừa tạo ra, giúp tôi mài giũa lại các số liệu (metrics) đo lường và xác định lại ranh giới của bài toán sao cho thực tế nhất.

### 2. AI đã sai ở đâu (Điểm yếu / Hallucination)?
Trong quá trình thảo luận, AI gặp một số vấn đề sau:
- **Over-engineering (Làm phức tạp hóa vấn đề):** Khi tôi hỏi về tối ưu xe bus nội khu, AI ngay lập tức đề xuất sử dụng "Mô hình LLM để dự đoán ý định di chuyển của khách", trong khi bài toán này hoàn toàn có thể giải quyết bằng hệ thống Rule-based (thuật toán tối ưu quãng đường thông thường) hiệu quả và rẻ hơn nhiều.
- **Hallucination (Ảo giác dữ liệu):** AI tự động bịa ra các con số không có cơ sở (ví dụ: "Nhân viên Vinmec tốn 45 phút cho mỗi bệnh án") dù tôi chưa hề cung cấp dữ liệu thực tế. 
- **Thiếu ranh giới:** AI đề xuất các giải pháp liên quan đến "AI tự động chẩn đoán bệnh thay bác sĩ", điều này vi phạm nghiêm trọng giới hạn về mặt đạo đức và pháp lý y tế (cần Human-in-the-loop).

### 3. Tôi đã sửa đổi và điều chỉnh Prompt ra sao?
Để ép AI trả về kết quả chính xác và thực tế hơn, tôi đã thực hiện các bước điều chỉnh prompt (Prompt Tuning) như sau:
1. **Thiết lập Operational Boundary rõ ràng:** Tôi bổ sung vào prompt: *"Chỉ tập trung vào các quy trình back-office, thủ tục giấy tờ hành chính. Tuyệt đối KHÔNG đề xuất các bài toán liên quan đến chẩn đoán y khoa tự động hoặc đưa ra quyết định thay thế con người mà không có bước phê duyệt."*
2. **Yêu cầu không bịa số liệu:** Thêm chỉ thị: *"Nếu không có dữ liệu thực tế, hãy sử dụng biến [X] phút thay vì tự tạo ra các con số ảo tưởng."*
3. **Giới hạn phạm vi giải pháp:** Yêu cầu AI: *"Hãy đề xuất giải pháp chỉ gói gọn trong 1 API call của LLM (LLM Feature) để xử lý text, đừng đề xuất các hệ thống Agent quá phức tạp."*
Nhờ những thay đổi này, AI đã đưa ra các bài toán sắc bén, thực tế và bám sát vào những tác vụ lặp đi lặp lại hàng ngày (như trích xuất hóa đơn, tóm tắt yêu cầu khách hàng).
