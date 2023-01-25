# Datatype

- Primary key
  + auto increment
  + uuid

- String 
  + Nếu độ dài cố định nên sử dụng char
  + tinyInt > Enum > char

- Tránh sử dụng text và blob
  + cột BLOB hoặc TEXT trong kết quả của một truy vấn được xử lý bằng bảng tạm thời khiến máy chủ sử dụng bảng trên đĩa thay vì trong bộ nhớ vì công cụ lưu trữ BỘ NHỚ không hỗ trợ các loại dữ liệu đó
