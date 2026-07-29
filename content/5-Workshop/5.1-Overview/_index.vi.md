---
title: "Giới thiệu"
weight: 1
chapter: false
disableTitle: true
pre: "<b>5.1.</b>"
---

# GIỚI THIỆU

Phần này trình bày bối cảnh, bài toán và kiến trúc triển khai EduCloud Lite trước khi tạo tài nguyên AWS. Nội dung giúp các bước thực hành phía sau liên kết thành một giải pháp hoàn chỉnh thay vì chỉ là các thao tác rời rạc trên console.

## Bối cảnh dự án và bài toán cần giải quyết

Một nền tảng học trực tuyến không chỉ hiển thị danh sách bài học. Giảng viên cần tạo khóa học, tổ chức bài học, tải tài nguyên học tập, tạo bài kiểm tra cuối khóa và cấp chứng nhận. Học viên cần một luồng rõ ràng để tìm khóa học đã công bố, đăng ký, học nội dung, theo dõi tiến độ, hoàn thành bài đánh giá và nhận kết quả có thể kiểm chứng.

Thách thức là cung cấp các chức năng này trong một ứng dụng đáng tin cậy, đồng thời tách riêng danh tính người dùng, dữ liệu nghiệp vụ, tài nguyên tải lên và secret production. Một bản chạy local là chưa đủ: nền tảng cần truy cập công khai, đủ an toàn cho tài khoản thật và đủ đơn giản để giảng viên hoặc người đánh giá có thể kiểm thử.

EduCloud Lite giải quyết bài toán này bằng nền tảng học tập phân quyền. Phạm vi hiện tại chủ động giữ toàn bộ khóa học miễn phí nên chưa bao gồm checkout hoặc xử lý thanh toán.

## Mục tiêu dự án

Dự án hướng đến một ứng dụng cloud hoàn chỉnh, có thể trình diễn, thay vì các ví dụ AWS rời rạc.

- Cho phép **học viên** xem khóa học, đăng ký, học bài, theo dõi tiến độ, làm bài đánh giá cuối khóa và xem chứng nhận hoàn thành.
- Cho phép **giảng viên** tạo và xuất bản khóa học, quản lý bài học và mục tiêu học tập, tải thumbnail/tài nguyên, đồng thời tạo câu hỏi với nhiều lựa chọn và luật đáp án đúng.
- Cho phép **quản trị viên** quản lý vai trò và yêu cầu trở thành giảng viên, xem tình trạng ứng dụng, log gần đây, mức sử dụng S3 và thống kê database.
- Sử dụng AWS phù hợp với dự án sinh viên: bảo mật mặc định, dễ trình diễn và có cân nhắc chi phí vận hành.

## Đối tượng hướng đến

### Học viên

Học viên là người sử dụng chính. Họ cần luồng thao tác ít rào cản từ đăng ký đến hoàn thành khóa học. Giao diện cung cấp thông tin khóa học, bài học, tiến độ assessment và chứng nhận ở cùng một nơi để người học luôn biết bước tiếp theo.

### Giảng viên

Giảng viên cần công cụ soạn nội dung mà không phải quản lý hạ tầng. Họ có thể tạo khóa học, chọn thumbnail, thêm bài học, tạo câu hỏi final assignment và xuất bản khi nội dung cần thiết đã sẵn sàng.

### Quản trị viên

Quản trị viên giám sát hoạt động nền tảng. Dashboard tập trung vào các tác vụ vận hành: vai trò người dùng, yêu cầu giảng viên, log Elastic Beanstalk, health signal, mức dùng S3, thống kê database và thông tin chi phí AWS có thể truy xuất.

## Các bài toán kỹ thuật cốt lõi

### 1. Danh tính, phân quyền và khôi phục tài khoản

Amazon Cognito quản lý đăng ký, xác minh email, đăng nhập, đổi mật khẩu lần đầu và quên mật khẩu. Backend xác thực token Cognito rồi ánh xạ danh tính đó sang vai trò ứng dụng được lưu trong Supabase.

### 2. Tính nhất quán của dữ liệu học tập và assessment

Tiến độ học, đăng ký khóa học, assessment, chứng nhận, đánh giá và yêu cầu giảng viên là các dữ liệu có liên hệ. API giữ quy tắc nghiệp vụ ở server để học viên không thể nhận chứng nhận chỉ bằng cách thay đổi state trên trình duyệt.

### 3. Phân phối tài nguyên học tập riêng tư

Thumbnail, video và tài liệu không nên nằm trên filesystem của Elastic Beanstalk hoặc trong S3 public. Backend dùng presigned URL để upload trực tiếp; S3 lưu object private và CloudFront phân phối các asset được cho phép thông qua Origin Access Control (OAC).

### 4. Triển khai độc lập frontend và API

Amplify build và host React frontend từ nhánh GitHub `main`. Elastic Beanstalk chạy FastAPI. CloudFront định tuyến `/api/*` đến Elastic Beanstalk và `/courses/*` đến S3, trong khi Amplify cung cấp giao diện web công khai.

### 5. Bảo mật, giám sát và tối ưu chi phí

Secret production được lưu trong Systems Manager Parameter Store thay vì repository. IAM role tuân theo nguyên tắc đặc quyền tối thiểu. Elastic Beanstalk gửi application log đến CloudWatch; khu vực Admin hiển thị thông tin vận hành hữu ích để xử lý sự cố.

## Mục lục phần giới thiệu

1. [Kiến trúc hệ thống](5.1.1-architecture/) – thành phần và ranh giới trách nhiệm
2. [Luồng request](5.1.2-request-flow/) – trình duyệt, xác thực, API, database và media
