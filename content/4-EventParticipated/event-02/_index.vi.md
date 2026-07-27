---
title: "FCAJ x Agentic AI Build Week 2026"
menuTitle: "Trao giải & trình diễn AABW"
weight: 2
pre: "<b>4.2.</b>"
---

# Sự kiện 2

<div class="event-hero-title">Báo cáo tóm tắt: “FCAJ x Agentic AI Build Week 2026 — Trao giải Hackathon và trình diễn dự án Agentic AI”</div>

## Thông tin sự kiện

**Tên sự kiện:** FCAJ x Agentic AI Build Week 2026 — Trao giải Hackathon và trình diễn dự án

**Ngày & thời gian:** 25/07/2026

**Địa điểm:** AWS Office, 26th Floor, Bitexco Tower, 02 Hai Trieu Street, Saigon Ward, Ho Chi Minh City

**Đơn vị tổ chức:** First Cloud AI Journey (FCAJ), Amazon Web Services (AWS) và cộng đồng Agentic AI Build Week

## Mục tiêu sự kiện

- Ghi nhận các đội và giải pháp nổi bật trong cuộc thi Agentic AI Build Week (AABW) Hackathon.
- Chia sẻ hành trình tham gia hackathon từ xây dựng ý tưởng, phân chia công việc, thử nghiệm thất bại, thu hẹp phạm vi đến trình diễn sản phẩm.
- Giới thiệu các sản phẩm Agentic AI thực tế trong đặt hàng, thiết kế kiến trúc cloud, phân tích chiến lược và an toàn đám đông.
- Giúp cộng đồng hiểu cách sử dụng dịch vụ AWS để đưa AI Agent từ nguyên mẫu đến một hệ thống có khả năng vận hành thực tế.

## Các đội trình bày

- **3KA — S.H.E.P.H.E.R.D:** Huỳnh An Khương, Nguyễn Quốc Huy, Ngô Quang Khôi, Hoàng Lê Thành Đức, Đặng Nguyễn Phước Lộc và Đặng Trường Hưng.
- **OneTeam — KFC Bot Agent:** Anh Duy, Trần Đồng, Đoàn Trung, Minh Việt và Anshul Roy.
- **Plan V — Solution Architect Professional Native App:** Phạm Tiến Thuận Phát, Huỳnh Hoàng Long, Lê Minh Nghĩa, Trần Đại Vĩ và Nguyễn An.
- **SignalScout:** Lê Tấn Lực, Đỗ Hoàng Hiếu, Triệu Quốc Hào, Nguyễn Văn Duy Khiêm, Nguyễn Công Minh và Nguyễn Trần Minh Quân.

## Nội dung nổi bật

### Hành trình hackathon của 3KA và S.H.E.P.H.E.R.D

- Đội 3KA trình bày toàn bộ quá trình từ chọn track, phân chia vai trò đến xây dựng, gặp lỗi, điều chỉnh phạm vi và chuẩn bị bản demo trong thời gian giới hạn.
- Dự án **S.H.E.P.H.E.R.D** đánh giá luồng người, dự đoán tắc nghẽn, phát hiện nguy hiểm và hỗ trợ quyết định phản ứng, điều phối.
- Nguyên mẫu kết hợp **YOLO và ByteTrack** để phát hiện, theo dõi đám đông; **Amazon SageMaker** cho tác vụ mô hình; **Amazon Bedrock AgentCore và Strands Agents** cho hành vi agent; cùng dashboard vận hành viết bằng React.
- Bài học quan trọng của đội là một sản phẩm nhỏ nhưng hoàn chỉnh, giải thích được và demo ổn định có giá trị hơn một ý tưởng quá lớn nhưng không thể hoàn thành.

### OneTeam và KFC Bot Agent đạt giải

