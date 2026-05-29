### 📝 List bài toán của tôi

| #   | Subsidiary | Lens             | Mô tả ngắn bài toán                                 | Quy trình thủ công hiện tại                                        | Tổn thất ước tính                           |
| --- | ---------- | ---------------- | --------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------- |
| 1   | VinFast    | Repetitive       | So khớp hóa đơn sạc điện và dữ liệu giao dịch       | Nhân viên đối chiếu dữ liệu từ trạm sạc, xe và hệ thống thanh toán | 400–800 giờ công/tháng                      |
| 2   | VinFast    | Time-consuming   | Phân loại và xử lý ticket bảo hành                  | Đọc mô tả lỗi, xem ảnh/video, chuyển ticket cho bộ phận phù hợp    | ~750 giờ công/tháng                         |
| 3   | VinFast    | AI-upgrade       | Hỗ trợ kỹ thuật và giải đáp khách hàng sau bán hàng | Tổng đài viên trả lời thủ công các câu hỏi lặp lại                 | ~1.000 giờ CSKH/tháng                       |
| 4   | VinFast    | Stakeholder Pain | Dự báo nhu cầu phụ tùng và tồn kho                  | Kế hoạch nhập hàng dựa trên kinh nghiệm và báo cáo Excel           | 15–30% tồn kho dư thừa, 5–10% đơn hàng chậm |
| 5   | VinFast    | Repetitive       | Kiểm tra ngoại quan xe cuối dây chuyền              | Công nhân QC kiểm tra thủ công bằng mắt thường                     | 2–5% lỗi lọt gây rework                     |
| 6   | VinFast    | Time-consuming   | Phân tích nguyên nhân lỗi sản xuất (RCA)            | Kỹ sư thu thập log và điều tra thủ công                            | ~600 giờ kỹ sư/tháng                        |

'''
QUICK PROBLEM CARD #1
| Trường | Nội dung |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)** | Tự động phân loại và điều phối ticket bảo hành từ đại lý |
| **Công ty thành viên** | VinFast |
| **Ai đang đau (Actor)?** | Nhân viên bảo hành, Kỹ sư hỗ trợ kỹ thuật, Đại lý dịch vụ |
| **Workflow thủ công hiện tại** | 1. Đại lý gửi ticket → 2. Nhân viên đọc mô tả lỗi, ảnh, video → 3. Phân loại lỗi → 4. Chuyển ticket cho nhóm phụ trách → 5. Kỹ sư xử lý |
| **Bước tốn thời gian nhất** | Phân loại và routing ticket (10–15 phút/ticket) |
| **AI hỗ trợ ở đâu?** | Đọc ticket, phân loại lỗi, tự động route ticket |
| **Metric thành công** | Giảm thời gian phân loại từ 15 phút xuống dưới 2 phút; Auto-routing Accuracy > 90%; Giảm 70% giờ công |
| **Quick Architecture** | LLM + Agent |

'''
'''
QUICK PROBLEM CARD #2
| Trường | Nội dung |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)** | Tự động phát hiện lỗi ngoại quan xe ở cuối dây chuyền |
| **Công ty thành viên** | VinFast |
| **Ai đang đau (Actor)?** | Nhân viên QC, Quản lý sản xuất, Bộ phận chất lượng |
| **Workflow thủ công hiện tại** | 1. Xe hoàn thiện → 2. QC kiểm tra bằng mắt → 3. Ghi nhận lỗi → 4. Chuyển khu vực sửa lỗi → 5. Kiểm tra lại |
| **Bước tốn thời gian nhất** | Kiểm tra ngoại quan thủ công (5–10 phút/xe) |
| **AI hỗ trợ ở đâu?** | Computer Vision phát hiện vết xước, lệch khe, lỗi sơn |
| **Metric thành công** | Giảm thời gian QC từ 10 phút xuống 2 phút/xe; Recall > 95%; Giảm 50% chi phí rework |
| **Quick Architecture** | AI Vision |

'''
'''
QUICK PROBLEM CARD #3
| Trường | Nội dung |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| **Bài toán (1 câu)** | Dự đoán pin EV có nguy cơ suy giảm trước khi khách hàng gặp lỗi |
| **Công ty thành viên** | VinFast |
| **Ai đang đau (Actor)?** | Chủ xe, Trung tâm bảo hành, Bộ phận vận hành sau bán hàng |
| **Workflow thủ công hiện tại** | 1. Xe gửi telemetry → 2. Pin suy giảm → 3. Khách phát hiện bất thường → 4. Mang xe kiểm tra → 5. Bảo hành/thay thế |
| **Bước tốn thời gian nhất** | Chỉ phát hiện sau khi sự cố xảy ra (có thể vài tuần đến vài tháng) |
| **AI hỗ trợ ở đâu?** | Phân tích telemetry, nhiệt độ pin, chu kỳ sạc, SOH |
| **Metric thành công** | Phát hiện sớm hơn 30 ngày; Giảm 20% chi phí bảo hành; Giảm 15% số xe hỏng đột xuất |
| **Quick Architecture** | ML + Agent |
'''
