# Python 基础整理

## 面向对象的接口如何实现
在Python中并没有具体的接口类型，其接口的应用只是人为规定的。Python中的接口是由抽象类与抽象方法实现的，接口本身并不实现具体功能，也不包括功能代码，而是由继承它的子类来实现接口所有的抽象方法，实现具体的功能。接口的实现方法如下：

```
from abc import ABCMeta,abstractmethod
# 抽象接口类
class Interface(metaclass=ABCMeta):
    #抽象方法
    @abstractmethod
    def get_name(self):
        pass
    
    @abstractmethod
    def get_age(self):
        pass
    
    @abstractmethod
    def get_height(self):
        pass
# 具体接口类
class Person(Interface):
    def __init__(self,name,age,height):
        self.name = name
        self.age = age
        self.height = height
        
    def get_name(self):
        return self.name
    
    def get_age(self):
        return self.age
    
    def get_height(self):
        return self.height
```

## Python 和 其他语言相比有什么区别？ 优势在哪里

### 1. 面向对象和面向过程
编程语言根据功能的实现方式可以分为面向过程编程与面向对象编程，面向过程的编程语言有C、Fortran等，面向对象的编程语言有Java、Python等。面向过程的编程语言采用自顶向下的方式，逐步实现功能，相对面向对象编程语言来讲，运行效率较高，但是不利于移植。面向对象的编程语言，将功能转换为一个个对象，比较符合人们的思维习惯，便于理解，也具有很强的移植性与扩展性。

### 2. 解释型语言与编译型语言
程语言根据运行的方式可以划分为解释型语言与编译型语言，解释型语言有Java等，编译型语言有C等。解释型语言是指通过解释器将程序代码翻译为计算机可以识别的类型，通常是翻译一行执行一行，可以很方便地对代码进行修改与调整，但是运行效率相对编译型语言来讲较低。编译型语言是指在运行程序之前先对程序进行编译，生成可执行文件，然后运行可执行文件，好处是运行速度快，缺点是每次对程序修改后都要重新进行编译。Python属于解释型语言。
### 3. 强语言和弱语言
编程语言根据约束条件和语法规则等内容可以分为强语言与弱语言。强语言有Java等，弱语言有Python等。强语言的规则较多，使用较为麻烦。例如，Java声明变量时需要声明变量类型、变量，执行语句的结尾需要使用分号。弱语言规则较少，使用简单方便。例如，Python声明变量时不需要声明变量类型，也不需要在变量与执行语句的结尾使用分号。

## Python中类方法、类实例方法、静态方法有什么区别

### 1. 实例方法
实例方法是类中权限最大的方法，第一个参数通常是“self”，该方法只能由实例对象调用。实例方法的创建方式如：


## Random模块
### 1. random.seed(a=None, version=2)
**功能：** 初始化随机数生成器。可以使用固定的种子来生成可预测的随机数序列。
**参数：**

	•	a：种子值。可以是任何可哈希对象。如果为None，则使用当前系统时间或其他系统状态作为种子。
	•	version：如果是2，使用默认的种子算法；如果是1，使用较旧的种子算法。

### 2.  random.random()

	•	功能：生成一个0到1之间的随机浮点数。
	•	返回值：[0.0, 1.0)区间内的浮点数。

### 3.  random.uniform(a, b)
**功能：** 生成一个范围在a到b之间的随机浮点数。
**参数：**
    	•	a：范围的下限。
	•	b：范围的上限。
**返回值：**
区间[a, b]内的浮点数。

### 4.  random.randint(a, b)
**功能：**
生成一个范围在a到b之间的随机整数，包括a和b。

**参数：**
	•	a：范围的下限。
	•	b：范围的上限。

**返回值：** 区间[a, b]内的浮点数。

### 5. random.choice(seq)
**功能：** 从非空序列seq中随机选择一个元素。

**参数：**
•	seq：序列（例如列表、元组、字符串）。

**返回值：** 序列中的一个随机元素。

### 6.  random.choices(population, weights=None, *, cum_weights=None, k=1)

**功能：**  从population中随机选择k个元素，允许有权重。
**参数：**

	•	population：序列或可迭代对象。

	•	weights：每个元素的权重，可以是一个列表或元组。

	•	cum_weights：累积权重。如果设置了这个参数，则会忽略weights参数。

	•	k：要选择的元素数量。

返回值：列表，包含k个随机选择的元素。

### 7. random.sample(population, k)

	•	功能：从population中随机选择k个不重复的元素。
	•	参数：
	•	    population：序列或可迭代对象。
	•	    k：要选择的元素数量。
	•	返回值：列表，包含k个随机选择的不重复元素。

### 8. random.shuffle(x)
	•	功能：对序列x进行原地洗牌（即随机打乱顺序）。
	•	参数：
	    •	x：列表。
	•	返回值：无（函数直接修改输入的列表）。

## 常用的模块

