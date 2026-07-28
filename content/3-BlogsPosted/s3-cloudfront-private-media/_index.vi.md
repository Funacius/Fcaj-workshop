---
title: "EduCloud có thể học gì từ nền tảng Amazon EKS của Riot Games?"
menuTitle: "Riot Games và Amazon EKS"
weight: 2
pre: "<b>3.2.</b>"
---

# EduCloud có thể học gì từ nền tảng Amazon EKS của Riot Games?

Game online quy mô lớn và nền tảng học tập nhìn khác nhau, nhưng cùng gặp một
bài toán hạ tầng: lưu lượng thay đổi nhanh, dịch vụ phải luôn sẵn sàng và nhóm
phát triển nên tập trung vào sản phẩm thay vì quản lý máy chủ.

Bài viết này nghiên cứu case study chính thức của AWS
[Riot Games Cuts $10M Annual Infrastructure Costs by Migrating to Amazon EKS](https://aws.amazon.com/solutions/case-studies/riot-games-case-study/)
và chuyển các bài học kỹ thuật đó sang bối cảnh EduCloud Lite. Đây là bài phân
tích độc lập; EduCloud Lite **chưa sử dụng Amazon EKS trong deployment hiện tại**.

## 1. Bài toán hạ tầng

Riot Games vận hành các game live-service như League of Legends và VALORANT.
Nền tảng cần phục vụ người chơi trên toàn cầu, độ trễ thấp, tính sẵn sàng cao,
tự động scale và triển khai lặp lại ở nhiều khu vực.

EduCloud có quy mô nhỏ hơn nhưng vẫn có các câu hỏi tương tự:

- Làm sao phát hành khóa học và lesson mà không sửa server thủ công?
- Làm sao chịu được đợt tăng đột biến khi nhiều người làm assessment?
- Làm sao chuẩn hóa quy trình triển khai cho các thành viên?
- Làm sao để chi phí tăng theo nhu cầu thực tế?

## 2. Riot Games đã thay đổi điều gì?

Theo case study của AWS, Riot bắt đầu chuyển sang Amazon EKS vào năm 2021 sau khi
đánh giá nền tảng điều phối container trước đó. EKS cung cấp managed Kubernetes
control plane, còn Riot chuẩn hóa developer environment và tự động hóa quản lý
node.

Các kết quả được AWS công bố gồm:

- hỗ trợ hơn 180 triệu monthly active users;
- tiết kiệm khoảng 10 triệu USD chi phí hạ tầng mỗi năm;
- thiết lập hạ tầng nhanh hơn 90%; và
- triển khai game infrastructure nhanh hơn 12 lần.

Riot cũng sử dụng Karpenter để quản lý vòng đời node, Terraform để tự động hóa,
cluster riêng cho từng game hoặc use case và AWS Local Zones/Outposts cho workload
cần độ trễ thấp. Đây là số liệu trong [case study chính thức của AWS](https://aws.amazon.com/solutions/case-studies/riot-games-case-study/),
không phải số liệu của EduCloud.

## 3. Các nguyên tắc kỹ thuật

### 3.1 Managed orchestration

EKS giảm nhu cầu tự vận hành Kubernetes control plane. Nhóm phát triển tập trung
vào container, health check, resource request và deployment policy, còn AWS quản
lý lớp control plane.

### 3.2 Tự động điều chỉnh capacity

Karpenter quan sát workload đang chờ và tạo node phù hợp thay vì duy trì một nhóm
server cố định. Mô hình này phù hợp với các workload có giờ thấp điểm và cao
điểm, chẳng hạn lúc ra mắt game hoặc tổ chức kỳ thi trực tuyến.

### 3.3 Developer platform chuẩn hóa

Riot tạo một môi trường quản lý tập trung để team yêu cầu compute, networking và
storage theo các pattern đã được kiểm soát. Developer không phải xử lý toàn bộ
chi tiết hạ tầng ở mỗi lần deploy.

### 3.4 Cô lập workload

Riot tiến tới cluster riêng cho từng game hoặc use case. Một sự cố vì vậy ít có
khả năng làm tiêu tốn capacity hoặc ảnh hưởng cấu hình của workload khác.

### 3.5 Phân phối theo vị trí

Với yêu cầu latency nghiêm ngặt, Riot sử dụng AWS Local Zones và Outposts để đặt
workload gần người chơi hơn. EduCloud hiện chưa cần mức độ phức tạp này, nhưng
đây là hướng cần cân nhắc nếu mở rộng nhiều Region.

## 4. Ví dụ cấu hình Kubernetes nhỏ

Ví dụ dưới đây minh họa dạng cấu hình khai báo mà EduCloud có thể dùng trong
tương lai. Đây **không phải** cấu hình deployment Elastic Beanstalk hiện tại.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: educloud-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: educloud-api
  template:
    metadata:
      labels:
        app: educloud-api
    spec:
      containers:
        - name: api
          image: ACCOUNT_ID.dkr.ecr.ap-southeast-1.amazonaws.com/educloud-api:1.0.0
          ports:
            - containerPort: 8000
          resources:
            requests:
              cpu: "250m"
              memory: "512Mi"
            limits:
              cpu: "1"
              memory: "1Gi"
```

Trong một thiết kế EKS production, Deployment có thể kết hợp với Horizontal Pod
Autoscaler, Application Load Balancer, Amazon ECR, CloudWatch Container Insights
và Karpenter.

## 5. Áp dụng bài học cho EduCloud Lite

| Nguyên tắc của Riot Games | EduCloud hiện tại | Hướng phát triển |
| --- | --- | --- |
| Managed compute | FastAPI trên Elastic Beanstalk | Container image trên EKS hoặc ECS |
| Tự động scale | Single instance cho môi trường demo | Auto Scaling hoặc HPA |
| Provisioning chuẩn hóa | Script và tài liệu triển khai | Terraform hoặc CloudFormation |
| Cô lập workload | Tách API, S3, Cognito và database | Cô lập namespace/service |
| Phân phối toàn cầu | CloudFront cho frontend và media | Regional API và edge-aware routing |
| Observability | Elastic Beanstalk health và CloudWatch logs | Container Insights, metrics, traces |

Bài học đúng không phải là thay Elastic Beanstalk ngay lập tức. Với bài nộp có
lưu lượng thấp, Elastic Beanstalk đơn giản và tiết kiệm hơn. EKS chỉ phù hợp khi
ứng dụng có nhiều container service, nhu cầu scale độc lập hoặc nhóm đã sẵn sàng
vận hành Kubernetes.

## 6. Đánh đổi về chi phí và độ phức tạp

EKS đem lại khả năng chuẩn hóa, portability và scale mạnh, nhưng cũng yêu cầu
quản lý cluster, networking, IAM cho service account, observability, container
image và Kubernetes security. Managed control plane không có nghĩa toàn bộ ứng
dụng trở thành serverless.

Lộ trình hợp lý của EduCloud là:

1. giữ deployment Elastic Beanstalk ổn định;
2. đóng gói FastAPI thành Docker image;
3. thêm infrastructure as code và health alarm;
4. đo request volume và nhu cầu scale; rồi
5. chỉ chuyển sang ECS/EKS khi lợi ích vận hành lớn hơn độ phức tạp.

## 7. Bài học chính

- Scale là năng lực của kiến trúc, không chỉ là chọn EC2 lớn hơn.
- Platform team giúp quy trình cloud lặp lại được cho developer.
- Capacity nên phản hồi theo workload và giới hạn chi phí.
- Cô lập workload giúp giảm blast radius khi có sự cố.
- EKS mạnh nhưng không bắt buộc cho mọi web application.

## Kết luận

Riot Games cho thấy một công ty game toàn cầu có thể sử dụng Amazon EKS,
Karpenter, Terraform và các dịch vụ AWS theo vị trí để đơn giản hóa hạ tầng và
tăng tốc phát hành. EduCloud Lite áp dụng các ý tưởng đó ở quy mô nhỏ hơn: tách
trách nhiệm, tự động hóa bước lặp lại, theo dõi health và chỉ đưa Kubernetes vào
khi workload cũng như nhóm phát triển đã sẵn sàng.

## Tài liệu tham khảo

- [Riot Games Cuts $10M Annual Infrastructure Costs by Migrating to Amazon EKS — AWS Case Study](https://aws.amazon.com/solutions/case-studies/riot-games-case-study/)
- [Tài liệu Amazon Elastic Kubernetes Service](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [Tài liệu Karpenter](https://karpenter.sh/)
- [Mã nguồn EduCloud Lite](https://github.com/Funacius/EduCloud)
