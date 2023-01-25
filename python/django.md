# Django

## Sql injection 
- Django raw query sql injection 
```python
  name = " 1 OR  1=1"
  queryset = Book.objects.raw("""SELECT * FROM book WHERE name = %s""", [name])
  queryset = Book.objects.raw("""SELECT * FROM book WHERE name = {}""".format(name))
  queryset = Book.objects.raw(f"""SELECT * FROM book WHERE name =  {name}""")
```
> Nên sử dụng cách 1, cách 2 và 3 sẽ bị sql injection

## Transaction
- Autocommit (default): Mỗi truy vấn được cam kết ngay lập tức với cơ sở dữ liệu, trừ khi một giao dịch đang hoạt động
- atomic
```python
    @action(methods=['GET'], detail=False, url_path='test')
    @transaction.atomic
    def test(self, request, *args, **kwargs):
        Book.objects.create(name=1)
        try:
            with transaction.atomic():
                Book.objects.create(name=2)
                print(1/0)
        except:
            pass
        Book.objects.create(name=3)

        return Response()
```
--> Đoạn code save thành công book1 và book3 trong database.
- on_commit: Thực hiện một hành động liên quan đến giao dịch cơ sở dữ liệu hiện tại, nhưng chỉ khi giao dịch được thực hiện thành công
```python
    # celery task
    transaction.on_commit(lambda: my_fav_task.delay(param1))
```

## Lazy queryset
> Tạo 1 QuerySet không đồng nghĩa với việc truy vấn CSDL
```python
query = Entry.objects.filter(name="a")
query = query.exclude(id=1)
print(query)
```
> Câu query trên chỉ thực hiện truy vấn 1 lần
- Khi nào query được thực thi
+ Vòng lặp
+ Slicing
+ repr()
+ len()
+ list
+ bool( Nếu bạn chỉ muốn check 1 QuerySet có tồn tại hay không, sẽ cho tốc độ truy vấn nhanh và hiệu quả hơn nếu bạn dùng exists() )
- prefetch_related và select_related
```python
students = Student.objects.all()
for student in students:
    print(student.class.name)
```
> Đoạn code trên thực hiện N + 1 câu query
> Nên sử dụng như sau:
```python
students = Student.objects.all().select_related('class')
for student in students:
    print(student.class.name)
```
- bulk_create và bulk_update
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
- Use foreign key values directly
