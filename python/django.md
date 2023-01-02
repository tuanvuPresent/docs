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
--> Đoạn code save thành công book1 và book2 trong database.
- on_commit: Thực hiện một hành động liên quan đến giao dịch cơ sở dữ liệu hiện tại, nhưng chỉ khi giao dịch được thực hiện thành công
```python
    # celery task
    transaction.on_commit(lambda: my_fav_task.delay(param1))
```
