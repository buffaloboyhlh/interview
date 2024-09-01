# asyncio 模块

Python 3.10 引入了一些新功能和改进，使 `asyncio` 模块更加强大和易用。以下是对 Python 3.10 中 `asyncio` 的详细解释。

### 1. `asyncio` 的基本概念

#### 1.1 协程（Coroutines）
协程是异步编程的核心。在 Python 中，协程是使用 `async def` 声明的函数，可以使用 `await` 关键字来暂停其执行，直到某个异步操作完成。

```python
import asyncio

async def my_coroutine():
    print("Start")
    await asyncio.sleep(1)  # 模拟 I/O 操作
    print("End")
```

#### 1.2 事件循环（Event Loop）
事件循环是异步程序的核心，用于调度并执行协程。事件循环不断检查是否有可以执行的任务（如完成的 I/O 操作），并运行这些任务。

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)
    print("World")

# 运行事件循环
asyncio.run(say_hello())
```

### 2. Python 3.10 中 `asyncio` 的新功能

#### 2.1 异步上下文管理器的增强

Python 3.10 引入了对 `asyncio.run()` 的改进，使其支持在没有事件循环的情况下在嵌套环境中运行。

```python
import asyncio

async def my_coroutine():
    print("Running coroutine")

async def main():
    await asyncio.run(my_coroutine())

asyncio.run(main())
```

#### 2.2 更简单的任务取消机制

在 Python 3.10 中，任务取消得到了增强，提供了更加直观的控制。

```python
import asyncio

async def my_task():
    try:
        await asyncio.sleep(5)
    except asyncio.CancelledError:
        print("Task was cancelled")

async def main():
    task = asyncio.create_task(my_task())
    await asyncio.sleep(1)
    task.cancel()
    await task  # 捕获取消异常

asyncio.run(main())
```

### 3. 高级功能

#### 3.1 `asyncio.TaskGroup`
Python 3.10 引入了 `TaskGroup`，它提供了一种管理多个并发任务的简便方式，类似于结构化并发。这使得同时管理和取消多个任务更加方便和直观。

```python
import asyncio

async def worker(name, delay):
    await asyncio.sleep(delay)
    print(f"{name} finished")

async def main():
    async with asyncio.TaskGroup() as tg:
        tg.create_task(worker("Task 1", 2))
        tg.create_task(worker("Task 2", 1))
        tg.create_task(worker("Task 3", 3))

asyncio.run(main())
```

`TaskGroup` 确保在上下文管理器退出时所有任务都已完成或被取消。

#### 3.2 `asyncio.Stream` API 的改进

Python 3.10 对 `asyncio.Stream` API 进行了改进，使流式处理更加简洁。`Stream` API 用于处理异步 I/O 流，如套接字通信。

```python
import asyncio

async def handle_echo(reader, writer):
    data = await reader.read(100)
    message = data.decode()
    writer.write(data)
    await writer.drain()
    writer.close()

async def main():
    server = await asyncio.start_server(handle_echo, '127.0.0.1', 8888)
    async with server:
        await server.serve_forever()

asyncio.run(main())
```

#### 3.3 `asyncio.to_thread()`

Python 3.9 引入的 `asyncio.to_thread()` 在 3.10 中得到了进一步优化，用于在异步代码中轻松调用阻塞的同步函数。

```python
import asyncio
import time

def blocking_io():
    time.sleep(2)
    return "Blocking I/O finished"

async def main():
    result = await asyncio.to_thread(blocking_io)
    print(result)

asyncio.run(main())
```

### 4. 新增的异常分组

Python 3.10 引入了 `ExceptionGroup` 和 `BaseExceptionGroup`，允许在协程中同时抛出和处理多个异常。`asyncio.gather()` 可以捕获多个任务中的异常并统一处理。

```python
import asyncio

async def raise_exception():
    raise ValueError("An error occurred")

async def main():
    try:
        await asyncio.gather(raise_exception(), raise_exception())
    except Exception as e:
        print(f"Caught exception: {e}")

asyncio.run(main())
```

### 5. 兼容性和性能改进

Python 3.10 对 `asyncio` 模块的性能进行了多项优化，特别是在 I/O 密集型任务和事件循环的执行效率上。此外，`asyncio` 的 API 也更加一致和直观，减少了错误使用的可能性。

### 6. 小结

Python 3.10 中 `asyncio` 模块的增强功能使得编写异步代码更加方便和强大。`TaskGroup` 提供了结构化并发的支持，改进的异常处理和任务管理功能使异步编程更加直观。通过 `asyncio.run()`、`asyncio.to_thread()` 和改进的流 API，可以轻松处理复杂的异步操作。Python 3.10 的这些改进使得 `asyncio` 更加适合现代异步编程需求。