# 通信过程控制

## 通信需要控制的部分

* 通信发码
* 发码后判断回码
* 发码后判断回码超时----另外需要考虑耳机收码后的需要怎么返回
* 发码的时机，----定时心跳包加临时命令发送
* 累计误码处理----直接进入unconnected模式

## 程序实现

### 主处理函数

### 模式：

* 普通模式，心跳包模式
* 临时发码模式，普通模式下设置标志位请求临时发码模式，普通模式发送完当前数据后进入临时发码模式，之后超时或者主动退出临时发码模式则进入普通模式。临时发码模式必须在短时间内完成发码并退出
* 模式切换都建立在完整的发码条件下，也就是必须等待当前发码结束后才能切换

### 发送状态：

* 空闲状态
  * 存在可发送命令，直接填入，然后进入发码模式，不存在则空闲
* 发码状态
  * 命令已发送，等待收码，
* 收码状态
  * 以收到回码，判断后进入空闲模式

* 误码或者超时处理
  * 收码超时同样进入空闲状态，但是错误计数会累加
  * 每次收码OK后错误清零

### 命令写入

* 临时模式下，按键或者霍尔状态处理函数专门处理，并不断轮询，代码可以很复杂
* 普通模式下，定时器写入心跳命令
* 