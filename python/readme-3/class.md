# Class

## Các loại biến trong class

* Biến instance (Instance Variables)

```python
class MyClass:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

* Biến class (Class Variables)

```python
class MyClass:
    count = 0
```

## Các loại method trong Python class

1. Instance Method: là các phương thức được gắn liền với instance của class và thường được sử dụng để truy xuất và thay đổi giá trị của instance variable.
2. Class Method: là các phương thức được gắn liền với class và thường được sử dụng để thao tác với class variable.
3. Static Method: là các phương thức không được gắn liền với instance hay class, và thường được sử dụng như các hàm thông thường.

* Instance Method

```python
class MyClass:
    def instance_method(self, arg1, arg2):
        # method code

```

* Class Method

```python
class MyClass:
    class_var = 0
    
    @classmethod
    def class_method(cls, arg1, arg2):
        cls.class_var += 1
        # method code
```

* Static Method

```python
class MyClass:
    @staticmethod
    def static_method(arg1, arg2):
        # method code

```

## Magic Methods

1. `__init__(self, ...)`: Phương thức khởi tạo, được gọi khi một đối tượng mới được tạo.
2. `__str__(self)`: Phương thức chuyển đổi đối tượng thành chuỗi, được sử dụng khi đối tượng được in ra.
3. `__add__(self, other)`: Phương thức cộng hai đối tượng với nhau.
4. `__sub__(self, other)`: Phương thức trừ hai đối tượng với nhau.
5. `__eq__(self, other)`: Phương thức so sánh hai đối tượng có bằng nhau hay không.
6. `__lt__(self, other)`: Phương thức so sánh hai đối tượng có đối tượng thứ nhất nhỏ hơn đối tượng thứ hai hay không.
7. `__len__(self)`: Phương thức trả về độ dài của đối tượng.
8. `__getattr__(self, name)`: Phương thức được gọi khi một thuộc tính không tồn tại trên đối tượng.
9. `__setattr__(self, name, value)`: Phương thức được gọi khi một giá trị thuộc tính được gán cho đối tượng.
10. `__del__(self)`: Phương thức được gọi khi đối tượng bị xóa.
11. `__new__` là một magic method khác trong Python, được sử dụng để tạo một instance mới của một class. Nó là method đầu tiên được gọi khi một instance của class được khởi tạo.

    `__new__` là một static method (method của class, không phải instance của class) và nó trả về một instance của class. Nó có thể được sử dụng để customize cách một instance được tạo ra, ví dụ như thay đổi thuộc tính mặc định của instance, hoặc kiểm tra xem instance đã được tạo trước đó chưa để tái sử dụng nó.

    Về cơ bản, `__new__` được sử dụng để tạo một instance mới và trả về nó cho `__init__`, method thứ hai được gọi khi một instance mới được tạo ra.
