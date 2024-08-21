# Django 基础

Django 是一个功能强大且广泛使用的 Python Web 框架，旨在帮助开发人员快速构建安全、可维护的 Web 应用程序。以下是一个详细的 Django 使用教程，从安装、配置、创建项目到开发 Web 应用，涵盖了 Django 的核心功能。

### 1. **安装 Django**

#### 1.1 **创建虚拟环境**
在开始之前，建议创建一个 Python 虚拟环境，以便隔离项目的依赖包。

```bash
python -m venv myenv
```

激活虚拟环境（Windows）：

```bash
myenv\Scripts\activate
```

激活虚拟环境（Linux/MacOS）：

```bash
source myenv/bin/activate
```

#### 1.2 **安装 Django**

使用 `pip` 安装 Django：

```bash
pip install django
```

验证安装是否成功：

```bash
django-admin --version
```

### 2. **创建 Django 项目**

#### 2.1 **创建项目**

使用 `django-admin` 命令创建一个新的 Django 项目：

```bash
django-admin startproject myproject
```

这个命令会创建一个名为 `myproject` 的目录，其中包含以下文件和子目录：

- **manage.py**: 一个命令行工具，用于与 Django 项目进行交互。
- **myproject/**: 包含项目的设置文件和应用配置。
  - **\_\_init\_\_.py**: 使 `myproject` 目录成为一个 Python 包。
  - **settings.py**: 项目的全局设置。
  - **urls.py**: 项目的 URL 路由配置。
  - **wsgi.py**: 项目的 WSGI 入口，用于部署。
  - **asgi.py**: 项目的 ASGI 入口，用于异步服务和 WebSocket 支持。

#### 2.2 **启动开发服务器**

进入项目目录并启动开发服务器：

```bash
cd myproject
python manage.py runserver
```

如果一切正常，你会看到以下输出，并可以在浏览器中访问 `http://127.0.0.1:8000/`：

```
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
```

### 3. **创建 Django 应用**

#### 3.1 **什么是 Django 应用**

在 Django 中，一个项目可以包含多个应用（App），每个应用负责一个功能模块。应用是 Django 项目的组成部分，可以独立运行、复用或共享。

#### 3.2 **创建应用**

使用 `startapp` 命令创建一个新的应用：

```bash
python manage.py startapp myapp
```

这将创建一个名为 `myapp` 的目录，包含以下内容：

- **admin.py**: 用于注册模型到 Django 管理后台。
- **apps.py**: 应用的配置文件。
- **models.py**: 定义数据库模型。
- **views.py**: 定义视图逻辑。
- **migrations/**: 存放数据库迁移文件。
- **tests.py**: 编写应用的单元测试。

#### 3.3 **注册应用**

要让 Django 知道你的应用，必须在 `settings.py` 文件中的 `INSTALLED_APPS` 列表中添加应用：

```python
# myproject/settings.py
INSTALLED_APPS = [
    ...
    'myapp',
]
```

### 4. **模型与数据库**

Django 使用 ORM（对象关系映射）来处理数据库，通过定义模型类将数据库表映射为 Python 对象。

#### 4.1 **定义模型**

在 `models.py` 中定义模型，例如创建一个简单的博客文章模型：

```python
# myapp/models.py
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    published_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

#### 4.2 **迁移数据库**

模型定义完成后，执行迁移命令以创建数据库表：

```bash
python manage.py makemigrations
python manage.py migrate
```

`makemigrations` 会根据模型的变化生成迁移文件，而 `migrate` 会应用这些迁移文件到数据库中。

### 5. **Django 管理后台**

Django 提供了一个强大的管理后台，允许你通过 Web 界面管理模型数据。

#### 5.1 **创建超级用户**

首先，创建一个超级用户，以便访问管理后台：

```bash
python manage.py createsuperuser
```

输入用户名、邮箱和密码后，超级用户创建完成。

#### 5.2 **注册模型到管理后台**

在 `admin.py` 中注册模型：

```python
# myapp/admin.py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

现在你可以通过 `http://127.0.0.1:8000/admin/` 访问管理后台，并使用超级用户登录。

### 6. **视图与模板**

#### 6.1 **创建视图**

视图是 Django 的核心部分，负责处理请求并返回响应。在 `views.py` 中定义视图函数：

```python
# myapp/views.py
from django.shortcuts import render
from .models import Post

def home(request):
    posts = Post.objects.all()
    return render(request, 'home.html', {'posts': posts})
```

#### 6.2 **配置 URL**

在 `urls.py` 中配置 URL 路由，将 URL 映射到视图函数：

```python
# myproject/urls.py
from django.contrib import admin
from django.urls import path
from myapp import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.home, name='home'),
]
```

#### 6.3 **创建模板**

模板是 Django 的前端部分，负责呈现数据。在 `myapp` 目录下创建一个名为 `templates` 的目录，并在其中创建 `home.html`：

```html
<!-- myapp/templates/home.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Blog</title>
</head>
<body>
    <h1>Blog Posts</h1>
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
        <small>Published on {{ post.published_date }}</small>
    {% endfor %}
</body>
</html>
```

### 7. **表单处理**

#### 7.1 **创建表单**

在 `forms.py` 文件中创建一个表单类，用于处理用户输入：

```python
# myapp/forms.py
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content']
```

#### 7.2 **处理表单提交**

在视图中处理表单的提交：

```python
# myapp/views.py
from .forms import PostForm

def create_post(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('home')
    else:
        form = PostForm()
    return render(request, 'create_post.html', {'form': form})
```

在 `urls.py` 中添加路由：

```python
path('create/', views.create_post, name='create_post'),
```

在模板中渲染表单：

```html
<!-- myapp/templates/create_post.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Create Post</title>
</head>
<body>
    <h1>Create a New Post</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Save</button>
    </form>
</body>
</html>
```

### 8. **静态文件和媒体文件**

#### 8.1 **静态文件**

Django 管理 CSS、JavaScript 和图像等静态文件。创建一个 `static` 目录，并在 `settings.py` 中配置 `STATIC_URL` 和 `STATICFILES_DIRS`：

```python
# myproject/settings.py
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]
```

在模板中使用静态文件：

```html
{% load static %}
<link rel="stylesheet" href="{% static 'style.css' %}">
```

#### 8.2 **媒体文件**

媒体文件是用户上传的文件。配置 `MEDIA_URL` 和 `MEDIA_ROOT`：

```python
# myproject/settings.py
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "media"
```

在 `urls.py` 中添加媒体文件的路由：

```python
from django.conf import settings
from django.conf.urls.static import static

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

### 9. **用户认证**

Django 内置了用户认证系统，包括登录、注册、权限管理等功能。

#### 9.1 **登录和注销**

使用 Django 提供的认证视图处理登录和注销：

```python
from django.contrib.auth import views as auth_views

urlpatterns = [
    ...
    path('login/', auth_views.LoginView.as_view

(), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='logout'),
]
```

#### 9.2 **用户注册**

创建一个用户注册表单和视图：

```python
# myapp/forms.py
from django.contrib.auth.forms import UserCreationForm

class SignUpForm(UserCreationForm):
    class Meta:
        model = User
        fields = ['username', 'email', 'password1', 'password2']

# myapp/views.py
def signup(request):
    if request.method == 'POST':
        form = SignUpForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('login')
    else:
        form = SignUpForm()
    return render(request, 'signup.html', {'form': form})
```

在 `urls.py` 中添加路由：

```python
path('signup/', views.signup, name='signup'),
```

### 10. **部署 Django 项目**

在开发完成后，你可以将 Django 项目部署到生产环境中。常见的部署方式包括使用 WSGI（如 Gunicorn）和反向代理（如 Nginx），或者使用 PaaS 服务（如 Heroku）。

#### 10.1 **生产环境设置**

在生产环境中，你需要做一些额外的配置，如关闭调试模式、设置允许的主机、使用安全的数据库连接等。

```python
# myproject/settings.py
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com']
```

#### 10.2 **使用 Gunicorn 和 Nginx 部署**

安装 Gunicorn 并启动应用：

```bash
pip install gunicorn
gunicorn myproject.wsgi:application --bind 0.0.0.0:8000
```

使用 Nginx 配置反向代理，将流量转发到 Gunicorn。

#### 10.3 **使用 Heroku 部署**

安装 Heroku CLI，创建 `Procfile`，并部署到 Heroku：

```bash
echo "web: gunicorn myproject.wsgi" > Procfile
heroku create
git push heroku master
```

### 11. **学习资源**

- **Django 官方文档**: [https://docs.djangoproject.com/](https://docs.djangoproject.com/)
- **Django 教程**: [Django Girls 教程](https://tutorial.djangogirls.org/)
- **书籍**: 《Django Web 开发实战》、《Django 官方教程》

通过以上内容，你应该能够创建、开发和部署 Django Web 应用。Django 提供了非常丰富的功能模块，建议深入学习官方文档，并通过实践进一步提升 Django 开发技能。