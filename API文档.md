# OrderProgram HTTP API 调用文档

## 服务器配置

**默认地址**: `http://127.0.0.1:9999`

可通过修改 `config.json` 文件中的配置来更改服务器地址和端口：

```json
{
  "http_server": {
    "host": "127.0.0.1",
    "port": 9999
  }
}
```

---

## API 接口列表

### 1. 服务器状态检查

**接口地址**: `GET /api/status`

**请求参数**: 无

**返回示例**:
```json
{
  "success": true,
  "message": "Server is running",
  "version": "1.0.0"
}
```

**说明**: 用于检查服务器是否正常运行

---

### 2. 查询资产信息

**接口地址**: `GET /api/query/assets`

**请求参数**: 无

**返回示例**:
```json
{
  "success": true,
  "data": {
    "availableMoney": "100000.00",
    "stockMarketValue": "50000.00",
    "totalAsset": "150000.00",
    "positionProfit": "5000.00",
    "todayProfit": "1000.00",
    "todayProfitRate": "0.67"
  }
}
```

**返回字段说明**:
- `availableMoney`: 可用资金
- `stockMarketValue`: 股票市值
- `totalAsset`: 总资产
- `positionProfit`: 持仓盈亏
- `todayProfit`: 今日盈亏
- `todayProfitRate`: 今日盈亏率 (%)

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to query assets"
}
```

---

### 3. 查询持仓信息

**接口地址**: `GET /api/query/positions`

**请求参数**: 无

**返回示例**:
```json
{
  "success": true,
  "data": "股票代码\t股票名称\t持仓数量\t可用数量\t成本价\t现价\t盈亏\n600036\t招商银行\t1000\t1000\t35.50\t36.20\t700.00\n"
}
```

**说明**: 返回的 `data` 字段为制表符分隔的文本数据，包含所有持仓股票信息

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to query positions"
}
```

---

### 4. 查询委托订单

**接口地址**: `GET /api/query/orders`

**请求参数**: 无

**返回示例**:
```json
{
  "success": true,
  "data": "委托编号\t股票代码\t股票名称\t买卖方向\t委托价格\t委托数量\t成交数量\t委托状态\n123456\t600036\t招商银行\t买入\t35.50\t1000\t0\t未成交\n"
}
```

**说明**: 返回的 `data` 字段为制表符分隔的文本数据，包含所有委托订单信息

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to query orders"
}
```

---

### 5. 查询成交记录

**接口地址**: `GET /api/query/trades`

**请求参数**: 无

**返回示例**:
```json
{
  "success": true,
  "data": "成交编号\t股票代码\t股票名称\t买卖方向\t成交价格\t成交数量\t成交时间\n789012\t600036\t招商银行\t买入\t35.50\t1000\t09:35:20\n"
}
```

**说明**: 返回的 `data` 字段为制表符分隔的文本数据，包含所有成交记录信息

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to query trades"
}
```

---

### 6. 买入股票

**接口地址**: `POST /api/trade/buy`

**请求头**: `Content-Type: application/json`

**请求参数**:
```json
{
  "stock_code": "600036",
  "price": "35.50",
  "volume": "1000"
}
```

**参数说明**:
- `stock_code` (string, 必填): 股票代码，如 "600036"
- `price` (string, 必填): 买入价格，如 "35.50"
- `volume` (string, 必填): 买入数量，如 "1000"

**成功返回**:
```json
{
  "success": true,
  "message": "Buy order placed successfully"
}
```

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to place buy order"
}
```

**错误返回**:
```json
{
  "success": false,
  "message": "Error: Invalid JSON format"
}
```

---

### 7. 卖出股票

**接口地址**: `POST /api/trade/sell`

**请求头**: `Content-Type: application/json`

**请求参数**:
```json
{
  "stock_code": "600036",
  "price": "36.00",
  "volume": "500"
}
```

**参数说明**:
- `stock_code` (string, 必填): 股票代码，如 "600036"
- `price` (string, 必填): 卖出价格，如 "36.00"
- `volume` (string, 必填): 卖出数量，如 "500"

**成功返回**:
```json
{
  "success": true,
  "message": "Sell order placed successfully"
}
```

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to place sell order"
}
```

