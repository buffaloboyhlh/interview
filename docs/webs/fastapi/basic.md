# FastAPI 基础

FastAPI 是一个现代、快速（高性能）的 Web 框架，用于使用 Python 构建 API。它基于 Python 3.7+ 的类型提示，并使用 Starlette 和 Pydantic 提供高性能的异步支持和数据验证。

下面是一个详细的 FastAPI 使用教程，涵盖从安装、配置、创建项目到开发和部署 API 的各个方面。

### 1. **安装 FastAPI 和 Uvicorn**

#### 1.1 **安装 FastAPI**

使用 `pip` 安装 FastAPI：

```bash
pip install fastapi
```

#### 1.2 **安装 Uvicorn**

Uvicorn 是一个快速的 ASGI 服务器，用于运行 FastAPI 应用。安装 Uvicorn：

```bash
pip install uvicorn
```

### 2. **创建 FastAPI 项目**

#### 2.1 **创建基本项目结构**

创建一个新目录作为项目根目录，并在其中创建一个 Python 文件，例如 `main.py`。

```bash
mkdir myfastapi
cd myfastapi
touch main.py
```

#### 2.2 **编写基本的 FastAPI 应用**

在 `main.py` 中编写一个简单的 FastAPI 应用：

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

#### 2.3 **运行 FastAPI 应用**

使用 Uvicorn 启动应用：

```bash
uvicorn main:app --reload
```

这里的 `main:app` 指的是 `main.py` 文件中的 `app` 对象，`--reload` 标志用于在代码更改时自动重新加载服务器。

在浏览器中访问 `http://127.0.0.1:8000`，你将看到返回的 JSON 响应 `{"Hello": "World"}`。

### 3. **定义路径操作**

在 FastAPI 中，路径操作（即 API 端点）由装饰器和对应的函数定义。路径操作可以处理各种 HTTP 方法，如 GET、POST、PUT、DELETE 等。

#### 3.1 **路径参数**

路径参数用于在 URL 路径中传递参数。例如，上述示例中的 `/items/{item_id}` 就是一个包含路径参数 `item_id` 的路径操作。

```python
@app.get("/users/{user_id}")
def read_user(user_id: int):
    return {"user_id": user_id}
```

#### 3.2 **查询参数**

查询参数通过 URL 中的 `?key=value` 形式传递，可以为可选参数提供默认值。

```python
@app.get("/items/")
def read_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

#### 3.3 **请求体**

对于需要在请求中发送 JSON 数据的情况，可以使用 Pydantic 模型定义请求体。Pydantic 提供了强大的数据验证功能。

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.post("/items/")
def create_item(item: Item):
    return item
```

### 4. **数据验证和 Pydantic**

FastAPI 使用 Pydantic 进行数据验证和解析。Pydantic 模型在定义时自动生成请求体的数据验证逻辑。

#### 4.1 **Pydantic 模型的使用**

```python
class User(BaseModel):
    username: str
    email: str
    password: str

@app.post("/users/")
def create_user(user: User):
    return {"username": user.username, "email": user.email}
```

#### 4.2 **字段校验**

Pydantic 提供了多种字段校验选项，例如默认值、字段限制、正则表达式等。

```python
class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

    @validator('price')
    def price_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError('price must be positive')
        return v
```

### 5. **依赖注入**

FastAPI 提供了强大的依赖注入系统，可以通过在路径操作中注入依赖函数来实现代码的模块化和复用。

#### 5.1 **定义依赖**

```python
from fastapi import Depends

def get_db():
    db = {"users": ["Alice", "Bob", "Charlie"]}
    try:
        yield db
    finally:
        pass  # 关闭数据库连接

@app.get("/users/")
def read_users(db: dict = Depends(get_db)):
    return db["users"]
```

#### 5.2 **类作为依赖**

你还可以使用类的实例作为依赖，这在处理多种依赖时尤其有用。

```python
class Settings:
    def __init__(self, db_url: str):
        self.db_url = db_url

@app.get("/settings/")
def get_settings(settings: Settings = Depends()):
    return {"db_url": settings.db_url}
```

