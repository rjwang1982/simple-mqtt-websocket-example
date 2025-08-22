# Simple MQTT WebSocket Example

一个简单的 MQTT WebSocket 客户端示例，演示如何在 Web 浏览器中通过 WebSocket 连接到 MQTT 代理（如 Mosquitto）并实时接收消息。

## 📋 项目概述

这个项目提供了一个基于 Web 的 MQTT 客户端实现，使用 WebSocket 协议与 MQTT 代理通信。它展示了如何在浏览器环境中订阅 MQTT 主题并实时显示接收到的消息。

## 🏗️ 项目结构

```
simple-mqtt-websocket-example/
├── index.html          # 主 HTML 页面
├── config.js           # MQTT 连接配置文件
├── mqttws31.js         # Paho MQTT JavaScript 客户端库
├── jquery.min.js       # jQuery 库（用于 DOM 操作）
└── README.md           # 项目文档
```

## 🚀 功能特性

### 核心功能
- **WebSocket 连接**: 通过 WebSocket 协议连接到 MQTT 代理
- **自动重连**: 连接断开时自动尝试重新连接
- **主题订阅**: 支持订阅单个或多个 MQTT 主题
- **实时消息显示**: 实时显示接收到的 MQTT 消息
- **连接状态监控**: 显示连接状态和错误信息

### 技术特性
- **纯前端实现**: 无需后端服务器，直接在浏览器中运行
- **配置灵活**: 通过配置文件轻松修改连接参数
- **跨平台兼容**: 支持所有现代 Web 浏览器
- **SSL/TLS 支持**: 可配置加密连接
- **身份验证**: 支持用户名/密码认证

## 📁 文件详解

### index.html
主要的 HTML 页面，包含：
- **用户界面**: 显示订阅主题、连接状态和消息列表
- **MQTT 客户端逻辑**: 
  - 连接管理（连接、断开、重连）
  - 消息订阅和接收
  - 错误处理和状态更新
- **DOM 操作**: 使用 jQuery 更新页面内容

### config.js
MQTT 连接配置文件，包含：
```javascript
host = '127.0.0.1';        // MQTT 代理主机地址
port = 9001;               // WebSocket 端口
topic = '#';               // 订阅主题（# 表示所有主题）
useTLS = false;            // 是否使用 SSL/TLS
username = null;           // 用户名（可选）
password = null;           // 密码（可选）
cleansession = true;       // 清理会话
```

### mqttws31.js
Paho MQTT JavaScript 客户端库，提供：
- **MQTT 协议实现**: 完整的 MQTT v3.1/v3.1.1 协议支持
- **WebSocket 传输**: 基于 WebSocket 的消息传输
- **客户端 API**: 连接、订阅、发布、断开等操作
- **消息处理**: 消息编码/解码和 QoS 支持

### jquery.min.js
jQuery 库的压缩版本，用于：
- DOM 元素操作
- 事件处理
- AJAX 请求（如需要）

## 🛠️ 安装和使用

### 前置要求

1. **MQTT 代理**: 需要一个支持 WebSocket 的 MQTT 代理
   - Mosquitto（推荐）
   - EMQ X
   - HiveMQ
   - AWS IoT Core

2. **Web 服务器**: 用于托管 HTML 文件
   - Apache HTTP Server
   - Nginx
   - Python HTTP Server
   - Node.js Express

### 快速开始

1. **克隆项目**
```bash
git clone https://github.com/rjwang1982/simple-mqtt-websocket-example.git
cd simple-mqtt-websocket-example
```

2. **配置 MQTT 代理**

#### 使用 Mosquitto
```bash
# 安装 Mosquitto
sudo apt-get install mosquitto mosquitto-clients

# 配置 WebSocket 支持
echo "listener 9001" | sudo tee -a /etc/mosquitto/mosquitto.conf
echo "protocol websockets" | sudo tee -a /etc/mosquitto/mosquitto.conf

# 重启服务
sudo systemctl restart mosquitto
```

#### 使用 Docker
```bash
# 运行 Mosquitto 容器
docker run -it -p 1883:1883 -p 9001:9001 \
  -v $(pwd)/mosquitto.conf:/mosquitto/config/mosquitto.conf \
  eclipse-mosquitto
```

3. **修改配置**
编辑 `config.js` 文件：
```javascript
host = 'your-mqtt-broker-ip';  // 修改为你的 MQTT 代理地址
port = 9001;                   // WebSocket 端口
topic = 'test/topic';          // 修改为你要订阅的主题
```

4. **启动 Web 服务器**

#### 使用 Python
```bash
# Python 3
python -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000
```

#### 使用 Node.js
```bash
npx http-server -p 8000
```

5. **访问应用**
打开浏览器访问：`http://localhost:8000`

## 📊 使用示例

### 基本订阅
```javascript
// 订阅单个主题
topic = 'sensor/temperature';

// 订阅多级通配符
topic = 'sensor/+/temperature';

// 订阅所有主题
topic = '#';
```

### 发布消息测试
使用 MQTT 客户端发布消息进行测试：

```bash
# 使用 mosquitto_pub
mosquitto_pub -h localhost -t "test/topic" -m "Hello World"

# 使用 Python paho-mqtt
python -c "
import paho.mqtt.client as mqtt
client = mqtt.Client()
client.connect('localhost', 1883, 60)
client.publish('test/topic', 'Hello from Python')
client.disconnect()
"
```