- OneTeam giới thiệu AI Agent đặt món đa kênh, cho phép khách hàng đặt hàng qua các ứng dụng nhắn tin quen thuộc như Zalo và WhatsApp.
- Giải pháp tuân theo chu trình **Mục tiêu → Lập kế hoạch → Công cụ → Hành động → Xác minh**, đồng thời tách adapter của từng kênh khỏi công cụ đặt hàng và logic nghiệp vụ dùng chung.
- Slide trình bày mức chi phí khoảng **0,006 USD cho mỗi đơn hàng**, ước tính **88 USD mỗi tháng** và độ trễ phản hồi khoảng **ba đến năm giây**.
- Dự án cho thấy Amazon Bedrock AgentCore có thể giảm mã hạ tầng trong khi vẫn giữ kiến trúc agent dạng module và dễ mở rộng sang kênh mới.

### Plan V và Solution Architect Professional Native App

- Plan V giải quyết công việc tốn thời gian của Solution Architect như trích xuất yêu cầu, phác thảo kiến trúc, tạo sơ đồ và ước tính chi phí.
- Ứng dụng chuyển yêu cầu ngôn ngữ tự nhiên thành bản kiến trúc theo tiêu chuẩn, sinh sơ đồ draw.io có thể chỉnh sửa với icon AWS chính thức và tạo ước tính chi phí định hướng cho Region `ap-southeast-1`.
- Hệ thống cũng hiển thị giả định và yêu cầu còn thiếu để Solution Architect kiểm tra, trao đổi và điều chỉnh kết quả thay vì chấp nhận một câu trả lời khó giải thích.

### SignalScout và quyết định chiến lược dựa trên bằng chứng

- SignalScout là nền tảng hỗ trợ quyết định bằng AI, có khả năng phát hiện tín hiệu tổ chức và thị trường, phân tích kịch bản kinh doanh, sau đó đề xuất duy trì, thích nghi hoặc tăng tốc chiến lược.
- Kiến trúc sử dụng các dịch vụ như Amazon Bedrock, AgentCore, Amazon Cognito, AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon S3, AWS WAF, AWS CloudTrail, Amazon CloudWatch và AWS Secrets Manager.
- Đội trình bày nhiều kịch bản chi phí thực tế và giải thích sự đánh đổi giữa thu thập dữ liệu, sử dụng mô hình, khả năng quan sát, bảo mật và quy mô vận hành.

## Bài học rút ra

### Kiến thức kỹ thuật

- Một AI Agent cần mục tiêu rõ ràng, kế hoạch cụ thể, công cụ đáng tin cậy, bước xác minh và kết quả có thể quan sát; chỉ có câu trả lời của mô hình chưa tạo thành hệ thống production.
- Tách adapter và giao diện công cụ giúp tái sử dụng cùng một agent cho nhiều kênh và thay đổi tích hợp bên ngoài mà không phải thiết kế lại toàn bộ ứng dụng.
- Sơ đồ kiến trúc, ước tính chi phí, giám sát, bảo mật và cơ chế con người kiểm tra nên được cân nhắc ngay từ đầu thay vì bổ sung sau khi demo.
- Hệ thống thị giác máy tính phụ thuộc nhiều vào chất lượng dữ liệu, góc camera, độ ổn định của tracking, độ trễ suy luận và cơ chế dự phòng.

### Giá trị thực tiễn

- Một đội hackathon hiệu quả cần giới hạn phạm vi sớm, phân vai rõ ràng, chuẩn bị luồng demo và ưu tiên một kịch bản end-to-end hoạt động được.
- Giá trị sản phẩm cần được chứng minh bằng số liệu như độ trễ, chi phí cho mỗi giao dịch, thời gian tiết kiệm, độ tin cậy và chất lượng quyết định.
- Giải thưởng và thứ hạng có ý nghĩa, nhưng kết quả có thể áp dụng lâu dài nhất là quy trình kỹ thuật: kiểm tra giả định, xử lý thất bại và giải thích lý do chọn từng dịch vụ.

### Liên hệ với EduCloud Lite

