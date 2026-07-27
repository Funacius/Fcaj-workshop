---
title: "Vì sao chọn Supabase"
weight: 1
chapter: false
pre: "<b>5.3.1.</b>"
---

# Vì sao dùng Supabase thay vì Aurora PostgreSQL?

EduCloud Lite là project thực tập cần triển khai nhanh, chi phí thấp và dễ để
người khác làm theo. Vì vậy Supabase PostgreSQL được chọn cho bản nộp đầu tiên.

| Tiêu chí | Supabase PostgreSQL | Amazon Aurora PostgreSQL |
|---|---|---|
| Mục tiêu phù hợp | Demo, MVP, project sinh viên, triển khai nhanh | Production workload cần AWS-native scaling, HA, read replica và kiểm soát vận hành sâu hơn |
| Chi phí ban đầu | Có Free plan cho project nhỏ | Thường phát sinh chi phí compute, storage và I/O/ACU tùy cấu hình |
| Thời gian setup | Tạo project, copy connection string, dùng ngay | Cần tạo cluster, subnet group, security group, VPC/networking và cấu hình truy cập |
| Độ phức tạp workshop | Thấp, người học tập trung vào app và AWS deployment | Cao hơn, người mới dễ bị kẹt ở networking/database operations |
| Khi nào nên chuyển | Khi project vượt phạm vi demo/MVP | Khi cần database AWS-native cho production |

Trong phạm vi bài nộp, Supabase giúp giảm thời gian cấu hình database và tránh
tạo thêm tài nguyên đắt hơn mức cần thiết. Aurora PostgreSQL vẫn là lựa chọn
mạnh hơn cho production, nhưng chưa cần thiết cho mục tiêu hiện tại: cung cấp
một website độc lập, backend hoạt động ổn định và quy trình triển khai có thể
tái lập.

Tài liệu tham khảo:

- [Supabase Pricing](https://supabase.com/pricing)
- [Amazon Aurora Pricing](https://aws.amazon.com/rds/aurora/pricing/)

