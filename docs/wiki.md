# WIKI

## 小红书 MCP 项目概述

这是一个基于 Model Context Protocol (MCP) 的小红书自动化工具，通过浏览器自动化技术控制小红书网页版，支持 AI 助手（如 Claude、Cursor 等）调用。

### 核心能力

- 登录认证（二维码扫描）
- 发布图文和视频笔记
- 搜索笔记、获取 Feed 列表
- 互动功能（点赞、收藏、评论、回复）
- 获取用户主页和资料
- MCP 协议支持

---

## 技术架构

| 组件 | 技术选型 |
|------|---------|
| 浏览器自动化 | `go-rod` 控制 Chrome/Chromium |
| HTTP 服务器 | Gin 框架 |
| MCP 协议 | `github.com/modelcontextprotocol/go-sdk` |
| 无头浏览器 | 支持 headless 模式运行 |
| 日志 | `sirupsen/logrus` |

---

## 目录结构

```
xiaohongshu-mcp/
├── main.go                 # 程序入口，解析命令行参数
├── app_server.go           # 应用服务器，管理 HTTP 和 MCP 服务
├── service.go              # 小红书业务服务，封装核心功能
├── mcp_server.go           # MCP 协议服务器初始化
├── mcp_handlers.go         # MCP 工具处理器（实现 MCP 工具）
├── handlers_api.go         # HTTP API 处理器
├── routes.go               # 路由配置
├── middleware.go           # 中间件（CORS、错误处理）
├── types.go                # 通用数据类型定义
│
├── browser/
│   └── browser.go          # 浏览器管理，创建和配置无头浏览器
│
├── xiaohongshu/            # 小红书核心业务逻辑
│   ├── login.go           # 登录功能（二维码、状态检查）
│   ├── publish.go         # 发布图文笔记
│   ├── publish_video.go   # 发布视频笔记
│   ├── search.go          # 搜索笔记
│   ├── feeds.go           # 获取 Feed 列表
│   ├── feed_detail.go     # Feed 详情和评论
│   ├── comment_feed.go     # 评论操作
│   ├── like_favorite.go    # 点赞和收藏
│   ├── user_profile.go     # 用户资料获取
│   ├── navigate.go         # 页面导航
│   └── types.go           # 小红书相关数据结构
│
├── cookies/               # Cookie 管理
│   └── cookies.go         # 持久化登录状态
│
├── configs/               # 配置管理
│   ├── browser.go         # 浏览器配置
│   ├── image.go          # 图片配置
│   └── username.go       # 用户名配置
│
├── pkg/
│   ├── downloader/        # 图片下载器
│   │   ├── images.go     # 图片下载逻辑
│   │   └── processor.go  # 图片处理
│   └── xhsutil/          # 工具函数
│       └── title.go      # 标题处理工具
│
├── errors/
│   └── errors.go         # 自定义错误类型
│
├── cmd/login/             # 独立登录工具
│   └── main.go
│
└── skills/               # 发布辅助脚本（Python）
    └── post-to-xhs/      # 小红书发布工作流
```

---

## 核心模块详解

### 1. 浏览器管理 (browser/browser.go)

负责创建和管理无头浏览器实例：

```go
func NewBrowser(headless bool, options ...Option) *headless_browser.Browser
```

- 支持自定义 Chrome 路径
- 支持代理设置（环境变量 `XHS_PROXY`）
- 自动加载 cookies 维持登录状态

### 2. 登录服务 (xiaohongshu/login.go)

提供二维码扫描登录能力：

| 方法 | 功能 |
|------|------|
| `FetchQrcodeImage()` | 获取登录二维码 Base64 图片 |
| `CheckLoginStatus()` | 检查当前登录状态 |
| `WaitForLogin()` | 阻塞等待用户扫码完成 |

登录状态通过 cookies 文件持久化存储在 `cookies/` 目录。

### 3. 发布服务 (xiaohongshu/publish.go)

发布图文笔记到小红书：

```go
type PublishImageContent struct {
    Title        string     // 标题（最多20个中文字或英文单词）
    Content      string     // 正文内容
    Tags         []string   // 话题标签（最多10个）
    ImagePaths   []string   // 图片路径（本地路径或HTTP链接）
    ScheduleTime *time.Time // 定时发布时间（可选）
    IsOriginal   bool       // 是否声明原创
    Visibility   string     // 可见范围
    Products     []string   // 带货商品关键词
}
```

### 4. MCP 工具 (mcp_handlers.go)

通过 MCP 协议暴露的工具列表：

| 工具名称 | 功能 |
|---------|------|
| `publish_content` | 发布图文笔记 |
| `publish_video` | 发布视频笔记 |
| `search_feeds` | 搜索笔记 |
| `get_feed_detail` | 获取笔记详情 |
| `get_user_profile` | 获取用户资料 |
| `post_comment` | 发表评论 |
| `reply_comment` | 回复评论 |
| `like_feed` | 点赞/取消点赞 |
| `favorite_feed` | 收藏/取消收藏 |

---

## API 接口

服务启动后提供以下 HTTP API：

### 登录相关

