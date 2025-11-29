# Databricks Keep-Alive Workers 部署说明.

这个 Worker 脚本用于监控和自动重启 Databricks App，确保它们保持运行状态。

Databricks部署节点视频教程：https://youtu.be/r35kK77PlLg

## 部署指南

### 1. 准备工作

在部署之前，你需要获取以下信息：

1. **Databricks 工作区 Host**: 例如 `https://abc-123456789.cloud.databricks.com`
2. **Databricks 访问 Token**: 用于 API 访问的个人访问令牌，点击右上角用户设置-选择"Developer" -> "Access Tokens"生成新的访问令牌
3. **(可选) Telegram BOT Token**: 用于发送通知的 Telegram Bot 令牌
4. **(可选) Telegram Chat ID**: 接收通知的聊天caht id

### 2. 部署
1. 登录你的cloudflare，创建一个新的workers，名称随意，编辑代码，删除原示例代码
2. 打开此项目的_worker.js文件，复制代码粘贴到workers代码框中，部署
3. 设置cron触发器，3分钟或5分钟

### 3. 配置变量

有两种方式配置必要的参数：

#### 方法一：修改代码中的默认配置（推荐用于测试）

在 [_worker.js](file:///c%3A/Users/Mr.wang/Desktop/Databricks-keepalive-workers-main/_worker.js) 文件中找到 `DEFAULT_CONFIG` 对象并修改：

#### 方法二：使用环境变量（推荐用于生产环境）

在部署平台设置以下环境变量：

- `DATABRICKS_HOST`: Databricks 工作区地址
- `DATABRICKS_TOKEN`: Databricks 访问令牌
- `CHAT_ID`: Telegram 聊天 ID（可选）
- `BOT_TOKEN`: Telegram Bot 令牌（可选）

### 3. 部署到 Cloudflare Workers

## 使用说明

部署完成后，你可以通过以下方式使用：

### Web 管理界面

访问 Worker 的根路径 `/` 可以打开 Web 管理界面，提供以下功能：

### API 端点

- `GET /status` - 获取当前所有 Apps 的状态
- `GET /check` - 检查并自动启动停止的 Apps
- `POST /start` - 手动启动所有停止的 Apps
- `GET /config` - 查看当前配置信息
- `POST /test-notification` - 测试 Telegram 通知

## Telegram 通知配置

要启用 Telegram 通知，需要：

1. 创建一个 Telegram Bot:
   - 在 Telegram 中搜索 @BotFather
   - 发送 `/newbot` 命令
   - 按照指示创建新 Bot 并获取令牌

2. 获取 Chat ID:
   - 访问 `https://t.me/laowang_serv00_bot`
   - 发送 `/start` 命令获取你的 chat_id

3. 在配置中设置 BOT_TOKEN 和 CHAT_ID

## 故障排除

### Apps 未自动启动

1. 检查 DATABRICKS_HOST 和 DATABRICKS_TOKEN 是否正确配置
2. 确认 Token 具有足够权限
3. 检查 Worker 日志以获取更多信息

### Telegram 通知未发送

1. 确认 BOT_TOKEN 和 CHAT_ID 已正确配置
2. 验证 Bot 是否有向指定 Chat ID 发送消息的权限
3. 使用 `/test-notification` 端点测试通知功能

### 定时任务未执行

1. 检查 Cron Trigger 配置是否正确
2. 确认 Worker 已正确部署
3. 查看 Worker 日志确认定时任务是否被触发