**错误返回**:
```json
{
  "success": false,
  "message": "Error: Invalid JSON format"
}
```

---

### 8. 撤销指定委托单

**接口地址**: `POST /api/trade/cancel`

**请求头**: `Content-Type: application/json`

**请求参数**:
```json
{
  "entrust_no": "123456"
}
```

**参数说明**:
- `entrust_no` (string, 必填): 委托编号，可从"查询委托订单"接口获取

**成功返回**:
```json
{
  "success": true,
  "message": "Order cancelled successfully"
}
```

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to cancel order"
}
```

**错误返回**:
```json
{
  "success": false,
  "message": "Error: Missing entrust_no parameter"
}
```

---

### 9. 撤销所有委托单

**接口地址**: `POST /api/trade/cancel_all`

**请求参数**: 无

**成功返回**:
```json
{
  "success": true,
  "message": "All orders cancelled successfully"
}
```

**失败返回**:
```json
{
  "success": false,
  "message": "Failed to cancel all orders"
}
```

**说明**: 此接口会撤销所有未成交的委托单

---

## 调用示例

### Python 示例

```python
import requests
import json

# 服务器地址
BASE_URL = "http://127.0.0.1:9999"

# 1. 检查服务器状态
response = requests.get(f"{BASE_URL}/api/status")
print("服务器状态:", response.json())

# 2. 查询资产信息
response = requests.get(f"{BASE_URL}/api/query/assets")
assets = response.json()
if assets["success"]:
    print("可用资金:", assets["data"]["availableMoney"])
    print("总资产:", assets["data"]["totalAsset"])

# 3. 买入股票
buy_data = {
    "stock_code": "600036",
    "price": "35.50",
    "volume": "1000"
}
response = requests.post(
    f"{BASE_URL}/api/trade/buy",
    headers={"Content-Type": "application/json"},
    data=json.dumps(buy_data)
)
print("买入结果:", response.json())

# 4. 查询委托订单
response = requests.get(f"{BASE_URL}/api/query/orders")
orders = response.json()
if orders["success"]:
    print("委托订单:", orders["data"])

# 5. 撤销指定委托单
cancel_data = {
    "entrust_no": "123456"
}
response = requests.post(
    f"{BASE_URL}/api/trade/cancel",
    headers={"Content-Type": "application/json"},
    data=json.dumps(cancel_data)
)
print("撤单结果:", response.json())

# 6. 卖出股票
sell_data = {
    "stock_code": "600036",
    "price": "36.00",
    "volume": "500"
}
response = requests.post(
    f"{BASE_URL}/api/trade/sell",
    headers={"Content-Type": "application/json"},
    data=json.dumps(sell_data)
)
print("卖出结果:", response.json())

# 7. 撤销所有委托单
response = requests.post(f"{BASE_URL}/api/trade/cancel_all")
print("全部撤单结果:", response.json())
```

---

### JavaScript (Node.js) 示例

```javascript
const axios = require('axios');

const BASE_URL = 'http://127.0.0.1:9999';

// 1. 检查服务器状态
async function checkStatus() {
    const response = await axios.get(`${BASE_URL}/api/status`);
    console.log('服务器状态:', response.data);
}

// 2. 查询资产信息
async function queryAssets() {
    const response = await axios.get(`${BASE_URL}/api/query/assets`);
    if (response.data.success) {
        console.log('可用资金:', response.data.data.availableMoney);
        console.log('总资产:', response.data.data.totalAsset);
    }
}

// 3. 买入股票
async function buyStock() {
    const buyData = {
        stock_code: '600036',
        price: '35.50',
        volume: '1000'
    };
    const response = await axios.post(`${BASE_URL}/api/trade/buy`, buyData);
    console.log('买入结果:', response.data);
}

