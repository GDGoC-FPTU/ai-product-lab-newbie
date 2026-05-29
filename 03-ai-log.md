# AI Log – Nhật ký chiêm nghiệm về việc tương tác với AI

## 1. AI đã giúp tôi những gì?

Trong buổi học, tôi sử dụng ChatGPT như một trợ lý đồng hành (thought-partner) để hỗ trợ quá trình phân tích và phát triển ý tưởng. Thay vì chỉ tìm kiếm câu trả lời, tôi sử dụng AI để trao đổi, phản biện và khám phá các hướng tiếp cận khác nhau cho bài toán.

Cụ thể, tôi đã sử dụng AI để:

- Brainstorm các bài toán AI có thể áp dụng cho hệ sinh thái Vingroup như VinFast, Xanh SM và Vinhomes.
- Phân tích các quy trình nghiệp vụ thủ công đang gây lãng phí thời gian và nguồn lực.
- Xây dựng Quick Problem Card cho từng bài toán nhằm đánh giá mức độ khả thi và tác động kinh doanh.
- Hỗ trợ viết và cải thiện prompt để thu được kết quả có cấu trúc rõ ràng hơn.
- Đề xuất kiến trúc triển khai AI như LLM, Agent, Computer Vision hoặc Machine Learning cho từng trường hợp cụ thể.
- Hỗ trợ định dạng kết quả dưới dạng Markdown để dễ dàng đưa vào báo cáo.

Trong quá trình làm việc, AI đóng vai trò như một người cộng sự giúp tôi mở rộng góc nhìn, gợi ý nhiều ý tưởng mà bản thân có thể chưa nghĩ tới.

## 2. AI đã sai ở đâu?

Mặc dù hỗ trợ khá tốt, AI vẫn xuất hiện một số hạn chế và sai sót.

Một trường hợp tôi gặp phải là khi yêu cầu AI tạo các Quick Problem Card theo định dạng khung (ASCII Box). AI tạo ra các card đúng về mặt nội dung nhưng định dạng quá dài và không phù hợp với môi trường Markdown hoặc các trình soạn thảo thông thường. Khi sao chép vào file Markdown, toàn bộ nội dung bị vỡ bố cục và trở nên khó đọc.

Ngoài ra, AI cũng đưa ra một số số liệu về thời gian xử lý, tỷ lệ tiết kiệm chi phí hoặc ROI cho các bài toán VinFast. Các con số này không dựa trên dữ liệu thực tế nội bộ mà chỉ là các giá trị ước lượng từ benchmark ngành. Nếu sử dụng trực tiếp mà không kiểm chứng có thể dẫn đến hiểu nhầm hoặc đánh giá sai tác động kinh doanh.

Một điểm khác là AI đôi khi có xu hướng đề xuất các giải pháp Agent hoặc LLM cho hầu hết các bài toán, trong khi một số trường hợp hoàn toàn có thể giải quyết bằng các rule đơn giản hoặc mô hình Machine Learning truyền thống.

## 3. Tôi đã điều chỉnh như thế nào?

Để cải thiện chất lượng kết quả, tôi đã thay đổi cách đặt prompt theo hướng cụ thể và có ràng buộc rõ ràng hơn.

Ví dụ, thay vì yêu cầu:

> Hãy tạo Quick Problem Card cho bài toán này.

Tôi đổi thành:

> Hãy tạo Quick Problem Card dưới dạng bảng Markdown, không sử dụng ký tự ASCII Box, nội dung ngắn gọn và tương thích với GitHub Markdown.

Việc bổ sung các ràng buộc về định dạng giúp AI trả về kết quả phù hợp hơn với môi trường sử dụng thực tế.

Đối với các số liệu kinh doanh, tôi yêu cầu AI ghi rõ đâu là số liệu thực tế và đâu là số liệu ước tính. Điều này giúp tránh việc sử dụng các thông tin chưa được kiểm chứng trong báo cáo.

Ngoài ra, khi AI đề xuất giải pháp quá phức tạp, tôi bổ sung thêm ràng buộc:

> Ưu tiên giải pháp đơn giản nhất có thể, chỉ sử dụng Agent hoặc LLM khi thật sự cần thiết.

Nhờ đó, các đề xuất trở nên thực tế hơn và phù hợp hơn với mục tiêu triển khai nhanh trong doanh nghiệp.

## 4. Bài học rút ra

Qua buổi học, tôi nhận thấy AI không nên được xem là công cụ đưa ra đáp án cuối cùng mà nên được sử dụng như một đối tác tư duy. Chất lượng kết quả phụ thuộc rất lớn vào cách đặt câu hỏi và mức độ kiểm chứng của người sử dụng.

AI có thể giúp tăng tốc quá trình phân tích, tìm ý tưởng và xây dựng giải pháp, nhưng con người vẫn cần đánh giá, phản biện và kiểm tra tính đúng đắn của kết quả trước khi đưa vào sử dụng thực tế.