| 方法 | 路径 | 功能 |
|------|------|------|
| GET | `/api/v1/login/status` | 检查登录状态 |
| GET | `/api/v1/login/qrcode` | 获取登录二维码 |
| DELETE | `/api/v1/login/cookies` | 删除 cookies（重置登录） |

### 发布相关

| 方法 | 路径 | 功能 |
|------|------|------|
| POST | `/api/v1/publish` | 发布图文笔记 |
| POST | `/api/v1/publish_video` | 发布视频笔记 |

### 内容获取

| 方法 | 路径 | 功能 |
|------|------|------|
| GET | `/api/v1/feeds/list` | 获取 Feed 列表 |
| GET/POST | `/api/v1/feeds/search` | 搜索笔记 |
| POST | `/api/v1/feeds/detail` | 获取 Feed 详情 |
| POST | `/api/v1/user/profile` | 获取用户资料 |
| GET | `/api/v1/user/me` | 获取当前用户信息 |

### 互动相关

| 方法 | 路径 | 功能 |
|------|------|------|
| POST | `/api/v1/feeds/comment` | 发表评论 |
| POST | `/api/v1/feeds/comment/reply` | 回复评论 |

### MCP 协议

| 方法 | 路径 | 功能 |
|------|------|------|
| ANY | `/mcp` | MCP HTTP 端点 |
| ANY | `/mcp/*path` | MCP 路径端点 |

### 健康检查

| 方法 | 路径 | 功能 |
|------|------|------|
| GET | `/health` | 服务健康检查 |

---

## 运行方式

### 命令行参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `-headless` | `true` | 是否无头模式运行 |
| `-bin` | 环境变量 `ROD_BROWSER_BIN` | Chrome 浏览器路径 |
| `-port` | `:18060` | HTTP 服务端口 |

### 示例

```bash
# 基本运行（默认 headless 模式）
go run .

# 非无头模式运行（可看到浏览器）
go run . -headless=false

# 指定端口
go run . -port=:8080

# 指定 Chrome 路径
go run . -bin=/usr/bin/google-chrome

# 使用代理
export XHS_PROXY="http://username:password@proxy.com:8080"
go run .
```

### Docker 运行

```bash
# 构建镜像
docker build -t xiaohongshu-mcp .

# 运行容器
docker run -d -p 18060:18060 \
  -e ROD_BROWSER_BIN=/usr/bin/google-chrome \
  xiaohongshu-mcp
```

---

## 数据结构

### Feed (笔记)

```go
type Feed struct {
    XsecToken string   // 访问令牌
    ID        string   // 笔记ID
    ModelType string   // 类型：video/image
    NoteCard  NoteCard // 笔记内容
}
```

### NoteCard (笔记内容)

```go
type NoteCard struct {
    Type         string       // 类型
    DisplayTitle string       // 显示标题
    User         User         // 作者信息
    InteractInfo InteractInfo // 互动数据
    Cover        Cover        // 封面图
    Video        *Video       // 视频信息（视频笔记）
}
```

### User (用户)

```go
type User struct {
    UserID   string // 用户ID
    Nickname string // 昵称
    Avatar   string // 头像URL
}
```

---

## MCP 集成

该项目可被支持 MCP 协议的 AI 助手调用，配置示例：

### Cursor

在 `Settings > MCP` 中添加：

```json
{
  "mcpServers": {
    "xiaohongshu": {
      "command": "go",
      "args": ["run", "."],
      "cwd": "/path/to/xiaohongshu-mcp"
    }
  }
}
```

### Claude Desktop

在 `~/Library/Application Support/Claude/claude_desktop_config.json` 中配置：

```json
{
  "mcpServers": {
    "xiaohongshu": {
      "command": "go",
      "args": ["run", "."],
      "cwd": "/path/to/xiaohongshu-mcp"
    }
  }
}
```

### n8n 工作流

提供 n8n 工作流模板，可自动化小红书内容发布。

---

## 开发指南

### 本地开发规范

1. 修改完成后格式化 Go 源码文件
2. 测试产生的临时文件若无必要则删除
3. 所有功能变更使用分支开发
4. 需要本地 review 和远程 PR review

### 代码审查重点

PR 代码中如果出现大量 JS 注入行为，检查是否必须，若可以用 Go 的 go-rod 替代则优先用 go-rod。

### 相关文档

- [API 文档](./docs/API.md)
- [Windows 部署指南](./docs/windows_guide.md)
- [示例工作流](./examples/)

---

## 环境变量

| 变量名 | 说明 |
|--------|------|
| `ROD_BROWSER_BIN` | Chrome 浏览器路径 |
| `XHS_PROXY` | 代理服务器地址 |

---

## 常见问题

### 登录失效

删除 `cookies/` 目录下的 cookies 文件，重新扫码登录。

### 浏览器无法启动

1. 确保已安装 Chrome/Chromium
2. 使用 `-bin` 参数指定浏览器路径
3. 检查 `ROD_BROWSER_BIN` 环境变量

### 发布失败

1. 检查图片路径是否有效
2. 确认网络连接正常
3. 查看日志中的具体错误信息
