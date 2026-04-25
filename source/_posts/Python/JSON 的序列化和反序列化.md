---
title: JSON 的序列化和反序列化
date: 2026-04-25 09:08:34
categories:
  - Python
tags:
  - Python
  - JSON
  - 序列化
  - 反序列化
---
在 Python 中，JSON 的序列化和反序列化主要通过内置的 `json` 模块来完成。处理起来非常简单，只需要记住四个核心方法（**带 `s` 的处理字符串，不带 `s` 的处理文件**）。

* **序列化 (Serialization)**：将 Python 对象（如字典、列表）转换为 JSON 格式的字符串或写入 JSON 文件。
* **反序列化 (Deserialization)**：将 JSON 格式的字符串或文件内容解析为 Python 对象。

以下是具体的处理方法和实战技巧：

### 1. 处理 JSON 字符串 (`dumps` 和 `loads`)

如果你是在内存中处理数据（比如处理网络请求的返回值），通常使用带有 `s` (代表 string) 的方法。

```python
import json

# 准备一个 Python 字典
python_data = {
    "name": "张三",
    "age": 25,
    "skills": ["Python", "SQL"],
    "is_student": False,
    "score": None
}

# ----------------- 1. 序列化 (Python对象 -> JSON字符串) -----------------
# ensure_ascii=False：保证中文字符正常显示，而不是变成 \uXXXX
# indent=4：格式化输出（美化），缩进 4 个空格
json_string = json.dumps(python_data, ensure_ascii=False, indent=4)

print("--- 序列化后的 JSON 字符串 ---")
print(json_string)
print("类型:", type(json_string))  # <class 'str'>


# ----------------- 2. 反序列化 (JSON字符串 -> Python对象) -----------------
parsed_data = json.loads(json_string)

print("\n--- 反序列化后的 Python 对象 ---")
print(parsed_data)
print("姓名:", parsed_data["name"])
print("类型:", type(parsed_data))  # <class 'dict'>
```

### 2. 处理 JSON 文件 (`dump` 和 `load`)

如果你需要将数据保存到本地文件，或者从本地文件中读取配置，使用不带 `s` 的方法。

```python
import json

data_to_save = {"name": "李四", "role": "admin"}

# ----------------- 1. 序列化并写入文件 -----------------
# 推荐加上 encoding='utf-8'，防止在 Windows 系统上出现中文乱码
with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(data_to_save, f, ensure_ascii=False, indent=4)
    print("✅ 数据已成功写入 data.json 文件")


# ----------------- 2. 从文件读取并反序列化 -----------------
with open('data.json', 'r', encoding='utf-8') as f:
    loaded_data = json.load(f)
    print("✅ 从文件中读取的数据:", loaded_data)
```

---

### 3. 高阶技巧：处理无法直接序列化的对象

默认的 `json` 模块只能处理基本数据类型（dict, list, string, int, float, bool, None）。如果你试图序列化一个 `datetime` 对象或自定义的类对象，会报错 `TypeError: Object of type ... is not JSON serializable`。

**解决方法：使用 `default` 参数提供自定义转换逻辑。**

#### 场景 A：序列化日期时间 (`datetime`)
```python
import json
from datetime import datetime

data = {
    "user": "Alice",
    "login_time": datetime.now()  # datetime 对象不能直接 JSON 序列化
}

# 自定义处理函数
def json_serial_fallback(obj):
    if isinstance(obj, datetime):
        return obj.strftime("%Y-%m-%d %H:%M:%S") # 转换为字符串
    raise TypeError(f"类型 {type(obj)} 无法被 JSON 序列化")

# 使用 default 参数指定 fallback 函数
json_str = json.dumps(data, default=json_serial_fallback)
print(json_str)
# 输出: {"user": "Alice", "login_time": "2026-03-17 19:47:00"}
```

#### 场景 B：序列化自定义类对象 (Class)
最简便的方法是利用对象的 `__dict__` 属性，将其转换为字典：

```python
import json

class User:
    def __init__(self, id, name):
        self.id = id
        self.name = name

user_obj = User(1, "Bob")

# 将对象的属性字典传给 json
json_str = json.dumps(user_obj.__dict__)
print(json_str)
# 输出: {"id": 1, "name": "Bob"}

# 或者像 datetime 一样使用 default=lambda o: o.__dict__
json_str2 = json.dumps(user_obj, default=lambda o: o.__dict__)
```

---

### 4. 性能优化（针对大型项目）

如果你的项目需要处理**海量 JSON 数据**（例如高并发 Web 服务），内置的 `json` 模块可能会成为性能瓶颈。这时可以考虑使用第三方库：

1. **`orjson` (极力推荐)**: 目前 Python 中最快、最正确的 JSON 库之一。原生支持 `datetime`、`UUID` 等对象的序列化。
    * 安装：`pip install orjson`
    * 用法：
      ```python
      import orjson
      
      data = {"name": "Alice"}
      # 序列化（注意：orjson.dumps 返回的是 bytes 类型，不是 str）
      json_bytes = orjson.dumps(data)
      
      # 反序列化
      parsed = orjson.loads(json_bytes)
      ```
2. **`pydantic`**: 如果你需要对接收到的 JSON 数据进行严格的**结构验证**和类型转换（FastAPI 默认使用该库）。