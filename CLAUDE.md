# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 1. 项目概述

### 1.1 项目简介

音乐词典（Music Dictionary）是一个多语言词典小程序系统，支持多端部署（微信小程序、支付宝小程序等），提供智能翻译功能。

### 1.2 核心特性

- **多端支持**：使用 uni-app 实现一套代码多端运行（微信、支付宝、H5等）
- **智能翻译**：四级查询机制（Redis缓存 → 本地词典 → 翻译API → AI翻译）
- **AI评估**：对翻译结果进行质量评估（评分阈值80分）
- **用户系统**：支持多小程序平台的统一用户体系
- **后台管理**：完整的管理后台系统（Vue 3 + Element Plus）

### 1.3 系统组成

- **后端服务**：Node.js + NestJS + TypeScript
- **小程序端**：uni-app（支持微信、支付宝等多端）
- **管理后台**：Vue 3 + Element Plus + Vite
- **数据库**：PostgreSQL 14+
- **缓存**：Redis 7+
- **消息队列**：Bull（基于Redis）

## 2. 技术栈

### 2.1 后端技术栈

- **核心框架**：Node.js 18+ + NestJS + TypeScript
- **数据库**：PostgreSQL 14+（支持JSON/JSONB、全文搜索）
- **ORM**：TypeORM（装饰器语法，支持迁移）
- **缓存**：Redis 7+（用于缓存和会话管理）
- **消息队列**：Bull（异步任务处理）
- **认证**：JWT（Access Token + Refresh Token）
- **日志**：Winston
- **API文档**：Swagger

### 2.2 前端技术栈

**小程序端（uni-app）**
- Vue.js 语法
- uView UI 组件库
- Pinia 状态管理
- 支持多端编译

**管理后台**
- Vue 3 + Composition API
- Element Plus 组件库
- TypeScript
- Vite 构建工具
- Pinia 状态管理

### 2.3 外部服务集成

**AI模型**
- OpenAI GPT-4/GPT-3.5
- 百度文心一言
- 阿里通义千问
- 讯飞星火

**翻译API**
- 百度翻译API
- 有道翻译API
- 腾讯翻译API
- Google翻译API

## 3. 项目结构

```
musicdictionary-ai/
├── backend/                    # 后端服务（NestJS）
│   ├── src/
│   │   ├── modules/           # 业务模块
│   │   │   ├── auth/         # 认证模块
│   │   │   ├── users/        # 用户模块
│   │   │   ├── dictionary/   # 词典模块
│   │   │   ├── translation/  # 翻译模块
│   │   │   ├── favorites/    # 收藏模块
│   │   │   └── admin/        # 管理模块
│   │   ├── common/           # 公共模块
│   │   │   ├── decorators/   # 装饰器
│   │   │   ├── filters/      # 异常过滤器
│   │   │   ├── guards/       # 守卫
│   │   │   ├── interceptors/ # 拦截器
│   │   │   └── pipes/        # 管道
│   │   ├── providers/        # 服务提供者
│   │   │   ├── ai/          # AI服务适配器
│   │   │   └── translation/ # 翻译服务适配器
│   │   ├── config/          # 配置文件
│   │   ├── database/        # 数据库相关
│   │   │   ├── entities/    # 实体
│   │   │   └── migrations/  # 迁移文件
│   │   └── main.ts          # 入口文件
│   ├── test/                # 测试文件
│   ├── .env.example         # 环境变量示例
│   ├── package.json
│   └── tsconfig.json
│
├── miniapp/                 # 小程序（uni-app）
│   ├── pages/              # 页面
│   │   ├── index/         # 首页
│   │   ├── search/        # 搜索页
│   │   ├── favorites/     # 收藏页
│   │   └── profile/       # 个人中心
│   ├── components/        # 组件
│   ├── api/              # API封装
│   ├── store/            # 状态管理
│   ├── utils/            # 工具函数
│   ├── static/           # 静态资源
│   ├── App.vue
│   ├── main.js
│   ├── manifest.json     # 应用配置
│   └── pages.json        # 页面配置
│
├── admin/                 # 管理后台（Vue 3）
│   ├── src/
│   │   ├── views/        # 页面
│   │   ├── components/   # 组件
│   │   ├── api/         # API封装
│   │   ├── router/      # 路由
│   │   ├── store/       # 状态管理
│   │   ├── utils/       # 工具函数
│   │   └── main.ts
│   ├── package.json
│   └── vite.config.ts
│
├── doc/                  # 文档
│   ├── 需求文档.md
│   ├── 技术方案.md
│   └── 实施方案.md
│
├── docker-compose.yml    # Docker编排文件
├── .gitignore
├── README.md
└── CLAUDE.md            # 本文件
```

