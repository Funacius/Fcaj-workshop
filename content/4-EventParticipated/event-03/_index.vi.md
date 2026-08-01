---
title: "AgentForge Ho Chi Minh City"
menuTitle: "AgentForge Workshop"
weight: 3
pre: "<b>4.3.</b>"
---

<div class="event-hero-title">Xây dựng và triển khai AI Agent trên Amazon Bedrock AgentCore với Kiro</div>

## Thông tin sự kiện

**Tên sự kiện:** AgentForge Ho Chi Minh City  
**Ngày và thời gian:** 01/08/2026, từ 09:00 đến 11:00  
**Địa điểm:** Bitexco Financial Tower, 2 Đường Hải Triều, Phường Sài Gòn, Thành phố Hồ Chí Minh 700000, Việt Nam  
**Hình thức:** Trình bày kỹ thuật kết hợp bài thực hành có hướng dẫn  
**Tài liệu workshop:** [Xây dựng và triển khai AI Agent trên Amazon Bedrock AgentCore bằng Vibe Coding với Kiro](http://agentforge-hcmc-workshop-p371s08u.s3-website-ap-southeast-1.amazonaws.com/00-Overview/00-Dashboard-Overview.html)

## Mục tiêu sự kiện

- Hiểu cách Kiro hỗ trợ phát triển bằng ngôn ngữ tự nhiên và quy trình dựa trên specification.
- Tìm hiểu trách nhiệm của Amazon Bedrock AgentCore khi vận hành agent trong production.
- Xây dựng, kiểm thử, triển khai và gọi một Strands agent cơ bản bằng AgentCore CLI.
- Khám phá cách tool, nguồn tri thức, authentication và Web UI tạo thành một ứng dụng agentic hoàn chỉnh.

## Nội dung chính

### Kiro và phát triển phần mềm có AI hỗ trợ

Buổi học giới thiệu Kiro như một môi trường phát triển AI-native. Tôi đã
tìm hiểu giao diện quản lý dự án, chat panel, terminal tích hợp và hai
phương thức làm việc bổ trợ lẫn nhau:

- **Vibe Coding** phù hợp cho prototype, tính năng nhỏ, sửa lỗi và lặp nhanh từ prompt ngôn ngữ tự nhiên.
- **Spec-Driven Development** phù hợp cho công việc phức tạp hoặc hướng production, cần review requirement, acceptance criteria, design và implementation task.
- **Steering document** trong `.kiro/steering/` cung cấp cho Kiro ngữ cảnh dự án và quy ước phát triển có tính liên tục.

Bài học quan trọng là mọi thay đổi do AI sinh ra vẫn cần con người kiểm tra.
Kiro có thể tăng tốc việc triển khai và chạy command, nhưng developer vẫn chịu
trách nhiệm review diff, permission, dependency, bảo mật và kết quả kiểm thử.

### Nền tảng Amazon Bedrock AgentCore

Workshop giải thích rằng việc tạo một agent sử dụng mô hình chỉ là một phần
của bài toán. Hệ thống production còn phải xử lý runtime deployment, auto-scaling,
IAM permission, truy cập an toàn, tích hợp tool, memory và observability. Amazon
Bedrock AgentCore cung cấp các khối chức năng managed cho những trách nhiệm vận hành này.

Ứng dụng tham chiếu là một Returns and Refunds Assistant có khả năng tra cứu đơn
hàng, kiểm tra điều kiện trả hàng, tính tiền hoàn và truy xuất chính sách trả
hàng theo từng quốc gia.

## Bài thực hành

### Tạo và triển khai agent đầu tiên

Tôi đã thực hiện quy trình AgentCore CLI:

1. Tạo cấu trúc dự án Python agent bằng `agentcore create`.
2. Chọn Strands Agents SDK và Amazon Bedrock là model provider.
3. Kiểm tra cấu trúc application và deployment được sinh ra.
4. Chạy agent local bằng `agentcore dev` và kiểm thử trong terminal.
5. Triển khai lên AgentCore Runtime bằng `agentcore deploy`.
6. Gọi agent đã triển khai trên cloud và kiểm tra phản hồi.

Quy trình này minh họa cách CLI đóng gói agent, chuẩn bị tài nguyên triển khai và
tạo vòng lặp nhất quán từ kiểm thử local đến managed runtime.

### Bổ sung hành vi nghiệp vụ và tool

Agent cơ bản sau đó được chuyên biệt hóa thành Returns and Refunds Assistant.
Bài lab bao gồm:

- Xác định system prompt tập trung cho vai trò và hành vi của agent.
- Tạo các hàm Python tool bằng decorator `@tool` của Strands.
- Dùng docstring rõ ràng và type hint để mô hình hiểu khi nào và cách gọi tool.
- Kiểm thử hành vi tra cứu order, customer, current time và return policy.
- Review code được sinh trước khi deploy lại agent.

### Kiến trúc end-to-end được tìm hiểu

Tài liệu workshop cũng minh họa cách agent có thể mở rộng ra ngoài các inline
mock tool:

- AgentCore Gateway định tuyến tool request một cách an toàn.
- AWS Lambda truy vấn dữ liệu customer, order và product trong Amazon DynamoDB.
- Amazon Bedrock Knowledge Base cung cấp chính sách trả hàng theo từng quốc gia.
- Amazon Cognito xác thực gateway call và người dùng Web UI.
- Giao diện chat Streamlit cho phép người dùng đã xác thực tương tác với agent.
- Amazon CloudWatch cung cấp log, trace và thông tin quan sát vận hành.

## Kiến thức đạt được

- Vibe Coding hữu ích cho việc lặp nhanh, trong khi Spec-Driven Development an toàn hơn cho tính năng liên quan nhiều file hoặc ranh giới hệ thống.
- Steering document giảm việc lặp lại prompt và giữ công việc được sinh phù hợp với quy ước dự án.
- Tool chuyển agent từ giao diện hội thoại thành hệ thống có thể thực hiện hành động có kiểm soát và truy xuất dữ liệu ứng dụng.
- Mô tả tool, type hint và giá trị trả về ảnh hưởng trực tiếp đến khả năng agent gọi đúng tool.
- Nên kiểm thử local trước khi deploy cloud để rút ngắn vòng phản hồi và giảm sử dụng tài nguyên không cần thiết.
- Runtime, identity, gateway và observability là thành phần thiết yếu của agent production, không phải phần bổ sung sau khi mô hình đã chạy.
- AI-assisted development cải thiện tốc độ, nhưng phán đoán kiến trúc, review bảo mật và xác minh vẫn là trách nhiệm của con người.

## Liên hệ với EduCloud Lite

Workshop cung cấp một hướng nghiên cứu tương lai cho EduCloud. Agent có thể hỗ trợ
tìm kiếm khóa học, trả lời câu hỏi dựa trên tài liệu khóa học đã được phê
duyệt hoặc hướng dẫn quản trị viên. Trước khi triển khai, EduCloud cần xác định
tool được phép, ranh giới identity, nguồn trả lời, tiêu chí đánh giá, logging,
giới hạn chi phí và cơ chế giám sát của con người. Phiên bản EduCloud hiện tại
chưa sử dụng AgentCore; đây là hướng phát triển tương lai được gợi ý từ workshop.

## Minh chứng sự kiện