### 6. **处理响应**

FastAPI 支持多种响应模式，例如返回 JSON、HTML、文件、流式数据等。

#### 6.1 **返回 JSON 响应**

默认情况下，FastAPI 会将返回的 Python 字典转换为 JSON 响应。

```python
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

#### 6.2 **返回 HTML 响应**

你可以使用 `HTMLResponse` 返回 HTML 内容。

```python
from fastapi.responses import HTMLResponse

@app.get("/html", response_class=HTMLResponse)
def get_html():
    return "<h1>Hello, World!</h1>"
```

#### 6.3 **返回文件**

使用 `FileResponse` 返回文件：

```python
from fastapi.responses import FileResponse

@app.get("/download")
def download_file():
    return FileResponse("path/to/your/file.zip")
```

### 7. **错误处理**

FastAPI 提供了内置的错误处理机制，可以使用 `HTTPException` 手动抛出 HTTP 错误。

```python
from fastapi import HTTPException

@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id == 0:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"item_id": item_id}
```

### 8. **安全与认证**

FastAPI 支持 OAuth2、JWT 等多种认证机制。这里以 OAuth2 Password Flow 为例：

#### 8.1 **OAuth2 密码流**

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

@app.post("/token")
def login(form_data: OAuth2PasswordRequestForm = Depends()):
    # 验证用户并生成 token
    pass

@app.get("/users/me")
def read_users_me(token: str = Depends(oauth2_scheme)):
    # 根据 token 返回当前用户信息
    return {"token": token}
```

### 9. **中间件**

FastAPI 支持中间件，可以在请求和响应之间添加自定义逻辑。

```python
from starlette.middleware.base import BaseHTTPMiddleware

class CustomMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request, call_next):
        response = await call_next(request)
        response.headers["X-Custom-Header"] = "Value"
        return response

app.add_middleware(CustomMiddleware)
```

### 10. **项目结构与模块化**

#### 10.1 **组织代码**

随着项目的增长，建议将代码模块化。例如，将路径操作、模型和依赖分到不同的文件中：

```
myfastapi/
│
├── main.py
├── routers/
│   ├── items.py
│   └── users.py
└── models/
    ├── item.py
    └── user.py
```

#### 10.2 **加载路由**

在 `main.py` 中导入并包含模块化的路由：

```python
from fastapi import FastAPI
from routers import items, users

app = FastAPI()

app.include_router(items.router)
app.include_router(users.router)
```

### 11. **部署 FastAPI 应用**

#### 11.1 **使用 Uvicorn 部署**

在生产环境中，可以使用 Uvicorn 或 Gunicorn 配合 Uvicorn 部署 FastAPI 应用：

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

#### 11.2 **使用 Docker 部署**

创建一个 `Dockerfile` 并构建 Docker 镜像：

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
```

构建和运行容器：

```bash
docker build -t myfastapi .
docker run -d -p

 80:80 myfastapi
```

### 12. **文档生成与交互式 API**

FastAPI 会自动生成基于 OpenAPI 的文档和 Swagger UI。你可以在以下 URL 访问这些文档：

- **Swagger UI**: `http://127.0.0.1:8000/docs`
- **ReDoc**: `http://127.0.0.1:8000/redoc`

### 13. **测试**

#### 13.1 **使用 pytest**

FastAPI 与 pytest 完美兼容，可以编写和运行单元测试。

```python
from fastapi.testclient import TestClient
from .main import app

client = TestClient(app)

def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"Hello": "World"}
```

运行测试：

```bash
pytest
```

### 14. **学习资源**

- **官方文档**: [https://fastapi.tiangolo.com/](https://fastapi.tiangolo.com/)
- **书籍**: 《FastAPI 官方教程》
- **视频教程**: [YouTube 上的 FastAPI 教程](https://www.youtube.com/results?search_query=fastapi)

通过以上内容，你可以从零开始构建一个功能完整的 FastAPI 项目，并逐步掌握 FastAPI 的核心功能与高级用法。建议通过实际项目和文档深入学习 FastAPI，以进一步提升开发技能。