- Các bài trình bày củng cố giá trị của việc tách riêng frontend, backend, xác thực, lưu trữ và cơ sở dữ liệu trong EduCloud Lite.
- Cách Plan V tạo sơ đồ AWS có thể chỉnh sửa và ghi rõ giả định hữu ích cho việc mô tả kiến trúc triển khai, giúp người đánh giá hiểu luồng request của EduCloud.
- Kiến trúc module của OneTeam phù hợp với cách EduCloud tách xác thực, API khóa học, phân phối tệp và giao diện người dùng.
- Các kịch bản chi phí của SignalScout nhắc tôi giữ hệ thống EduCloud ở quy mô vừa đủ, theo dõi đúng dịch vụ đang sử dụng và không thêm hạ tầng không cần thiết.

## Áp dụng vào công việc

- Xác định một luồng nghiệp vụ nhỏ nhưng có thể trình diễn hoàn chỉnh trước khi thêm tính năng tùy chọn.
- Tài liệu hóa kiến trúc, trách nhiệm, luồng dữ liệu, ranh giới bảo mật và chi phí vận hành cùng với mã nguồn.
- Xem dịch vụ bên ngoài là tích hợp có thể thay thế và luôn đặt thông tin bí mật bên ngoài repository.
- Kiểm thử toàn bộ hành trình người dùng và chuẩn bị demo ổn định thay vì chỉ kiểm tra từng thành phần riêng lẻ.
- Giữ bước con người kiểm tra đối với quyết định quan trọng và bảo đảm kết quả tự động có thể giải thích.

## Trải nghiệm sự kiện

Sự kiện giúp tôi thấy cách nhiều đội chuyển ý tưởng Agentic AI thành sản phẩm có thể trình diễn, đo lường và giải thích bằng giá trị kinh doanh. Phần trao giải và chia sẻ dự án đặc biệt hữu ích vì các đội không chỉ nói về kết quả thành công mà còn trình bày những lần thử sai, giới hạn và quyết định thiết kế.

### Học hỏi từ các đội thi

- Tôi học được cách trình bày kiến trúc phức tạp bằng việc bắt đầu từ vấn đề của người dùng, sau đó ánh xạ từng dịch vụ AWS vào một trách nhiệm cụ thể.
- So sánh bốn dự án cho thấy cùng một nguyên tắc Agentic AI có thể áp dụng cho nhiều lĩnh vực khác nhau: đặt hàng bán lẻ, thiết kế giải pháp cloud, phân tích chiến lược và an toàn không gian đông người.

### Trải nghiệm cộng đồng và thảo luận

- Các phần trình bày cung cấp ví dụ thực tế về quản lý phạm vi, làm việc nhóm, kể câu chuyện kỹ thuật và chuẩn bị demo trong thời gian ngắn.
- Việc quan sát cả giải pháp đạt giải và các đội nổi bật khác giúp tôi hiểu rõ hơn tiêu chí đánh giá: kết quả hữu ích, khả năng thực thi, kiến trúc giải thích được và tư duy sẵn sàng cho production.

### Kinh nghiệm rút ra

- Một triển khai tập trung và hoạt động end-to-end mạnh hơn một thiết kế rộng nhưng chưa hoàn thiện luồng quan trọng.
- Chi phí, độ trễ, khả năng quan sát, an toàn và niềm tin của người dùng là yêu cầu sản phẩm, không chỉ là vấn đề hạ tầng.
- Agentic AI nên hỗ trợ con người bằng bằng chứng có căn cứ và hành động có thể kiểm soát, không loại bỏ trách nhiệm của con người.

## Minh chứng sự kiện

![Ảnh minh chứng tại buổi trao giải và trình diễn dự án FCAJ x Agentic AI Build Week 2026](/images/events/event-02.jpg)

> Nhìn chung, FCAJ x Agentic AI Build Week 2026 giúp tôi có góc nhìn thực tế về cách các đội xây dựng, đánh giá, trình bày và cải tiến giải pháp Agentic AI trên AWS. Sự kiện cũng mang lại nhiều bài học có thể áp dụng trực tiếp cho EduCloud Lite trong tài liệu kiến trúc, kế hoạch triển khai, kiểm soát chi phí và kiểm thử end-to-end.
