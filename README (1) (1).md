# Message queue

## Phân loại

### Message Base: RabitMQ, ActiveMQ, SQS, ZeroMQ, MSMQ, IronMQ

1. Việc đảm bảo mỗi consumer đều nhận được message và duy nhất một lần là quan trọng nhất.
2. Các đặc trưng của loại Message Base như: lưu trạng thái của các consumer nhằm đảm bảo tất cả các consumer đều nhận được message từ topic mà đã subscribe.
3. Sau khi tất cả các consumer nhận được message thì message đó sẽ bị xóa.
4. Khi có một message mới một consumer chỉ có thể lấy được môt message duy nhất

### Data Pipeline: Kafka, Kinesis, RocketMQ

1. Việc đảm bảo mỗi consumer đều phải nhận được message và duy nhất một lần không phải là ưu tiên số một, mà thay vào đó là khả năng lưu trũ message vả tốc độ truyền tải message.&#x20;
2. Khi có message mới, consumer sẽ lựa chọn số lượng message mà mình muốn lấy, chính vì thế mà cùng một message consumer có thể nhận đi nhận lại nhiều lần.&#x20;
3. Những hệ thống sử dụng message queue loại này thường là hệ thống Event Sourcing, hoặc hệ thống đồng bộ dữ liệu từ những database khác nhau Các đặc trưng của loại Data Pipeline như: không lưu trạng thái của consumer, message được xóa sau một khoảng thời gian nhất định,&#x20;
4. Khi có một message mới, consumer có thể tùy chọn lấy về một danh sách message bao gồm cả message cũ hoặc chỉ lấy message mới.&#x20;

## Một số loại message queue

### RabbitMQ (AMQP)

* 50k message/second.
* Khả năng của bên tiêu thụ tin nhắn: One-to-one và one-to-many: hỗ trợ cả hai.

### Kafka

* 1millions message/second.
* Khả năng của bên tiêu thụ tin nhắn: Chỉ hỗ trợ one-to-many.
* Lưu trên file

### Redis

* 1millions message/second.
* Khả năng của bên tiêu thụ tin nhắn:: One-to-one và one-to-many: hỗ trợ cả hai.

## Các use case với message broker

Lưu tin nhắn ngắn hạn thì dùng Redis Cơ sở dữ liệu trong bộ nhớ của Redis phù hợp với các trường hợp sử dụng các thông điệp ngắn hạn mà việc peristent data không quan trọng, chúng ta có thể chấp nhận được trong trường hợp mất mát dữ liệu.

Xử lý dữ liệu lớn, cần persistent data thì dùng Kafka Kafka là một distributed queue phù hợp với những trường hợp cần thông lượng cao và cần lưu trữ một lượng lớn dữ liệu trong thời gian dài. (phù hợp với trường hợp cần mô hình pub/sub dạng one-to-may và persistent data)

Định tuyến phức tạp thì dùng RabbitMQ Phù hợp với những trường hợp cần hỗ trợ định tuyến phức tạp với khả năng khoảng vài chục nghìn message/second).

## Kafka

### Brokers cluster

* Dữ liệu nhận được được lưu trữ trên cụm Kafka được lưu trữ ở ổ đĩa mà Broker được cấu hình, sẽ tự động xóa sau 1 khoảng thời gian retention time (mặc đinh là 1 tuần).

### Producer

* Ack = 0: sẽ không đợi phản hồi từ consumer trước khi cho rằng thông báo đã được gửi thành công
* Ack = 1: sẽ nhận được phản hồi thành công từ consumer tại thời điểm bản sao lãnh đạo nhận được thông báo
* Ack = all: sẽ nhận được phản hồi thành công từ consumer khi tất cả các bản sao đồng bộ nhận được thông báo
* Giả sử topic\_1 có 2 partition là p1 và p2:
  * 1 producer publish bản ghi mới vào, bản ghi này có thể nằm ở p1 hoặc p2, tùy cách producer đẩy (mặc định là round-robin, tức là bản ghi trước đã vào p1 thì bản ghi sau sẽ vào p2 cho đều).
  * Để tăng tốc, ta có thể viết 2 producer, mỗi producer chi đẩy dữ liệu vào 1 partition, như vậy về logic tốc độ ghi đã tăng gấp 2.

### Consumer

* Kafka cluster không đẩy dữ liệu cho consumer, tiến trình consumer sẽ chủ động lấy dữ liệu từ hệ thống ra (pull) chứ kafka cluster không đẩy dữ liệu cho consumer.
* 4 partitions - 1 consumer ==> consumer sẽ đọc hết các message
* 4 partitions - 2 consumer (cùng 1 group) ==> mỗi consumer sẽ đọc 2 partitions
* 4 partitions - 4 consumer (cùng 1 group) ==> mỗi consumer sẽ đọc 1 partitions
* partitions < consumer ==> một số consumer sẽ nhàn rỗi không đọc message nào cả

### Topic - Partitions và segments

* Topic là khái niệm về mặt logic để tổ chức dữ liệu, kafka tất nhiên cần phải lưu trữ các bản ghi trong topic này trên các server cài kafka (kafka broker), partitions là việc tổ chức dữ liệu trên các server này, mỗi partition là 1 thư mục vật lý, mỗi partition gồm nhiều file khác nhau, các file này chính là các file chứa dữ liệu.
* 1 Kafka cluster có nhiều Topic, mỗi topic gồm nhiều partition, mỗi partition là tập hợp nhiều segment

## So sánh Kafka với RabbitMQ

* push (RabbitMQ): Provider push message vào queue cho Consumer -> nếu consumer ko xử lý kịp thì có thể tràn queue.
* pull(Kafka): Kafka dùng topic chứ không phải queue. Các messages trong topic được lưu trữ dạng log và được xoá đi sau 1 thời gian. Trong khi đó các consumer theo dõi 1 offset trong log này để lấy dữ liệu. Ưu điểm là bạn có thể tua lại thời điểm xảy ra lỗi: VD hệ thống payment xử lý lỗi trong 24h bạn chỉ cần dịch chuyển offset về trước đó 24h. Trong khi đó với RabbitMQ bạn chỉ có cách republish lại các request, vì các message đã được lấy ra khỏi queue -> Khả năng xử lý lỗi của Kafka tốt hơn.

{% hint style="info" %}
Kafka sinh ra để stream, vì nó đảm bảo được thứ tự của các message, dựa vào key trong mỗi partition. Không ai dùng kafka để làm queue chạy background job cả, vì khi số consumer > số partion, thì việc scale là vô nghĩa. rabbitmq sinh ra để làm queue chạy background job, rabbitmq không mạnh trong việc đảm bảo thứ tự của msg, có thể cố đấm ăn xôi bằng cách set header priority, nhưng không linh hoạt được. kafka persistance data tốt hơn, có thể cấu hình thời gian msg có thể được lưu lại disk. Có thể đọc lại bất kỳ msg cũ từ cách khai báo offset. Nên consumer có thể "đọc lại" msg bất cứ khi nào muốn. Rabbitmq không làm được điều này, từ version 3.8 có hỗ trợ Quorum Queue để HA message, còn các queue khác, thì khi msg đã được subscriber báo ACK đọc msg rồi, thì nó sẽ bị xóa khỏi queue.
{% endhint %}
