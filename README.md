# 噪声传感器案例



## 案例介绍

本案例描述[YoMo](https://github.com/yomorun/yomo)在工业互联网数据采集中的应用，以收集噪声传感器的数据为例，涉及数据收集/处理/工作流/数据展示的全过程，为了便于体验运行效果，还会对其进行容器化，并通过docker快速部署体验版。



## 案例述语

- xxx-source: 表示一个数据源收集程序
- xxx-zipper: 表示一个工作流和控制平面
- xxx-flow: 表示一个工作流单元，用于实际的业务逻辑处理，被zipper调度。
- xxx-sink: 表示一个数据的传送目的地，一般落地数据库或者传递给下一级代理，被zipper调度。



## 案例架构

![](https://github.com/yomorun/example-noise/blob/main/docs/arch1.png?raw=true)

从图中可见，区分了边缘端和云端两个独立区域，区域之间是通过弱网或者互联网连接，这里简单介绍一下各个服务：

- 边缘端部署了传感器设备(Noise)和数据收集网关(ZLAN)，网关会定时向设备请求状态数据，并转换为MQTT协议数据发送给noise-source收集器，source起到转换编码并与YoMo工作流引擎zipper建立连接的作用。关于传感器设备和数据收集网关的硬件选购和配置可以参与这篇文章：[https://yomo.run/zh/aiot](https://yomo.run/zh/aiot)。
- 对于不想购买硬件设备的开发者，这里也提供了一个noise-emitter模拟器用来产生噪声数据。
- zipper是一个强大的工作流引擎，通过编排(workflow.yaml)可以调度多个flow和sink，让他们以流的方式把业务逻辑串联起来，以满足复杂的需求。与之相连的所有通信和编解码均以QUIC+Y3进行，提供可靠实时的流式处理，全程体验流式编程的乐趣。
- noise-flow 实现把噪音值除以10的简单处理，并且监控如果超过一定阀值后输出日志进行警报。
- noise-sink 没有真的输出到数据库，而是通过搭建一个WebSocket服务器，把实时的噪音状态输出给任意的网页进行展示消费。
- noise-web 是一个消费WebSocket的网页服务，他部署在哪里都可以，只要能访问到noise-sink提供的WebSocket服务地址即可，这里我们假设部署回边缘端也是没有问题的。



## 案例代码

下表提供了案例的全部代码，供感兴趣的朋友查看，参照这个案体的代码，可以轻松开发出类拟场景的案例。

| 项目          | 地址                                                         | 说明                          |
| :------------ | :----------------------------------------------------------- | :---------------------------- |
| noise-source  | [yomo-source-noise-example](https://github.com/yomorun/yomo-source-noise-example) | 收集MQTT消息格式的噪音数据    |
| noise-zipper  | [yomo-zipper-noise-example](https://github.com/yomorun/yomo-zipper-noise-example) | 编排本案体的工作流和数据流向  |
| noise-flow    | [yomo-flow-noise-example](https://github.com/yomorun/yomo-flow-noise-example) | 对噪音数据进行预处理和警报    |
| noise-sink    | [yomo-sink-socketio-server-example](https://github.com/yomorun/yomo-sink-socketio-server-example) | 提供WebSocket服务用于数据展示 |
| noise-web     | [yomo-sink-socket-io-example](https://github.com/yomorun/yomo-sink-socket-io-example) | 消费WebSocket服务展示噪音状态 |
| noise-emitter | [yomo-source-noise-emitter-example](https://github.com/yomorun/yomo-source-noise-emitter-example) | 模拟产生噪声数据              |



## 容器化

通过下载上节的项目代码可以进行本地原生部署，体验YoMo开发的乐趣，但是对于想急于马上看到效果的朋来说，更爽的方式当然是先快速运行起来看看效果，所以对上节的项目也做了容器化处理，每个项目的根目录均提供了Dockerfile文件，并且在hub.docker.com提供了官方镜像下载：

| 项目          | 镜像地址                                                     | 最新版本                     |
| ------------- | ------------------------------------------------------------ | ---------------------------- |
| noise-source  | [yomorun/noise-source](https://hub.docker.com/r/yomorun/noise-source) | yomorun/noise-source:latest  |
| noise-zipper  | [yomorun/noise-zipper](https://hub.docker.com/r/yomorun/noise-zipper) | yomorun/noise-zipper:latest  |
| noise-flow    | [yomorun/noise-flow](https://hub.docker.com/r/yomorun/noise-flow) | yomorun/noise-flow:latest    |
| noise-sink    | [yomorun/noise-sink](https://hub.docker.com/r/yomorun/noise-sink) | yomorun/noise-sink:latest    |
| noise-web     | [yomorun/noise-web](https://hub.docker.com/r/yomorun/noise-web) | yomorun/noise-web:latest     |
| noise-emitter | [yomorun/noise-emitter](https://hub.docker.com/repository/docker/yomorun/noise-emitter) | yomorun/noise-emitter:latest |
| quic-mqtt     | [yomorun/quic-mqtt](https://hub.docker.com/r/yomorun/quic-mqtt) | yomorun/quic-mqtt:latest     |

yomorun/quic-mqtt:latest 是开发xxx-source的基础镜像，可以快速打包自定义代码，但本案例中可以暂时忽略。



## 运行案例

### 快速运行

有了上述的官方镜像就简单多了，只需简单的步骤就可以体验案体的效果：

- 下载[docker-compose.yml](https://github.com/yomorun/example-noise/blob/main/docker-compose.yml)文件。
- 运行 `docker-compose up -d`
- 先喝杯茶稍作等待，通过访问 [http://localhost:3000/]( http://localhost:3000/) 可看到如下效果图：

 ![show1](https://github.com/yomorun/example-noise/blob/main/docs/show1.jpg?raw=true)

这里Delay的值要注意一下，因为是通过Docker容器部署，各个容器的时钟并不对齐得这么完美，如果要查看到最精确的延时值，需要把noise-source和noise-web原生部署到同一个宿主机上。

### 查看状态

通过 `docker-compose ps`查看服务状态

```text
    Name                   Command               State                Ports              
-----------------------------------------------------------------------------------------
noise-emitter   sh -c go run main.go             Up                                      
noise-flow      sh -c yomo run app.go -p 4242    Up      4242/udp                        
noise-sink      sh -c go run main.go             Up      4141/udp, 0.0.0.0:8000->8000/tcp
noise-source    sh -c go run main.go             Up      1883/tcp                        
noise-web       ./docker-entrypoint.sh yar ...   Up      0.0.0.0:3000->3000/tcp          
noise-zipper    sh -c yomo wf run workflow ...   Up      9999/udp    
```

- noise-sink 暴露了8000的WebSocket端口提给noise-web展示消费。
- noise-web 暴露了3000的http端口用于展示实时的噪声值和延时。
- noise-zipper/noise-flow/noise-sink 均提供了udp端口的QUIC服务，全流程QUIC通信。
- noise-source 是我们对接不同设备的关键，提供1883的MQTT端口，当然也可以修改的。

### 查看日志

- 查看noise-emitter `docker-compose logs -f noise-emitter`

  ```text
  noise-emitter    | 2021-04-26 10:11:03: Publish counter=12438, topic=NOISE, payload={"noise":12438}
  noise-emitter    | 2021-04-26 10:11:04: Publish counter=12439, topic=NOISE, payload={"noise":12439}
  noise-emitter    | 2021-04-26 10:11:05: Publish counter=12440, topic=NOISE, payload={"noise":12440}
  noise-emitter    | 2021-04-26 10:11:06: Publish counter=12441, topic=NOISE, payload={"noise":12441}
  ```

  这个模拟发生器产生了MQTT数据：主题是`NOISE`, 值是不断递增的序号(JSON格式)。

- 查看noise-source `docker-compose logs -f noise-source`

  ```text
  noise-source     | 2021/04/26 15:27:32 receive: topic=NOISE, payload={"noise":2638}
  noise-source     | 2021/04/26 15:27:32 write: sendingBuf=[]byte{0x81, 0x1b, 0x90, 0x19, 0x11, 0x3, 0x45, 0x24, 0xe0, 0x12, 0x6, 0xaf, 0x90, 0xe8, 0xce, 0x83, 0x1c, 0x13, 0xa, 0x31, 0x37, 0x32, 0x2e, 0x31, 0x39, 0x2e, 0x30, 0x2e, 0x36}
  noise-source     | 2021/04/26 15:27:33 receive: topic=NOISE, payload={"noise":2639}
  noise-source     | 2021/04/26 15:27:33 write: sendingBuf=[]byte{0x81, 0x1b, 0x90, 0x19, 0x11, 0x3, 0x45, 0x24, 0xf0, 0x12, 0x6, 0xaf, 0x90, 0xe8, 0xce, 0x8b, 0x5, 0x13, 0xa, 0x31, 0x37, 0x32, 0x2e, 0x31, 0x39, 0x2e, 0x30, 0x2e, 0x36}
  ```

  - receive: 表示source收到的MQTT数据。
  - write: 表示把经过Y3转码的字节码向noise-zipper工作流引擎发送。

- 查看noise-zipper  `docker-compose logs -f noise-zipper`

  ```text
  noise-zipper     | 2021/04/26 14:39:28 Found 1 flows in zipper config
  noise-zipper     | 2021/04/26 14:39:28 Flow 1: Noise Serverless on noise-flow:4242
  noise-zipper     | 2021/04/26 14:39:28 Found 1 sinks in zipper config
  noise-zipper     | 2021/04/26 14:39:28 Sink 1: Socket.io Server on noise-sink:4141
  noise-zipper     | 2021/04/26 14:39:28 Running YoMo workflow...
  noise-zipper     | 2021/04/26 14:39:28 ✅ Listening on 0.0.0.0:9999
  noise-zipper     | 2021/04/26 14:43:32 ✅ Connect to Noise Serverless (noise-flow:4242) successfully.
  noise-zipper     | 2021/04/26 14:44:33 ✅ Connect to Socket.io Server (noise-sink:4141) successfully.
  ```

  工作流引擎连接上了noise-flow和noise-sink这两个工作流的处理单元了。

- 查看noise-flow  `docker-compose logs -f noise-flow`

  ```text
  noise-flow       | ❗ value: 561.700012 reaches the threshold 16! 45.700012
  noise-flow       | [172.19.0.6] 1619425035923 > value: 561.799988 ⚡️=0ms
  noise-flow       | ❗ value: 561.799988 reaches the threshold 16! 45.799988
  noise-flow       | [172.19.0.6] 1619425036923 > value: 561.900024 ⚡️=1ms
  ```

  噪声数据是模拟器产生的，远远超过了预设的阀值，打印出警告信息。

## 引用参考

上面就是这个噪声传感器案例从数据收集处理到展示的所有代码了。对于需要扩展或者引申到别的应用场景的开发者，可以点开每个项目的链接进行详细阅读，每个项目都是微服务化构建，服务角色明确，代码清晰易懂，如果有什么问题欢迎提出Issues或者讨论。参考链接：

- https://yomo.run/
- https://github.com/yomorun/yomo
- https://github.com/yomorun/example-noise