### os模块
```
import os

print("操作系统的名字：",os.name)
print("当前工作的目录：",os.getcwd())

#------------ 路径操作 ------------#
os.chdir('/path/to/directory')  # 切换到指定目录
# 智能地连接一个或多个路径组件。
full_path = os.path.join('/usr', 'bin', 'python')
print(full_path)  # 输出：/usr/bin/python
# 返回路径的绝对路径。
absolute_path = os.path.abspath('myfile.txt')
print(absolute_path)  # 输出文件的绝对路径
# 返回路径中的文件名部分。
filename = os.path.basename('/path/to/myfile.txt')
print(filename)  # 输出：myfile.txt
# 返回路径中的目录部分。
directory = os.path.dirname('/path/to/myfile.txt')
print(directory)  # 输出：/path/to
# 判断路径（文件或目录）是否存在
exists = os.path.exists('/path/to/myfile.txt')
print(exists)  # 输出：True 或 False
# os.path.isabs(path)
is_absolute = os.path.isabs('/usr/bin/python')
print(is_absolute)  # 输出：True

# 判断路径是否为文件
is_file = os.path.isfile('/path/to/myfile.txt')
print(is_file)  # 输出：True 或 False

# 判断路径是否为目录
is_dir = os.path.isdir('/path/to/directory')
print(is_dir)  # 输出：True 或 False

#------------ 文件和目录操作 ------------#
# 返回指定目录中的文件和目录列表
items = os.listdir('/path/to/directory')
print(items)  # 输出目录中的文件和子目录列表

# 创建目录 	path：要创建的目录路径。mode：目录的权限模式，默认是 0o777。
os.mkdir('/path/to/new_directory')  # 创建新目录


# 递归地创建目录及其父目录
os.makedirs(name, mode=0o777, exist_ok=False)   # name：要创建的目录路径。 exist_ok：如果目录已存在，不会引发异常。

# 删除指定路径的文件
os.remove('/path/to/myfile.txt')  # 删除文件

# 删除指定路径的目录（仅当目录为空时）
os.rmdir('/path/to/empty_directory')  # 删除空目录

# 重命名文件或目录
os.rename('/path/to/old_name.txt', '/path/to/new_name.txt')  # 重命名文件

# 重命名文件或目录，如果目标路径存在则会被替换
os.replace(src, dst)
#------------ 进程管理 ------------#
# 执行系统命令，返回命令的退出状态码
os.system('ls -l')  # 在Unix系统中列出当前目录内容

# 执行系统命令并打开管道以读写其输出
os.popen(command, mode='r', buffering=-1)  # command：要执行的命令字符串。mode：打开模式，默认是 'r'。buffering：缓冲区大小，默认是 -1。

# 返回当前进程的ID
os.getpid()  # 返回当前进程的ID

# 返回父进程的ID
os.getppid()  # 返回父进程的ID

# 创建一个子进程，返回子进程ID（在父进程中），或返回0（在子进程中）
pid = os.fork()
if pid == 0:
    print("这是子进程")
else:
    print("这是父进程，子进程ID:", pid)

#------------ 环境变量 ------------#
# 获取当前环境变量的字典
print(os.environ)  # 输出所有环境变量


# 获取指定环境变量的值
os.getenv(key, default=None) # key：环境变量名。default：默认值。

#------------ 文件描述符 ------------#
# 打开文件，返回文件描述符
fd = os.open(file, flags, mode=0o777)  # file：要打开的文件路径。flags：打开模式。mode：文件的权限模式，默认是 0o777。

# 从文件描述符 fd 中读取最多 n 个字节
os.read(fd, n)  # fd：文件描述符。n：读取的字节数。

# 将字符串 str 写入文件描述符 fd。
os.write(fd, str)  # fd：文件描述符。str：要写入的字符串。

# 关闭文件描述符 fd
os.close(fd)  # fd：文件描述符。
# 复制文件描述符 fd，返回一个新的文件描述符
new_fd = os.dup(fd)  # 复制文件描述符

# 复制文件描述符 fd 到 fd2
os.dup2(fd, fd2)  # 将 fd 复制到 fd2

# os 模块引发的错误的通用别名
os.error

# 将错误代码转换为错误消息字符串
error_message = os.strerror(2)
print(error_message)  # 输出错误消息，例如 "No such file or directory"
```

### time模块
time 模块是Python标准库中用于处理时间相关操作的模块。它提供了获取当前时间、测量时间间隔、暂停执行等多种功能。

```
import time

#------------ 获取当前时间 ------------#
# 返回当前时间的时间戳，表示自1970年1月1日00:00:00（UTC）以来的秒数。
current_time = time.time()
print("当前时间的时间戳:", current_time)

#  将时间戳（可选）转换为人类可读的时间字符串。如果不提供参数，返回当前时间的字符串表示。
time.ctime([secs])  # secs：时间戳。

# 将时间戳（可选）转换为本地时间的 struct_time 对象。如果不提供参数，使用当前时间
time.localtime([secs])  # secs：时间戳。

#------------ 格式化时间 ------------#
# 将 struct_time 对象格式化为指定格式的字符串。如果不提供时间对象 t，使用当前时间
time.strftime(format, t=None)  # format：格式化字符串 格式字符串（如 "%Y-%m-%d %H:%M:%S"）。。t：时间对象。

# 将字符串根据指定的格式解析为 struct_time 对象  time.strptime(string, format)
time_string = "2024-08-09 12:34:56"
parsed_time = time.strptime(time_string, "%Y-%m-%d %H:%M:%S")
print("解析后的时间:", parsed_time)

#------------ 时间延迟 ------------#
# 使程序暂停执行指定的秒数
print("暂停3秒")
time.sleep(3)
print("继续执行")

#------------ 测量时间间隔 ------------#
# 返回一个高精度的时间计数器，用于测量时间间隔。这个计数器是系统运行时间的一部分，因此适合测量短时间间隔
start = time.perf_counter()
# 模拟耗时操作
time.sleep(1)
end = time.perf_counter()
print(f"操作耗时: {end - start} 秒")

# 返回当前进程使用CPU的时间总和，不包括睡眠时间
start = time.process_time()
# 模拟耗时操作
time.sleep(1)
end = time.process_time()
print(f"CPU时间耗时: {end - start} 秒")
```

### re模块
re 模块是Python的正则表达式库，用于字符串模式匹配和文本处理。正则表达式是一种强大的工具，可以通过定义特定的模式来搜索、替换或操作文本。

### re.match(pattern, string, flags=0)

	•	功能：从字符串的起始位置匹配正则表达式。如果起始位置匹配成功，返回匹配对象；否则返回 None。
	•	参数：
	•	pattern：要匹配的正则表达式。
	•	string：要搜索的字符串。
	•	flags：可选标志位，用于控制正则表达式的匹配方式（如 re.IGNORECASE 忽略大小写）。

### re.search(pattern, string, flags=0)
	•	功能：在整个字符串中搜索与正则表达式匹配的第一个位置。返回匹配对象，如果没有匹配则返回 None。

### re.findall(pattern, string, flags=0)
功能：搜索字符串，以列表形式返回所有与正则表达式匹配的子串。
```
results = re.findall(r'\d+', 'abc123xyz456')
print("所有匹配项:", results)  # 输出：['123', '456']
```

### re.finditer(pattern, string, flags=0)
搜索字符串，返回一个迭代器，产生匹配的 MatchObject 对象。

```
for match in re.finditer(r'\d+', 'abc123xyz456'):
    print("匹配到的内容:", match.group())  # 依次输出：123, 456
```

### re.fullmatch(pattern, string, flags=0)
如果整个字符串与正则表达式完全匹配，返回匹配对象；否则返回 None。
```
result = re.fullmatch(r'\d+', '123456')
if result:
    print("完全匹配")  # 输出：完全匹配
```