### SSL/TLS 配置
```javascript
// 启用 SSL/TLS
useTLS = true;
port = 8084;  // 通常 SSL WebSocket 使用不同端口

// 配置用户名密码
username = "your_username";
password = "your_password";
```

## 🔧 高级配置

### 自定义路径
```javascript
// 自定义 WebSocket 路径
path = "/mqtt";

// 带查询参数的路径
path = "/data/cloud?device=12345";
```

### 连接选项
```javascript
var options = {
    timeout: 3,              // 连接超时（秒）
    useSSL: useTLS,         // 使用 SSL/TLS
    cleanSession: true,      // 清理会话
    keepAliveInterval: 60,   // 心跳间隔（秒）
    reconnectPeriod: 2000,   // 重连间隔（毫秒）
    onSuccess: onConnect,    // 连接成功回调
    onFailure: onFailure     // 连接失败回调
};
```

### 消息处理
```javascript
function onMessageArrived(message) {
    var topic = message.destinationName;
    var payload = message.payloadString;
    var qos = message.qos;
    var retained = message.retained;
    
    console.log('Topic:', topic);
    console.log('Payload:', payload);
    console.log('QoS:', qos);
    console.log('Retained:', retained);
    
    // 自定义消息处理逻辑
    processMessage(topic, payload);
}
```

## 🌐 部署选项

### 静态网站托管
- **GitHub Pages**: 免费托管静态网站
- **Netlify**: 自动部署和 CDN
- **Vercel**: 现代化部署平台
- **AWS S3**: 静态网站托管

### 容器化部署
```dockerfile
# Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```bash
# 构建和运行
docker build -t mqtt-websocket-example .
docker run -p 8080:80 mqtt-websocket-example
```

## 🔒 安全考虑

### 网络安全
- **使用 HTTPS**: 在生产环境中始终使用 HTTPS
- **WSS 连接**: 使用加密的 WebSocket 连接
- **CORS 配置**: 正确配置跨域资源共享

### 认证授权
- **用户认证**: 使用用户名/密码或证书认证
- **主题权限**: 限制客户端可访问的主题
- **连接限制**: 限制并发连接数

### 配置安全
```javascript
// 生产环境配置示例
host = 'secure-mqtt-broker.example.com';
port = 8084;
useTLS = true;
username = process.env.MQTT_USERNAME;  // 从环境变量获取
password = process.env.MQTT_PASSWORD;
```

## 🐛 故障排除

### 常见问题

1. **连接失败**
   - 检查 MQTT 代理是否运行
   - 验证 WebSocket 端口是否开放
   - 确认防火墙设置

2. **消息接收不到**
   - 检查主题订阅是否正确
   - 验证消息发布到正确的主题
   - 确认 QoS 设置

3. **频繁断线**
   - 检查网络连接稳定性
   - 调整心跳间隔
   - 检查代理配置

### 调试技巧
```javascript
// 启用详细日志
console.log("Host="+ host + ", port=" + port + ", path=" + path + 
           " TLS = " + useTLS + " username=" + username);

// 监听所有事件
mqtt.onConnectionLost = function(responseObject) {
    console.log("Connection lost:", responseObject.errorMessage);
};

mqtt.onMessageArrived = function(message) {
    console.log("Message arrived:", message);
};
```

## 📈 性能优化

### 客户端优化
- **消息缓存**: 实现本地消息缓存
- **批量处理**: 批量处理大量消息
- **内存管理**: 定期清理旧消息

### 网络优化
- **压缩**: 启用 WebSocket 压缩
- **心跳优化**: 调整心跳间隔
- **重连策略**: 实现指数退避重连

## 🔄 扩展功能

### 消息发布
```javascript
function publishMessage(topic, message) {
    var message = new Paho.MQTT.Message(message);
    message.destinationName = topic;
    message.qos = 0;
    mqtt.send(message);
}
```

### 多主题订阅
```javascript
function subscribeToMultipleTopics(topics) {
    topics.forEach(function(topic) {
        mqtt.subscribe(topic, {qos: 0});
    });
}
```

### 消息过滤
```javascript
function onMessageArrived(message) {
    var topic = message.destinationName;
    var payload = message.payloadString;
    
    // 根据主题过滤消息
    if (topic.startsWith('sensor/')) {
        handleSensorData(payload);
    } else if (topic.startsWith('alert/')) {
        handleAlert(payload);
    }
}
```

## 📚 相关资源

### 文档链接
- [MQTT 协议规范](http://mqtt.org/)
- [Paho JavaScript 客户端](https://www.eclipse.org/paho/clients/js/)
- [Mosquitto 文档](https://mosquitto.org/documentation/)
- [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

### 示例项目
- [MQTT Dashboard](https://github.com/mqtt/mqtt-dashboard)
- [IoT Device Simulator](https://github.com/aws-samples/iot-device-simulator)
- [MQTT Explorer](https://github.com/thomasnordquist/MQTT-Explorer)

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来改进这个项目：

1. Fork 项目
2. 创建功能分支
3. 提交更改
4. 推送到分支
5. 创建 Pull Request

## 📄 许可证

本项目基于原始博客文章示例代码，仅供学习和参考使用。

## 🙏 致谢

- 原始示例来自 [Jan-Piet Mens 的博客文章](http://jpmens.net/2014/07/03/the-mosquitto-mqtt-broker-gets-websockets-support/)
- Eclipse Paho 项目提供的 JavaScript MQTT 客户端库
- Mosquitto 项目提供的优秀 MQTT 代理

---

**⚠️ 注意**: 这是一个基础示例项目，在生产环境使用前请进行充分的安全评估和性能测试。
