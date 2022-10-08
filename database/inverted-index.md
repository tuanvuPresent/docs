# Inverted Index

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
