---
title: "Supercell sử dụng Amazon Aurora để mở rộng hệ thống game như thế nào?"
menuTitle: "Supercell và Amazon Aurora"
weight: 2
pre: "<b>3.2.</b>"
---

# Supercell sử dụng Amazon Aurora để mở rộng hệ thống game như thế nào?

Game mobile hoạt động liên tục. Người chơi mong muốn tiến độ, giao dịch, tương
tác xã hội và các sự kiện luôn sẵn sàng, kể cả khi game trở nên phổ biến trên
toàn cầu. Vì vậy, khả năng sẵn sàng của database là một yêu cầu của sản phẩm,
không chỉ là vấn đề hạ tầng.

Bài viết này nghiên cứu AWS customer story
[Supercell Leverages AWS for Seamless and Scalable Gaming Experience](https://aws.amazon.com/solutions/case-studies/supercell-aurora-case-study/)
và rút ra các bài học về relational database managed, high availability và vận
hành hệ thống. Đây là phân tích độc lập, không khẳng định biết các chi tiết nội
bộ của Supercell.

## 1. Bài toán của một game studio toàn cầu

Supercell phát triển các game mobile như Clash of Clans. Theo AWS, Supercell hỗ
trợ khoảng 200 triệu người dùng hoạt động hàng tháng trên các game đã phát hành
toàn cầu. Ở quy mô này, một database outage có thể đồng thời ảnh hưởng tới tiến
độ, giao dịch, matchmaking và các live event.

Các yêu cầu chính gồm:

- khả năng sẵn sàng cao trong giờ bình thường và peak event;
- hiệu năng ổn định khi số người chơi tăng;
- backup và recovery mà không làm gián đoạn dài; và
- giảm thời gian xử lý lỗi phần cứng database.

## 2. Vì sao Amazon Aurora phù hợp?

AWS cho biết Supercell đã sử dụng Amazon Aurora, một relational database tương
thích với MySQL. Việc chuyển đổi giúp giảm downtime và cho phép kỹ sư tập trung
nhiều hơn vào tính năng game thay vì xử lý phần cứng.

Aurora tách database compute khỏi distributed storage layer. Storage volume được
replicate trên nhiều Availability Zone, đồng thời reader instance có thể dùng
cho read scaling và failover. Các đặc điểm này được trình bày trong [tài liệu
high availability của Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html).

Aurora không chỉ là “database nhanh hơn”. Đây là một mô hình vận hành managed,
giúp tự động hóa một phần việc provisioning, replication, backup, phát hiện lỗi
và recovery; trong khi schema design và query tuning vẫn thuộc trách nhiệm của
đội phát triển.

## 3. Luồng request Aurora mang tính khái niệm

Ví dụ sau chỉ minh họa một pattern ứng dụng, không phải mã nguồn riêng của
Supercell:

```python
import os
import psycopg2

writer = os.environ["AURORA_WRITER_ENDPOINT"]
reader = os.environ["AURORA_READER_ENDPOINT"]

def read_player_profile(player_id: str):
    with psycopg2.connect(host=reader, dbname="game") as conn:
        with conn.cursor() as cursor:
            cursor.execute(
                "SELECT level, inventory FROM player_profiles WHERE id = %s",
                (player_id,),
            )
            return cursor.fetchone()

def save_purchase(player_id: str, item_id: str):
    with psycopg2.connect(host=writer, dbname="game") as conn:
        with conn.cursor() as cursor:
            cursor.execute(
                "INSERT INTO purchases(player_id, item_id) VALUES (%s, %s)",
                (player_id, item_id),
            )
        conn.commit()
```

Nguyên tắc là route write tới writer endpoint và phân phối các thao tác đọc tới
reader capacity. Topology, connection pool, transaction strategy và yêu cầu
consistency phải được thiết kế riêng cho từng game.

## 4. Bài học rút ra

### 4.1 Reliability là một phần của trải nghiệm người chơi

Người chơi không phân biệt được lỗi application và database outage. Nếu progress
bị mất hoặc giao dịch bị trễ, sản phẩm sẽ bị đánh giá là không đáng tin cậy.

### 4.2 Managed service giảm phần việc không tạo khác biệt

Giá trị của Aurora không chỉ nằm ở engine compatibility. Managed service giúp
giảm thời gian bảo trì phần cứng, sửa storage, backup và xử lý failover, trong
khi đội phát triển vẫn kiểm soát schema và query.

### 4.3 Tách read workload và write workload

Game backend thường có rất nhiều thao tác đọc như profile, inventory,
leaderboard, cấu hình và event data. Tách read capacity khỏi writer giúp bảo vệ
các giao dịch quan trọng khi traffic tăng.

### 4.4 Thiết kế cho event peak, không chỉ traffic trung bình

Live game có launch, seasonal event, promotion và content update. Database chạy
ổn trong một tuần bình thường có thể thất bại trong event lớn. Vì vậy cần test
peak concurrency và write burst.

### 4.5 Managed không có nghĩa là bỏ qua monitoring

Đội vận hành vẫn cần alarm cho connection saturation, query latency, replication
lag, storage, failed transaction và write volume bất thường.

## 5. Khi nào nên cân nhắc Aurora?

Aurora phù hợp khi ứng dụng cần relational engine managed, high availability qua
nhiều Availability Zone, read replica, automated backup hoặc global database.
AWS mô tả Aurora là database tương thích với MySQL và PostgreSQL, sử dụng
distributed storage có khả năng tăng trưởng theo dữ liệu. [Aurora overview](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

Tuy nhiên, Aurora không tự động là lựa chọn tốt nhất cho mọi dự án. Prototype
nhỏ có thể phù hợp hơn với một PostgreSQL managed đơn giản. Cần đánh giá:

- traffic và write volume dự kiến;
- recovery point objective và recovery time objective;
- số Availability Zone và Region;
- yêu cầu read replica và failover;
- connection pooling và query behavior; và
- tổng chi phí instance, storage, I/O, backup và data transfer.

## 6. Giới hạn khi diễn giải case study

AWS customer story chỉ mô tả ở mức tổng quan, không cung cấp schema, cluster
size, query design hay failover runbook đầy đủ của Supercell. Vì vậy bài viết này
không suy đoán các chi tiết độc quyền đó. Code và mô hình phía trên chỉ là các
pattern kỹ thuật để minh họa bài học.

## Kết luận

Case study của Supercell cho thấy database reliability ảnh hưởng trực tiếp đến
niềm tin của người chơi. Amazon Aurora có thể giảm phần việc vận hành nhờ
replication, distributed storage, backup và failover managed. Bài học rộng hơn
là cần chọn database dựa trên yêu cầu reliability và peak workload, không chỉ
dựa vào traffic trung bình hoặc benchmark.

## Tài liệu tham khảo

- [Supercell Leverages AWS for Seamless and Scalable Gaming Experience — AWS Customer Story](https://aws.amazon.com/solutions/case-studies/supercell-aurora-case-study/)
- [What is Amazon Aurora? — AWS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)
- [High availability for Amazon Aurora — AWS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html)
- [Amazon Aurora storage reliability — AWS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.StorageReliability.html)
