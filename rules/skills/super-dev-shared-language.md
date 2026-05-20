# super-dev-shared-language — 共享语言建立

## 定位
建立团队**统一的领域语言**（Ubiquitous Language），消除业务-开发-测试之间的翻译损耗，让代码直接表达业务意图。

## 何时激活
- 新项目启动
- 领域建模讨论
- 需求文档评审
- 与领域专家沟通后
- 发现代码中命名不统一
- DDD 战略设计阶段

## 核心理念

```
业务语言 ≈ 代码语言

不是：业务说"客户"，代码写 CustomerEntity
而是：业务说"客户"，代码就写 Customer
      业务说"下单"，代码就写 PlaceOrder
```

## 工作流程

### Step 1: 术语采集
```
来源：
  1. 需求文档
  2. 与领域专家的对话记录
  3. 现有代码中的命名
  4. 行业标准术语

方法：
  - 逐句过需求，标注每个名词和动词
  - 问"这个术语在业务中是什么意思？"
  - 发现同义词 → 标记合并
  - 发现多义词 → 标记拆分
```

### Step 2: 术语统一
```
冲突解决：
  同一概念多个名称 → 选最常用/最精确的
  同一名称多个含义 → 用上下文限定词区分
  代码用英文，业务用中文 → 建立映射表
```

### Step 3: 术语表建立
```markdown
# [项目名称] 统一语言术语表

## 核心领域
| 中文 | English | 类型 | 定义 | 别名（不用） | 代码映射 |
|------|---------|------|------|-------------|----------|
| 订单 | Order | 实体 | 用户发起的购买请求 | 交易、单据 | Order |
| 订单项 | LineItem | 值对象 | 订单中的单个商品 | 订单明细 | LineItem |
| 下单 | PlaceOrder | 操作 | 创建新订单的过程 | 创建订单、提交 | PlaceOrderCommand |
| 待支付 | Pending | 状态 | 订单已创建但未付款 | 未付款 | OrderStatus.Pending |
| 库存 | Inventory | 实体 | 可售商品的库存量 | 存货 | Inventory |
| 锁定库存 | ReserveStock | 操作 | 下单时临时扣减库存 | 预占 | ReserveStockCommand |

## 边界上下文
- 订单上下文的"用户" = Buyer（买家）
- 评价上下文的"用户" = Reviewer（评价者）
- 售后上下文的"用户" = Customer（客户）

## 否定清单
以下术语**不再使用**，统一替换为：
- "交易" → "订单(Order)"
- "创建订单" → "下单(PlaceOrder)"
- "订单明细" → "订单项(LineItem)"
```

### Step 4: 代码对齐
```
命名规范检查：
□ Entity 命名 = 术语表中的实体名
□ Service 方法 = 术语表中的操作名
□ DTO/Command = 术语表中的操作 + Command/Query
□ 数据库表名 = 术语表实体名的复数下划线
□ API 路径 = 术语表的资源名（复数）
□ 前端组件 = 术语表的视图/页面名
```

## 代码命名约定

### 分层命名规范
```
Entity:    Order, Customer, Product           (名词)
Service:   PlaceOrder, CancelOrder            (动词+名词)
Repository: OrderRepository.save(Order)       (名词+Repository)
DTO:       CreateOrderRequest, OrderResponse  (操作+名词+后缀)
Event:     OrderPlaced, PaymentReceived       (名词+动词过去式)
Command:   PlaceOrderCommand                  (动词+名词+Command)
Query:     GetOrderQuery, ListOrdersQuery      (动词+名词+Query)
```

### 反模式：翻译损耗
```java
// ❌ 翻译损耗：业务说"下单"，代码写 createOrderRecord
public void createOrderRecord(OrderEntity entity) { ... }

// ✅ 无损耗：直接表达业务意图
public void placeOrder(PlaceOrder command) { ... }
```

## DDD 战术模式对齐

### 实体 (Entity)
```
命名：单数名词
示例：Order, Customer, Product
标识：有唯一 ID
```

### 值对象 (Value Object)
```
命名：名词短语
示例：Money, Address, Email, OrderLine
标识：无 ID，属性相等即相等
```

### 聚合 (Aggregate)
```
命名：根实体名
示例：Order（聚合根），包含 OrderLine
规则：外部只引用聚合根
```

### 领域事件 (Domain Event)
```
命名：名词 + 动词过去式
示例：OrderPlaced, PaymentConfirmed, StockReserved
时机：业务状态变化后
```

### 领域服务 (Domain Service)
```
命名：动词 + 名词 / 名词 + Service
示例：PricingService, FraudDetectionService
使用：当逻辑不属于任何单一实体时
```

### 仓储 (Repository)
```
命名：聚合根名 + Repository
示例：OrderRepository, CustomerRepository
职责：聚合的持久化和检索
```

## 代码评审检查

提交 PR 时检查：
```
□ 新增的类/方法名是否在术语表中？
□ 是否引入了术语表的"否定清单"中的词？
□ 方法和变量名是否表达了业务意图？
□ 注释中是否使用了统一术语？
□ PR 描述是否使用了业务语言而非技术俚语？
```

## 与上下游交互

```
上游：requirements（输出术语表初稿）
下游：architect（模块命名）、tdd（测试命名）、ux（UI 文案）
关联：memory（术语表作为项目记忆持久化）
```

## 反模式警示

| 反模式 | 表现 | 纠正 |
|--------|------|------|
| 中英混杂 | 同一个概念一会儿中文一会儿英文 | 代码统一英文，术语表做映射 |
| 过度翻译 | 把简单术语复杂化 | KISS，用最自然的表达 |
| 术语通胀 | 术语表膨胀到不可维护 | 只记录有歧义/冲突的术语 |
| 纸上统一 | 术语表有但代码不遵守 | CI 中加入命名规范检查 |
| 开发独裁 | 开发人员单方面决定术语 | 必须与领域专家共同确认 |
