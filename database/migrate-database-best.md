# Migrate database best

### Thay đổi tên column

* Tạo cột mới
* Sao chép dữ liệu hiện tại sang cột mới
* Thay đổi code
* Đổi tên cột cữ ( hoặc xóa)

### ALTER TABLE on a large table with an indexed column

```sql
SET SQL_LOG_BIN = 0;
ALTER TABLE WorkingTable RENAME WorkingTableOld;
CREATE TABLE WorkingTableNew LIKE WorkingTableOld;
ALTER TABLE WorkingTableNew MODIFY BigColumn VARCHAR(50);
INSERT INTO WorkingTableNew SELECT SQL_NO_CACHE * FROM WorkingTableOld;
ALTER TABLE WorkingTableNew RENAME WorkingTable;
DROP TABLE WorkingTableOld;
```
