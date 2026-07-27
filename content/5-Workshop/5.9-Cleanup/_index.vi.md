---
title: "Dọn dẹp tài nguyên"
weight: 9
chapter: false
pre: "<b>5.9.</b>"
---

# Dọn dẹp tài nguyên

Chỉ xóa tài nguyên sau khi giáo viên đã xem website và đã lưu đầy đủ minh chứng.

Thứ tự đề xuất:

1. Xóa Amplify app nếu không còn dùng.
2. Disable rồi xóa CloudFront distribution.
3. Gỡ OAC bucket policy, empty và xóa S3 bucket.
4. Terminate Elastic Beanstalk environment.
5. Xóa Cognito User Pool và Lambda nếu không cần.
6. Xóa hai Parameter Store value.
7. Xóa IAM role/policy sau khi chắc chắn không còn tài nguyên sử dụng.
8. Export dữ liệu rồi pause hoặc xóa Supabase project.
9. Kiểm tra AWS Billing và Cost Explorer.

> Lưu ý: Các thao tác này có thể xóa vĩnh viễn tài khoản, file, secret và dữ
> liệu. Phải kiểm tra đúng tên resource và Region trước khi xóa.
