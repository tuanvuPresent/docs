# mysql perfomance
 
---

- B-tree
- Phân loại index
  + Clustered Index: lưu trữ và sắp xếp dữ liệu vật lý trong table hoặc view dựa trên các giá trị khóa của chúng
  + Non-Clustered Index: có một cấu trúc tách biệt với data row trong table hoặc view. Mỗi một index loại này chứa các giá trị của các cột khóa trong khai báo của index, và mỗi một bản ghi giá trị của key trong index này chứa một con trỏ tới dòng dữ liệu tương ứng của nó trong table
  + Unique Index, Primary Key, Composite index
- where
- join
  + loop
  + Hash
  + merge
- sort
- group by 
  + sort/group algorithm
  + hash algorithm
- Partial
  + Offset and limit
  + Seek
  + Using Window Functions for Pagination
- Thứ tự Composite index ảnh hưởng đến query
```sql
  // Composite index (c1, c2)
  select c1, c2, count(c3)
  from t1 
  where c1 = 'x'
  group by c1, c2
  order by c1, c2
  ```
---

- Lock row và lock table
- Table Conversions
    + alter
    + dump and import
    + create and select
- Show profile
- Optimal
    + number vs string
    + Not null
    + Int(1) vs int(20) lưu trữ và tính toán như nhau
    + Temp table không hỗ trợ blob và text ( chuyển blob thành chuỗi trong query thì có thế lưu trên mem)
    + Đếm không chính xác hoặc đếm chính xác với các truy vấn phạm vi nhỏ
- Hash index
    + not sort
    + where >, <
- Index:
    + giảm dữ liệu phải kiểm tra
    + tránh sắp xếp và bảng tạm thời
    + I/O ngẫu nhiên thành tuần tự
- Id
    + auto increase
    + uuid
    + Chèn giá trị với id không tuần tự khiến dữ liệu bị phân mảnh
- Optimazing
    + Chia nhỏ query
    + sort ( Two passes (old) , single pass)
    + count('*') đếm số hàng, count('c1') đếm số hàng có c1 != null
- Partitions
    + khi thêm, sửa, xoá ( cập nhật lại data vào đúng phân vùng )
    + các loại phân vùng (range,list, hash, key )
    + Tương tự như index ( Year(DAY) = 2000 partition không hoạt động )

