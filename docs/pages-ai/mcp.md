---
    outline: deep
---

# mcp

## Spec Workflow MCP 使用指南

一份完整的 Spec Workflow MCP 使用教程，从安装配置到实际项目开发的全流程指南。

## 目录

- [快速开始](#快速开始)
- [安装与配置](#安装与配置)
- [基础使用](#基础使用)
- [完整项目流程](#完整项目流程)
- [配置文件详解](#配置文件详解)
- [进阶使用](#进阶使用)
- [团队协作](#团队协作)
- [故障排除](#故障排除)
- [最佳实践](#最佳实践)

---

## 快速开始

### 30秒快速体验

```bash
# 1. 进入项目目录
cd your-project

# 2. 启动 MCP 服务器（带仪表板）
npx -y @pimzino/spec-workflow-mcp@latest $(pwd) --dashboard

# 3. 在新终端启动 Claude CLI
claude

# 4. 在 Claude CLI 中创建第一个规范
"Create a spec for user authentication system"
```

---

## 安装与配置

### 方法一：Claude CLI 集成（推荐）

#### 1. 添加到 Claude CLI
```bash
# 添加 MCP 服务器
claude mcp add spec-workflow npx -y @pimzino/spec-workflow-mcp@latest -- /path/to/your/project
```

#### 2. 启动 Claude CLI
```bash
claude
```

#### 3. 验证安装
```
# 在 Claude CLI 中测试
"List available MCP tools"
```

### 方法二：手动配置

#### 1. 创建配置文件
编辑 Claude CLI 配置文件 (`~/.config/claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "spec-workflow": {
      "command": "npx",
      "args": [
        "-y",
        "@pimzino/spec-workflow-mcp@latest",
        "/absolute/path/to/your/project"
      ]
    }
  }
}
```

#### 2. 重启 Claude CLI
```bash
claude restart
```

### 方法三：独立运行（开发调试）

#### 1. 创建配置文件
```bash
# 创建 config.toml
touch config.toml
```

#### 2. 配置文件内容
```toml
[project]
name = "my-project"
path = "/path/to/project"
description = "My awesome project"

[server]
host = "localhost"
port = 3000
auto_open_browser = true

[dashboard]
enabled = true
theme = "dark"
language = "zh-CN"

[workflow]
auto_approve = false
require_review = true
```

#### 3. 启动服务器
```bash
npx -y @pimzino/spec-workflow-mcp@latest --config ./config.toml --dashboard
```

---

## 基础使用

### 1. 创建项目结构

#### 初始化指导文档
```
# 在 Claude CLI 中执行
"Create steering documents for this project - it's a web application with user management and content creation features"
```

**生成的文件结构**：
```
your-project/
├── .spec-workflow/
│   ├── steering/
│   │   ├── product.md      # 产品愿景
│   │   ├── tech.md         # 技术决策
│   │   └── structure.md    # 项目结构
│   ├── specs/
│   └── approval/
```

### 2. 创建功能规范

#### 简单创建
```
"Create a spec for user registration"
```

#### 详细创建
```
"Create a spec called user-auth with the following features:
- User registration with email verification
- Login with email/password
- Password reset functionality
- Two-factor authentication
- Session management
- OAuth integration (Google, GitHub)"
```

### 3. 查看项目状态

#### 列出所有规范
```
"List my specs"
```

#### 查看特定规范进度
```
"Show me the progress of user-auth spec"
```

#### 查看任务详情
```
"Show me task 1.2 from user-auth spec"
```

### 4. 执行开发任务

#### 执行特定任务
```
"Execute task 1.1 from user-auth spec"
```

#### 继续下一个任务
```
"Continue with the next task in user-auth"
```

#### 跳转到特定任务
```
"I want to work on the frontend. Execute task 3.1 from user-auth"
```

---

## 完整项目流程

### 场景：开发一个博客系统的评论功能

#### 第1步：项目初始化

```bash
# 1. 创建项目目录
mkdir blog-project
cd blog-project

# 2. 启动 MCP 服务器
npx -y @pimzino/spec-workflow-mcp@latest $(pwd) --dashboard

# 3. 在新终端启动 Claude CLI
claude
```

#### 第2步：创建指导文档

```
"Create steering documents for this project. It's a blog platform with:
- User-generated content
- Comment system
- User management
- Content moderation
- Real-time features

Tech stack: React, Node.js, MongoDB, Redis"
```

**生成结果**：
- `product.md` - 博客平台产品愿景
- `tech.md` - 技术栈和架构决策
- `structure.md` - 项目组织结构

#### 第3步：创建评论系统规范

```
"Create a spec called comment-system with these features:
- Users can comment on blog posts
- Nested replies (max 3 levels)
- Comment editing and deletion
- Admin moderation
- Real-time comment updates
- Like/dislike functionality
- Spam detection"
```

#### 第4步：需求确认阶段

**AI 生成 `requirements.md`**：
```markdown
# 评论系统需求规范

## 用户故事
- 作为读者，我想对博客文章发表评论
- 作为读者，我想回复其他人的评论
- 作为作者，我想管理我的评论
...

## 功能需求
1. 评论发布功能
2. 嵌套回复系统
3. 评论管理功能
...
```

**确认需求**：
```
"The requirements look comprehensive. Proceed to design phase."
```

#### 第5步：技术设计阶段

**AI 生成 `design.md`**：
```markdown
# 评论系统技术设计

## 系统架构
- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: MongoDB
- Cache: Redis
- Real-time: Socket.io

## 数据模型
```javascript
// Comment Schema
{
  _id: ObjectId,
  postId: ObjectId,
  authorId: ObjectId,
  content: String,
  parentId: ObjectId,
  level: Number,
  likes: Number,
  createdAt: Date,
  updatedAt: Date,
  isModerated: Boolean
}
```

**API 设计**
- POST /api/posts/:id/comments
- GET /api/posts/:id/comments
- PUT /api/comments/:id
- DELETE /api/comments/:id
```

**确认设计**：
```
"Great design! The nested comment structure looks good. Create the implementation tasks."
```

#### 第6步：任务分解阶段

**AI 生成 `tasks.md`**：
```markdown
# 评论系统实施任务

## 1. 数据库设计 (4小时)
### 1.1 设计 Comment 数据模型
- 创建 MongoDB Schema
- 设置索引优化
- 数据验证规则

### 1.2 设置数据库连接
- MongoDB 连接配置
- 数据库迁移脚本

## 2. 后端 API 开发 (12小时)
### 2.1 评论 CRUD API
- POST /api/posts/:id/comments
- 输入验证和清理
- 权限检查

### 2.2 嵌套评论查询
- GET /api/posts/:id/comments
- 构建评论树结构
- 分页和排序

### 2.3 评论管理 API
- 编辑评论 API
- 删除评论 API
- 审核功能 API

## 3. 前端组件开发 (16小时)
### 3.1 评论显示组件
- CommentList 组件
- CommentItem 组件
- 嵌套渲染逻辑

### 3.2 评论交互组件
- CommentForm 组件
- ReplyForm 组件
- EditComment 组件

### 3.3 评论管理界面
- 管理员审核界面
- 用户评论管理页面

## 4. 实时功能 (8小时)
### 4.1 WebSocket 集成
- Socket.io 服务器设置
- 实时评论更新
- 在线用户显示

## 5. 测试与部署 (6小时)
### 5.1 单元测试
### 5.2 集成测试
### 5.3 部署配置
```

**确认任务**：
```
"Perfect task breakdown! Let's start implementing. Begin with task 1.1"
```

#### 第7步：实施开发

##### 执行任务 1.1
```
"Execute task 1.1 from comment-system spec"
```

**AI 提供实现代码**：
```javascript
// models/Comment.js
const mongoose = require('mongoose');

const commentSchema = new mongoose.Schema({
  postId: {
    type: mongoose.Schema.Types.ObjectId,
    required: true,
    ref: 'Post',
    index: true
  },
  authorId: {
    type: mongoose.Schema.Types.ObjectId,
    required: true,
    ref: 'User'
  },
  content: {
    type: String,
    required: true,
    maxlength: 2000,
    trim: true
  },
  parentId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Comment',
    default: null
  },
  level: {
    type: Number,
    default: 0,
    min: 0,
    max: 2
  },
  likes: {
    type: Number,
    default: 0
  },
  isModerated: {
    type: Boolean,
    default: false
  }
}, {
  timestamps: true
});

// 索引优化
commentSchema.index({ postId: 1, createdAt: -1 });
commentSchema.index({ parentId: 1 });
commentSchema.index({ authorId: 1 });

// 虚拟字段：回复数量
commentSchema.virtual('replyCount', {
  ref: 'Comment',
  localField: '_id',
  foreignField: 'parentId',
  count: true
});

module.exports = mongoose.model('Comment', commentSchema);
```

##### 继续后续任务
```
"Task 1.1 completed successfully. Continue with task 1.2"
```

##### 检查进度
```
"Show me the current progress of comment-system spec"
```

**显示进度**：
```
📊 评论系统开发进度

✅ 任务 1.1 - Comment 数据模型设计 (已完成)
🔄 任务 1.2 - 数据库连接设置 (进行中)
⏳ 任务 2.1 - 评论 CRUD API (待开始)
⏳ 任务 2.2 - 嵌套评论查询 (待开始)
...

总体进度: 12% (2/17 任务)
预计完成时间: 还需 42 小时
```

#### 第8步：使用 Web 仪表板

**访问仪表板**：
- 打开浏览器访问 `http://localhost:3000`
- 实时查看项目进度
- 浏览所有文档
- 管理任务状态

**仪表板功能**：
- 📊 进度可视化
- 📋 任务列表管理
- 📄 文档在线查看
- ✅ 审批工作流程
- 🔄 实时更新通知

---

## 配置文件详解

### 基础配置 (config.toml)

```toml
# 项目基础信息
[project]
name = "my-awesome-project"           # 项目名称
path = "/absolute/path/to/project"    # 项目绝对路径
description = "Project description"    # 项目描述
version = "1.0.0"                     # 版本号

# 服务器配置
[server]
host = "localhost"                    # 服务器地址
port = 3000                          # 端口号
auto_open_browser = true             # 自动打开浏览器
cors_enabled = true                  # 启用 CORS

# Web 仪表板配置
[dashboard]
enabled = true                       # 启用仪表板
theme = "dark"                       # 主题: light/dark/auto
language = "zh-CN"                   # 界面语言
auto_refresh = 5000                  # 自动刷新间隔(毫秒)
show_tasks_progress = true           # 显示任务进度条
enable_notifications = true          # 启用通知

# 工作流配置
[workflow]
auto_approve = false                 # 自动审批
require_review = true                # 需要人工审核
default_assignee = "developer"       # 默认任务分配者
max_nesting_level = 3               # 最大嵌套层级

# 文件监控
[file_watch]
enabled = true                       # 启用文件监控
ignore_patterns = [                  # 忽略的文件模式
  "node_modules",
  ".git",
  "dist",
  "build",
  "*.log"
]
auto_reload = true                   # 自动重载

# 模板配置
[templates]
requirements_template = "templates/requirements.md"
design_template = "templates/design.md"
tasks_template = "templates/tasks.md"
use_custom_templates = false

# 通知配置
[notifications]
enabled = true                       # 启用通知
slack_webhook = ""                   # Slack Webhook URL
email_enabled = false                # 邮件通知
discord_webhook = ""                 # Discord Webhook URL

# 集成配置
[integrations]
vscode_extension = true              # VSCode 扩展支持
github_integration = false          # GitHub 集成
jira_integration = false             # Jira 集成
trello_integration = false           # Trello 集成

# 性能优化
[performance]
cache_enabled = true                 # 启用缓存
cache_ttl = 3600                    # 缓存过期时间(秒)
max_concurrent_tasks = 5             # 最大并发任务数
request_timeout = 30000              # 请求超时时间(毫秒)

# 安全配置
[security]
enable_auth = false                  # 启用身份验证
api_key = ""                        # API 密钥
allowed_origins = ["*"]              # 允许的域名
rate_limit = 100                    # 速率限制(请求/分钟)

# 日志配置
[logging]
level = "info"                      # 日志级别: debug/info/warn/error
file_enabled = true                 # 启用文件日志
file_path = "logs/spec-workflow.log" # 日志文件路径
console_enabled = true              # 启用控制台日志
```

### 团队协作配置示例

```toml
# 团队协作专用配置
[project]
name = "team-project"
path = "/workspace/team-project"
description = "Team collaboration project"

[workflow]
auto_approve = false                 # 禁用自动审批
require_review = true                # 必须人工审核
reviewers = ["lead-dev", "architect", "pm"]
approval_threshold = 2               # 需要2人审批通过

[notifications]
enabled = true
slack_webhook = "https://hooks.slack.com/services/xxx"
email_enabled = true
email_list = ["team@company.com"]

[integrations]
github_integration = true
jira_integration = true
confluence_integration = true
```

---

## 进阶使用

### 1. 自定义模板

#### 创建自定义需求模板
```markdown
# templates/custom-requirements.md

# {{spec_name}} - 需求规范

## 项目背景
{{project_background}}

## 业务目标
{{business_objectives}}

## 用户故事
{{user_stories}}

## 功能需求
{{functional_requirements}}

## 非功能需求
{{non_functional_requirements}}

## 验收标准
{{acceptance_criteria}}

## 风险评估
{{risk_assessment}}
```

#### 在配置中使用
```toml
[templates]
requirements_template = "templates/custom-requirements.md"
use_custom_templates = true
```

### 2. 批量操作

#### 批量创建多个规范
```
"Create specs for the following features:
1. user-authentication - login, registration, password reset
2. content-management - create, edit, delete posts
3. comment-system - nested comments, moderation
4. notification-system - real-time notifications, email alerts"
```

#### 批量执行任务
```
"Execute all setup tasks for user-authentication spec"
```

### 3. 规范依赖管理

#### 创建有依赖关系的规范
```
"Create a spec for payment-system that depends on user-authentication spec being completed"
```

#### 检查依赖关系
```
"Show me the dependency graph for all specs"
```

### 4. 高级查询和报告

#### 生成项目报告
```
"Generate a project status report including:
- All specs and their progress
- Overdue tasks
- Resource allocation
- Timeline estimation"
```

#### 查询特定状态的任务
```
"Show me all blocked tasks across all specs"
"List all tasks assigned to backend development"
```

---

## 团队协作

### 1. 角色和权限

#### 配置团队角色
```toml
[team]
roles = [
  { name = "developer", permissions = ["create_spec", "execute_task", "update_progress"] },
  { name = "reviewer", permissions = ["approve_spec", "reject_spec", "comment"] },
  { name = "manager", permissions = ["all"] }
]
```

#### 分配任务责任人
```
"Assign task 2.1 from user-auth spec to @john-doe"
"Set reviewer for user-auth design document to @jane-smith"
```

### 2. 审批工作流程

#### 提交审批
```
"Submit user-auth requirements document for review"
```

#### 审批操作
```
# 审批通过
"Approve user-auth requirements with comment: 'Looks good, proceed to design'"

# 请求修改
"Request changes on user-auth design: 'Please add error handling details'"
```

#### 查看审批状态
```
"Show approval status for all documents in user-auth spec"
```

### 3. 协作最佳实践

#### 每日站会报告
```
"Generate daily standup report for the team showing:
- Yesterday's completed tasks
- Today's planned tasks
- Any blockers or issues"
```

#### 里程碑跟踪
```
"Set milestone 'MVP Release' for user-auth, comment-system, and content-management specs"
"Show progress towards MVP Release milestone"
```

---

## 故障排除

### 常见问题及解决方案

#### 1. 端口被占用
```bash
# 错误信息
Error: listen EADDRINUSE: address already in use :::3000

# 解决方案
# 方法1: 使用不同端口
npx -y @pimzino/spec-workflow-mcp@latest --config ./config.toml --dashboard --port 3001

# 方法2: 杀死占用端口的进程
# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# macOS/Linux
lsof -i :3000
kill -9 <PID>
```

#### 2. 配置文件错误
```bash
# 错误信息
Error: Invalid configuration file

# 解决方案
# 验证 TOML 语法
npx -g install @iarna/toml-cli
toml-cli verify config.toml

# 使用最小配置
cat > config.toml << EOF
[project]
name = "test-project"
path = "$(pwd)"
EOF
```

#### 3. MCP 连接失败
```bash
# 错误信息
MCP connection failed

# 解决方案
# 检查 Claude CLI 配置
claude config show

# 重新添加 MCP 服务器
claude mcp remove spec-workflow
claude mcp add spec-workflow npx -y @pimzino/spec-workflow-mcp@latest -- $(pwd)

# 重启 Claude CLI
claude restart
```

#### 4. 文件权限问题
```bash
# 错误信息
Permission denied: cannot write to project directory

# 解决方案
# 修改目录权限
chmod -R 755 /path/to/project

# 或者使用当前用户权限
sudo chown -R $(whoami):$(whoami) /path/to/project
```

### 调试模式

#### 启用详细日志
```bash
npx -y @pimzino/spec-workflow-mcp@latest \
  --config ./config.toml \
  --dashboard \
  --verbose \
  --log-level debug
```

#### 查看日志文件
```bash
# 实时查看日志
tail -f logs/spec-workflow.log

# 搜索错误信息
grep -i error logs/spec-workflow.log
```

---

## 最佳实践

### 1. 项目组织

#### 推荐的项目结构
```
your-project/
├── .spec-workflow/
│   ├── steering/           # 指导文档
│   │   ├── product.md
│   │   ├── tech.md
│   │   └── structure.md
│   ├── specs/              # 功能规范
│   │   ├── user-auth/
│   │   ├── content-mgmt/
│   │   └── notification/
│   ├── approval/           # 审批记录
│   └── templates/          # 自定义模板
├── src/                    # 源代码
├── docs/                   # 其他文档
├── tests/                  # 测试文件
└── config.toml             # MCP 配置
```

### 2. 规范命名约定

#### 规范名称
```
# 好的命名
user-authentication
content-management  
payment-processing
notification-system

# 避免的命名
userAuth
content_mgmt
payments
notifications
```

#### 任务描述
```
# 好的任务描述
"Create user registration API with email verification"
"Implement password reset flow with secure token generation"

# 避免的任务描述
"Create API"
"Fix login"
```

### 3. 文档质量

#### 需求文档最佳实践
- 使用用户故事格式："作为...，我想要...，以便..."
- 包含验收标准
- 定义明确的功能边界
- 考虑错误处理场景

#### 设计文档最佳实践
- 包含架构图（文字描述）
- 定义清晰的 API 接口
- 说明数据流和状态管理
- 考虑性能和安全因素

#### 任务分解最佳实践
- 每个任务不超过 1 天工作量
- 任务之间有明确的依赖关系
- 包含验证和测试步骤
- 提供预估工作时间

### 4. 版本管理

#### 与 Git 集成
```bash
# 提交规范文档
git add .spec-workflow/
git commit -m "feat: add user authentication specification"

# 标记里程碑
git tag -a v1.0.0-spec -m "Complete specification for MVP features"
```

#### 变更管理
```
"Update user-auth requirements to include OAuth2 integration"
"Archive completed payment-system spec"
```

### 5. 质量保证

#### 代码审查集成
```
"Generate code review checklist for user-auth implementation"
"Validate that all tasks in user-auth spec have been implemented"
```

#### 测试策略
```
"Create test plan for user-auth spec covering:
- Unit tests for all API endpoints
- Integration tests for auth flow
- E2E tests for user journey"
```

### 6. 性能优化

#### 大型项目管理
```toml
# 配置优化
[performance]
cache_enabled = true
max_concurrent_tasks = 10
batch_size = 50

[file_watch]
ignore_patterns = [
  "node_modules",
  "dist",
  "build",
  "*.log",
  "coverage"
]
```

#### 资源监控
```
"Show system resource usage and performance metrics"
"Optimize spec loading for large projects"
```

---

## 总结

Spec Workflow MCP 提供了一套完整的、结构化的软件开发工作流程。通过本指南，您应该能够：

### ✅ 基础能力
- 安装和配置 MCP 服务器
- 创建和管理功能规范
- 执行开发任务
- 使用 Web 仪表板

### ✅ 进阶能力
- 自定义配置和模板
- 团队协作和审批流程
- 批量操作和高级查询
- 故障排除和性能优化

### ✅ 专业实践
- 项目组织最佳实践
- 文档质量标准
- 版本管理集成
- 质量保证流程

### 下一步建议

1. **开始小项目**：在一个简单功能上练习完整流程
2. **建立团队标准**：制定适合团队的配置和流程
3. **持续改进**：根据实际使用情况优化工作流程
4. **分享经验**：在团队内推广结构化开发方法