## 4. 开发环境配置

### 4.1 环境要求

- **Node.js**: 18.x 或更高版本
- **PostgreSQL**: 14.x 或更高版本
- **Redis**: 7.x 或更高版本
- **微信开发者工具**: 最新稳定版
- **Git**: 2.x 或更高版本

### 4.2 本地开发环境搭建

**1. 安装数据库和缓存**

```bash
# 使用 Docker 快速启动（推荐）
docker-compose up -d postgres redis

# 或手动安装 PostgreSQL 和 Redis
```

**2. 后端服务配置**

```bash
# 进入后端目录
cd backend

# 安装依赖
npm install

# 复制环境变量配置文件
cp .env.example .env

# 编辑 .env 文件，配置数据库连接、Redis、API密钥等
# DATABASE_URL=postgresql://admin:password@localhost:5432/musicdict
# REDIS_URL=redis://localhost:6379
# JWT_SECRET=your-secret-key
# ...

# 运行数据库迁移
npm run migration:run

# 启动开发服务器
npm run start:dev
```

**3. 小程序端配置**

```bash
# 进入小程序目录
cd miniapp

# 安装依赖
npm install

# 配置小程序 AppID（在 manifest.json 中）
# 使用微信开发者工具打开 miniapp 目录
```

**4. 管理后台配置**

```bash
# 进入管理后台目录
cd admin

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

## 5. 常用开发命令

### 5.1 后端命令

```bash
# 开发模式启动
npm run start:dev

# 生产模式启动
npm run start:prod

# 构建项目
npm run build

# 运行测试
npm run test

# 运行测试（监听模式）
npm run test:watch

# 测试覆盖率
npm run test:cov

# 数据库迁移
npm run migration:generate -- -n MigrationName  # 生成迁移文件
npm run migration:run                           # 运行迁移
npm run migration:revert                        # 回滚迁移

# 代码格式化
npm run format

# 代码检查
npm run lint
```

### 5.2 小程序命令

```bash
# 安装依赖
npm install

# 编译到微信小程序
npm run dev:mp-weixin

# 编译到支付宝小程序
npm run dev:mp-alipay

# 编译到 H5
npm run dev:h5

# 生产环境构建
npm run build:mp-weixin
npm run build:mp-alipay
npm run build:h5
```

### 5.3 管理后台命令

```bash
# 开发模式
npm run dev

# 生产构建
npm run build

# 预览生产构建
npm run preview

# 代码检查
npm run lint

# 类型检查
npm run type-check
```

### 5.4 Docker 命令

```bash
# 启动所有服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止所有服务
docker-compose down

# 重启服务
docker-compose restart

# 查看服务状态
docker-compose ps
```

## 6. 核心架构

### 6.1 系统架构图

```
客户端层
├── 微信小程序 (uni-app)
├── 支付宝小程序 (uni-app)
├── H5应用 (uni-app)
└── 后台管理系统 (Vue3 + Element Plus)
         ↓
API网关层 (Nginx + 负载均衡 + SSL)
         ↓