### re.sub(pattern, repl, string, count=0, flags=0)
替换字符串中所有匹配正则表达式的子串，并返回替换后的字符串。repl 可以是字符串或一个函数。

```
result = re.sub(r'\d+', '#', 'abc123xyz456')
print("替换后的字符串:", result)  # 输出：abc#xyz#
```

### re.subn(pattern, repl, string, count=0, flags=0)
类似 re.sub()，但返回一个元组，包含替换后的字符串和替换的次数。
```
result, num_replacements = re.subn(r'\d+', '#', 'abc123xyz456')
print("替换后的字符串:", result)  # 输出：abc#xyz#
print("替换次数:", num_replacements)  # 输出：2
```

### re.split(pattern, string, maxsplit=0, flags=0)
按照匹配正则表达式的子串分割字符串，返回分割后的列表。maxsplit 参数控制分割次数。

```
result = re.split(r'\d+', 'abc123xyz456')
print("分割后的列表:", result)  # 输出：['abc', 'xyz', '']
```

### 正则表达式模式
	•	. ：匹配除换行符 \n 以外的任意字符。
	•	示例：a.b 可以匹配 aab、acb 等。
	•	^ ：匹配字符串的开头。
	•	示例：^abc 只能匹配以 abc 开头的字符串。
	•	$ ：匹配字符串的结尾。
	•	示例：abc$ 只能匹配以 abc 结尾的字符串。
	•	* ：匹配前一个字符0次或多次。
	•	示例：ab*c 可以匹配 ac、abc、abbc 等。
	•	+ ：匹配前一个字符1次或多次。
	•	示例：ab+c 可以匹配 abc、abbc，但不能匹配 ac。
	•	? ：匹配前一个字符0次或1次。
	•	示例：ab?c 可以匹配 ac、abc。
	•	{m,n} ：匹配前一个字符至少 m 次，至多 n 次。
	•	示例：a{2,4} 可以匹配 aa、aaa、aaaa。
	•	[] ：匹配方括号内的任意字符。
	•	示例：[abc] 可以匹配 a、b、c 中的任何一个。
	•	| ：表示逻辑或操作。
	•	示例：abc|def 可以匹配 abc 或 def。
	•	\ ：用于转义字符。
	•	示例：\. 匹配字符 .。

### 特殊序列
	•	\d ：匹配任意数字，等价于 [0-9]。
	•	示例：\d+ 匹配一个或多个数字。
	•	\D ：匹配任意非数字字符，等价于 [^0-9]。
	•	示例：\D+ 匹配一个或多个非数字字符。
	•	\w ：匹配任意字母数字字符和下划线，等价于 [a-zA-Z0-9_]。
	•	示例：\w+ 匹配一个或多个字母数字字符。
	•	\W ：匹配任意非字母数字字符，等价于 [^a-zA-Z0-9_]。
	•	示例：\W+ 匹配一个或多个非字母数字字符。
	•	\s ：匹配任意空白字符，包括空格、制表符等，等价于 [ \t\n\r\f\v]。
	•	示例：\s+ 匹配一个或多个空白字符。
	•	\S ：匹配任意非空白字符，等价于 [^ \t\n\r\f\v]。
	•	示例：\S+ 匹配一个或多个非空白字符。

### 匹配对象 MatchObject
当使用 re.match()、re.search()、re.finditer() 等函数时，如果匹配成功，返回一个 MatchObject 对象，包含以下常用方法：

#### group([group1, ...])
返回匹配的子组（子表达式），组0代表整个匹配。可以通过编号或名称访问组。
```
match = re.search(r'(\d+)-(\d+)', 'Phone: 123-456')
print(match.group(0))  # 输出：123-456
print(match.group(1))  # 输出：123
print(match.group(2))  # 输出：456
```
#### groups(default=None)
返回所有匹配的子组，作为一个元组。
```
print(match.groups())  # 输出：('123', '456')
```

#### start([group])
返回指定组匹配开始的位置（索引），默认为组0。
```
print(match.start(1))  # 输出第一个子组匹配的起始位置
```

#### end([group])
返回指定组匹配结束的位置（索引），默认为组0。
```
print(match.end(1))  # 输出第一个子组匹配的结束位置
```

#### span([group])
返回指定组匹配的起始和结束位置作为一个元组 (start, end)
```
print(match.span(1))  # 输出第一个子组匹配
```

### 编译正则表达式
#### re.compile(pattern, flags=0)
编译正则表达式，返回一个正则表达式对象，以提高性能。特别适合需要多次使用同一个正则表达式的场景。
```
pattern = re.compile(r'\d+')
result = pattern.search('abc123xyz')
print("匹配到的内容:", result.group())  # 输出：123
```

## 字符串和字节对比
在Python中，字符串（str）和字节（bytes）是两种不同的数据类型，它们主要的区别在于数据的存储方式和用途。下面是它们的详细区别：
### 数据表示
str（字符串）：用于表示文本数据。Python中的字符串是Unicode字符串，支持多种字符集（如UTF-8、UTF-16等），用于表示和处理人类可读的文本。

#### 1. 数据表示
str（字符串）：用于表示文本数据。Python中的字符串是Unicode字符串，支持多种字符集（如UTF-8、UTF-16等），用于表示和处理人类可读的文本。
```
s = "Hello, 世界"
print(type(s))  # 输出：<class 'str'>
```

bytes（字节）：用于表示二进制数据。字节序列是不可变的，通常用于处理原始的二进制数据，如文件内容、网络数据等。
```
b = b"Hello, world"
print(type(b))  # 输出：<class 'bytes'>
```

### 编码和解码
str 到 bytes：要将字符串转换为字节，需要通过编码（encode）操作。编码将文本数据转换为指定的字符集格式。

```
s = "Hello, 世界"
b = s.encode('utf-8')
print(b)  # 输出：b'Hello, \xe4\xb8\x96\xe7\x95\x8c'
```
bytes 到 str：要将字节转换为字符串，需要通过解码（decode）操作。解码将字节数据按照指定的字符集还原为文本。
```
b = b"Hello, \xe4\xb8\x96\xe7\x95\x8c"
s = b.decode('utf-8')
print(s)  # 输出：Hello, 世界
```
### 使用场景
str（字符串）：主要用于处理文本数据，如文件内容、用户输入、显示在界面上的文字等。

