# 一，整体结构

---

<img src="../图片/后端代码结构.PNG" alt="后端代码结构" />

---

# 二，具体代码

---

## 1，config配置

### （1），WebSocketConfig

```java
package cn.lpc.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

@Configuration
public class WebSocketConfig {

​    */**
\*     *** *注入**Bean**服务器端点导出程序
\*     *** *@return* *ServerEndpointExporter
\*     **/
\*    @Bean
​    public ServerEndpointExporter serverEndpointExporter() {
​        return new ServerEndpointExporter();
​    }
}
```

---



## 2，controller层

### （1）,WebSocketController

```java
package cn.lpc.controller;

import cn.lpc.entity.Friends;
import cn.lpc.entity.Messages;
import com.alibaba.fastjson.JSON;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;

import javax.websocket.OnClose;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ConcurrentHashMap;


@Slf4j
@Component
@ServerEndpoint(value = "/websocket/{nickname}")
public class WebSocketController {

​    private Session session;


    */**
\*     *** *好友列表
\*     **/
\*    public static List<Friends> *friendsList* = new ArrayList<>();


    */**
\*     *** *定义并发**HashMap**存储好友**WebSocket**集合
\*     **/
\*    public static ConcurrentHashMap<String, WebSocketController> *webSocketSession* = new ConcurrentHashMap<>();


    @OnOpen
    public void onOpen(@PathParam(value = "nickname") String nickname, Session session) {
        // 设置session
        this.session = session;
        // Put添加当前类
        *webSocketSession*.put(nickname, this);
        // Add添加当前好友信息
        *friendsList*.add(Friends.*builder*().nickname(nickname).build());
        // 通知更新好友信息列表
        updateFriendInformationList();
        *log*.info("【WebSocket消息】有新的连接[{}], 连接总数:{}", nickname, *webSocketSession*.size());

​    }

​    */**
\*     *** *通知更新好友信息列表
\*     **/
\*    private synchronized void updateFriendInformationList() {
​        *webSocketSession*.forEach((key, val) -> {
​            // 初始化存储属于自己的好友列表 排除自己
​            List<Friends> friends = new ArrayList<>();
​            // 迭代所有好友列表
​            *friendsList*.forEach((friend) -> {
​                // 在 所有好友信息列表 中验证非自己
​                if (!friend.getNickname().equals(key)) {
​                    // 追加非自己的好友信息
​                    friends.add(friend);
​                }
​            });
​            // 发送消息
​            *sendP2PMessage*(key, JSON.*toJSONString*(Messages.*builder*().type("updateFriendsList").receiveNickname(key).messages(friends).build()));
​        });
​    }

​    @OnMessage
​    public void onMessage(@PathParam(value = "nickname") String nickname, String message) {
​        *log*.info("【WebSocket消息】 收到客户端[{}] 发送消息:{} 连接总数:{}", nickname, message, *webSocketSession*.size());
​        // 验证消息内容
​        if (StringUtils.*hasLength*(message)) {
​            try {
​                // 消息内容转消息对象
​                Messages messages = JSON.*parseObject*(message, Messages.class);
​                // 发送消息
​                *sendP2PMessage*(messages.getReceiveNickname(), message);
​            } catch (Exception e) {
​                *log*.error("WebSocket消息异常:", e);
​            }
​        }
​    }

​    @OnClose
​    public void onClose(@PathParam(value = "nickname") String nickname) {
​        *friendsList*.remove(*friendsList*.stream().filter((friends -> friends.getNickname().equals(nickname))).findAny().orElse(null));
​        *webSocketSession*.remove(nickname);
​        // 通知更新好友信息列表
​        updateFriendInformationList();
​        *log*.info("【WebSocket消息】客户端[{}]连接断开, 剩余连接总数:{}", nickname, *webSocketSession*.size());
​    }

​    */**
\*     *** *点对点发送
\*     **/
\*    public static synchronized void sendP2PMessage(String nickname, String message) {
​        *log*.info("【WebSocket消息】点对点发送消息, nickname={} , message={}", nickname, message);
​        try {
​            *webSocketSession*.get(nickname).session.getBasicRemote().sendText(message);
​        } catch (IOException e) {
​            *log*.error("点对点发送异常:", e);
​        }
​    }

}
```



## 3，entity实体包

### （1），Friends类

```java
package cn.lpc.entity;

import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Friends {
    */**
\*     *** *好友昵称
\*     **/
    private String nickname;
}
```

---



### （2），Messages类

```java
package cn.lpc.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Messages {
    */**
\*     *** *消息类型
\*     **/
\*    private String type;
    */**
\*     *** *发送好友昵称
\*     **/
\*    private String sendNickname;
    */**
\*     *** *接收好友昵称
\*     **/
\*    private String receiveNickname;
    */**
\*     *** *消息内容
\*     **/
\*    private Object messages;
}
```

