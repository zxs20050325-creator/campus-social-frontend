# 校园轻社交+资源置换平台

这是一个基于 Vue3 + Vuetify3 和 Python + FastAPI 的校园社交和资源置换平台。

## 项目概述

本项目旨在为校园用户提供一个轻社交和资源置换的平台，主要功能包括：

- 用户注册和登录
- 发布和浏览社交动态
- 发布和浏览可置换的物品
- 关注其他用户
- 评论和点赞功能

## 技术栈

### 前端
- Vue 3
- Vuetify 3 (UI 框架)
- Vue Router (路由管理)
- Vuex (状态管理)
- Vite (构建工具)

### 后端
- Python
- FastAPI (Web 框架)
- SQLAlchemy (ORM)
- SQLite (数据库)

## 项目结构

```
my-project/
├── backend/                # 后端代码
│   ├── main.py             # FastAPI 主应用
│   ├── models.py           # 数据库模型
│   ├── schemas.py          # Pydantic 数据模型
│   ├── database.py         # 数据库配置
│   ├── crud.py             # CRUD 操作
│   └── requirements.txt    # 依赖包列表
└── frontend/               # 前端代码
    ├── src/
    │   ├── components/     # 组件
    │   ├── views/          # 页面视图
    │   ├── router/         # 路由配置
    │   ├── store/          # 状态管理
    │   └── assets/         # 静态资源
    ├── public/             # 公共资源
    ├── package.json        # 项目配置
    └── vite.config.js      # Vite 配置
```

## 快速开始

### 后端设置

1. 安装 Python 依赖：
```bash
cd backend
pip install -r requirements.txt
```

2. 运行后端服务：
```bash
python main.py
```

后端服务将在 http://localhost:8000 上运行。

### 前端设置

1. 安装 Node.js 依赖：
```bash
cd frontend
npm install
```

2. 运行前端开发服务器：
```bash
npm run dev
```

前端开发服务器将在 http://localhost:3000 上运行。

## API 接口

### 用户相关

- `POST /users/` - 创建用户
- `GET /users/{user_id}` - 获取指定用户信息
- `GET /users/` - 获取用户列表

### 资源置换相关

- `POST /exchanges/` - 创建置换物品
- `GET /exchanges/` - 获取置换物品列表
- `GET /exchanges/{exchange_id}` - 获取指定物品详情

### 社交帖子相关

- `POST /posts/` - 创建帖子
- `GET /posts/` - 获取帖子列表

## 数据库模型

### 用户 (User)
- id: 主键
- username: 用户名
- email: 邮箱
- hashed_password: 加密密码
- full_name: 全名
- bio: 个人简介
- avatar: 头像URL
- is_active: 是否激活
- created_at: 创建时间
- updated_at: 更新时间

### 交换物品 (ExchangeItem)
- id: 主键
- owner_id: 所属用户ID
- title: 物品标题
- description: 物品描述
- category: 类别
- condition: 物品状况
- image_urls: 图片URL列表
- is_available: 是否可用
- created_at: 创建时间
- updated_at: 更新时间

### 帖子 (Post)
- id: 主键
- author_id: 作者ID
- title: 标题
- content: 内容
- image_urls: 图片URL列表
- likes_count: 点赞数
- comments_count: 评论数
- created_at: 创建时间
- updated_at: 更新时间

## 开发说明

1. 项目采用前后端分离架构
2. 前端通过代理访问后端API（开发环境）
3. 后端使用JWT进行身份验证（生产环境需要添加）
4. 数据库使用SQLite（可替换为其他数据库）

## 注意事项

1. 生产环境部署时，请确保：
   - 更改默认的数据库配置
   - 添加身份验证机制
   - 配置HTTPS
   - 设置适当的CORS策略

2. 项目目前处于开发阶段，某些功能可能尚未完善