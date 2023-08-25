# index sql

---
- Các loại index
    + B-tree
    + Hash
    + Bitmap
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
