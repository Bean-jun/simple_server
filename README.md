# A simple python web server frame

> `wranning`: same flask, not flask!

### demo

-> main.py


### web server 自定义协议

```python
from bframe.ctx import request
from bframe.frame import Frame


app = Frame(__name__)


@app.get("/favicon.ico")
def index():
    with open("favicon.ico", "rb") as f:
        data = f.read()
    return data


@app.get("/main")
@app.get("/home")
def home():
    return request.Headers


if __name__ == "__main__":
    app.run(address="0.0.0.0")
```

### wsgi支持

若您需要使用wsgi功能的支持，使用`WSGIProxy`类进行包装即可

```python
from bframe.ctx import request
from bframe.frame import Frame


app = Frame(__name__)


@app.get("/favicon.ico")
def index():
    with open("favicon.ico", "rb") as f:
        data = f.read()
    return data


@app.get("/main")
@app.get("/home")
def home():
    return request.Headers


if __name__ == "__main__":
    from bframe.wsgi_server import WSGIProxy
    from wsgiref.simple_server import make_server

    with make_server('', 7256, WSGIProxy(app)) as httpd:
        print("Serving on port 7256...")
        httpd.serve_forever()
```