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