应用服务层 (NestJS)
├── 用户模块 (Auth/User)
├── 词典模块 (Dictionary)
├── 翻译模块 (Translation)
└── 管理模块 (Admin)
         ↓
业务逻辑层 + 外部服务层
├── 词典查询策略
├── 翻译评估逻辑
├── AI服务适配器
└── 翻译API适配器
         ↓
数据访问层
├── PostgreSQL (主数据库)
├── Redis (缓存)
├── Bull (消息队列)
└── OSS (文件存储)
```

### 6.2 核心业务流程：智能翻译

**四级查询机制**

1. **第一级：Redis缓存**
   - 缓存key格式：`dict:{word}:{from}:{to}`
   - 缓存时间：24小时
   - 命中率预期：60-70%

2. **第二级：本地词典**
   - PostgreSQL全文搜索
   - 支持模糊匹配
   - 查询时间：<50ms

3. **第三级：翻译API**
   - 轮询多个翻译服务（百度、有道、腾讯）
   - 失败自动切换备用服务
   - 超时时间：5秒

4. **第四级：AI翻译**
   - 仅在翻译API评分不达标时调用（<80分）
   - 异步处理，避免阻塞
   - 超时时间：10秒

**翻译流程**

```
用户输入单词 → 选择源语言和目标语言
         ↓
    查询Redis缓存
         ↓
    缓存命中？
    ├─是→ 返回缓存结果
    └─否→ 查询本地词典
         ↓
    词典有数据？
    ├─是→ 返回词典结果 → 写入缓存
    └─否→ 调用翻译API
         ↓
    AI评估翻译质量
         ↓
    评分>=80？
    ├─是→ 返回翻译结果 → 写入缓存
    └─否→ 调用AI翻译 → 返回AI翻译 → 写入缓存
         ↓
    (可选)保存到词典
```

### 6.3 设计模式

**策略模式 + 工厂模式**

```typescript
// 统一的AI接口
interface AIProvider {
  translate(text: string, from: string, to: string): Promise<string>;
  evaluate(translation: string, context: any): Promise<number>;
}

// 统一的翻译接口
interface TranslationProvider {
  translate(text: string, from: string, to: string): Promise<string>;
}

// 工厂类负责创建具体的服务实例
class AIProviderFactory {
  static create(provider: string): AIProvider {
    // 根据配置创建对应的AI服务实例
  }
}
```

## 7. 数据库设计

### 7.1 核心数据表

**主要数据表**
- `users` - 用户表
- `user_platform_bindings` - 用户平台绑定表（支持多平台）
- `languages` - 语言表
- `dictionary_entries` - 词典表
- `favorite_folders` - 收藏夹表
- `favorites` - 收藏表
- `miniapp_configs` - 小程序配置表
- `ai_provider_configs` - AI模型配置表
- `translation_provider_configs` - 翻译API配置表

### 7.2 关键设计要点

**1. 用户多平台绑定**
- 一个用户可以绑定多个小程序平台（微信、支付宝等）
- 通过 `user_platform_bindings` 表实现
- 支持 `open_id` 和 `union_id`

**2. 词典数据存储**
- 使用 JSONB 字段存储例句（`examples`）
- 支持全文搜索（PostgreSQL 内置功能）
- 记录词典来源（`source`: manual/api/ai）
- 质量评分字段（`quality_score`: 0-100）

**3. 配置动态管理**
- AI和翻译服务配置存储在数据库中
- 支持优先级设置（`priority` 字段）
- 支持启用/禁用状态（`status` 字段）
- 使用 JSONB 存储额外配置信息

**4. 索引优化**
```sql
-- 词典查询优化
CREATE INDEX idx_dict_word ON dictionary_entries(word, source_lang, target_lang);

-- 用户查询优化
CREATE INDEX idx_users_phone ON users(phone);

