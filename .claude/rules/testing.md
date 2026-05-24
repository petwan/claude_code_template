# 测试规范

## 测试原则

1. **可重复**：测试结果稳定，不依赖外部状态
2. **独立性**：每个测试可以单独运行
3. **可读性**：测试名称清晰，描述测试目的
4. **快速**：单元测试应在毫秒级完成

## 测试结构

```
tests/
├── unit/           # 单元测试
├── integration/    # 集成测试
├── e2e/           # 端到端测试
└── fixtures/      # 测试数据
```

## 命名规范

### 测试文件和函数

```python
# 文件名：test_<module_name>.py
test_user_service.py

# 函数名：test_<功能>_<预期结果>
def test_login_with_valid_credentials_returns_token():
    pass

def test_login_with_invalid_password_returns_error():
    pass
```

### 测试数据命名

```python
# 合理描述的测试数据
valid_user = {"name": "张三", "email": "zhang@example.com"}
invalid_user = {"name": "", "email": "invalid"}
```

## 单元测试结构

```python
import pytest

class TestUserService:
    def setup_method(self):
        """每个测试前执行"""
        self.service = UserService()

    def teardown_method(self):
        """每个测试后执行"""
        self.service = None

    def test_create_user_success(self):
        # Arrange - 准备测试数据
        user_data = {"name": "张三", "email": "zhang@example.com"}

        # Act - 执行被测操作
        result = self.service.create(user_data)

        # Assert - 验证结果
        assert result.id is not None
        assert result.name == "张三"

    def test_create_user_with_invalid_email_raises_error(self):
        with pytest.raises(ValidationError) as exc_info:
            self.service.create({"email": "invalid"})
        assert "email" in str(exc_info.value)
```

## 测试覆盖率要求

| 项目类型 | 最低覆盖率 |
|----------|-----------|
| 核心业务逻辑 | 80%+ |
| API 层 | 70%+ |
| 工具类/辅助函数 | 90%+ |

## Mock 使用

```python
from unittest.mock import Mock, patch

def test_order_service_sends_notification():
    mock_sender = Mock()
    with patch('app.services.notification', mock_sender):
        service = OrderService(notification_sender=mock_sender)
        service.complete_order(1)
        mock_sender.send.assert_called_once()
```

## 集成测试

```python
@pytest.mark.integration
async def test_user_crud_flow():
    """用户完整 CRUD 流程"""
    # 创建
    user = await user_service.create({"name": "张三"})

    # 读取
    retrieved = await user_service.get(user.id)
    assert retrieved.name == "张三"

    # 更新
    updated = await user_service.update(user.id, {"name": "李四"})
    assert updated.name == "李四"

    # 删除
    await user_service.delete(user.id)
    with pytest.raises(NotFoundError):
        await user_service.get(user.id)
```