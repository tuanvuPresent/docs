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

```python
import time
import functools

# Define a decorator function to cache the result of a function with a timeout
def cache(timeout):
    memo = {}
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args):
            if args in memo and time.time() - memo[args]['time'] < timeout:
                return memo[args]['result']
            else:
                result = func(*args)
                memo[args] = {'result': result, 'time': time.time()}
                return result
        return wrapper
    return decorator
```
