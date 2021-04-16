# 1618535961 python-flask-get-query-params

```python
from flask import request

@app.route('/data')
def data():
    # here we want to get the value of user (i.e. ?user=some-value)
    user = request.args.get('user')
```

## Links
