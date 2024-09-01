# pandas 面试

以下是一些常见的 Pandas 面试题及其详细解析，这些问题涵盖了 Pandas 的基本操作、数据处理技巧以及高级功能，帮助你在面试中脱颖而出。

### 1. **Pandas 是什么？它的主要数据结构是什么？**
   - **回答要点**：
     - Pandas 是一个用于数据操作和分析的 Python 库，广泛用于数据清洗、转换、分析、可视化等任务。
     - 主要数据结构是 `Series` 和 `DataFrame`。
       - `Series`：类似于一维数组的对象，具有索引标签。
       - `DataFrame`：类似于二维表格的对象，由多个 `Series` 组成，每个 `Series` 代表一列数据。

### 2. **如何创建一个 DataFrame？**
   - **回答要点**：
     - 使用字典、列表、NumPy 数组等数据结构创建 DataFrame。
     - 也可以从文件（如 CSV、Excel）中读取数据，使用 `pd.read_csv()`、`pd.read_excel()` 等方法创建 DataFrame。

   - **示例代码**：
     ```python
     import pandas as pd

     data = {
         'Name': ['Alice', 'Bob', 'Charlie'],
         'Age': [25, 30, 35],
         'City': ['New York', 'Los Angeles', 'Chicago']
     }
     df = pd.DataFrame(data)
     print(df)
     ```

### 3. **如何在 Pandas 中选择和过滤数据？**
   - **回答要点**：
     - 使用 `loc[]` 和 `iloc[]` 选择行或列。
       - `loc[]` 根据标签选择数据。
       - `iloc[]` 根据位置选择数据。
     - 使用布尔索引和条件语句进行数据过滤。

   - **示例代码**：
     ```python
     df = pd.DataFrame({
         'Name': ['Alice', 'Bob', 'Charlie'],
         'Age': [25, 30, 35],
         'City': ['New York', 'Los Angeles', 'Chicago']
     })
     
     # 使用 loc 按标签选择数据
     print(df.loc[df['Age'] > 25])
     
     # 使用 iloc 按位置选择数据
     print(df.iloc[0:2, 1:3])
     ```

### 4. **如何处理缺失值？**
   - **回答要点**：
     - 使用 `isna()` 或 `isnull()` 检查缺失值。
     - 使用 `dropna()` 删除包含缺失值的行或列。
     - 使用 `fillna()` 填充缺失值，可以指定常数或使用方法如前向填充 `ffill` 或后向填充 `bfill`。

   - **示例代码**：
     ```python
     df = pd.DataFrame({
         'Name': ['Alice', 'Bob', None],
         'Age': [25, None, 35],
         'City': ['New York', 'Los Angeles', 'Chicago']
     })
     
     # 检查缺失值
     print(df.isna())
     
     # 删除包含缺失值的行
     df_cleaned = df.dropna()
     
     # 填充缺失值
     df_filled = df.fillna('Unknown')
     ```

### 5. **如何合并 DataFrame？**
   - **回答要点**：
     - 使用 `concat()` 沿着指定轴拼接 DataFrame。
     - 使用 `merge()` 进行数据库风格的连接（如 `INNER JOIN`、`LEFT JOIN` 等）。
     - 使用 `join()` 方法基于索引合并 DataFrame。

   - **示例代码**：
     ```python
     df1 = pd.DataFrame({'key': ['A', 'B', 'C'], 'value': [1, 2, 3]})
     df2 = pd.DataFrame({'key': ['A', 'B', 'D'], 'value': [4, 5, 6]})
     
     # 内连接
     df_inner = pd.merge(df1, df2, on='key', how='inner')
     
     # 外连接
     df_outer = pd.merge(df1, df2, on='key', how='outer')
     
     # 按行拼接
     df_concat = pd.concat([df1, df2], axis=0)
     ```

### 6. **如何对 DataFrame 进行分组操作？**
   - **回答要点**：
     - 使用 `groupby()` 按列对数据进行分组。
     - `groupby()` 常用于聚合操作，如 `sum()`、`mean()`、`count()` 等。
     - `groupby()` 可以与多种聚合函数链式使用，生成分组后的汇总数据。

   - **示例代码**：
     ```python
     df = pd.DataFrame({
         'Category': ['A', 'A', 'B', 'B'],
         'Value': [10, 15, 10, 25]
     })
     
     grouped = df.groupby('Category')
     
     # 求和
     print(grouped.sum())
     
     # 求均值
     print(grouped.mean())
     ```

### 7. **如何使用 Pandas 处理时间序列数据？**
   - **回答要点**：
     - 使用 `pd.to_datetime()` 将字符串或其他格式转换为时间戳。
     - 设置时间戳为索引，使用 `set_index()` 方法。
     - 支持时间序列的重采样和移动窗口操作，如 `resample()`、`rolling()`。

   - **示例代码**：
     ```python
     date_rng = pd.date_range(start='2022-01-01', end='2022-01-10', freq='D')
     df = pd.DataFrame(date_rng, columns=['date'])
     df['data'] = np.random.randint(0, 100, size=(len(date_rng)))
     
     # 转换为时间序列
     df['date'] = pd.to_datetime(df['date'])
     df.set_index('date', inplace=True)
     
     # 重采样
     df_resampled = df.resample('W').mean()
     ```

### 8. **如何对 DataFrame 进行数据透视表操作？**
   - **回答要点**：
     - 使用 `pivot_table()` 创建数据透视表。
     - `pivot_table()` 支持多种聚合函数，可以基于多个维度进行数据汇总。
     - 通过 `margins=True` 参数添加总计行/列。

   - **示例代码**：
     ```python
     df = pd.DataFrame({
         'Category': ['A', 'A', 'B', 'B'],
         'Sub-Category': ['X', 'Y', 'X', 'Y'],
         'Sales': [100, 150, 200, 250]
     })
     
     pivot_table = df.pivot_table(values='Sales', index='Category', columns='Sub-Category', aggfunc='sum', margins=True)
     print(pivot_table)
     ```

### 9. **Pandas 中的 apply() 函数如何使用？**
   - **回答要点**：
     - `apply()` 函数用于对 DataFrame 或 Series 的每个元素、行或列应用函数。
     - 可以传递自定义函数或 lambda 表达式，应用范围包括单列、多列或整个 DataFrame。
     - `applymap()` 用于对 DataFrame 中的每个元素应用函数。

   - **示例代码**：
     ```python
     df = pd.DataFrame({
         'A': [1, 2, 3],
         'B': [10, 20, 30]
     })
     
     # 对单列应用函数
     df['C'] = df['A'].apply(lambda x: x ** 2)
     
     # 对行应用函数
     df['D'] = df.apply(lambda row: row['A'] + row['B'], axis=1)
     ```

### 10. **如何优化 Pandas 代码的性能？**
   - **回答要点**：
     - 避免循环操作，尽量使用向量化操作。
     - 使用 `apply()` 和 `map()` 代替显式循环。
     - 使用合适的数据类型（如 `category`）减少内存占用。
     - 使用 `df.query()` 和 `df.eval()` 提升代码运行速度。
     - 对于大数据集，可以考虑使用 `Dask`、`Vaex` 等库实现并行化处理。

### 总结

这些 Pandas 面试题涵盖了从基础操作到高级应用的多个方面，通过熟悉这些问题及其解答，你不仅可以提升 Pandas 的使用能力，还能为面试做好充分准备。Pandas 是数据科学和工程中的重要工具，掌握它将显著提升你在数据处理和分析中的效率。