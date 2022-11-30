# Django

## Best practices
- Django raw query sql injection 
```python
  name = " 1 OR  1=1"
  queryset = Book.objects.raw("""SELECT * FROM book WHERE name = %s""", [name])
  queryset = Book.objects.raw("""SELECT * FROM book WHERE name = {}""".format(name))
  queryset = Book.objects.raw(f"""SELECT * FROM book WHERE name =  {name}""")
```
> Nên sử dụng cách 1
