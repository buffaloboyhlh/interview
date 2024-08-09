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