bytes（字节）：主要用于处理二进制数据，如网络通信、文件的读写、图片或音频文件的处理等。

## urllib模块
### urllib.request模块
urllib.request 模块用于打开和读取URLs。它支持HTTP、HTTPS、FTP等协议，可以发送简单的HTTP请求并接收响应。
#### urlopen()
打开指定的URL，并返回一个类文件对象，类似于open()函数的作用。可以通过它读取响应的内容。
```
from urllib import request

response = request.urlopen('http://www.example.com')
html = response.read()
print(html.decode('utf-8'))  # 输出网页的HTML内容
```

#### request对象
可以创建一个 Request 对象，包含更详细的HTTP请求信息（如方法、headers、data等）。
```
from urllib import request

url = 'http://www.example.com'
req = request.Request(url, headers={'User-Agent': 'Mozilla/5.0'})
response = request.urlopen(req)
print(response.read().decode('utf-8'))
```

### urlretrieve()
用于直接下载文件并保存到本地。
```
from urllib import request

url = 'http://www.example.com/somefile.zip'
request.urlretrieve(url, 'localfile.zip')
```

### build_opener() 和 install_opener()
自定义请求处理器，设置代理、cookie等。

```
from urllib import request

proxy_handler = request.ProxyHandler({'http': 'http://www.example.com:3128/'})
opener = request.build_opener(proxy_handler)
request.install_opener(opener)
response = request.urlopen('http://www.example.com')
print(response.read().decode('utf-8'))
```
### HTTPError 和 URLError
处理HTTP请求时可能出现的错误。
```
from urllib import request, error

try:
    response = request.urlopen('http://www.example.com/404')
except error.HTTPError as e:
    print('HTTPError:', e.code)
except error.URLError as e:
    print('URLError:', e.reason)
else:
    print('Success:', response.read().decode('utf-8'))
```

## urllib.parse 模块
urllib.parse 模块用于解析URL，并对URL进行拆分、组合、编码、解码等操作。
### urlparse()
将URL拆分成组件，如协议、域名、路径、参数等。
```
from urllib.parse import urlparse

url = 'http://www.example.com/index.html;param?arg=val#anchor'
result = urlparse(url)
print(result)
# 输出：ParseResult(scheme='http', netloc='www.example.com', path='/index.html', params='param', query='arg=val', fragment='anchor')
```


###  urlunparse()
将URL的各部分重新组合成一个完整的URL。
```
from urllib.parse import urlunparse

data = ('http', 'www.example.com', 'index.html', 'param', 'arg=val', 'anchor')
url = urlunparse(data)
print(url)  # 输出：http://www.example.com/index.html;param?arg=val#anchor
```

### urlencode()
将字典或元组形式的数据编码成URL查询参数的格式。
```
from urllib.parse import urlencode

params = {'name': 'value', 'key': 'word'}
query_string = urlencode(params)
print(query_string)  # 输出：name=value&key=word
```


### quote() 和 unquote()
用于URL编码和解码，处理特殊字符。

```
from urllib.parse import quote, unquote

url = 'http://www.example.com/?key=中国'
encoded_url = quote(url)
print(encoded_url)  # 输出：http%3A//www.example.com/%3Fkey%3D%E4%B8%AD%E5%9B%BD

decoded_url = unquote(encoded_url)
print(decoded_url)  # 输出：http://www.example.com/?key=中国
```

### urllib.error 模块
urllib.error 模块包含 URLError 和 HTTPError 异常，主要用于处理 urllib.request 中可能发生的错误。
#### HTTPError
继承自 URLError，表示HTTP请求时的错误，如404 Not Found等。包含属性 code、reason 和 headers。

```
from urllib import request, error

try:
    response = request.urlopen('http://www.example.com/404')
except error.HTTPError as e:
    print('HTTPError:', e.code, e.reason)
```

#### URLError
用于捕获所有与URL处理相关的错误，包含 reason 属性。
```
from urllib import request, error

try:
    response = request.urlopen('http://www.nonexistentwebsite.com')
except error.URLError as e:
    print('URLError:', e.reason)
```

### urllib.robotparser 模块
urllib.robotparser 模块用于解析 robots.txt 文件，判断某个网站的页面是否允许被抓取。

####  RobotFileParser
解析 robots.txt 文件并检查给定的User-agent是否被允许访问某个URL。

```
from urllib.robotparser import RobotFileParser

rp = RobotFileParser()
rp.set_url('http://www.example.com/robots.txt')
rp.read()
can_fetch = rp.can_fetch('*', 'http://www.example.com/somepage')
print('可以抓取:', can_fetch)
```

## Json模块
Python 的 json 模块用于在 Python 和 JSON（JavaScript Object Notation）数据格式之间进行转换。JSON 是一种轻量级的数据交换格式，广泛用于客户端和服务器之间的数据传输。Python 的 json 模块提供了简单易用的函数，可以将 Python 数据结构转换为 JSON 格式，以及将 JSON 数据解析为 Python 数据结构。

### json.dumps()
将 Python 对象转换为 JSON 字符串。

常用参数：

	•	indent: 指定缩进级别，使输出的 JSON 更易读。
	•	separators: 设置项与项之间的分隔符。
	•	ensure_ascii: 如果为 True（默认值），所有非 ASCII 字符将被转义；如果为 False，输出原始字符。

```
import json

data = {'name': 'Alice', 'age': 25, 'city': 'New York'}
json_string = json.dumps(data, indent=4)
print(json_string)
```

###  json.dump()
功能：将 Python 对象序列化为 JSON 格式，并将其写入文件。
常用参数：
    与 json.dumps() 相同，如 indent 和 ensure_ascii。

###  json.loads()
将 JSON 字符串解析为 Python 对象。

```
import json

json_string = '{"name": "Charlie", "age": 28, "city": "Chicago"}'
data = json.loads(json_string)
print(data)
```

### json.load()
从文件中读取 JSON 数据，并将其解析为 Python 对象。
```
import json

with open('data.json', 'r') as f:
    data = json.load(f)
print(data)
```

##  functiools 模块
functools 是 Python 标准库中的一个模块，提供了一些用于高阶函数（即操作或返回其他函数的函数）和可调用对象的工具。这个模块非常有用，特别是在需要进行函数式编程或优化代码时。