-- 平台绑定查询优化
CREATE INDEX idx_upb_platform_openid ON user_platform_bindings(platform, open_id);
```

## 8. API 规范

### 8.1 RESTful API 设计

**基础URL**
- 开发环境：`http://localhost:3000/api/v1`
- 生产环境：`https://api.yourdomain.com/api/v1`

**统一响应格式**

```typescript
// 成功响应
{
  "code": 0,
  "message": "success",
  "data": { ... }
}

// 错误响应
{
  "code": 1001,
  "message": "错误描述",
  "error": "详细错误信息"
}
```

**状态码规范**
- `0`: 成功
- `1001-1999`: 客户端错误（参数错误、权限不足等）
- `2001-2999`: 服务端错误（数据库错误、第三方服务错误等）
- `3001-3999`: 业务逻辑错误（用户不存在、词典未找到等）

### 8.2 核心API接口

**用户认证**
```
POST   /auth/login              # 用户登录
POST   /auth/register           # 用户注册
POST   /auth/refresh            # 刷新token
POST   /auth/logout             # 退出登录
GET    /auth/miniapp/session    # 小程序登录
```

**词典查询**
```
POST   /dictionary/translate    # 翻译查询（核心接口）
GET    /dictionary/search       # 搜索词典
GET    /dictionary/history      # 查询历史
DELETE /dictionary/history/:id  # 删除查询历史
```

**收藏管理**
```
GET    /favorites               # 获取收藏列表
POST   /favorites               # 添加收藏
DELETE /favorites/:id           # 删除收藏
PUT    /favorites/:id           # 更新收藏

GET    /favorites/folders       # 获取收藏夹列表
POST   /favorites/folders       # 创建收藏夹
PUT    /favorites/folders/:id   # 更新收藏夹
DELETE /favorites/folders/:id   # 删除收藏夹
```

## 9. 开发规范

### 9.1 代码规范

**后端（NestJS/TypeScript）**

- 类名使用 PascalCase（如 `UserService`）
- 方法名和变量名使用 camelCase（如 `getUserById`）
- 常量使用 UPPER_SNAKE_CASE（如 `MAX_RETRY_COUNT`）
- 接口名以 I 开头（如 `IUserService`）
- 每个模块包含：controller、service、entity、dto
- 使用 class-validator 进行参数验证
- 使用 class-transformer 进行数据转换
- 使用 ESLint + Prettier 进行代码格式化

**前端（Vue/uni-app）**

- 组件名使用 PascalCase（如 `UserCard`）
- 文件名使用 kebab-case（如 `user-card.vue`）
- 变量和方法使用 camelCase（如 `getUserData`）
- 使用 Composition API（Vue 3）
- 使用 TypeScript 进行类型检查
- 使用 Pinia 进行状态管理

### 9.2 提交规范

遵循 Conventional Commits 规范：

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type 类型**
- `feat`: 新功能
- `fix`: 修复bug
- `docs`: 文档更新
- `style`: 代码格式调整
- `refactor`: 重构
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建工具或辅助工具变动

**示例**
```
feat(dictionary): 实现词典查询接口

- 添加Redis缓存机制
- 实现四级查询策略
- 添加查询历史记录

Closes #123
```

### 9.3 安全规范

**认证与授权**
- JWT Token 机制（Access Token 2小时，Refresh Token 7天）
- 使用 RBAC 权限控制
- Token 存储在 HTTP-only Cookie 中

**数据安全**
- 密码使用 bcrypt 加密（加盐轮次10）
- API密钥使用 AES-256 加密存储
- 使用 TypeORM 参数化查询防止 SQL 注入
- 前端输入进行 HTML 转义防止 XSS

**API安全**
- 使用 Redis 实现请求限流（普通用户100次/分钟）
- 定期轮换 API 密钥
- 不在代码中硬编码密钥

## 10. Git 分支策略

本项目采用 Git Flow 分支模型，适合多人协作和分阶段开发。

### 10.1 主要分支

