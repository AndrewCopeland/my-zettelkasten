# 1617906476 python3-custom-exception

```python3
class CustomError(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)
```

## Links