###  functools.partial
partial 函数用于创建一个新的函数，这个新函数是原函数的一个“部分应用”，即你可以预先为原函数的一些参数赋值，从而简化函数调用。

```
from functools import partial

def multiply(x, y):
    return x * y

# 创建一个新的函数，它总是将第一个参数设为 2
double = partial(multiply, 2)

# 现在只需要传递一个参数
result = double(5)
print(result)  # 输出: 10
```
###  functools.lru_cache
lru_cache 是一个装饰器，用于缓存函数的返回值，从而加快函数的执行速度，特别是对于需要多次计算的函数。LRU 是 “Least Recently Used” 的缩写，即缓存会自动丢弃最久未使用的结果。
```
from functools import lru_cache

@lru_cache(maxsize=32)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))  # 输出: 55
```
在这个示例中，fibonacci 函数会缓存最近 32 个计算结果，从而避免重复计算。

###  functools.reduce
reduce 函数用于对序列中的元素进行累计操作。它从序列的第一个元素开始，依次将前一个累计结果和下一个元素传递给函数，最终将整个序列简化为一个值。

```
from functools import reduce

numbers = [1, 2, 3, 4, 5]
result = reduce(lambda x, y: x + y, numbers)
print(result)  # 输出: 15
```

### functools.wraps
wraps 是一个装饰器，用于保持原函数的元数据（如文档字符串、函数名等），当你用装饰器包装一个函数时，这个函数的元数据通常会丢失。使用 functools.wraps 可以避免这种情况。

```
from functools import wraps

def my_decorator(f):
    @wraps(f)
    def wrapped(*args, **kwargs):
        print("Before calling", f.__name__)
        result = f(*args, **kwargs)
        print("After calling", f.__name__)
        return result
    return wrapped

@my_decorator
def say_hello():
    """This function says hello."""
    print("Hello!")

say_hello()

# 查看函数的元数据
print(say_hello.__name__)  # 输出: say_hello
print(say_hello.__doc__)   # 输出: This function says hello.
```

### functools.total_ordering
total_ordering 是一个类装饰器，用于简化实现比较操作符。你只需要定义一个或两个基本比较方法（如 __lt__ 和 __eq__），然后 total_ordering 装饰器会自动为你生成其他的比较方法。


```
from functools import total_ordering

@total_ordering
class Number:
    def __init__(self, value):
        self.value = value

    def __eq__(self, other):
        return self.value == other.value

    def __lt__(self, other):
        return self.value < other.value

# 现在 Number 类自动支持 <, <=, >, >= 比较
n1 = Number(5)
n2 = Number(10)

print(n1 < n2)  # 输出: True
print(n1 <= n2)  # 输出: True
print(n1 > n2)  # 输出: False
```

### functools.singledispatch
singledispatch 是一个通用函数装饰器，用于实现基于参数类型的单分派（Single Dispatch）函数。通过 @singledispatch，你可以根据第一个参数的类型来选择调用不同的函数实现。

```
from functools import singledispatch

@singledispatch
def process(data):
    raise NotImplementedError("Cannot process this type")

@process.register(int)
def _(data):
    print(f"Processing an integer: {data}")

@process.register(str)
def _(data):
    print(f"Processing a string: {data}")

process(10)   # 输出: Processing an integer: 10
process("hi") # 输出: Processing a string: hi
```
## map 和 reduce函数
在 Python 中，map 和 reduce 是两个常用的函数式编程工具。它们允许你以简洁的方式对序列进行操作，特别是在处理列表、元组等可迭代对象时非常有用。

### map() 函数
map() 函数用于将一个函数应用到一个或多个可迭代对象的所有元素上，并返回一个包含所有结果的迭代器。

####  map(function, iterable, ...)

	•	function: 要应用的函数。
	•	iterable: 一个或多个可迭代对象。

```
numbers = [1, 2, 3, 4, 5]
squared_numbers = map(lambda x: x ** 2, numbers)
print(list(squared_numbers))  # 输出: [1, 4, 9, 16, 25]
```
map() 也可以同时处理多个可迭代对象，前提是这些对象的长度相同：
```
numbers1 = [1, 2, 3]
numbers2 = [4, 5, 6]

sum_numbers = map(lambda x, y: x + y, numbers1, numbers2)
print(list(sum_numbers))  # 输出: [5, 7, 9]
```

### reduce()函数
reduce() 函数用于对一个可迭代对象的元素进行累计操作，将其简化为单个值。reduce() 函数位于 functools 模块中，因此使用时需要先导入该模块。

```
from functools import reduce

reduce(function, iterable[, initializer])

	•	function: 一个接受两个参数的函数，用于累计计算。
	•	iterable: 一个可迭代对象。
	•	initializer（可选）: 一个初始值，如果提供，这个值会放在计算的最前面。

```

```
from functools import reduce

numbers = [1, 2, 3, 4, 5]

result = reduce(lambda x, y: x + y, numbers, 10)
print(result)  # 输出: 25
```