**main 分支**
- 生产环境分支，始终保持可发布状态
- 只接受来自 release 和 hotfix 分支的合并
- 每次合并都应该打上版本标签（如 v1.0.0）
- 受保护分支，需要 Pull Request 和代码审查

**develop 分支**
- 开发主分支，包含最新的开发进度
- 所有功能分支从此分支创建，完成后合并回此分支
- 定期合并到 release 分支进行测试
- 受保护分支，需要 Pull Request 和代码审查

### 10.2 辅助分支

**feature/* 分支（功能分支）**
- 从 develop 分支创建
- 用于开发新功能或改进现有功能
- 完成后合并回 develop 分支
- 命名规范：`feature/阶段-功能描述`
  - 例如：`feature/phase1-user-auth`（阶段一：用户认证）
  - 例如：`feature/phase3-dictionary-search`（阶段三：词典搜索）

**release/* 分支（发布分支）**
- 从 develop 分支创建
- 用于准备新版本发布
- 只允许修复 bug，不允许添加新功能
- 完成后合并到 main 和 develop 分支
- 命名规范：`release/v版本号`（如 `release/v1.0.0`）

**hotfix/* 分支（热修复分支）**
- 从 main 分支创建
- 用于紧急修复生产环境的 bug
- 完成后合并到 main 和 develop 分支
- 命名规范：`hotfix/v版本号-问题描述`（如 `hotfix/v1.0.1-fix-login-error`）

### 10.3 版本号规范

采用语义化版本号（Semantic Versioning）：`v主版本号.次版本号.修订号`

**版本号递增规则**
- **主版本号（Major）**：不兼容的 API 修改
- **次版本号（Minor）**：向下兼容的功能性新增
- **修订号（Patch）**：向下兼容的问题修正

**版本号与实施阶段对应关系**
- v0.1.0：阶段一完成（基础设施搭建）
- v0.2.0：阶段二完成（用户认证和基础功能）
- v0.3.0：阶段三完成（词典核心功能）
- v0.4.0：阶段四完成（AI翻译集成）
- v0.5.0：阶段五完成（管理后台开发）
- v0.6.0：阶段六完成（测试和优化）
- v1.0.0：阶段七完成（正式发布）

## 11. 测试规范

### 11.1 测试策略

**单元测试**
- 测试独立的函数和方法
- 覆盖率要求：核心业务逻辑 >80%
- 使用 Jest 测试框架
- Mock 外部依赖

**集成测试**
- 测试模块间的交互
- 测试数据库操作
- 测试 API 接口
- 使用测试数据库

**端到端测试**
- 测试完整的业务流程
- 测试用户操作路径
- 使用真实环境数据

### 11.2 测试命名规范

```typescript
// 后端测试
describe('UserService', () => {
  describe('getUserById', () => {
    it('should return user when user exists', async () => {
      // 测试代码
    });

    it('should throw NotFoundException when user not found', async () => {
      // 测试代码
    });
  });
});

// 前端测试
describe('UserCard Component', () => {
  it('should render user information correctly', () => {
    // 测试代码
  });
});
```

### 11.3 测试最佳实践

- 每个测试应该独立运行
- 使用 AAA 模式（Arrange、Act、Assert）
- 测试名称应该描述测试的内容
- 避免测试实现细节，关注行为
- 定期运行测试，确保代码质量

## 12. 部署说明

### 12.1 Docker 部署（推荐）

**docker-compose.yml 配置**

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:14
    environment:
      POSTGRES_DB: musicdict
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  backend:
    build: ./backend
    environment:
      NODE_ENV: production
      DATABASE_URL: postgresql://admin:${DB_PASSWORD}@postgres:5432/musicdict
      REDIS_URL: redis://redis:6379
    ports:
      - "3000:3000"
    depends_on:
      - postgres
      - redis

volumes:
  postgres_data:
  redis_data:
```

**部署步骤**

```bash
# 1. 构建并启动服务
docker-compose up -d

# 2. 查看服务状态
docker-compose ps

# 3. 查看日志
docker-compose logs -f backend

# 4. 停止服务
docker-compose down
```

### 12.2 生产环境配置

**服务器要求**
- CPU: 4核+
- 内存: 8GB+
- 硬盘: 100GB+ SSD
- 带宽: 5Mbps+

**环境变量配置**
```bash
# 数据库配置
DATABASE_URL=postgresql://admin:password@localhost:5432/musicdict

# Redis配置
REDIS_URL=redis://localhost:6379

# JWT配置
JWT_SECRET=your-secret-key
JWT_EXPIRES_IN=2h
JWT_REFRESH_EXPIRES_IN=7d

# AI服务配置（在数据库中管理）
# 翻译服务配置（在数据库中管理）
```

### 12.3 小程序发布

**微信小程序发布流程**
1. 在微信开发者工具中点击"上传"
2. 填写版本号和项目备注
3. 在微信公众平台提交审核
4. 审核通过后发布上线

**注意事项**
- 确保小程序 AppID 和密钥配置正确
- 提前了解小程序审核规范
- 准备详细的功能说明文档

## 13. 实施阶段

本项目分为七个阶段实施，详细内容请参考 [实施方案.md](doc/实施方案.md)。

### 阶段一：基础设施搭建（v0.1.0）
- 项目初始化（后端、小程序、管理后台）
- 数据库设计与实施
- 缓存和消息队列配置
- Docker容器化

### 阶段二：用户认证和基础功能（v0.2.0）
- 用户认证模块（JWT、小程序登录）
- 权限控制（RBAC）
- 用户管理
- 小程序端基础页面

### 阶段三：词典核心功能（v0.3.0）
- 词典查询模块
- 翻译服务适配器
- 收藏功能
- 词典管理

### 阶段四：AI翻译集成（v0.4.0）
- AI服务适配器
- 翻译质量评估
- 智能翻译流程（四级查询机制）
- 配置管理

### 阶段五：管理后台开发（v0.5.0）
- 后台框架搭建
- 用户管理模块
- 词典管理模块
- 配置管理模块
- 数据统计模块

### 阶段六：测试和优化（v0.6.0）
- 功能测试（单元测试、集成测试、端到端测试）
- 性能测试和优化
- 安全测试
- 用户体验优化

### 阶段七：部署和上线（v1.0.0）
- 生产环境准备
- Docker部署
- 小程序发布
- 监控和日志配置
- 数据迁移和初始化

## 14. 相关文档

- [需求文档.md](doc/需求文档.md) - 项目需求说明
- [技术方案.md](doc/技术方案.md) - 详细技术方案
- [实施方案.md](doc/实施方案.md) - 分阶段实施计划

## 15. 注意事项

### 15.1 开发注意事项

- **API密钥管理**：不要将 AI API 密钥硬编码在代码中，应该通过数据库配置管理
- **请求频率限制**：注意 AI API 的调用频率限制和配额
- **错误处理**：AI 和翻译服务可能不稳定，需要完善的错误处理和重试机制
- **用户体验**：AI 响应可能较慢，需要添加加载状态和超时处理
- **缓存策略**：合理使用缓存，提高系统响应速度
- **数据库优化**：注意索引优化和查询性能

### 15.2 安全注意事项

- 所有用户输入必须进行验证和过滤
- 敏感数据必须加密存储
- 定期更新依赖包，修复安全漏洞
- 实施请求限流，防止恶意攻击
- 定期备份数据库

### 15.3 性能优化建议

- 使用 Redis 缓存热门查询结果
- 实施数据库连接池
- 使用 CDN 加速静态资源
- 异步处理耗时操作（AI翻译、批量导入）
- 定期分析慢查询日志

---

**文档版本**：1.0
**最后更新**：2026-01-16
**维护者**：开发团队
