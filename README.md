# 腾讯云直播服务

本服务基于Model Context Protocol（MCP）实现提供腾讯云直播API调用Tools的MCP Server，提供腾讯云直播域名管理、拉流转推、直播流管理和流管理等相关接口。

## 简介

本项目主要为大语言模型提供通过自然语言调用腾讯云直播API，管理腾讯云直播服务的能力。

## 依赖环境

- Python 1.12
- 腾讯云API密钥（ [腾讯云控制台申请](https://console.cloud.tencent.com/cam/capi) ）
    - TENCENTCLOUD_SECRET_ID
    - TENCENTCLOUD_SECRET_KEY
- 腾讯云SDK-Python

## 获取安装

### 本地运行Server

``` 
# 配置环境变量
export TENCENTCLOUD_SECRET_ID="您的腾讯云SecretId"
export TENCENTCLOUD_SECRET_KEY="您的腾讯云SecretId"

# 启动服务
uv run src/server.py
```
### Cursor中使用
#### 通过发布在PyPI的包使用

先决条件：[uv包管理器](https://docs.astral.sh/uv/getting-started/installation/)

需要注意的是，你必须安装好了uvx才可以进行Cursor的配置。

``` 
{
  "mcpServers": {
    "tencentcloud-live-mcp-server": {
      "command": "uvx",
      "args": [
        "tencentcloud-live-mcp-server"
      ],
      "env": {
        "TENCENTCLOUD_SECRET_ID": "------> 替换成你自己的 ID <------",
        "TENCENTCLOUD_SECRET_KEY": "------> 替换成你自己的 KEY <------"
      }
    }
  }
}
```
完成后重启Cursor完成配置。

## 功能
- Tools
    - 域名管理
        - 添加域名
        - 删除域名
        - 启用域名
        - 禁用域名
        - 查询域名信息
        - 查询域名列表
    - 拉流转推
        - 删除直播拉流任务
        - 查询直播拉流任务
        - 创建直播拉流任务
        - 更新直播拉流任务
    - 直播流管理
        - 查询流状态
        - 查询直播中的流
    - 流管理
        - 断开直播流
        - 恢复直播流
        - 禁播直播流
        - 查询推断流事件
        - 设置延时直播
        - 取消直播延时
    - 转码模版
        - 创建转码模版
        - 删除转码模版
        - 创建转码规则
        - 删除转码规则

## 使用场景

### MCP 如何赋能 AI Agent

- MCP 提供统一的 protocol + params 语义化接口。
- 签名、证书管理、API版本兼容等问题由 MCP Server 处理，AI Agent 只需关注业务逻辑。
- 标准化错误处理
- 降低开发门槛

### MCP AI Agent 增强场景

- AI 直播运维管家：自动化故障检测与自愈
  - 监控探测：Agent 周期性调用 describe_live_stream_online_list 和 describe_live_stream_state，主动监测数百/上千直播流的状态（在线/卡顿/断流）。
  - 技术故障：Agent 自动调用 resume_live_stream 尝试恢复推流；若多次失败，自动调用 create_live_pull_stream_task 从备用源拉流恢复直播。
- AI 直播导播助手：动态内容编排
  - 理解意图：接收人类指令或结合上下文自动触发
  - 智能优化：分析观众地域分布，自动创建不同区域优化的 TranscodeRule 降低延迟。
- AI 直播合规官：实时内容风控
  - 风控引擎：将直播流实时传输给 AI (视频/音频/文本分析)。
  - 分级干预：根据风险等级，通过 MCP 秒级自动执行 delete_live_pull_stream_task 或 drop_live_stream。
- AI 主播经纪人：个性化频道管理
  - 周期管理：新主播签约、开户；老主播解约。
  - 内容增值：回看精彩片段自动调用 create_live_pull_stream_task 将指定回放切片推流生成精彩集锦。
