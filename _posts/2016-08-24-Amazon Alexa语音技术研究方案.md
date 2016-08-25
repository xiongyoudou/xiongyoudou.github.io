---
title: "Amazon Alexa语音技术研究方案"
categories:
- Essays
tags:
- essay
- iOS
---

@(markdown笔记)

**Alexa**，是使Echo设备强大的语音服务，它能够提供一种能力(capabilities, skills)从而给用户一种贴近直觉的方式来通过语音与设备交互。这些能力包括：播放音乐、回答常见问题、设定定时闹铃等。Alexa是基于云端的，所以它每时每刻都在变得更智能，越多的客户使用Alexa，它越能适应说话方式、词汇和个人喜好等方面。利用Alexa，开发者能够有以下几个益处：
1. **Get in Early**：让开发者更专注用户体验，不用考虑语音识别的技术。
2. **Reach More Customers**：能够接触到通过Alexa语音服务驱动的所有设备
3. **Build Quickly for Free**：使用AVS和ASK来构建自己的语音服务完全免费。可以用cloud-based service或者AWS Lambda，其中cloud-based service每月的前100万是免费的(is free for the first one million calls per month)。ASK is free to use. AWS Lambda is also free for the first one million calls per month, which can support skill hosting for most developers.

### **Alexa Skills Kit**
ASK是一个包括**自主服务API**、**工具**、**文档**、**代码示例**的集合，利用它可以快速简单的让你为Alexa添加能力(to add skills to Alexa)。所有的代码都运行在云端，在用户设备上不需要安装任何东西。用户只需要简单的问一个问题或者下一个命令，就可在任何Alexa-enabled的设备上访问这些skills。
ASK能够让你添加新的skills，然后通过提问和发出请求来访问这些skills。你可以建立哪些提供给用户各式各样的不同能力，比如：
- 寻找特定问题的答案
- 猜谜或者玩游戏
- 控制点灯或者其他家具设备

首先需要确定创建什么样的Skill，ASK支持创建两种不同类型的skills：自定义的skills和智能家居skills。

### Smart Home Skill API
该API是新加入到ASK中，让开发者能够添加新的能力到Alexa上。Alexa已经提供一系列的内嵌的智能家居能力(built-in smart home capabilities)。包括开关灯、控制温度等，用户可以通过提问已经发出请求的方式来访问这些功能。

### Alexa Voice Service
AVS是一个智能并且可扩展的云端服务，利用它可以添加强大的语音功能给任何连接了的产品上(adds voice-enabled experiences to any connected product)。利用Amazon Alexa app，用户在任何地方可以轻易的控制和管理它们的产品(products)。

#### Authorization
若要访问AVS API，产品需要拥有一个access token，获取token有两种方式：
- **远程授权**：通过 [网站](https://developer.amazon.com/public/solutions/alexa/alexa-voice-service/docs/authorizing-your-alexa-enabled-product-from-a-website)或者[移动app](https://developer.amazon.com/public/solutions/alexa/alexa-voice-service/docs/authorizing-your-alexa-enabled-product-from-an-android-or-ios-mobile-app)
- **本地授权**：通过一个AVS-enabled的产品来授权，[相关地址](https://developer.amazon.com/public/solutions/alexa/alexa-voice-service/docs/authorizing-your-alexa-enabled-mobile-app)

#### 传输协议

##### [与AVS建立HTTP/2连接](https://developer.amazon.com/public/solutions/alexa/alexa-voice-service/docs/managing-an-http-2-connection)
**关键术语**
-  **帧(Frame)**：HTTP/2中基本的协议单元，每一帧都提供不同的目的，比如HTTP请求和响应中的HEADERS and DATA 
-  **流(Stream)**：在客户端和服务器端的连接中，一个独立的、双向的、连续的帧的交换。
-  **Cloud-initiated Directives**：从云端发往客户端的指令，比如当用户通过Amazon Alexa App来调节设备上的音量，此时云端就会发送一个相应的操作音量的请求到产品上。
-  **Downchannel**：是一个在HTTP/2连接上的用来发送从云端到客户端指令的流，主要用来发送cloud-initiated指令和音频附件到客户端。

在创建与AVS的HTTP/2连接之前，
-  需要首先获取到一个access token，这个获取到的access token必须放置在每一次发送给AVS的event中。如果授权失败，则连接关闭。
-  其次选择一个HTTP/2 Client Library

这个连接可以用来控制所有的指令和事件(directives and events)，创建与AVS的连接需要：
-  发起一个GET请求到**指令路径(directives path)**来建立downchannel stream
-  利用POST请求在同一个连接上发送一个同步状态事件(SynchronizeState event)到**事件路径(events path)**来报告当前客户端组件的状态到AVS

当结束后，客户端便可以利用这个连接来
-  发送events和从AVS接收指令
-  从downchannel接收cloud-initiated指令

每一个event都会在各自的stream上发送，大部分情况下这些stream都会在收到指令和相应的音频附件后关闭

##### [组织一个访问AVS的HTTP/2请求](https://developer.amazon.com/public/solutions/alexa/alexa-voice-service/docs/avs-http2-requests)
请求结构图：
![Alt text](./alexa-mms.png)



#### 接口
| 	接口    		|    说明  |
| :----------- | -----------:|
| **SpeechRecognizer**  		| AVS核心接口 |
| **Alerts**      	| 设置、暂停、删除定时与闹钟的接口 |
| **AudioPlayer**     	| 管理和控制语音的播放 |
| **PlaybackController**  	| 控制播放队列的接口 |
| **Speaker**     	| 设备或者应用音量控制的接口 |
| **SpeechSynthesizer**     	| Alexa发音的接口 |
| **System**     	| 提供关于Alexa客户端信息的接口 |

#### 语音识别(SpeechRecognizer)

#### 语音识别状态
SpeechRecognizer具有以下状态：
-  **(闲置)IDLE**：在用户语音之前，SpeechRecognizer处于一个**idle**的状态
-  **识别中(RECOGNIZING)**：用户开始与client进行交互，捕捉到的音频流正在向AVS输送，此时状态由**idle**转化为**RECOGNIZING**，音频流停止之后，状态转为**BUSY**
-  **忙碌(BUSY)**：当正在处理请求时，状态为**BUSY**，此状态时不能开始其他的语音识别请求。当成功处理完成之后，状态变为**idle**，或者当需要用户额外的语音输入时状态变为**EXPECTING SPEECH**。
-  **等待其他语音(EXPECTING SPEECH)**：需要其他语音输入时为此状态
 ![Alt text](./speechrecognizer-state.png)

#### 识别事件(Recognize Event)
这个**Recognize Event**可以用来发送我们的语音到AVS，然后把语音转化为1个或者多个**指令(directives)**。这个event包括捕捉的音频和请求的参数，发送给AVS的event应该以**多部分消息(multipart message)**形式发送：第一部分是JSON形式的对象，第二部分是二进制音频流。
We encourage streaming (chunking) captured audio to the Alexa Voice Service to reduce latency; and recommend streaming 10ms of captured audio per chunk (320 bytes).
所有捕获的音频应该按照以下编码：
- 16位线性PCM
- 16kHz 采样率
- 单通道
- 小端字节顺序