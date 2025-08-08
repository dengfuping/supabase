# Supabase 技术栈详细分析

## 项目架构概览

Supabase 是一个基于 PostgreSQL 的开源 Firebase 替代方案，采用 Monorepo 架构。

## 核心技术栈

### 1. 项目结构
- **包管理器**: pnpm v9.15.5
- **构建工具**: Turbo v2.3.3
- **Node.js**: >=22
- **TypeScript**: ~5.5.0

### 2. 前端技术栈

#### 应用层 (apps/)
- **Studio**: Next.js + React - 管理界面
- **Docs**: Next.js + MDX - 文档网站  
- **WWW**: Next.js + Contentlayer - 主网站
- **UI Library**: Next.js - UI组件展示
- **Design System**: Next.js - 设计系统
- **CMS**: Payload CMS - 内容管理

#### 核心框架
- **React**: 18.x
- **Next.js**: 15.3.1
- **TypeScript**: ~5.5.0

#### UI 组件
- **Tailwind CSS**: ^3.4.1
- **Radix UI**: Dialog, Accordion, Select等
- **Headless UI**: ^1.7.17
- **Framer Motion**: ^11.11.17
- **Lucide React**: ^0.436.0

#### 状态管理
- **TanStack Query**: 4.35.7
- **React Hook Form**: ^7.45.0

### 3. 后端技术栈

#### 数据库
- **PostgreSQL**: 15.x (核心)
- **pg_graphql**: GraphQL API
- **postgres-meta**: 数据库管理

#### API 服务
- **PostgREST**: RESTful API
- **GoTrue**: JWT 认证
- **Realtime**: Elixir WebSocket
- **Storage API**: 文件存储
- **Edge Functions**: Deno 运行时

#### 网关
- **Kong**: API 网关
- **Supavisor**: PostgreSQL 连接池

### 4. 开发工具

#### 代码质量
- **ESLint**: ^8.57.0
- **Prettier**: 3.2.4
- **Vitest**: ^3.0.5
- **Knip**: 未使用代码检测

#### 部署
- **Vercel**: 部署平台
- **Docker**: 容器化
- **GitHub Actions**: CI/CD

### 5. 第三方服务

#### 认证支付
- **Stripe**: 支付处理
- **GitHub OAuth**: 社交登录
- **hCaptcha**: 验证码

#### 监控分析
- **Sentry**: 错误监控
- **Google Tag Manager**: 分析
- **Logflare**: 日志管理

#### AI 功能
- **OpenAI**: AI 服务
- **AWS Bedrock**: AI 模型
- **Vector Search**: 向量搜索

## 架构特点

1. **模块化**: 每个服务独立部署
2. **类型安全**: 全栈 TypeScript
3. **实时能力**: WebSocket 支持
4. **可扩展**: PostgreSQL 原生
5. **开发者友好**: 类似 Firebase 体验

## 数据流

```
客户端 → Kong Gateway → 核心服务 → PostgreSQL
                ↓
            WebSocket → Realtime → 客户端
```

## 优势

- 开源优先
- PostgreSQL 原生能力
- 实时数据同步
- 类型安全
- 可扩展架构
- 丰富的开发者工具