---

### （3），Result类

```java
package cn.lpc.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Result {
    */**
\*     *** *状态 （**status**）
\*     **/
\*    private int status;
    */**
\*     *** *返回信息（**message**）
\*     **/
\*    private String message;
    */**
\*     *** *返回数据（**data**）
\*     **/
\*    private Object data;
}
```

---



## 4，enums枚举包

---

### 1, ResultsEnum类：

```java
package cn.lpc.enums;

public class ResultsEnum {
    */**
\*     *** *失败状态* *FAIL_STATUS_CODE
\*     **/
\*    public static final int *FAIL_STATUS_CODE* = -200;
    */**
\*     *** *成功状态* *SUCCESS_STATUS_CODE
\*     **/
\*    public static final int *SUCCESS_STATUS_CODE* = 200;
    */**
\*     *** *失败返回信息* *FAIL_MESSAGE
\*     **/
\*    public static final String *FAIL_MESSAGE* = "Failure";
    */**
\*     *** *成功返回信息* *SUCCESS_MESSAGE
\*     **/
\*    public static final String *SUCCESS_MESSAGE* = "Success";
}
```

---

## 5,vo.results状态返回

---

### 1，Results类：

---

```java
package cn.lpc.vo.results;

import cn.lpc.entity.Result;
import cn.lpc.enums.ResultsEnum;
import com.alibaba.fastjson.JSONObject;

public class Results<T> {

​    */**
\*     *** *自定义 生成结果* *0 Parameter
\*     **
\*     *** *@param* *status*  *状态码
\*     *** *@param* *message* *信息
\*     *** *@param* *data*    *数据
\*     *** *@return* *Result
\*     **/
\*    public static <T> Result buildResults(int status, String message, T data) {
​        Result result = new Result();
​        result.setStatus(status);
​        result.setMessage(message);
​        String jsonString = JSONObject.*toJSONString*(data);
​        if (jsonString.startsWith("\"") && jsonString.endsWith("\"")) {
​            jsonString = jsonString.substring(1, jsonString.length() - 1);
​        }
​        result.setData(jsonString);
​        return result;
​    }


    */**
\*     *** *生成成功结果* *3 Parameter
\*     **
\*     *** *@param* *status*  *状态码
\*     *** *@param* *message* *信息
\*     *** *@param* *data*    *数据
\*     *** *@return* *Result
\*     **/
\*    public static <T> Result success(int status, String message, T data) {
        return *buildResults*(status, message, data);
    }


    */**
\*     *** *生成成功结果* *2 Parameter
\*     **
\*     *** *@param* *status*  *状态码
\*     *** *@param* *message* *信息
\*     *** *@return* *Result
\*     **/
\*    public static Result success(int status, String message) {
        return *buildResults*(status, message, true);
    }


    */**
\*     *** *生成成功结果* *1 Parameter
\*     **
\*     *** *@param* *data* *数据
\*     *** *@return* *Result
\*     **/
\*    public static <T> Result success(T data) {
        return *buildResults*(ResultsEnum.*SUCCESS_STATUS_CODE*, ResultsEnum.*SUCCESS_MESSAGE*, data);
    }

​    */**
\*     *** *生成成功结果* *0 Parameter
\*     **
\*     *** *@return* *Result
\*     **/
\*    public static <T> Result success() {
​        return (Result) *success*(true);
​    }



​    */**
\*     *** *生成失败结果* *3 Parameter
\*     **
\*     *** *@param* *status*  *状态码
\*     *** *@param* *message* *信息
\*     *** *@param* *data*    *数据
\*     *** *@return* *Result
\*     **/
\*    public static <T> Result error(int status, String message, T data) {
​        return *buildResults*(status, message, data);
​    }

​    */**
\*     *** *生成失败结果* *2 Parameter
\*     **
\*     *** *@param* *status*  *状态码
\*     *** *@param* *message* *信息
\*     *** *@return* *Result
\*     **/
\*    public static Result error(int status, String message) {
​        return *buildResults*(status, message, false);
​    }


    */**
\*     *** *生成失败结果* *1 Parameter
\*     **
\*     *** *@param* *data* *数据
\*     *** *@return* *Result
\*     **/
\*    public static <T> Result error(T data) {
        return *buildResults*(ResultsEnum.*FAIL_STATUS_CODE*, ResultsEnum.*FAIL_MESSAGE*, data);
    }



    */**
\*     *** *生成失败结果* *0 Parameter
\*     **
\*     *** *@return* *Result
\*     **/
\*    public static Result error() {
        return *buildResults*(ResultsEnum.*FAIL_STATUS_CODE*, ResultsEnum.*FAIL_MESSAGE*, false);
    }
}
```

---

这是后端代码
