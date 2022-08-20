# Iterators and Iterables and generators&#x20;

## Iterators

iterators là một đối tượng có thể được lặp lại, có nghĩa là bạn có thể duyệt qua tất cả các giá trị.

{% code lineNumbers="true" %}
```python
items = [1, 2, 3, 4, 5]
x = iter(items)
x.__next__()
# output
1
```
{% endcode %}

next() sẽ trả về giá trị phần tử tiếp theo hoặc ngoại lệ StopIteration

iterator cũng là iterable&#x20;

{% code lineNumbers="true" %}
```python
items = [1, 2, 3, 4, 5]
x = iter(items)
for s in x:
    print(s)
# output
1 
2
3
4
5
```
{% endcode %}

## Iterables

Một đối tượng là **iterable** nghĩa là nó có thể lặp qua, hiểu là một **iterable** thì:

1. Có thể lặp qua a được, tức có thể viết \*“for x in a” \*
2. gọi _iter(a)_, sẽ trả về một iterator
3. Có phương thức _\_\_iter\_\__ cũng trả về một iterator, hoặc đôi khi có phương thức _\getitem\\_ nếu a thuộc nhóm dữ liệu tuần tử có thể truy cập phần tử theo chỉ số index đã nói ở trên.

```python
items = [1, 2, 3, 4, 5]
for item in items:
    print(item)
```

## Generators

**generato**r hay gọi là **hàm generato**r, cho phép bạn tạo ra một hàm hoạt động tương tự như một iterator, tức là nó cũng là iterable và có thể dùng với vòng lặp for.

Cách tạo gennerators

```python
def list_n_generator(n):
    num = 0
    while num < n:
        yield num
        num += 1
```

```python
a = (x for x in [1, 2, 3, 4, 5)
```
