# 一，websocket

## 1，什么是WebSocket?

---

​	WebSocket协议是==基于TCP==的一种新的网络协议。它实现了浏览器与服务器全双工(full-duplex)通信——允许服务器主动发送信息给客户端。

- WebSocket是HTML5下一种新的协议（websocket协议本质上是一个基于tcp的协议）。
- 它实现了浏览器与服务器全双工通信，能更好的节省服务器资源和带宽并达到实时通讯的目的。
- Websocket是一个**持久化**的协议。
- WebSocket是双向通信协议，模拟Socket协议，可以双向发送或接受信息。
- 在WebSocket API中，==浏览器和服务器只需要完成一次握手==，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

![img](https://img-blog.csdn.net/20180510223926952?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21vc2hvd2dhbWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

---

## 2，websocket原理

---

1. websocket约定了一个通信的规范，通过一个==握手的机制==，客户端和服务器之间能建立一个类似tcp的连接，从而方便它们之间的通信。
2. websocket是一种全新的协议，不属于http无状态协议，协议名为"ws"。
3. websocket与http的关系

![img](https://img-blog.csdnimg.cn/45259e81df6b4455bdcfcad460a3a8bf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAb2hhbmHvvIE=,size_20,color_FFFFFF,t_70,g_se,x_16)

---

## 3，websocket形象比喻

---

​	==websocket 是一个基于应用层的网络协议，建立在tcp 协议之上==，和 http 协议可以说是兄弟的关系，但是这个兄弟有点依赖 http ，为什么这么说呢？我们都知道 HTTP 实现了三次握手来建立通信连接，实际上 websocket 的创始人很聪明，他不想重复的去造轮子，反正我兄弟已经实现了握手了，我干嘛还要重写一套呢？先让它去冲锋陷阵呢，我坐收渔翁之利不是更香 吗，**所以一般来说，我们会先用 HTTP 先进行三次握手，再向服务器请求升级为websocket 协议**，这就好比说，**嘿兄弟你先去给我排个队占个坑位建个小房子，到时候我在把这房子改造成摩天大楼。**而且一般来说 80 和 443 端口一般 web 服务端都会外放出去，这样可以有效的避免防火墙的限制。当然，你创建的 websocket 服务端进程的端口也需要外放出去。

---

## 4，websocket用途

​	WebSocket是Html5平台的一个重要组件也是开发者强有力的工具。简单的说，你需要WebSocket创建世界级的web应用。它弥补了http不适合实时通信的重大缺陷。**异步、双向通信模式，通过传输层协议使WebSocket具有普遍灵活性。想象一下你能用WebSocket创建正真实实时应用的所有方式。比如聊天、协作文档编辑、大规模多人在线游戏（MMO）,股票交易应用等等。**

​	WebSocket是一个协议，但也有一个**WebSocket API，这让你的应用去控制WebSocket的协议去响应被服务端触发的事件。**API是W3C开发，协议是IETE制定。现代浏览器支持WebSocket API，这包括使用全双工和双向链接的方法和特性。让你执行像打开关闭链接、发送接收消息、监听服务端事件等必要操作。

---

## 5，WebSocket API

---

### （1），websocket api简介

​	**WebSocket API其实就是一个使用WebSocket协议的接口，通过它来建立全双工通道来收发消息，简单易学，要连接远程服务器，只需要创建一个==WebSocket对象实体==，并传入一个服务端的URL。**在客户端和服务端一开始握手的期间，http协议升级到WebSocket协议就建立了连接，底层都是TCP协议。**一旦建立连接，通过WebSocket接口可以反复的发送消息。**在你的代码里面，你可以使用==异步事件==监听连接生命周期的每个阶段。

​	WebSocket API是纯事件驱动，一旦建立全双工连接，当服务端给客户端发送数据或者资源，它能自动发送状态改变的数据和通知。所以你不需要为了状态的更新而去轮训Server，在客户端监听即可。

---

## 6，websocket事件

---

### （1），事件简介-监听事件

​	WebSocket API是纯事件驱动，**通过监听事件可以处理到来的数据和改变的链接状态**。客户端不需要为了更新数据而轮训服务器。服务端发送数据后，消息和事件会异步到达。WebSocket编程遵循一个**异步编程模型，只需要对WebSocket对象增加回调函数就可以监听事件**。你也可以使用addEventListener()方法来监听。而一个WebSocket对象分四类不同事件。

### （2），引入maven依赖

---



​	**SpringBoot2.0对WebSocket的支持简直太棒了，直接就有包可以引入：**

```java
	<dependency>  
       <groupId>org.springframework.boot</groupId>  
       <artifactId>spring-boot-starter-websocket</artifactId>  
   </dependency> 
```

---



### （3），WebSocketConfig配置

---



​	**启用WebSocket的支持也是很简单，几句代码搞定：**

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

/**

 * 开启WebSocket支持

 * @author zhengkai.blog.csdn.net
   */
   @Configuration  
   public class WebSocketConfig {  

   @Bean  
   public ServerEndpointExporter serverEndpointExporter() {  
       return new ServerEndpointExporter();  
   }  

} 
```

---



### （4），WebSocketServer

---



1. 因为WebSocket是类似客户端服务端的形式(采用ws协议)，那么这里的**WebSocketServer其实就相当于一个ws协议的Controller。**
2. **直接`@ServerEndpoint("/imserver/{userId}")` 、`@Component`启用即可，然后在里面实现`@OnOpen`开启连接，`@onClose`关闭连接，`@onMessage`接收消息等方法。**
3. **新建一个`ConcurrentHashMap` webSocketMap 用于接收当前userId的WebSocket**，方便IM之间对userId进行推送消息。`单机版`实现到这里就可以。
4. `集群版`（多个ws节点）还需要借助mysql或者redis等进行处理，改造对应的`sendMessage`方法即可。

**源代码：**

```java
package com.softdev.system.demo.config;

import java.io.IOException;
import java.util.concurrent.ConcurrentHashMap;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import org.apache.commons.lang.StringUtils;
import org.springframework.stereotype.Component;
import cn.hutool.log.Log;
import cn.hutool.log.LogFactory;


/**

 * @author zhengkai.blog.csdn.net
   */
   @ServerEndpoint("/imserver/{userId}")
   @Component
   public class WebSocketServer { //相当于webcontroller

   static Log log=LogFactory.get(WebSocketServer.class);
   /**静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。*/
   private static int onlineCount = 0;
   /**concurrent包的线程安全Set，用来存放每个客户端对应的MyWebSocket对象。*/
   private static ConcurrentHashMap<String,WebSocketServer> webSocketMap = new ConcurrentHashMap<>();
   /**与某个客户端的连接会话，需要通过它来给客户端发送数据*/
   private Session session;
   /**接收userId*/
   private String userId="";

   /**

    * 连接建立成功调用的方法*/
      @OnOpen
      public void onOpen(Session session,@PathParam("userId") String userId) {
       this.session = session;
       this.userId=userId;
       if(webSocketMap.containsKey(userId)){
           webSocketMap.remove(userId);
           webSocketMap.put(userId,this);
           //加入set中
       }else{
           webSocketMap.put(userId,this);
           //加入set中
           addOnlineCount();
           //在线数加1
       }

       log.info("用户连接:"+userId+",当前在线人数为:" + getOnlineCount());

       try {
           sendMessage("连接成功");
       } catch (IOException e) {
           log.error("用户:"+userId+",网络异常!!!!!!");
       }
      }

   /**

    * 连接关闭调用的方法
      */
      @OnClose
      public void onClose() {
      if(webSocketMap.containsKey(userId)){
          webSocketMap.remove(userId);
          //从set中删除
          subOnlineCount();
      }
      log.info("用户退出:"+userId+",当前在线人数为:" + getOnlineCount());
      }

   /**

    * 收到客户端消息后调用的方法
      *
    * @param message 客户端发送过来的消息*/
      @OnMessage
      public void onMessage(String message, Session session) {
       log.info("用户消息:"+userId+",报文:"+message);
       //可以群发消息
       //消息保存到数据库、redis
       if(StringUtils.isNotBlank(message)){
           try {
               //解析发送的报文
               JSONObject jsonObject = JSON.parseObject(message);
               //追加发送人(防止串改)
               jsonObject.put("fromUserId",this.userId);
               String toUserId=jsonObject.getString("toUserId");
               //传送给对应toUserId用户的websocket
               if(StringUtils.isNotBlank(toUserId)&&webSocketMap.containsKey(toUserId)){
                   webSocketMap.get(toUserId).sendMessage(jsonObject.toJSONString());
               }else{
                   log.error("请求的userId:"+toUserId+"不在该服务器上");
                   //否则不在这个服务器上，发送到mysql或者redis
               }
           }catch (Exception e){
               e.printStackTrace();
           }
       }
      }

   /**
    *

    * @param session
    * @param error
      */
      @OnError
      public void onError(Session session, Throwable error) {
      log.error("用户错误:"+this.userId+",原因:"+error.getMessage());
      error.printStackTrace();
      }
      /**
    * 实现服务器主动推送
      */
      public void sendMessage(String message) throws IOException {
      this.session.getBasicRemote().sendText(message);
      }

/**

 * 发送自定义消息
 * */
   public static void sendInfo(String message,@PathParam("userId") String userId) throws IOException {
    log.info("发送消息到:"+userId+"，报文:"+message);
    if(StringUtils.isNotBlank(userId)&&webSocketMap.containsKey(userId)){
        webSocketMap.get(userId).sendMessage(message);
    }else{
        log.error("用户"+userId+",不在线！");
    }
   }

public static synchronized int getOnlineCount() {
    return onlineCount;
}

public static synchronized void addOnlineCount() {
    WebSocketServer.onlineCount++;
}

public static synchronized void subOnlineCount() {
    WebSocketServer.onlineCount--;
}

}
```

---



### （5），消息推送

---



​	**至于推送新信息，可以再自己的Controller写个方法调用WebSocketServer.sendInfo();即可**：



* ```java
   import com.softdev.system.demo.config.WebSocketServer;
   import org.springframework.http.ResponseEntity;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.web.servlet.ModelAndView;
   import java.io.IOException;
      
   /**
      
    
    * WebSocketController
    
  * @author zhengkai.blog.csdn.net
    */
    @RestController
    public class DemoController {
        
    @GetMapping("index")
    public ResponseEntity<String> index(){
        return ResponseEntity.ok("请求成功");
    }
  
    @GetMapping("page")
    public ModelAndView page(){
        return new ModelAndView("websocket");
    }
  
    @RequestMapping("/push/{toUserId}")
    public ResponseEntity<String> pushToWeb(String message, @PathVariable String toUserId) throws IOException {
        WebSocketServer.sendInfo(message,toUserId);
        return ResponseEntity.ok("MSG SEND SUCCESS");
    }
  }
  ```
  
  




​     
​    


---

 

 ### （6），页面发起（JavaScript）

---

 ​	**页面用js代码`调用websocket`，当然，太古老的浏览器是不行的，一般新的浏览器或者谷歌浏览器是没问题的。还有一点，记得协议是`ws`的，如果使用了一些路径类，可以replace(“http”,“ws”)来替换协议。**

 <!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8">
     <title>websocket通讯</title>
 </head>
 <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
 <script>
     var socket;
     function openSocket() {
         if(typeof(WebSocket) == "undefined") {
             console.log("您的浏览器不支持WebSocket");
         }else{
             console.log("您的浏览器支持WebSocket");
             //实现化WebSocket对象，指定要连接的服务器地址与端口  建立连接
             //等同于socket = new WebSocket("ws://localhost:8888/xxxx/im/25");
             //var socketUrl="${request.contextPath}/im/"+$("#userId").val();
             var socketUrl="http://localhost:9999/demo/imserver/"+$("#userId").val();
             socketUrl=socketUrl.replace("https","ws").replace("http","ws");
             console.log(socketUrl);
             if(socket!=null){
                 socket.close();
                 socket=null;
             }
             socket = new WebSocket(socketUrl);
             //打开事件
             socket.onopen = function() {
                 console.log("websocket已打开");
                 //socket.send("这是来自客户端的消息" + location.href + new Date());
             };
             //获得消息事件
             socket.onmessage = function(msg) {
                 console.log(msg.data);
                 //发现消息进入    开始处理前端触发逻辑
             };
             //关闭事件
             socket.onclose = function() {
                 console.log("websocket已关闭");
             };
             //发生了错误事件
             socket.onerror = function() {
                 console.log("websocket发生了错误");
             }
         }
     }
     function sendMessage() {
         if(typeof(WebSocket) == "undefined") {
             console.log("您的浏览器不支持WebSocket");
         }else {
             console.log("您的浏览器支持WebSocket");
             console.log('{"toUserId":"'+$("#toUserId").val()+'","contentText":"'+$("#contentText").val()+'"}');
             socket.send('{"toUserId":"'+$("#toUserId").val()+'","contentText":"'+$("#contentText").val()+'"}');
         }
     }
 </script>
 <body>
 <p>【userId】：<div><input id="userId" name="userId" type="text" value="10"></div>
 <p>【toUserId】：<div><input id="toUserId" name="toUserId" type="text" value="20"></div>
 <p>【toUserId】：<div><input id="contentText" name="contentText" type="text" value="hello websocket"></div>
 <p>【操作】：<div><a onclick="openSocket()">开启socket</a></div>
 <p>【操作】：<div><a onclick="sendMessage()">发送消息</a></div>
 </body>
 </html>
---

