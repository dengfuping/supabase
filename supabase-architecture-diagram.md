# Supabase 架构图

## 整体架构图

```mermaid
graph TB
    subgraph "客户端层"
        A[Web 应用] 
        B[移动应用]
        C[桌面应用]
    end
    
    subgraph "API 网关层"
        D[Kong Gateway<br/>API 路由和认证]
    end
    
    subgraph "应用服务层"
        E[Studio App<br/>Next.js + React]
        F[Docs App<br/>Next.js + MDX]
        G[WWW App<br/>Next.js + Contentlayer]
        H[CMS App<br/>Payload CMS]
    end
    
    subgraph "核心服务层"
        I[GoTrue<br/>JWT 认证服务]
        J[PostgREST<br/>RESTful API]
        K[Realtime<br/>Elixir WebSocket]
        L[Storage API<br/>文件存储服务]
        M[pg_graphql<br/>GraphQL API]
        N[pg_meta<br/>数据库管理]
        O[Edge Functions<br/>Deno 运行时]
    end
    
    subgraph "数据层"
        P[PostgreSQL<br/>主数据库]
        Q[Supavisor<br/>连接池]
        R[S3/Storage<br/>文件存储]
    end
    
    subgraph "监控和工具"
        S[Sentry<br/>错误监控]
        T[Logflare<br/>日志管理]
        U[Vercel<br/>部署平台]
    end
    
    A --> D
    B --> D
    C --> D
    D --> E
    D --> F
    D --> G
    D --> H
    D --> I
    D --> J
    D --> K
    D --> L
    D --> M
    D --> N
    D --> O
    
    I --> P
    J --> P
    K --> P
    L --> R
    M --> P
    N --> P
    O --> P
    
    P --> Q
    
    E --> S
    F --> S
    G --> S
    H --> S
    
    E --> T
    F --> T
    G --> T
    H --> T
    
    E --> U
    F --> U
    G --> U
    H --> U
```

## 数据流架构

```mermaid
sequenceDiagram
    participant Client as 客户端
    participant Kong as Kong Gateway
    participant Auth as GoTrue
    participant API as PostgREST
    participant DB as PostgreSQL
    participant Realtime as Realtime
    participant Storage as Storage API
    participant S3 as S3 Storage
    
    Client->>Kong: 1. 请求认证
    Kong->>Auth: 2. 转发认证请求
    Auth->>DB: 3. 验证用户
    DB-->>Auth: 4. 返回用户信息
    Auth-->>Kong: 5. 返回 JWT Token
    Kong-->>Client: 6. 返回认证结果
    
    Client->>Kong: 7. API 请求 (带 Token)
    Kong->>API: 8. 转发 API 请求
    API->>DB: 9. 执行数据库操作
    DB-->>API: 10. 返回数据
    API-->>Kong: 11. 返回 API 响应
    Kong-->>Client: 12. 返回数据
    
    DB->>Realtime: 13. 数据库变更通知
    Realtime-->>Client: 14. WebSocket 推送
    
    Client->>Kong: 15. 文件上传请求
    Kong->>Storage: 16. 转发存储请求
    Storage->>S3: 17. 存储文件
    S3-->>Storage: 18. 返回存储结果
    Storage-->>Kong: 19. 返回存储响应
    Kong-->>Client: 20. 返回上传结果
```

## 前端技术栈架构

```mermaid
graph LR
    subgraph "前端应用"
        A[Studio App]
        B[Docs App]
        C[WWW App]
        D[UI Library]
    end
    
    subgraph "共享包"
        E[UI Components]
        F[Common Utils]
        G[Config]
        H[Icons]
    end
    
    subgraph "核心框架"
        I[React 18]
        J[Next.js 15]
        K[TypeScript]
    end
    
    subgraph "UI 框架"
        L[Tailwind CSS]
        M[Radix UI]
        N[Framer Motion]
        O[Lucide React]
    end
    
    subgraph "状态管理"
        P[TanStack Query]
        Q[React Hook Form]
    end
    
    A --> E
    B --> E
    C --> E
    D --> E
    
    A --> F
    B --> F
    C --> F
    D --> F
    
    A --> I
    B --> I
    C --> I
    D --> I
    
    A --> L
    B --> L
    C --> L
    D --> L
    
    A --> P
    B --> P
    C --> P
    D --> P
```

## 开发工作流

```mermaid
graph LR
    A[开发] --> B[本地测试]
    B --> C[代码审查]
    C --> D[CI/CD 流水线]
    D --> E[部署到测试环境]
    E --> F[集成测试]
    F --> G[部署到生产环境]
    G --> H[监控和反馈]
    H --> A
    
    subgraph "工具链"
        I[pnpm]
        J[Turbo]
        K[ESLint]
        L[Prettier]
        M[Vitest]
    end
    
    A --> I
    A --> J
    A --> K
    A --> L
    A --> M
```

## 服务依赖关系

```mermaid
graph TD
    subgraph "外部依赖"
        A[GitHub OAuth]
        B[Stripe]
        C[OpenAI]
        D[AWS Bedrock]
    end
    
    subgraph "核心服务"
        E[GoTrue]
        F[PostgREST]
        G[Realtime]
        H[Storage API]
    end
    
    subgraph "数据库"
        I[PostgreSQL]
        J[pg_graphql]
        K[pg_meta]
    end
    
    subgraph "监控"
        L[Sentry]
        M[Logflare]
        N[Google Tag Manager]
    end
    
    E --> A
    E --> B
    F --> I
    G --> I
    H --> I
    J --> I
    K --> I
    
    E --> L
    F --> L
    G --> L
    H --> L
    
    E --> M
    F --> M
    G --> M
    H --> M
```

## 技术栈总结

### 前端技术
- **框架**: React 18 + Next.js 15
- **语言**: TypeScript
- **样式**: Tailwind CSS
- **组件**: Radix UI + Headless UI
- **动画**: Framer Motion
- **状态**: TanStack Query + React Hook Form

### 后端技术
- **数据库**: PostgreSQL 15
- **API**: PostgREST (REST) + pg_graphql (GraphQL)
- **认证**: GoTrue (JWT)
- **实时**: Realtime (Elixir WebSocket)
- **存储**: Storage API + S3
- **函数**: Edge Functions (Deno)

### 基础设施
- **网关**: Kong
- **连接池**: Supavisor
- **部署**: Vercel + Docker
- **监控**: Sentry + Logflare
- **CI/CD**: GitHub Actions

### 开发工具
- **包管理**: pnpm
- **构建**: Turbo
- **测试**: Vitest
- **代码质量**: ESLint + Prettier + Knip
