# super-dev-tdd — 测试驱动开发

## 定位
严格遵循 **Red → Green → Refactor** 循环，确保每一行代码都有测试护航，实现零缺陷交付。

## 何时激活
- 编写任何业务逻辑代码
- 修复 Bug（先写复现测试）
- 重构现有代码
- 用户说"实现……功能"
- CI 红灯需要修复

## 核心循环

```
┌──────────────────────────────────────┐
│              TDD 循环                 │
│                                      │
│   1. RED    写一个失败的测试          │
│       ↓                              │
│   2. GREEN  写最少代码让测试通过       │
│       ↓                              │
│   3. REFACTOR 消除重复，优化结构      │
│       ↓                              │
│   4. REPEAT 下一个测试用例            │
└──────────────────────────────────────┘
```

## 详细步骤

### Step 1: RED — 写失败的测试

```
规则：
- 每个测试只测一件事
- 测试命名：should_[预期行为]_when_[条件]
- 使用 Given-When-Then 结构
- 先写断言，再写调用代码
- 运行测试 → 确认失败（不是编译/语法错误）
```

### Step 2: GREEN — 最少代码通过

```
规则：
- 写刚好让测试通过的代码
- 不要预测未来需求
- 允许"丑陋"的代码（稍后重构）
- 运行所有测试 → 全部通过
```

### Step 3: REFACTOR — 消除坏味道

```
规则：
- 测试必须保持绿色
- 一次只重构一个方面
- 消除重复、改善命名、提取方法
- 遵循 SOLID 原则
- 每小步后运行测试
```

## 测试分类与策略

### 测试金字塔
```
         /\
        /E2E\        少量：关键流程
       /------\
      /集成测试\      适量：API/DB/外部服务
     /----------\
    /  单元测试   \    大量：业务逻辑/纯函数
   /--------------\
```

### 测试编写顺序（Outside-In TDD）
```
1. E2E 测试（定义功能边界）
2. 集成测试（定义接口契约）
3. 单元测试（驱动实现细节）
```

### 测试编写顺序（Inside-Out TDD）
```
1. 核心领域逻辑单元测试
2. 组装为服务层集成测试
3. 验证整体流程 E2E
```

## 不同场景的 TDD 策略

### 纯函数/算法
```python
# RED
def test_should_calculate_discount_when_bulk_order():
    assert calculate_discount(quantity=100, unit_price=10) == 50

# GREEN
def calculate_discount(quantity, unit_price):
    if quantity >= 100:
        return unit_price * quantity * 0.05
    return 0

# REFACTOR
DISCOUNT_THRESHOLD = 100
DISCOUNT_RATE = 0.05
```

### API 端点
```python
# RED
async def test_should_return_404_when_user_not_found(client):
    response = await client.get("/api/users/99999")
    assert response.status_code == 404
    assert response.json() == {"error": "User not found"}

# GREEN（最小实现）
async def get_user(user_id: int):
    user = await db.users.find_by_id(user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

### 数据库操作
```python
# 使用测试数据库 + 事务回滚
@pytest.fixture
async def db_session():
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
        yield conn
        await conn.rollback()  # 测试后回滚
```

## Bug 修复流程

```
1. 先写复现 Bug 的测试 → RED (确认 Bug 存在)
2. 写修复代码 → GREEN (Bug 消失)
3. 分析根因 → 补充边界测试
4. REFACTOR → 防止同类 Bug
```

## 代码质量检查

重构阶段检查清单：
- [ ] 函数是否短小（<20行）？
- [ ] 每个函数只做一件事？
- [ ] 是否存在重复代码？
- [ ] 命名是否表达意图（不依赖注释）？
- [ ] 是否有不必要的依赖？
- [ ] 异常处理是否完整？
- [ ] 是否有硬编码的魔法数字？

## 与 Orchestrator 交互

```
输入：需求文档 + 架构设计方案
产出：通过测试的代码 + 测试套件 + 重构记录
触发：任务完成时自动触发 memory 沉淀
```

## 反模式警示

| 反模式 | 表现 | 纠正 |
|--------|------|------|
| 跳过 RED | 写代码前不写测试 | 严格执行 RED→GREEN→REFACTOR |
| 过度断言 | 一个测试测太多 | 每个测试只测一个行为 |
| 测试污染 | 测试之间有状态依赖 | 每个测试独立，setup/teardown |
| 假绿色 | 测试通过但没测到正确的东西 | 先看测试失败信息再写代码 |
| 跳过重构 | 堆积技术债务 | 每个循环都做重构 |
