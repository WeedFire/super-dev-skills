# super-dev-security — 供应链安全门禁

## 定位
建立**代码供应链安全门禁**，在每次变更时自动化安全检查，确保交付绝对安全可靠的软件。

## 何时激活
- 新增或更新依赖（npm/pip/cargo/gomod 等）
- 代码提交前（pre-commit 阶段）
- Pull Request 审查
- 发布/部署前（release gate）
- 用户提出安全相关需求
- 定期安全审计

## 安全门禁体系

```
代码变更
    │
    ▼
┌─────────────────┐
│ Gate 1: 依赖扫描  │ ← 已知漏洞、恶意包、许可证
└────────┬────────┘
         ▼
┌─────────────────┐
│ Gate 2: 代码审计  │ ← 注入、XSS、Auth、敏感信息
└────────┬────────┘
         ▼
┌─────────────────┐
│ Gate 3: 配置检查  │ ← 硬编码密钥、不安全配置
└────────┬────────┘
         ▼
┌─────────────────┐
│ Gate 4: 构建签名  │ ← SBOM、签名验证、完整性
└────────┬────────┘
         ▼
      放行 / 阻断
```

## Gate 1: 依赖安全扫描

### 检查项
```
□ 已知 CVE 漏洞（CVSS ≥ 7 阻断）
□ 恶意/钓鱼包检测
□ 许可证合规性检查
□ 依赖过期/未维护警告
□ 传递依赖风险评估
□ 依赖混淆攻击检测
```

### 工具链
```
Node.js:  npm audit / snyk / socket.dev
Python:   pip-audit / safety / bandit
Rust:     cargo-audit / cargo-deny
Go:       govulncheck
Java:     OWASP Dependency-Check
通用:     Trivy / Grype / OSV-Scanner
```

### .npmrc / pyproject.toml 加固
```ini
# .npmrc
audit=true
fund=false
ignore-scripts=true
engine-strict=true
```

## Gate 2: 代码安全审计

### OWASP Top 10 检查
```
□ 注入（SQL/NoSQL/OS Command/LDAP）
□ 失效的身份认证
□ 敏感数据暴露
□ XML 外部实体（XXE）
□ 失效的访问控制
□ 安全配置错误
□ 跨站脚本（XSS）
□ 不安全的反序列化
□ 使用含有已知漏洞的组件
□ 日志和监控不足
```

### 代码审计规则

**输入验证**
```
✓ 所有用户输入必须验证和净化
✓ 使用参数化查询，禁止字符串拼接 SQL
✓ 文件上传检查类型、大小、内容
✓ URL 重定向限制白名单域名
```

**认证与授权**
```
✓ 密码必须哈希存储（bcrypt/argon2）
✓ API 密钥不得硬编码
✓ JWT 设置合理过期时间
✓ 权限检查在服务端执行
```

**数据保护**
```
✓ 敏感数据传输使用 HTTPS
✓ 日志中不得包含密码/Token
✓ PII 数据加密存储
✓ CSRF Token 用于状态变更请求
```

**前端安全**
```
✓ React: 避免 dangerouslySetInnerHTML
✓ Vue: 避免 v-html 用于用户内容
✓ CSP 头配置合理
✓ 第三方脚本使用 integrity hash
```

### 敏感信息扫描（Gitleaks/TruffleHog）
```
模式匹配：
- API Keys (sk-..., AIza..., etc.)
- Private Keys (BEGIN RSA PRIVATE KEY)
- Connection Strings (mongodb://, postgres://)
- Tokens & Secrets
- Internal URLs & IPs
```

## Gate 3: 配置安全检查

### 检查清单
```
□ .env 文件不提交到仓库
□ .gitignore 包含敏感文件类型
□ CI/CD Secret 使用平台密钥管理
□ 默认密码已更改
□ DEBUG 模式生产环境关闭
□ CORS 配置不宽松（不使用 *）
□ CSP 头正确配置
□ 安全 Headers 完整（HSTS, X-Frame-Options, etc.）
```

### 安全 Headers 基线
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 0
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

## Gate 4: 供应链完整性

### SBOM（Software Bill of Materials）
```
每次构建生成 SBOM：
- CycloneDX 或 SPDX 格式
- 包含所有直接和传递依赖
- 记录版本、许可证、哈希
- 归档用于事后审计
```

### 构建签名
```
□ Git commit 签名（GPG/SSH）
□ 发布包签名验证
□ Docker 镜像签名（Cosign/Notary）
□ 构建产物哈希记录
```

## 安全事件响应

### 发现 CVE 时
```
1. 评估影响范围（是否在攻击面内）
2. 记录风险等级
3. 立即修复：可修补 → 升级版本
4. 无法立即修复：加 WAF 规则 / 临时缓解
5. 通知相关方
6. 事后复盘 → 沉淀到 memory
```

## 与上下游交互

```
上游：architect（架构威胁建模）
      tdd（安全测试用例）
下游：memory（沉淀安全事件教训）
      evolution（优化安全规则）
```

## 反模式警示

| 反模式 | 表现 | 纠正 |
|--------|------|------|
| 安全后补 | 开发完再加安全 | 左移安全（Shift Left） |
| 依赖盲目 | 不加审查引入依赖 | 每个新依赖必须经过 Gate 1 |
| 假阳性疲劳 | 忽略所有安全告警 | 分级处理，高危必查 |
| 秘钥泄露 | 硬编码或误提交 | pre-commit hook + GitGuardian |
| 信任客户端 | 前端校验代替后端校验 | 所有校验在后端重复 |
