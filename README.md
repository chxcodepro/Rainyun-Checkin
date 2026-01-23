# Rainyun-Qiandao

https://github.com/SerendipityR-2022/Rainyun-Qiandao 的 docker 版，在 arm 和 amd64 平台均可运行

## 🔧 二改版本说明

本版本基于原项目进行了稳定性和优化，修复了多个潜在的运行时错误：

### ✅ 已修复的问题

| 问题 | 严重级别 | 说明 |
|------|----------|------|
| 验证码识别空字典漏洞 | High | 增加了结果完整性验证，避免空字典导致的后续错误 |
| 图片下载失败未验证 | High | 添加下载结果验证和网络异常处理 |
| 图片读取失败未检查 | High | 增加 cv2.imread 返回值 None 检查 |
| 正则解析缺少空值保护 | Medium | 为所有样式解析函数添加空值和格式验证 |
| 异常捕获范围过宽 | Medium | 精确化异常处理，仅重试可恢复错误，真实 bug 直接抛出 |
| 通知模块条件逻辑错误 | Low | 修复企业微信应用推送的参数验证逻辑 |

### 🎯 核心改进

- **自定义异常类**：引入 `CaptchaRetryableError` 明确区分可重试错误和程序 bug
- **防御性编程**：所有外部依赖（网络、文件、正则）都有完整的验证和异常处理
- **容器稳定性**：修复 Selenium 依赖安装问题，确保 Docker 环境下正常运行

### 📌 测试状态

✅ 已在 Docker 环境下完整测试通过（验证码识别、登录、签到、通知推送）


## 食用方法

### 快速开始
```bash
# 编辑 .env 文件，填入你的雨云账号
# RAINYUN_USER=你的用户名
# RAINYUN_PWD=你的密码

# 构建并运行
docker-compose up --build
```

### 环境变量说明
| 变量名 | 必填 | 默认值 | 说明 |
|--------|------|--------|------|
| RAINYUN_USER | ✅ | - | 雨云用户名 |
| RAINYUN_PWD | ✅ | - | 雨云密码 |
| TIMEOUT | ❌ | 15 | 连接超时等待(秒) |
| MAX_DELAY | ❌ | 90 | 最大随机等待延时(分钟) |
| DEBUG | ❌ | false | 调试模式 |

### 定时任务 (Cron)
```bash
# 每天早上 8 点执行签到
0 8 * * * docker compose -f /path/to/docker-compose.yml run --rm rainyun-qiandao
```