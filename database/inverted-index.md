## Fulltext Search
- Đặt vấn đề
    + Tìm kiếm text bắt đầu bằng 1 từ bất kỳ
    + Tìm kiếm text bằng 1 số từ trong câu
- Fulltext search (Inverted Index ): là một cấu trúc dữ liệu, nhằm mục đích map giữa các từ, chữ số và các document chứa chúng
```text
    D1 = "I am student"
    D2 = "Developer"
    D3 = "I am Developer"
```

```text
-> Inverted Index:
    "I" => {D1, D3}
    "am" => {D1, D3}
    "student" => {D1}
    "Developer" => {D2, D3}
```

- N-Gram: N-gram là kĩ thuật tách một chuỗi thành các chuỗi con, thông qua việc chia đều chuỗi đã có thành các chuỗi con đều nhau, có độ dài là N.
- Morphological Analysis: Về cơ bản Morphological Analysis là một kĩ thuật phổ biến trong xử lý ngôn ngữ tự nhiên (Natural Language Processing). Tức là chúng ta sẽ tách chuỗi thành những từ có nghĩa.
---
- Tạo fulltext search
```sql
ALTER TABLE table_name  ADD FULLTEXT(column_name1, column_name2,…)
CREATE FULLTEXT INDEX index_name ON table_name(idx_column_name,...)
```

---
- Match...against

Một tính năng rất quan trọng trong việc xử lý tìm kiếm đó là sắp xếp kết quả trả về theo thứ tự: tài liệu nào giống nhiều nhất thì nằm trên cùng, giống ít nhất thì nằm dưới cùng.

Khi bạn sử dụng hàm MATCH() ở lệnh WHERE thì MySQL sẽ trả về giá trị có mức độ liên quan lên đầu tiên.
```sql
 SELECT * FROM books_book WHERE MATCH(`description`) AGAINST('student')
```
- IN BOOLEAN MODE
```sql
 SELECT * FROM books_book WHERE MATCH(`description`) AGAINST('student' IN BOOLEAN MODE)
```

- Query Expansion
```sql
 SELECT * FROM books_book WHERE MATCH(`description`) AGAINST('student' WITH QUERY EXPANSION)
```

>Tóm lai, bạn cần lưu ý những vấn đề sau khi làm việc với full text search trong MySQL:
>- Độ dài tối thiểu cho từ cần tìm là 4. Ví dụ bạn tìm từ "và" hoặc "tôi" thì mặc định MySQL sẽ xác định đó là những từ vô nghĩa. Vì trong tiếng Anh những từ có 3 chữ cái đều là vô nghĩa. Tuy nhiên bạn có thể mở file /etc/mysql/my.cnf và tìm đến dòng ft_min_word_len = 3 chỉnh lại số mong muốn.
>- Có một số từ Stop Words sẽ bị bỏ qua nằm trong file storage/myisam/ft_static.c. Bạn muốn thay đổi thì hãy vào file đó nhé.
