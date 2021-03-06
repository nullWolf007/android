# EventBus的使用3.0

## EventBus的优势
### 简化组件间的通信
1. 对发送事件和接受事件解耦
2. 可以在activity，fragment，后台线程间执行
3. 避免了复杂的和容易出错的依赖和生命周期问题

## EventBus前提知识
### EventBus的四种ThreadMode(线程模型)
* POSTING(默认)：事件从哪个线程发布出来，事件处理函数就会在这个线程中运行。发布事件和接受事件在同一个线程。在线程模型为POSTING的事件处理函数中尽量避免执行耗时操作，会阻塞事件的传递，甚至有可能会引起ANR
* MAIN:事件的处理会在UI线程中执行，事件处理时间不能太长，否则会ANR
* BACKGROUND:如果事件时在UI线程中发布出来的，那么该事件处理函数会在新的线程中运行，如果事件本来就是子线程中发布出来的，那么事件处理函数直接在发布事件的线程中执行，在此事件处理函数中禁止进行UI更新操作
* ASYNC:无论事件在哪个线程中发布，该事件的梳理函数都会在新建的子线程中执行，同样，此事件处理函数禁止进行UI更新操作

## EventBus的基本使用
1. 自定义一个事件类(有的时候不需要)
```java
public class MessageEvent {
    ...
}
```
2. 发送事件
```java
EventBus.getDefault().post(messageEvent);
```
3. 在需要订阅事件的地方注册事件
```java
EventBus.getDefault().register(this);
```
4. 处理事件
```java
@Subscribe(threadMode = ThreadMode.MAIN)
public void XXX(MessageEvent messageEvent) {
    ...
}
```
5. 取消订阅事件
```java
EventBus.getDefault().unregister(this);
```
