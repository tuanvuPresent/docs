# Decorators&#x20;

Function decorator đơn giản là wrapper của một function có sẵn

{% code lineNumbers="true" %}
```python
def test_decorator(test_func):

    def wrap_func():
        print("start")
        test_func()
        print("end")

    return wrap_func

@test_decorator
def my_func():
    print('run')
    
my_func()
```
{% endcode %}

```python
# output
start
run
end

```
