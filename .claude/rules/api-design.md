# API 设计规范

## RESTful 设计原则

### URL 规范

- 使用名词而非动词：`/users` 而非 `/getUsers`
- 使用小写字母和连字符：`/user-profiles` 而非 `/userProfiles`
- 资源嵌套：`/users/{user_id}/orders`
- 版本控制：`/api/v1/users`

### HTTP 方法

| 方法 | 用途 | 幂等性 |
|------|------|--------|
| GET | 读取资源 | ✅ |
| POST | 创建资源 | ❌ |
| PUT | 更新资源（完整） | ✅ |
| PATCH | 更新资源（部分） | ❌ |
| DELETE | 删除资源 | ✅ |

### 状态码

| 状态码 | 含义 |
|--------|------|
| 200 | 成功 |
| 201 | 创建成功 |
| 204 | 无内容（删除成功）|
| 400 | 请求参数错误 |
| 401 | 未认证 |
| 403 | 无权限 |
| 404 | 资源不存在 |
| 500 | 服务器错误 |

## 请求/响应格式

### 请求格式

```json
{
  "name": "张三",
  "email": "zhang@example.com"
}
```

### 响应格式

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "张三",
    "email": "zhang@example.com"
  },
  "message": "操作成功"
}
```

### 错误响应

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "参数验证失败",
    "details": [
      {"field": "email", "message": "邮箱格式不正确"}
    ]
  }
}
```

## 分页

```json
{
  "success": true,
  "data": [...],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

## 版本控制

- 在 URL 中体现：`/api/v1/users`
- 变更时新增版本，而非修改旧版本
- 维护旧版本至少一个版本周期