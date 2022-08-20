# Nosql và RDBMS

## Nosql DBMS

### Các loại NoSQL DBMS:

* Key-value store
* Document store
* Wide comlumn store
* Graph store

#### Key-value store

* DBMS: Redis, Amazon DynamoDB , FoundationDB.

#### Document store

* DBMS: MongoDB, CouchDB, Elasticsearch, Solr.

#### Wide column store (Big table)

* DBMS: Cassandra, HBase,Microsoft Azure Cosmos DB.

#### Graph store

* DBMS: Neo4j, Titan.

#### **Search Engine:**

* Elasticsearch, Splunk, Solr

#### **Multi-Model:**

* Amazon DynamoDB: sử dụng 2 mô hình là Document store và Key-value store.
* OrientDB: sử dụng 3 mô hình là Document store, Key-value store, Graph store.
* Microsoft Azure Cosmos DB: sử dụng cả 4 mô hình trên.

## RDBMS

* Oracle, MySQL, MS Server, PostgreSQL
* Các hệ thống RDBMS hữu dụng trong việc xử lí các dữ liệu được cấu trúc kỹ càng và hỗ trợ ACID – 4 thuộc tính quan trọng của bất kì hệ thống cơ sở dữ liệu nào:

{% hint style="info" %}
Tính nguyên tố (Atomicity). Một giao dịch có nhiều thao tác khác biệt thì hoặc là toàn bộ các thao tác hoặc là không một thao tác nào được hoàn thành. Chẳng hạn việc chuyển tiền có thể thành công hay trục trặc vì nhiều lý do nhưng tính nguyên tố bảo đảm rằng một tài khoản sẽ không bị trừ tiền nếu như tài khoản kia chưa được cộng số tiền tương ứng.
{% endhint %}

{% hint style="info" %}
Tính nhất quán (Consistency). Một giao dịch hoặc là sẽ tạo ra một trạng thái mới và hợp lệ cho dữ liệu, hoặc trong trường hợp có lỗi sẽ chuyển toàn bộ dữ liệu về trạng thái trước khi thực thi giao dịch.
{% endhint %}

{% hint style="info" %}
Tính độc lập (Isolation). Một giao dịch đang thực thi và chưa được xác nhận phải bảo đảm tách biệt khỏi các giao dịch khác.
{% endhint %}

{% hint style="info" %}
Tính bền vững (Durability). Dữ liệu được xác nhận sẽ được hệ thống lưu lại sao cho ngay cả trong trường hợp hỏng hóc hoặc có lỗi hệ thống, dữ liệu vẫn đảm bảo trong trạng thái chuẩn xác.
{% endhint %}

## CAP theorem

{% hint style="info" %}
#### &#x20;Consistency – tính nhất quán <a href="#2-1-consistency-tinh-nhat-quan" id="2-1-consistency-tinh-nhat-quan"></a>

Tính nhất quán có nghĩa là tại cùng một thời điểm, dữ liệu mà tất cả các máy khách nhìn thấy phải là giống nhau, bất kể nó kết nối với node nào.
{% endhint %}

{% hint style="info" %}
#### Availability – tính sẵn sàng <a href="#2-2-availability-tinh-san-sang" id="2-2-availability-tinh-san-sang"></a>

Dữ liệu cũng phải sẵn sàng cho người dùng. Khi một node hoặc một service con down, các service khác vẫn có thể hoạt động độc lập.
{% endhint %}

{% hint style="info" %}
#### Partition tolerance – dung sai phân vùng <a href="#2-3-partition-tolerance-dung-sai-phan-vung" id="2-3-partition-tolerance-dung-sai-phan-vung"></a>

Phân vùng là sự đứt gãy liên lạc trong hệ thống phân tán, hay cụ thể hơn, là việc kết nối giữa hai node bị mất hoặc tạm thời bị trì hoãn. Dung sai phân vùng có nghĩa là cluster phải duy trì được trạng thái hoạt động dù cho có bất kỳ sự cố giao tiếp nào giữa các node trong hệ thống.
{% endhint %}
