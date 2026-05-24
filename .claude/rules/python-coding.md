# Python Coding Rules

## 代码风格

- 使用 4 空格缩进
- 最大行长度 120 字符
- 每行最多一条语句

## 命名规范

- 变量/函数：`snake_case`
- 类名：`PascalCase`
- 常量：`UPPER_SNAKE_CASE`
- 私有成员：前缀 `_`

## 类型注解

- 鼓励使用类型注解（type hints）
- 复杂类型使用 `typing` 模块

## 文档字符串

- 所有公共类和函数需要 docstring
- 使用 Google 或 NumPy 风格

## 最佳实践

- 优先使用列表/字典推导式
- 使用 `is None` 而非 `== None`
- 避免使用 `from module import *`
- 异常处理：具体捕获，具体处理