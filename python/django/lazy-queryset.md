# Lazy queryset


> Tạo 1 QuerySet không đồng nghĩa với việc truy vấn CSDL
```python
query = Entry.objects.filter(name="a")
query = query.exclude(id=1)
print(query)
```

> Câu query trên chỉ thực hiện truy vấn 1 lần
* Khi nào query được thực thi
  * Vòng lặp
  * Slicing
  * repr()
  * len()
  * list
  * bool( Nếu bạn chỉ muốn check 1 QuerySet có tồn tại hay không, sẽ cho tốc độ truy vấn nhanh và hiệu quả hơn nếu bạn dùng exists() )
* prefetch\_related và select\_related

```python
students = Student.objects.all()
for student in students:
    print(student.class.name)
```

> Đoạn code trên thực hiện N + 1 câu query Nên sử dụng như sau:
```python
students = Student.objects.all().select_related('class')
for student in students:
    print(student.class.name)
```

* bulk\_create và bulk\_update

```python
Entry.objects.bulk_create([
    Entry(headline='This is a test'),
    Entry(headline='This is only a test'),
])
```

```python
entries[0].headline = 'This is not a test'
entries[1].headline = 'This is no longer a test'
Entry.objects.bulk_update(entries, ['headline'])
```

* Use foreign key values directly
