# Context Managers

## VD Context Managers

```python
with open('foo', 'w') as f:
    f.write('Hora! We opened this file')
```

## Cách tạo Context Managers

```python
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        if self.file:
            self.file.close()
            
with FileManager('example.txt', 'w') as f:
    f.write('Hello, world!')
```

```python
from contextlib import contextmanager
import datetime

@contextmanager
def measure_contextmanager():
    try:
        start = datetime.datetime.now()
        yield
    finally:
        end = datetime.datetime.now()
        diff = (end - start)
        print(f'Run: {diff.total_seconds()}s')
        
with measure_contextmanager() as _:
    print('test')
```
