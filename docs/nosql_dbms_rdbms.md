## Nosql DBMS

---
Các loại NoSQL DBMS:

- Key-value store
- Document store
- Wide comlumn store
- Graph store

---
Key-value store
- DBMS: Redis, Amazon DynamoDB , FoundationDB.
---
Document store
- DBMS: MongoDB, CouchDB, Elasticsearch, Solr.
---
Wide column store (Big table)
- DBMS: Cassandra, HBase,Microsoft Azure Cosmos DB.
---
Graph store
- DBMS: Neo4j, Titan.
---
#### Search Engine: 
- Elasticsearch, Splunk, Solr
---
#### Multi-Model:
- Amazon DynamoDB: sử dụng 2 mô hình là Document store và Key-value store.
- OrientDB: sử dụng 3 mô hình là Document store, Key-value store, Graph store.
- Microsoft Azure Cosmos DB: sử dụng cả 4 mô hình trên.

## RDBMS

---

- Oracle, MySQL, MS Server, PostgreSQL
- Các hệ thống RDBMS hữu dụng trong việc xử lí các dữ liệu được cấu trúc kỹ càng và hỗ trợ ACID – 4 thuộc tính quan trọng của bất kì hệ thống cơ sở dữ liệu nào:
> Tính nguyên tố (Atomicity). Một giao dịch có nhiều thao tác khác biệt thì hoặc là toàn bộ các thao tác hoặc là không một thao tác nào được hoàn thành. Chẳng hạn việc chuyển tiền có thể thành công hay trục trặc vì nhiều lý do nhưng tính nguyên tố bảo đảm rằng một tài khoản sẽ không bị trừ tiền nếu như tài khoản kia chưa được cộng số tiền tương ứng.

> Tính nhất quán (Consistency). Một giao dịch hoặc là sẽ tạo ra một trạng thái mới và hợp lệ cho dữ liệu, hoặc trong trường hợp có lỗi sẽ chuyển toàn bộ dữ liệu về trạng thái trước khi thực thi giao dịch.

> Tính độc lập (Isolation). Một giao dịch đang thực thi và chưa được xác nhận phải bảo đảm tách biệt khỏi các giao dịch khác.

> Tính bền vững (Durability). Dữ liệu được xác nhận sẽ được hệ thống lưu lại sao cho ngay cả trong trường hợp hỏng hóc hoặc có lỗi hệ thống, dữ liệu vẫn đảm bảo trong trạng thái chuẩn xác.
