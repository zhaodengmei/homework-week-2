# SQL执行引擎架构设计

## 1. 系统概述
本系统是一个简化版SQL执行引擎，用于接收SQL文本并返回执行结果，核心流程包括解析、计划生成、执行与数据存储交互。

## 2. 模块划分
### 2.1 Parser（解析模块）
- **职责**：将SQL文本转换为抽象语法树（AST）
- **子模块**：
  - Lexer：词法分析，将SQL文本拆分为Token流
  - Parser：语法分析，将Token流转换为AST

### 2.2 Planner（计划模块）
- **职责**：将AST转换为可执行的物理计划
- **子模块**：
  - Logical Plan：生成逻辑执行计划（描述“做什么”）
  - Physical Plan：生成物理执行计划（描述“怎么做”）

### 2.3 Executor（执行模块）
- **职责**：执行物理计划，读取存储数据并返回结果

### 2.4 Storage（存储模块）
- **职责**：管理内存与磁盘数据，为执行器提供数据读写能力
- **子模块**：
  - Buffer Pool：内存缓冲池，缓存热点数据
  - Disk：磁盘持久化存储

## 3. 数据流向
SQL Text → Lexer → Token Stream → Parser → AST → Logical Plan → Physical Plan → Executor → Result
Executor ↔ Buffer Pool ↔ Disk

## 4. 架构图
```mermaid
graph TB
    subgraph Parser
        A[SQL Text] --> B[Lexer]
        B --> C[Token Stream]
        C --> D[Parser]
        D --> E[AST]
    end

    subgraph Planner
        E --> F[Logical Plan]
        F --> G[Physical Plan]
    end

    subgraph Executor
        G --> H[Executor]
        H --> I[Result]
    end

    subgraph Storage
        H --> J[Buffer Pool]
        J --> K[Disk]
    end
