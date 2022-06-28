### Có 2 loại Message queue gồm:
– Message Base: RabitMQ, ActiveMQ, SQS, ZeroMQ, MSMQ, IronMQ
Message Base là loại message queue truyền thống, thích hợp làm hệ thống trao đổi message giữa các service. Việc đảm bảo mỗi consumer đều nhận được message và duy nhất một lần là quan trọng nhất.Các đặc trưng của loại Message Base như: lưu trạng thái của các consumer nhằm đảm bảo tất cả các consumer đều nhận được message từ topic mà đã subscribe, sau khi tất cả các consumer nhận được message thì message đó sẽ bị xóa, khi có một message mới một consumer chỉ có thể lấy được môt message duy nhất.

– Data Pipeline: Kafka, Kinesis, RocketMQ
Data Pipeline có cách lưu trữ message cũng như truyền tải message đến consumer hoàn toán khác với hệ thống message queue truyền thống. Việc đảm bảo mỗi consumer đều phải nhận được message và duy nhất một lần không phải là ưu tiên số một, mà thay vào đó là khả năng lưu trũ message vả tốc độ truyền tải message. Khi có message mới, consumer sẽ lựa chọn số lượng message mà mình muốn lấy, chính vì thế mà cùng một message consumer có thể nhận đi nhận lại nhiều lần. Những hệ thống sử dụng message queue loại này thường là hệ thống Event Sourcing, hoặc hệ thống đồng bộ dữ liệu từ những database khác nhau
Các đặc trưng của loại Data Pipeline như: không lưu trạng thái của consumer, message được xóa sau một khoảng thời gian nhất định, khi có một message mới, consumer có thể tùy chọn lấy về một danh sách message bao gồm cả message cũ hoặc chỉ lấy message mới.
Khi các bạn lựa chọn message queue cho hệ thống của mình, các bạn nên xác định rõ mục địch của hệ thống messague queue để xem mình cần loại trong hai loại trên. Việc xác định được loại message queue nào mình cần sẽ giúp các bạn giảm bớt thời gian tìm hiểu cũng như tìm được chính sác cái mà mình cần.

### RabbitMQ (AMQP)
- 50k message/second.
- One-to-one và one-to-many: hỗ trợ cả hai.

### Kafka
- 1millions message/second.
- Chỉ hỗ trợ one-to-many. 
- Lưu trên file

### Redis
- 1millions message/second.
- One-to-one và one-to-many: hỗ trợ cả hai.

### Các use case với message broker
Lưu tin nhắn ngắn hạn thì dùng Redis
Cơ sở dữ liệu trong bộ nhớ của Redis phù hợp với các trường hợp sử dụng các thông điệp ngắn hạn mà việc peristent data không quan trọng, chúng ta có thể chấp nhận được trong trường hợp mất mát dữ liệu.

Xử lý dữ liệu lớn, cần persistent data thì dùng Kafka
Kafka là một distributed queue phù hợp với những trường hợp cần thông lượng cao và cần lưu trữ một lượng lớn dữ liệu trong thời gian dài. (phù hợp với trường hợp cần mô hình pub/sub dạng one-to-may và persistent data)

Định tuyến phức tạp thì dùng RabbitMQ
Phù hợp với những trường hợp cần hỗ trợ định tuyến phức tạp với khả năng khoảng vài chục nghìn message/second).

### Mô hình Kafka với RabbitMQ khác nhau lớn nhất có lẽ là 1 thằng pull và 1 thằng push.

- push (RabbitMQ): Provider push message vào queue cho Consumer -> nếu consumer ko xử lý kịp thì có thể tràn queue.
- pull(Kafka): Kafka dùng topic chứ không phải queue. Các messages trong topic được lưu trữ dạng log và được xoá đi sau 1 thời gian. Trong khi đó các consumer theo dõi 1 offset trong log này để lấy dữ liệu. Ưu điểm là bạn có thể tua lại thời điểm xảy ra lỗi: VD hệ thống payment xử lý lỗi trong 24h bạn chỉ cần dịch chuyển offset về trước đó 24h. Trong khi đó với RabbitMQ bạn chỉ có cách republish lại các request, vì các message đã được lấy ra khỏi queue -> Khả năng xử lý lỗi của Kafka tốt hơn.

kafka sinh ra để stream, vì nó đảm bảo được thứ tự của các message, dựa vào key trong mỗi partition. Không ai dùng kafka để làm queue chạy background job cả, vì khi số consumer > số partion, thì việc scale là vô nghĩa.

rabbitmq sinh ra để làm queue chạy background job, rabbitmq không mạnh trong việc đảm bảo thứ tự của msg, có thể cố đấm ăn xôi bằng cách set header priority, nhưng không linh hoạt được.

kafka persistance data tốt hơn, có thể cấu hình thời gian msg có thể được lưu lại disk. Có thể đọc lại bất kỳ msg cũ từ cách khai báo offset. Nên consumer có thể "đọc lại" msg bất cứ khi nào muốn.

Rabbitmq không làm được điều này, từ version 3.8 có hỗ trợ Quorum Queue để HA message, còn các queue khác, thì khi msg đã được subscriber báo ACK đọc msg rồi, thì nó sẽ bị xóa khỏi queue.