// 4. 卖出股票
async function sellStock() {
    const sellData = {
        stock_code: '600036',
        price: '36.00',
        volume: '500'
    };
    const response = await axios.post(`${BASE_URL}/api/trade/sell`, sellData);
    console.log('卖出结果:', response.data);
}

// 5. 撤销指定委托单
async function cancelOrder(entrustNo) {
    const cancelData = {
        entrust_no: entrustNo
    };
    const response = await axios.post(`${BASE_URL}/api/trade/cancel`, cancelData);
    console.log('撤单结果:', response.data);
}

// 6. 撤销所有委托单
async function cancelAllOrders() {
    const response = await axios.post(`${BASE_URL}/api/trade/cancel_all`);
    console.log('全部撤单结果:', response.data);
}

// 执行示例
(async () => {
    await checkStatus();
    await queryAssets();
    await buyStock();
    await cancelOrder('123456');
})();
```

---

### cURL 示例

#### Linux/Mac 系统

```bash
# 1. 检查服务器状态
curl http://127.0.0.1:9999/api/status

# 2. 查询资产信息
curl http://127.0.0.1:9999/api/query/assets

# 3. 查询持仓信息
curl http://127.0.0.1:9999/api/query/positions

# 4. 查询委托订单
curl http://127.0.0.1:9999/api/query/orders

# 5. 查询成交记录
curl http://127.0.0.1:9999/api/query/trades

# 6. 买入股票
curl -X POST http://127.0.0.1:9999/api/trade/buy \
  -H "Content-Type: application/json" \
  -d "{\"stock_code\":\"600036\",\"price\":\"35.50\",\"volume\":\"1000\"}"

# 7. 卖出股票
curl -X POST http://127.0.0.1:9999/api/trade/sell \
  -H "Content-Type: application/json" \
  -d "{\"stock_code\":\"600036\",\"price\":\"36.00\",\"volume\":\"500\"}"

# 8. 撤销指定委托单
curl -X POST http://127.0.0.1:9999/api/trade/cancel \
  -H "Content-Type: application/json" \
  -d "{\"entrust_no\":\"123456\"}"

# 9. 撤销所有委托单
curl -X POST http://127.0.0.1:9999/api/trade/cancel_all
```

#### Windows CMD 系统

```cmd
REM 1. 检查服务器状态
curl http://127.0.0.1:9999/api/status

REM 2. 查询资产信息
curl http://127.0.0.1:9999/api/query/assets

REM 3. 查询持仓信息
curl http://127.0.0.1:9999/api/query/positions

REM 4. 查询委托订单
curl http://127.0.0.1:9999/api/query/orders

REM 5. 查询成交记录
curl http://127.0.0.1:9999/api/query/trades

REM 6. 买入股票
curl -X POST http://127.0.0.1:9999/api/trade/buy -H "Content-Type: application/json" -d "{\"stock_code\":\"600036\",\"price\":\"35.50\",\"volume\":\"1000\"}"

REM 7. 卖出股票
curl -X POST http://127.0.0.1:9999/api/trade/sell -H "Content-Type: application/json" -d "{\"stock_code\":\"600036\",\"price\":\"36.00\",\"volume\":\"500\"}"

REM 8. 撤销指定委托单
curl -X POST http://127.0.0.1:9999/api/trade/cancel -H "Content-Type: application/json" -d "{\"entrust_no\":\"123456\"}"

REM 9. 撤销所有委托单
curl -X POST http://127.0.0.1:9999/api/trade/cancel_all
```

#### Windows PowerShell 系统

```powershell
# 1. 检查服务器状态
curl http://127.0.0.1:9999/api/status

# 2. 查询资产信息
curl http://127.0.0.1:9999/api/query/assets

# 3. 查询持仓信息
curl http://127.0.0.1:9999/api/query/positions

# 4. 查询委托订单
curl http://127.0.0.1:9999/api/query/orders

