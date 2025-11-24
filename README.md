# KuaiT 自动下单系统

<div align="center">

[![License](https://img.shields.io/badge/license-Proprietary-red.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Windows-lightgrey.svg)](https://www.microsoft.com/windows)
[![Language](https://img.shields.io/badge/language-C%2B%2B-orange.svg)](https://isocpp.org/)

**同花顺独立下单程序的 HTTP API 封装**

支持通过 HTTP 接口进行股票交易操作，包括买入、卖出、撤单、查询等功能

[快速开始](#快速开始) • [API 文档](#api-文档) • [功能特性](#功能特性) • [使用示例](#使用示例)

</div>

---

## 📖 项目简介

KuaiT 自动下单系统是一个基于 C++ 开发的 Windows 桌面应用程序，通过 HTTP API 接口封装同花顺独立下单程序的交易功能。支持远程调用、自动化交易、量化策略对接等场景。

### 核心功能

- 🔌 **HTTP API 接口** - 提供 RESTful API，支持多种编程语言调用
- 📊 **实时查询** - 查询资产、持仓、委托、成交等信息
- 💰 **交易操作** - 支持买入、卖出、撤单等交易功能
- 🔐 **验证码识别** - 自动识别并处理交易验证码（基于 Tesseract OCR）
- 🎯 **系统托盘** - 后台运行，托盘图标显示运行状态
- 🔄 **自动重连** - 自动检测交易软件状态，断线重连
- 📝 **日志记录** - 详细的操作日志，便于调试和追踪

---

## 🚀 快速开始

### 环境要求

- **操作系统**: Windows 7 及以上（推荐 Windows 10/11）
- **交易软件**: 同花顺独立下单程序
- **运行库**: Visual C++ Redistributable 2015-2022

### 安装步骤

1. **下载程序**
   
   从 [Releases](https://github.com/taoersvip/KuaiT.git) 页面下载最新版本

2. **解压文件**
   
   将压缩包解压到任意目录，例如 `D:\KuaiT\`

3. **配置交易软件路径**
   
   首次运行会自动生成 `config.json` 文件，修改其中的交易软件路径：
   
   ```json
   {
     "trading_software": {
       "exe_path": "D:\\同花顺\\xiadan.exe",
       "process_name": "xiadan.exe"
     },
     "http_server": {
       "host": "127.0.0.1",
       "port": 9999
     }
   }
   ```

4. **启动程序**
   
   双击运行 `KuaiT自动下单.exe`，程序会最小化到系统托盘

5. **测试接口**
   
   浏览器访问 `http://127.0.0.1:9999/api/status` 检查服务器状态

---

## 📋 API 文档

### 接口概览

| 功能 | 方法 | 路径 | 说明 |
|------|------|------|------|
| 服务器状态 | GET | `/api/status` | 检查服务器运行状态 |
| 查询资产 | GET | `/api/query/assets` | 获取账户资产信息 |
| 查询持仓 | GET | `/api/query/positions` | 获取持仓股票列表 |
| 查询委托 | GET | `/api/query/orders` | 获取委托订单列表 |
| 查询成交 | GET | `/api/query/trades` | 获取成交记录列表 |
| 买入股票 | POST | `/api/trade/buy` | 提交买入委托 |
| 卖出股票 | POST | `/api/trade/sell` | 提交卖出委托 |
| 撤销委托 | POST | `/api/trade/cancel` | 撤销指定委托单 |
| 全部撤单 | POST | `/api/trade/cancel_all` | 撤销所有委托单 |

### 详细文档

- 📘 [完整 API 文档](OrderProgram/API文档.md) - 包含所有接口的详细说明和示例
- 📗 [快速参考手册](OrderProgram/API快速参考.md) - 接口速查表和简洁示例
- 📦 [Postman 集合](OrderProgram/OrderProgram_API.postman_collection.json) - 可直接导入测试

---

## 💡 使用示例

### Python 调用示例

```python
import requests

BASE_URL = "http://127.0.0.1:9999"

# 查询资产信息
response = requests.get(f"{BASE_URL}/api/query/assets")
assets = response.json()
if assets["success"]:
    print(f"可用资金: {assets['data']['availableMoney']}")
    print(f"总资产: {assets['data']['totalAsset']}")

# 买入股票
buy_data = {
    "stock_code": "600036",
    "price": "35.50",
    "volume": "1000"
}
response = requests.post(f"{BASE_URL}/api/trade/buy", json=buy_data)
print(response.json())

# 查询委托订单
response = requests.get(f"{BASE_URL}/api/query/orders")
orders = response.json()
if orders["success"]:
    print(orders["data"])

# 撤销委托
cancel_data = {"entrust_no": "123456"}
response = requests.post(f"{BASE_URL}/api/trade/cancel", json=cancel_data)
print(response.json())
```

### cURL 调用示例

```bash
# 查询资产
curl http://127.0.0.1:9999/api/query/assets

# 买入股票
curl -X POST http://127.0.0.1:9999/api/trade/buy \
  -H "Content-Type: application/json" \
  -d '{"stock_code":"600036","price":"35.50","volume":"1000"}'

# 撤销委托
curl -X POST http://127.0.0.1:9999/api/trade/cancel \
  -H "Content-Type: application/json" \
  -d '{"entrust_no":"123456"}'
```

更多示例请参考 [API 文档](OrderProgram/API文档.md)

---

## ✨ 功能特性

### 1. HTTP API 服务器
- 支持 RESTful API 风格
- JSON 格式数据交互
- 跨语言调用支持

### 2. 验证码自动识别
- 集成 Tesseract OCR 引擎
- 自动截取验证码图片
- 智能识别并输入
- 失败自动重试（最多 5 次）

### 3. 交易操作封装
- 通过 Windows UI Automation 控制交易软件
- 自动查找窗口和控件
- 模拟键盘输入和鼠标点击
- 线程安全的交易锁机制

### 4. 系统托盘集成
- 最小化到系统托盘
- 托盘图标显示运行状态（红色=运行中，灰色=已停止）
- 右键菜单快速操作
- 实时显示资金信息

### 5. 配置管理
- JSON 格式配置文件
- 支持自定义服务器地址和端口
- 交易软件路径配置
- 自动保存和加载

---

## 🏗️ 项目结构

```
KuaiT自动下单/
├── KuaiT自动下单.exe          # 主程序
├── order.dll                  # 交易核心 DLL
├── CaptchaRecognizer.dll      # 验证码识别 DLL
├── config.json                # 配置文件
├── dll_log.txt                # 日志文件
├── tessdata/                  # Tesseract OCR 数据文件
│   └── eng.traineddata
├── small.ico                  # 程序图标
├── tray_running.ico           # 托盘运行图标
└── tray_stopped.ico           # 托盘停止图标
```

---

## ⚙️ 技术架构

### 核心组件

- **OrderProgram** - 主程序，提供 HTTP 服务器和系统托盘界面
- **order.dll** - 交易核心库，封装所有交易操作
- **CaptchaRecognizer.dll** - 验证码识别库，基于 Tesseract OCR

### 技术栈

- **语言**: C++20
- **编译器**: MSVC 2022 (v145)
- **HTTP 库**: [cpp-httplib](https://github.com/yhirose/cpp-httplib)
- **JSON 库**: [nlohmann/json](https://github.com/nlohmann/json)
- **OCR 引擎**: [Tesseract](https://github.com/tesseract-ocr/tesseract)
- **UI 自动化**: Windows UI Automation API

### 模块说明

| 模块 | 文件 | 功能 |
|------|------|------|
| HTTP 服务器 | `HttpServer.cpp` | 处理 HTTP 请求和响应 |
| 交易 API | `OrderAPI.cpp` | 封装 DLL 调用接口 |
| 配置管理 | `Config.cpp` | 读取和保存配置文件 |
| 托盘窗口 | `OrderProgram.cpp` | 系统托盘和主窗口逻辑 |
| 通知窗口 | `NotificationWindow.cpp` | 弹窗通知功能 |

---

## ⚠️ 注意事项

### 使用前必读

1. **交易软件要求**
   - 必须先启动同花顺独立下单程序
   - 确保交易软件已登录账户
   - 不要最小化交易软件窗口

2. **参数格式**
   - 所有价格、数量参数使用**字符串类型**
   - 股票代码格式：6 位数字（如 "600036"）
   - 价格格式：保留 2 位小数（如 "35.50"）
   - 数量格式：整数字符串（如 "1000"）

3. **安全建议**
   - 仅在本地网络使用（127.0.0.1）
   - 不要将服务暴露到公网
   - 定期备份配置文件
   - 谨慎使用自动化交易

4. **性能优化**
   - 控制 API 调用频率，避免过于频繁
   - 建议每次操作间隔至少 500ms
   - 大量操作时使用批量接口

5. **错误处理**
   - 所有接口都会返回 `success` 字段
   - 失败时检查 `message` 字段获取错误信息
   - 查看 `dll_log.txt` 获取详细日志

---


## 📊 性能指标

- **API 响应时间**: < 100ms（查询接口）
- **交易操作时间**: 1-3 秒（包括验证码识别）
- **验证码识别率**: > 90%（取决于图片质量）
- **并发支持**: 支持多线程并发请求
- **内存占用**: < 50MB
- **CPU 占用**: < 5%（空闲时）

---

## 🔄 更新日志

### v1.0.1 (2025-01-24)
- ✅ 修复撤单功能的严重 BUG（错误查询持仓而非委托数据）
- ✅ 修复验证码识别失败导致的交易锁泄漏问题
- ✅ 优化日志输出，仅保留失败日志
- ✅ 将程序名称改为 `KuaiT自动下单.exe`
- ✅ 更新程序图标为 `small.ico`

### v1.0.0 (2025-01-23)
- 🎉 初始版本发布
- ✨ 支持 9 个 HTTP API 接口
- ✨ 支持买入、卖出、撤单、查询等功能
- ✨ 集成验证码自动识别
- ✨ 系统托盘集成

---

## 📄 许可证

本软件为**专有软件**，采用授权许可制。详见 [LICENSE](LICENSE) 文件。

### 授权说明

- 🔐 **非开源软件** - 源代码不公开
- 📜 **需要授权** - 使用前需获得有效授权
- 💼 **商业软件** - 提供个人版和商业版授权
- 🎁 **试用版** - 提供 30 天免费试用

### 获取授权

如需获取授权，请联系：
- **Email**: misstepsci@gmail.com

### 免责声明

⚠️ **重要提示**：
- 本软件仅为交易工具，不构成任何投资建议
- 使用本软件进行股票交易存在风险，所有风险由使用者自行承担
- 请遵守当地法律法规和证券交易规则
- 建议先使用试用版进行评估

---

## 🤝 问题反馈

欢迎提交 Issue 报告问题或提出建议！

**注意**：本软件为专有软件，不接受代码贡献（Pull Request）。


## 🙏 致谢

- [cpp-httplib](https://github.com/yhirose/cpp-httplib) - 轻量级 HTTP 库
- [nlohmann/json](https://github.com/nlohmann/json) - 现代 C++ JSON 库
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) - 开源 OCR 引擎

---

<div align="center">

**⭐ 如果这个项目对你有帮助，请给个 Star！⭐**

Made with ❤️ by KuaiT Team

</div>

