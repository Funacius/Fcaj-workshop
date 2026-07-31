---
title: "Từ log đến cảnh báo theo từng tài nguyên với Amazon CloudWatch"
menuTitle: "Cảnh báo CloudWatch"
weight: 3
pre: "<b>3.3.</b>"
---

EduCloud Lite hiện stream log ứng dụng từ Elastic Beanstalk sang Amazon
CloudWatch và hiển thị một số thông tin vận hành gần đây trên Admin dashboard.
Log rất cần thiết cho quá trình điều tra, nhưng vận hành production còn cần khả
năng thông báo chủ động: hệ thống phải cho biết tài nguyên nào vượt ngưỡng mà
không đợi quản trị viên tự mở dashboard để tìm kiếm.

Bài viết này nghiên cứu bài
[Getting per-resource alarm notifications with Amazon CloudWatch](https://aws.amazon.com/blogs/mt/getting-per-resource-alarm-notifications-with-amazon-cloudwatch/)
trên AWS Cloud Operations Blog, xuất bản ngày 30/07/2026. Nội dung chính gồm cơ
chế `GROUP BY` trong CloudWatch Metrics Insights alarm, thông báo theo từng
contributor và vai trò của Amazon EventBridge khi cần định tuyến hoặc tự động xử
lý sự cố.

## 1. Bài toán vận hành

Theo dõi một tài nguyên bằng một alarm tương đối đơn giản. Khi hệ thống mở rộng,
một fleet có thể chứa nhiều EC2 instance, account hoặc Region. Tạo riêng một
alarm cho mỗi tài nguyên làm tăng khối lượng cấu hình và bảo trì, trong khi một
alarm tổng hợp có thể che khuất phạm vi thật của sự cố.

Ví dụ, một EC2 instance vượt ngưỡng CPU và đưa alarm sang trạng thái `ALARM`.
Sau đó vài phút, ba instance khác tiếp tục vượt ngưỡng. Đội vận hành vẫn cần
nhận thông báo cho từng instance mới gặp lỗi, dù alarm tổng chưa từng quay lại
trạng thái `OK` giữa các lần vi phạm.

Nếu thông báo không chứa chi tiết tài nguyên, người xử lý phải mở dashboard và
tự tìm các instance bị ảnh hưởng. Điều này làm tăng thời gian phát hiện, thời
gian phản hồi và nguy cơ đánh giá thiếu phạm vi sự cố.

## 2. Metrics Insights alarm và contributor

CloudWatch Metrics Insights hỗ trợ truy vấn metric theo cú pháp gần với SQL.
Một truy vấn sử dụng `GROUP BY` trả về nhiều time series, chẳng hạn một time
series cho mỗi `InstanceId`. CloudWatch xem mỗi tổ hợp dimension duy nhất là một
**contributor** và đánh giá chúng độc lập.

Một truy vấn EC2 đơn giản có dạng:

```sql
SELECT AVG(CPUUtilization)
FROM SCHEMA("AWS/EC2", InstanceId)
GROUP BY InstanceId
```

Điểm quan trọng là contributor mới vượt ngưỡng vẫn có thể kích hoạt alarm action
dù contributor khác đã khiến alarm tổng ở trạng thái `ALARM`. Notification chứa
định danh và thuộc tính contributor, chẳng hạn account và instance, giúp xác
định chính xác tài nguyên gây cảnh báo.

## 3. Các đường gửi thông báo mặc định

CloudWatch hỗ trợ alarm action theo contributor cho nhu cầu thông thường:

- **Amazon SNS:** gửi notification tới email, hệ thống chat, ticket hoặc
  subscriber khác.
- **AWS Lambda:** gọi mã xử lý hoặc remediation tùy chỉnh.

Nếu yêu cầu chỉ là gửi một thông báo chuẩn và chỉ rõ từng tài nguyên vượt
ngưỡng, native alarm action thường đã đủ. Hệ thống không cần thêm một lớp xử lý
sự kiện không cần thiết.

Đây cũng là một nguyên tắc thiết kế quan trọng: chọn giải pháp nhỏ nhất đáp ứng
đúng yêu cầu vận hành. Chỉ bổ sung event-driven extension khi thực sự cần khả
năng kiểm soát mở rộng.

## 4. Mở rộng bằng EventBridge

CloudWatch đồng thời phát Contributor State Change event sang Amazon EventBridge
khi một contributor vượt ngưỡng hoặc phục hồi. Event này độc lập với alarm
action thông thường và mang dữ liệu tài nguyên ở dạng có cấu trúc.

EventBridge rule có thể định tuyến event tới các target được hỗ trợ. Cách này
hữu ích khi cần:

- lọc sự kiện theo account, tài nguyên, trạng thái hoặc thuộc tính khác;
- gửi từng nhóm tài nguyên tới đội vận hành phù hợp;
- bổ sung dữ liệu và định dạng lại notification;
- tự động mở incident hoặc ticket;
- kích hoạt runbook hay Lambda remediation; hoặc
- fan-out cùng một event tới nhiều đích.

Luồng khái niệm:

```text
Metric của tài nguyên AWS
        -> CloudWatch Metrics Insights alarm
        -> Contributor State Change event
        -> EventBridge rule
        -> SNS / Lambda / hệ thống incident
```

EventBridge tách alarm khỏi logic xử lý phía sau. Notification và remediation
có thể phát triển độc lập mà không phải xây lại truy vấn monitoring ban đầu.

## 5. Liên hệ với EduCloud Lite

EduCloud hiện dùng Elastic Beanstalk để chạy FastAPI backend. Application log
được gửi sang CloudWatch, còn quản trị viên có thể xem log gần đây và thông tin
health trên Admin dashboard.

Đây là nền tảng hợp lý nhưng vẫn chủ yếu mang tính phản ứng. Quản trị viên thường
phải chủ động mở dashboard hoặc CloudWatch mới phát hiện và điều tra vấn đề.

Nếu môi trường sau này chuyển sang Elastic Beanstalk load-balanced và auto
scaling, cảnh báo theo từng tài nguyên sẽ có giá trị rõ rệt hơn. Nhiều EC2
instance cùng phục vụ ứng dụng và lỗi có thể chỉ xảy ra trên một contributor.
Fleet alarm phát hiện điều kiện chung, còn contributor data chỉ ra chính xác
instance gặp vấn đề.

Vì vậy, giải pháp trong AWS Blog là một **hướng cải thiện monitoring trong tương
lai**, không phải tính năng được tuyên bố đã hoàn thành trong phiên bản EduCloud
hiện tại.

## 6. Metric và alarm có thể áp dụng

| Tín hiệu | Ý nghĩa vận hành | Hướng xử lý |
| --- | --- | --- |
| EC2 `CPUUtilization` | Instance quá tải hoặc xử lý traffic bất thường | Kiểm tra request, scale capacity hoặc phân tích code |
| EC2 status-check failure | Lỗi hạ tầng hoặc hệ điều hành | Phục hồi hoặc thay instance không khỏe |
| Load balancer HTTP 5xx | Backend trả về lỗi server | Kiểm tra log FastAPI và database |
| Target response time | Độ trễ API tăng | Kiểm tra database connection và hiệu năng ứng dụng |
| Unhealthy host count | Load balancer không sử dụng được một số target | Thông báo và kiểm tra health check |
| Disk hoặc memory custom metric | Runtime chịu áp lực tài nguyên | Dọn dữ liệu tạm, chỉnh worker hoặc đổi instance size |

Metric phải xuất phát từ failure mode thật. Tạo quá nhiều alarm nhưng không xác
định người chịu trách nhiệm hoặc hành động xử lý sẽ gây alert fatigue thay vì
tăng observability.

## 7. Lộ trình áp dụng theo từng bước

Một project sinh viên cần tiết kiệm chi phí có thể triển khai dần:

1. Giữ CloudWatch Logs và Elastic Beanstalk health làm nền tảng vận hành.
2. Thêm một alarm có hành động rõ ràng cho backend health hoặc HTTP 5xx.
3. Gửi alarm tới một SNS email subscription.
4. Kiểm tra cả luồng vượt ngưỡng và phục hồi.
5. Chỉ thêm Metrics Insights query có `GROUP BY` khi thực sự có nhiều tài nguyên.
6. Chỉ dùng EventBridge khi cần lọc tùy chỉnh, nhiều đích nhận hoặc remediation
   tự động.

Thứ tự này tránh triển khai hạ tầng chưa tạo ra giá trị. Mỗi alarm cũng có mối
liên hệ rõ ràng với người chịu trách nhiệm và quy trình phản hồi.

## 8. Bài học rút ra

- **Log và alarm giải quyết hai vấn đề khác nhau:** log cung cấp bằng chứng để
  điều tra; alarm giúp phát hiện kịp thời.
- **Fleet alarm phải giữ được danh tính tài nguyên:** người vận hành cần biết
  contributor nào vượt ngưỡng, không chỉ biết fleet đang có vấn đề.
- **Trạng thái `ALARM` kéo dài không được che mất sự cố mới:** contributor-level
  action vẫn thông báo khi các tài nguyên khác tiếp tục vượt ngưỡng.
- **Ưu tiên native action:** SNS hoặc Lambda đã đủ cho notification theo
  contributor thông thường.
- **EventBridge là lớp mở rộng:** dùng khi cần lọc, fan-out, enrich, tích hợp và
  remediation.
- **Mỗi alarm phải dẫn tới một hành động:** cảnh báo không có owner hoặc runbook
  chỉ tạo thêm nhiễu.

## Kết luận

CloudWatch Metrics Insights alarm tạo ra sự cân bằng giữa monitoring toàn fleet
và khả năng quan sát từng tài nguyên. Một grouped alarm làm giảm số lượng alarm
phải quản lý nhưng vẫn thông báo về mọi contributor mới vượt ngưỡng.

Đối với EduCloud Lite, ưu tiên hiện tại vẫn là log ổn định và application health.
Per-resource notification là bước tiếp theo phù hợp khi backend mở rộng hơn một
instance. EventBridge chỉ nên được bổ sung khi project cần định tuyến có điều
kiện hoặc tự động khắc phục sự cố.

**Nguồn chính:** [Getting per-resource alarm notifications with Amazon CloudWatch – AWS Cloud Operations Blog](https://aws.amazon.com/blogs/mt/getting-per-resource-alarm-notifications-with-amazon-cloudwatch/)  
**Tài liệu CloudWatch:** [Create an alarm on a Metrics Insights query](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create_Metrics_Insights_Alarm.html)  
**Mã nguồn EduCloud:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)