# 5. 查询成交记录
curl http://127.0.0.1:9999/api/query/trades

# 6. 买入股票
curl -X POST http://127.0.0.1:9999/api/trade/buy -H "Content-Type: application/json" -d '{\"stock_code\":\"000001\",\"price\":\"11.70\",\"volume\":\"100\"}'

curl -X POST http://127.0.0.1:9999/api/trade/buy -H "Content-Type: application/json" -d "{\"stock_code\":\"000001\",\"price\":\"11.70\",\"volume\":\"100\"}"

# 7. 卖出股票
curl -X POST http://127.0.0.1:9999/api/trade/sell -H "Content-Type: application/json" -d '{\"stock_code\":\"600036\",\"price\":\"36.00\",\"volume\":\"500\"}'

# 8. 撤销指定委托单
curl -X POST http://127.0.0.1:9999/api/trade/cancel -H "Content-Type: application/json" -d '{\"entrust_no\":\"123456\"}'

# 9. 撤销所有委托单
curl -X POST http://127.0.0.1:9999/api/trade/cancel_all
```

**注意**:
- Windows CMD 中使用双引号，JSON 内部的双引号需要转义 `\"`
- Windows PowerShell 中外层使用单引号，JSON 内部使用双引号并转义 `\"`
- Linux/Mac 中使用反斜杠 `\` 续行，Windows 中所有参数写在一行

---

## 注意事项

1. **字符编码**: 所有接口使用 UTF-8 编码，支持中文股票名称和信息

2. **数据类型**:
   - 股票代码、价格、数量等参数均使用**字符串类型**
   - 不要使用数字类型，避免精度问题

3. **错误处理**:
   - 所有接口都会返回 `success` 字段表示操作是否成功
   - 失败时 `message` 字段会包含错误信息

4. **交易软件要求**:
   - 同花顺独立下单程序
   - 确保 `config.json` 中配置的交易软件路径正确

5. **委托编号**:
   - 委托编号由交易软件生成
   - 撤单时需要使用正确的委托编号
   - 可通过"查询委托订单"接口获取

6. **并发请求**:
   - 服务器支持并发请求
   - 建议控制请求频率，避免过于频繁

7. **数据格式**:
   - 查询接口返回的持仓、委托、成交数据为制表符分隔的文本
   - 需要自行解析处理

---

## 常见问题

### Q1: 如何修改服务器端口？

编辑 `config.json` 文件，修改 `http_server.port` 的值，然后重启程序。

### Q2: 买入/卖出失败怎么办？

1. 检查交易软件是否正常运行
2. 检查股票代码、价格、数量是否正确
3. 检查账户资金是否充足
4. 查看交易软件界面是否有错误提示

### Q3: 查询接口返回空数据？

1. 确认交易软件已登录
2. 确认交易软件窗口未被最小化
3. 尝试手动在交易软件中查询，确认有数据

### Q4: 如何获取委托编号？

调用"查询委托订单"接口 (`GET /api/query/orders`)，从返回的数据中解析委托编号。

---

## 版本信息

- **API 版本**: 1.0.1
- **文档更新日期**: 2025-01-23
- **支持的交易软件**: 同花顺独立下单程序

---

## 更新日志

### v1.0.1 (2025-01-23)
- **修复**: 修复了撤单功能的严重 BUG
  - 问题：撤单时错误地查询持仓数据而不是委托数据
  - 原因：`QueryDataInternal` 函数缺少 `queryType == 3` 的处理
  - 修复：撤单界面现在正确通过 F3 进入，而不是 F4 查询界面
  - 影响：撤单功能现在可以正常工作

### v1.0.0 (2025-01-23)
- 初始版本发布
- 支持 9 个 HTTP API 接口
- 支持买入、卖出、撤单、查询等功能

---

## 技术支持

如有问题，请检查：
1. 程序日志文件（`dll_log.txt`）
2. 交易软件是否正常运行
3. 网络连接是否正常
4. 配置文件是否正确

