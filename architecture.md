# Supabase 架构图

## 整体架构

```mermaid
graph TB
    subgraph "客户端"
        A[Web App] 
        B[Mobile App]
        C[Desktop App]
    end
    
    subgraph "网关"
        D[Kong Gateway]
    end
    
    subgraph "应用"
        E[Studio]
        F[Docs]
        G[WWW]
        H[CMS]
    end
    
    subgraph "服务"
        I[GoTrue]
        J[PostgREST]
        K[Realtime]
        L[Storage]
        M[pg_graphql]
        N[pg_meta]
        O[Edge Functions]
    end
    
    subgraph "数据"
        P[PostgreSQL]
        Q[Supavisor]
        R[S3]
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
```

## 技术栈

### 前端
- React 18 + Next.js 15
- TypeScript
- Tailwind CSS
- Radix UI
- TanStack Query

### 后端
- PostgreSQL 15
- PostgREST (REST API)
- pg_graphql (GraphQL)
- GoTrue (认证)
- Realtime (WebSocket)
- Edge Functions (Deno)

### 工具
- pnpm + Turbo
- ESLint + Prettier
- Vitest
- Docker + Vercel