## 面试必备：Python内存管理机制
[面试必备：Python内存管理机制](https://juejin.cn/post/6856235545220415496)

## 并发编程
### 线程
_thread 和 threading 是 Python 中两个用于实现多线程编程的模块。它们都可以用于创建和管理线程，但它们的使用方式和提供的功能有所不同。
#### _thread模块
	•	基础和低级别：_thread 是一个较为底层的线程模块，它提供了基本的线程功能，如创建线程、获取线程 ID、加锁和释放锁等。

	•	简单但不够灵活：由于它是一个低级别的模块，使用起来相对简单，但也缺乏一些高级功能，如线程的生命周期管理、守护线程、条件变量等。

	•	不推荐直接使用：由于 threading 模块提供了更高层次的 API，且更易于使用和管理线程，官方更推荐使用 threading 模
	块。
```
import _thread
import time


def task(name):
    print(f"线程：{name}开始执行")
    time.sleep(2)
    print(f"线程：{name}结束执行")


# 创建两个线程
_thread.start_new_thread(task, ("线程1",))
_thread.start_new_thread(task, ("线程2",))

# 主线程暂停，以便子线程完成
time.sleep(3)
print("主线程执行完毕")

```

#### threading模块
	•	高级和功能丰富：threading 是基于 _thread 构建的更高级别的模块，提供了更多的功能和更好的线程管理。它可以方便地创建和管理线程，还支持守护线程、线程同步、条件变量等高级功能。

	•	线程对象：threading 提供了 Thread 类，允许你通过面向对象的方式来管理线程。这使得代码更具可读性和可维护性。

	•	推荐使用：threading 模块功能更强大且易用，因此在大多数情况下都推荐使用它来进行多线程编程。

```
import threading
import time

def task(name):
    print(f'线程 {name} 开始')
    time.sleep(2)
    print(f'线程 {name} 结束')

# 创建线程对象
t1 = threading.Thread(target=task, args=("Thread-1",))
t2 = threading.Thread(target=task, args=("Thread-2",))

# 启动线程
t1.start()
t2.start()

# 等待线程完成
t1.join()
t2.join()

print("主线程结束")
```

#### Thread类
```
import threading


def task(name):
    print(f"线程-{name} 执行任务")


# 创建线程
my_thread = threading.Thread(target=task, args=("张三",))

# 启动线程
my_thread.start()

# 等待线程执行完毕
my_thread.join()

```

继承Thread类

```
import threading


class MyThread(threading.Thread):

    def __init__(self, name):
        super().__init__()
        self.name = name

    def run(self):
        print(f"线程-{self.name}执行任务")


# 创建线程
t1 = MyThread("t1")

# 启动线程
t1.start()

# 等待线程执行完毕
t1.join()

print("线程执行完毕")

```

#### Thread类的常用方法
	•	start()：启动线程，调用线程的 run() 方法。

	•	run()：定义线程执行的任务逻辑。通常不需要直接调用，而是通过 start() 方法自动调用。

	•	join([timeout])：阻塞主线程，等待子线程完成。可选的 timeout 参数指定最长等待时间。

	•	is_alive()：返回线程是否还在运行。

	•	name：线程的名称，可以自定义名称，便于调试。

	•	daemon：设定为 True 时，线程在主线程结束时自动终止。默认值为 False。

#### 守护线程（Daemon Thread）
守护线程在主线程结束时自动退出。通常用于在后台运行的任务。

```
import threading
import time

def daemon_task():
    while True:
        print("守护线程运行中")
        time.sleep(1)

# 创建守护线程
thread = threading.Thread(target=daemon_task)
thread.daemon = True

thread.start()

time.sleep(3)
print("主线程结束")
```

#### 线程同步

##### Lock对象
Lock 是线程同步的基本工具，用于确保同一时间只有一个线程访问共享资源。

```
import threading

lock = threading.Lock()
counter = 0


def increment():
    global counter  # 共享资源
    with lock:  # 加锁
        counter += 1


threads = []
for _ in range(100):
    t = threading.Thread(target=increment)
    threads.append(t)
    t.start()

for t in threads:
    t.join()

print("计算器值：", counter)

```
##### RLock对象
RLock 是可重入锁，与 Lock 类似，但同一线程可以多次获取它而不会引起死锁。

```
import threading

rlock = threading.RLock()

def task():
    with rlock:
        print("第一次获取锁")
        with rlock:
            print("第二次获取锁")

thread = threading.Thread(target=task)
thread.start()
thread.join()
```

#####  Condition 对象
Condition 用于线程间的复杂同步。它允许一个线程等待，直到另一个线程发出某个条件已经满足的信号。

```
import threading

condition = threading.Condition()  # 创建条件变量


def consumer():
    with condition:
        condition.wait()  # 等待条件变量
        print("消费者消费了")


def producer():
    with condition:
        print("生产者生产了")
        condition.notify()  # 通知所有等待的线程


thread1 = threading.Thread(target=consumer)
thread2 = threading.Thread(target=producer)

thread1.start()
thread2.start()

thread1.join()
thread2.join()

```

##### Event 对象
Event 对象用于线程间简单的信号通信。线程可以等待事件的触发，或在触发事件后继续运行。
```
import threading

event = threading.Event()

def task():
    print("等待事件触发...")
    event.wait()
    print("事件已触发，继续运行")

thread = threading.Thread(target=task)
thread.start()

input("按回车键触发事件\n")
event.set()
thread.join()
```

#####  Semaphore 对象
Semaphore 限制同时访问某资源的线程数量，适用于控制对某些资源的并发访问。

```
import threading
import time

semaphore = threading.Semaphore(2)

def task():
    with semaphore:
        print(f"{threading.current_thread().name} 获取信号量")
        time.sleep(2)
        print(f"{threading.current_thread().name} 释放信号量")

threads = [threading.Thread(target=task) for _ in range(4)]

for thread in threads:
    thread.start()

for thread in threads:
    thread.join()
```

##### 线程池
虽然 threading 模块本身没有线程池，但可以使用 concurrent.futures 模块的 ThreadPoolExecutor 来创建线程池，简化多线程任务的管理。

```
from concurrent.futures import ThreadPoolExecutor

def task(n):
    return n * 2

with ThreadPoolExecutor(max_workers=4) as executor:
    results = executor.map(task, range(10))

print(list(results))
```
	•	max_workers 参数指定线程池或进程池中的最大工作线程或进程数。

	•	submit(fn, *args, **kwargs) 方法用于提交一个任务给池，并返回一个 Future 对象。

## concurrent模块
concurrent 模块是 Python 标准库的一部分，用于并发编程。它提供了高层次的接口，帮助开发者轻松管理线程、进程以及异步任务。concurrent 包含两个主要子模块：

	•	concurrent.futures：提供线程池和进程池接口，用于异步执行任务。

	•	concurrent.futures.process：包含与进程池相关的实现，通常不直接使用，而是通过 concurrent.futures 进行访问。

### concurrent.futures 模块概述
concurrent.futures 模块提供了一个高级接口，允许你使用线程池和进程池来并发地执行任务。它支持两种执行器：

	•	ThreadPoolExecutor：用于管理线程池，适合 I/O 密集型任务。

	•	ProcessPoolExecutor：用于管理进程池，适合 CPU 密集型任务。

#####  创建线程池和进程池
```
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

# 创建线程池
with ThreadPoolExecutor(max_workers=4) as executor:
    # 执行任务
    future = executor.submit(pow, 2, 3)
    print(future.result())  # 输出 8

# 创建进程池
with ProcessPoolExecutor(max_workers=4) as executor:
    # 执行任务
    future = executor.submit(pow, 2, 3)
    print(future.result())  # 输出 8
```

##### 使用 map 方法
map 方法允许你将一个函数应用到多个输入上，返回结果的迭代器。
```
from concurrent.futures import ThreadPoolExecutor

def task(n):
    return n * n

with ThreadPoolExecutor(max_workers=4) as executor:
    results = executor.map(task, range(10))

print(list(results))
```

##### Future 对象
Future 对象表示一个异步执行的操作。你可以使用 Future 对象来获取任务的结果、检查任务的状态或取消任务。

	•	result()：阻塞等待任务完成并返回结果。
	•	done()：检查任务是否完成。
	•	cancel()：尝试取消任务（如果任务还未开始）。
	•	exception()：获取任务执行期间引发的异常。

```
from concurrent.futures import ThreadPoolExecutor

def task(n):
    return n * n

with ThreadPoolExecutor(max_workers=2) as executor:
    future = executor.submit(task, 5)
    print(future.result())  # 输出 25
```

##### 使用 as_completed 函数
as_completed 函数返回一个迭代器，当每个任务完成时，可以立即获取结果。

```
from concurrent.futures import ThreadPoolExecutor, as_completed

def task(n):
    return n * n

with ThreadPoolExecutor(max_workers=4) as executor:
    futures = [executor.submit(task, i) for i in range(10)]
    
    for future in as_completed(futures):
        print(future.result())
```

##### 使用 ProcessPoolExecutor
ProcessPoolExecutor 类似于 ThreadPoolExecutor，但它使用独立的进程来执行任务，适合处理 CPU 密集型任务。

```
from concurrent.futures import ProcessPoolExecutor

def task(n):
    return n * n

with ProcessPoolExecutor(max_workers=4) as executor:
    results = executor.map(task, range(10))

print(list(results))
```

##### 取消任务
任务提交后，如果任务还未开始执行，可以通过 Future 对象的 cancel() 方法取消任务。
```
from concurrent.futures import ThreadPoolExecutor
import time

def task(n):
    time.sleep(2)
    return n * n

with ThreadPoolExecutor(max_workers=2) as executor:
    future = executor.submit(task, 5)
    if future.cancel():
        print("任务取消成功")
    else:
        print("任务已经开始，无法取消")
```

##### 处理异常
concurrent.futures 模块允许你在任务中处理异常。可以通过 Future 对象的 exception() 方法来获取异常信息。
```
from concurrent.futures import ThreadPoolExecutor

def task(n):
    if n == 0:
        raise ValueError("除零错误")
    return 10 / n

with ThreadPoolExecutor(max_workers=2) as executor:
    futures = [executor.submit(task, n) for n in range(3)]
    
    for future in futures:
        if future.exception():
            print(f"任务引发异常: {future.exception()}")
        else:
            print(f"任务结果: {future.result()}")
```


##### Executor.shutdown() 方法
shutdown() 方法用于清理资源。当使用 with 语句时，shutdown() 方法会自动调用。你可以选择 wait=False 来立即返回而不等待池中的所有任务完成。

```
from concurrent.futures import ThreadPoolExecutor

def task(n):
    return n * n

executor = ThreadPoolExecutor(max_workers=4)
futures = [executor.submit(task, i) for i in range(10)]

executor.shutdown(wait=False)

for future in futures:
    print(future.result())
```

## 进程（Process）模块
在 Python 中，进程（Process）是计算机程序运行时的一个独立实例。每个进程都有自己独立的内存空间、系统资源和执行状态。Python 提供了多种方式来创建和管理进程，最常用的是通过 multiprocessing 模块。

### Process 类
Process 类用于创建一个独立的进程，可以通过以下几种方式使用：
```
from multiprocessing import Process

def worker(name):
    print(f'Worker {name} is running')

if __name__ == '__main__':
    p = Process(target=worker, args=('A',))
    p.start()  # 启动进程
    p.join()   # 等待进程结束
```
	•	target：指定进程要执行的函数。
	•	args：传递给目标函数的参数。

	
### 进程的生命周期：
	•	start()：启动进程。
	•	join()：阻塞主进程，直到子进程完成。
	•	is_alive()：检查进程是否还在运行。

### Pool 类
Pool 类用于创建一个进程池，可以控制同时执行的进程数量。它提供了多个方法来提交任务：

map()：将函数应用于输入列表的每一个元素，并返回结果列表。

```
from multiprocessing import Pool

def square(x):
    return x * x

if __name__ == '__main__':
    with Pool(4) as p:
        result = p.map(square, [1, 2, 3, 4])
    print(result)
```

apply()：将函数应用于给定的参数，并返回结果。它是同步调用，等效于普通的函数调用。
```
result = p.apply(square, (10,))
```

apply_async()：异步版本的 apply()，不会阻塞主进程，结果通过回调函数获取。

```
res = p.apply_async(square, (10,))
print(res.get())  # 获取结果
```

starmap()：类似 map()，但允许传递多个参数。

```
def multiply(x, y):
    return x * y

result = p.starmap(multiply, [(1, 2), (3, 4)])
```

### 进程间通信

#### Queue：线程和进程安全的队列，用于进程间的数据传输。

```
from multiprocessing import Process, Queue

def worker(q):
    q.put('Hello from worker')

if __name__ == '__main__':
    q = Queue()
    p = Process(target=worker, args=(q,))
    p.start()
    print(q.get())  # 获取队列中的数据
    p.join()
```

#### Pipe：创建一个双向通道，允许两个进程之间进行直接通信。

```
from multiprocessing import Process, Pipe

def worker(conn):
    conn.send('Hello from worker')
    conn.close()

if __name__ == '__main__':
    parent_conn, child_conn = Pipe()
    p = Process(target=worker, args=(child_conn,))
    p.start()
    print(parent_conn.recv())  # 从管道中接收数据
    p.join()
```

#### 同步原语
Lock：用于防止多个进程同时访问共享资源，确保数据一致性。
```
from multiprocessing import Process, Lock

def worker(lock, num):
    with lock:
        print(f'Lock acquired by {num}')

if __name__ == '__main__':
    lock = Lock()
    for i in range(5):
        p = Process(target=worker, args=(lock, i))
        p.start()
        p.join()
```

	•	Semaphore：用于控制同时访问特定资源的进程数量。
	•	Event：用于在多个进程间共享状态，类似于线程的 Event 对象。

#### 数据共享与同步
multiprocessing 提供了 Value 和 Array 这两个类用于在进程之间共享数据：

Value：用于共享单个数据值，可以是整数、浮点数、字符等。

```
from multiprocessing import Process, Value

def worker(v):
    v.value += 1

if __name__ == '__main__':
    v = Value('i', 0)  # 'i' 表示整数
    processes = [Process(target=worker, args=(v,)) for _ in range(5)]

    for p in processes:
        p.start()

    for p in processes:
        p.join()

    print(v.value)  # 输出 5
```

Array：用于共享数组

```
from multiprocessing import Process, Array

def worker(arr):
    for i in range(len(arr)):
        arr[i] = arr[i] * 2

if __name__ == '__main__':
    arr = Array('i', [1, 2, 3, 4])  # 'i' 表示整数数组
    p = Process(target=worker, args=(arr,))
    p.start()
    p.join()
    print(arr[:])  # 输出 [2, 4, 6, 8]
```

#### Manager 对象
Manager 对象：提供了更高层次的共享数据管理工具，可以用于管理字典、列表、命名空间等复杂数据结构。

```
from multiprocessing import Manager, Process

def worker(d, key, value):
    d[key] = value

if __name__ == '__main__':
    with Manager() as manager:
        d = manager.dict()
        p1 = Process(target=worker, args=(d, 'key1', 1))
        p2 = Process(target=worker, args=(d, 'key2', 2))
        p1.start()
        p2.start()
        p1.join()
        p2.join()
        print(d)  # 输出 {'key1': 1, 'key2': 2}
```

## 异步编程

### asyncio 模块
asyncio 是 Python 标准库中的异步编程模块，它主要用于编写并发的 I/O 密集型代码。asyncio 的核心概念包括事件循环、协程、任务、同步原语等。以下是一个详细的 asyncio 使用教程，帮助你逐步理解和掌握这个强大的工具。


#### 入门示例
```
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)
    print("World")

asyncio.run(say_hello())
```
	•	async def：定义一个协程函数。
	•	await：暂停协程的执行，等待另一个协程完成。
	•	asyncio.run()：启动事件循环，并运行指定的协程。


#### 事件循环
事件循环是 asyncio 的核心。它负责调度和执行协程任务，管理所有异步操作的执行顺序。

```
import asyncio

async def main():
    print("Starting main")
    await asyncio.sleep(2)
    print("Main is done")

# 获取默认的事件循环
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```
在 asyncio.run() 之前，常见的启动事件循环方式是 get_event_loop() 和 run_until_complete()。


#### 创建和管理任务
任务 (Task) 是对协程的进一步封装，用于并发地运行多个协程。asyncio 允许你创建和管理任务，以便在事件循环中并发地执行它们。

```
import asyncio

async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main():
    task1 = asyncio.create_task(say_after(1, "Hello"))
    task2 = asyncio.create_task(say_after(2, "World"))

    print("Tasks created")
    await task1
    await task2
    print("Tasks finished")

asyncio.run(main())
```
	•	asyncio.create_task()：创建一个任务对象，它将协程调度到事件循环中执行。

	•	await：等待任务完成。

#### 并发运行多个任务

asyncio 提供了多种方法来并发运行多个协程或任务，常用的是 asyncio.gather() 和 asyncio.wait()。

asyncio.gather() 将多个协程并发地运行，并返回一个包含所有结果的列表。

```
import asyncio

async def say_after(delay, what):
    await asyncio.sleep(delay)
    return what

async def main():
    result = await asyncio.gather(
        say_after(1, "Hello"),
        say_after(2, "World")
    )
    print(result)

asyncio.run(main())
```

asyncio.wait() 提供了更多控制选项，允许你选择在所有任务完成后或任意一个任务完成后继续。

```
import asyncio

async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main():
    task1 = asyncio.create_task(say_after(1, "Hello"))
    task2 = asyncio.create_task(say_after(2, "World"))

    done, pending = await asyncio.wait([task1, task2])
    for task in done:
        print(f"Task completed: {task.result()}")

asyncio.run(main())
```

	•	done：包含已完成的任务或协程。
	•	pending：包含尚未完成的任务或协程。


#### 异步IO
asyncio 允许你异步执行 I/O 操作，例如网络请求、文件读写等。

使用 aiohttp 库进行异步 HTTP 请求。

```
import aiohttp
import asyncio

async def fetch(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def main():
    html = await fetch('http://example.com')
    print(html)

asyncio.run(main())
```

asyncio 提供了超时控制的功能，可以为某个任务设置超时限制。

```
import asyncio

async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main():
    try:
        await asyncio.wait_for(say_after(3, "Hello"), timeout=2)
    except asyncio.TimeoutError:
        print("Timeout!")

asyncio.run(main())
```

#### 同步原语
asyncio 提供了多种同步原语来控制协程之间的执行顺序，如 Lock、Event、Semaphore 等。

##### Lock
```
import asyncio

async def safe_print(lock, text):
    async with lock:
        print(text)
        await asyncio.sleep(1)

async def main():
    lock = asyncio.Lock()
    await asyncio.gather(
        safe_print(lock, "Task 1"),
        safe_print(lock, "Task 2")
    )

asyncio.run(main())
```

##### 使用 Semaphore
Semaphore 限制并发协程的数量。

```
import asyncio

async def limited_task(sem, i):
    async with sem:
        print(f"Task {i} is running")
        await asyncio.sleep(1)

async def main():
    sem = asyncio.Semaphore(2)  # 限制同时运行的任务数量
    tasks = [limited_task(sem, i) for i in range(5)]
    await asyncio.gather(*tasks)

asyncio.run(main())
```

##### 异步生成器
asyncio 支持异步生成器，用于生成一系列异步值。

```
import asyncio

async def async_gen():
    for i in range(5):
        await asyncio.sleep(1)
        yield i

async def main():
    async for value in async_gen():
        print(value)

asyncio.run(main())
```

##### 取消任务
任务可以被取消，常用于处理超时或错误情况。

```
import asyncio

async def long_running_task():
    try:
        print("Task started")
        await asyncio.sleep(10)
    except asyncio.CancelledError:
        print("Task was cancelled")
        raise

async def main():
    task = asyncio.create_task(long_running_task())
    await asyncio.sleep(1)
    task.cancel()  # 取消任务
    try:
        await task
    except asyncio.CancelledError:
        print("Task cancellation handled")

asyncio.run(main())
```



