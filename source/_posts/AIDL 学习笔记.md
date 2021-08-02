---
title: AIDL 学习笔记总结
date: 2021-08-01 23:00:44
tags: AIDL
categories: AIDL
description:  AIDL 学习笔记总结。
---

# 一、应用场景

### AIDL可以解决什么问题？

- 可以实现多个应用程序共享同一个Service的功能，比如：IM服务可以提供给多个APP使用，先在推送基本都是采取这种方案
- 可以跨进程调用服务里的方法

# 二 、是什么

首先进程间通信（IPC，Inter-process Communication）的方式有很多，例如Bundle、文件共享、AIDL、Messenger、ContentProvider以及Socket，方法是多种多样，而我们更应该根据自己的情况去选择合适的方式，下面列举出每种方式的适用场景：



| 名称            | 优点                                           | 缺点                                                        | 适用场景                                             |
| --------------- | ---------------------------------------------- | ----------------------------------------------------------- | ---------------------------------------------------- |
| Bundle          | 简单易用                                       | 只能传输Bundle支持的数据类型                                | 四大组件间的进程通信                                 |
| 文件共享        | 简单易用                                       | 不适合高并发场景，无法及时通信                              | 无高并发访问，交换简单数据，实时性不高               |
| **AIDL**        | **功能强大，支持一对多并发通信，支持及时通信** | **使用复杂，需要处理线程同步**                              | **一对多且有远程调用需求**                           |
| Messenger       | 功能一般，支持一对多串行，支持实时通信         | 不能很好处理高并发，不支持远程调用，数据只能使用Message传输 | 低并发的一对多即时通信，无须直接返回结果，无远程调用 |
| ContentProvider | 数据访问方面功能强大，支持一对多并发数据共享   | 受约束的AIDL，主要提供数据的增删改查                        | 一对多进程间数据共享                                 |
| Socket          | 功能强大，支持一对多并发实时通信               | 实现复杂，不支持直接的远程调用                              | 网络数据交互                                         |

*注：来自《Android开发艺术探究》*

AIDL出现的目的就是为了解决一对多并发及时通信了，虽然socket也支持，但其更多应用于网络数据交互，也适合多个手机，不同终端进行通信。所以在Android中，google为我们提供了AIDL来方便开发者实现该功能。





# 三、支持的数据类型

在进程间通信中，对象的引用是无法传递的，因为其内存地址都不在一个区域内，所以数据的传输都需要序列化后才行，基础数据类型自然是没问题，系统提供的ArrayList以及HashMap也实现了序列化，所以对于自定义的对象实现序列化就只能实现serializable或者parcelable接口，其中前者是java提供的，后者是Android提供的且效率更高，所以在之后的数据序列化中都只使用实现parcelable接口的方法即可，总的来说**支持的数据类型包括：**

- 基本数据类型：int、long、char、boolean、double等
- 系统已实现类型：String、CharSequence、ArrayList、HashMap（后两个的每个元素也得被AIDL支持才行）
- 正确实现parcelable接口的对象
- 所有AIDL本身



**定向tag:**

Java 中的基本类型和 String ，CharSequence 的定向 tag **默认且只能是 in** 。

对于非基础数据类型的类型，在aidl中作为参数使用时有三个标记，分别时in、out、inout，表示数据的流向：

| 名称  | 含义                                                         |
| ----- | ------------------------------------------------------------ |
| in    | 表示能读取到传入的对象中的值，而修改后不会修改传入对象的内容 |
| out   | 表示读取到传入的对象为空，修改后会把传入对象的内容改为修改的内容 |
| inout | 表示既能读取到对象，又能修改对象，效果就好比统一进程中对象的传递一样 |

请注意，**请不要滥用定向 tag** ，而是要根据需要选取合适的——要是不管三七二十一，全都一上来就用 inout ，等工程大了系统的开销就会大很多——因为排列整理参数的开销是很昂贵的。

扩展阅读：[你真的理解AIDL中的in，out，inout么？](https://blog.csdn.net/luoyanglizi/article/details/51958091)

**两种AIDL文件：**在我的理解里，所有的AIDL文件大致可以分为两类。一类是用来定义parcelable对象，以供其他AIDL文件使用AIDL中非默认支持的数据类型的。一类是用来定义方法接口，以供系统使用来完成跨进程通信的。可以看到，两类文件都是在“定义”些什么，而不涉及具体的实现，这就是为什么它叫做“Android接口定义语言”。
注：所有的非默认支持数据类型必须通过第一类AIDL文件定义才能被使用。

# 四、如何使用

根据介绍，总结为以下三部分：

- 定义AIDL接口
- 在服务端返回其Binder以及所需实现的接口
- 在客户端拿到Binder进行调用

流程小结：在服务端实现AIDL中定义的方法接口的具体逻辑，然后在客户端调用这些方法接口，从而达到跨进程通信的目的。

1. 定义AIDL接口

    - ##### 创建aidl目录



```java
IBinder messageSender = new MessageSender.Stub() {

        @Override
        public void sendMessage(MessageModel messageModel) throws RemoteException {
            Log.e(TAG, "messageModel: " + messageModel.toString());
        }

        @Override
        public void registerReceiveListener(MessageReceiver messageReceiver) throws RemoteException {
            listenerList.register(messageReceiver);
        }

        @Override
        public void unregisterReceiveListener(MessageReceiver messageReceiver) throws RemoteException {
            listenerList.unregister(messageReceiver);
        }

        @Override
        public boolean onTransact(int code, Parcel data, Parcel reply, int flags) throws RemoteException {
            /**
             * 包名验证方式
             */
            String packageName = null;
            String[] packages = getPackageManager().getPackagesForUid(getCallingUid());
            if (packages != null && packages.length > 0) {
                packageName = packages[0];
            }
            if (packageName == null || !packageName.startsWith("com.example.aidl")) {
                Log.d("onTransact", "拒绝调用：" + packageName);
                return false;
            }

            return super.onTransact(code, data, reply, flags);
        }
    };
```

实现的service为AIDL定义的接口，并在OnBind方法中进行关联。

```java
  @Override
    public IBinder onBind(Intent intent) {
        //自定义permission方式检查权限
        if (checkCallingOrSelfPermission("com.example.aidl.permission.REMOTE_SERVICE_PERMISSION") == PackageManager.PERMISSION_DENIED) {
            return null;
        }
        return messageSender;
    }
```

扩展阅读：

[Android自定义权限使用总结](https://www.jianshu.com/p/b60cde583237)

注意点：

- Android不允许两个不同的应用定义一个相同名字的权限（除非这两个应用拥有相同的签名），所以在命名的时候，需要特别注意。
- 拥有相同自定义权限的软件必须使用同样的签名，否则后一个程序无法安装。

所以这里是否需要自定义，需要根据业务场景来确定。



A.服务端：

- 创建一个**aidl文件**；
- 创建一个**Service**，实现AIDL的接口函数并暴露AIDL接口。

B.客户端：

- 通过**bindService**绑定服务端的Service；
- 绑定成功后，将服务端返回的Binder对象**转化**成AIDL接口所属的类型，进而调用相应的AIDL中的方法。

> **总结**：服务端里的某个Service给和它绑定的特定客户端进程提供Binder对象，客户端通过AIDL接口的静态方法asInterface() 将Binder对象转化成AIDL接口的代理对象，通过这个代理对象就可以发起远程调用请求。



思考：如果要实现双向通信，那么就需要将AIDL的接口都实现，即依赖相同的部分。



## 可能产生ANR的情形

A.客户端：

- 调用服务端的方法是运行在服务端的Binder线程池中，若**主线程**所调用的方法里执行了较**耗时**的任务，同时会导致客户端线程长时间阻塞，易导致客户端ANR。
- 在**onServiceConnected()**和**onServiceDisconnected()**里直接调用服务端的耗时方法，易导致客户端ANR。

B.服务端：

- 服务端的方法本身就运行在服务端的**Binder线程**中，可在其中执行耗时操作，而**无需再开启子线程**。
- **回调客户端Listener的方法**是运行在客户端的Binder线程中，若所调用的方法里执行了较**耗时**的任务，易导致服务端ANR。

> 解决客户端频繁调用服务器方法导致性能极大损耗的办法：实现**观察者模式**。
>
> 即当客户端关注的数据发生变化时，再让服务端通知客户端去做相应的业务处理。

## 解注册失败的问题

- 原因: Binder进行对象传输实际是通过序列化和反序列化进行，即Binder会把客户端传递过来的对象**重新转化**并生成一个新的对象，虽然在注册和解注册的过程中使用的是同一个客户端传递的对象，但经过Binder传到服务端后会生成两个不同的对象。另外，多次跨进程传输的同一个客户端对象会在服务端生成**不同**的对象，但它们在底层的Binder对象是**相同**的。
- 解决办法：当客户端解注册的时候，遍历服务端所有的Listener，找到和解注册Listener具有相同的Binder对象的服务端Listener，删掉即可。

> 需要用到**RemoteCallBackList**：Android系统专门提供的用于删除跨进程listener的接口。其内部自动实现了线程同步的功能。

# aidl传输数据的大小限制



# 一个Service管理多个AIDL接口

https://blog.csdn.net/lyl0530/article/details/81839503

Binder连接池，以及怎样实现一个Service管理多个ADIL接口

# AIDL终极篇之AIDL架构设计

https://blog.csdn.net/wangqiubo2010/article/details/78625846

其他开源框架

https://github.com/Xiaofei-it/Hermes

A smart, novel and easy-to-use framework for Android Inter-Process Communication (IPC). （简单易用的安卓进程间通信IPC框架）



# FAQ

1. 难点：

   服务端如何处理分发和并发

# android – 如何使用AIDL远程服务来处理不同客户端的并发请求

http://www.voidcn.com/article/p-zhjgplln-btp.html



参考源码：

https://github.com/arvinljw/AndroidArt

参考资料：

[Android Binder设计与实现-设计篇](https://blog.csdn.net/universus/article/details/6211589)(强烈推荐阅读)

[Android学习感悟之进程间通信——AIDL详解](https://www.jianshu.com/p/cb8fa31a459f)

[Binder学习指南](http://weishu.me/2016/01/12/binder-index-for-newer/)

[[Android开发艺术探索](http://www.amazon.cn/Android开发艺术探索-任玉刚/dp/B014HV1X3K/ref=sr_1_1?ie=UTF8&qid=1451616304&sr=8-1&keywords=Android开发艺术探索)读书笔记: IPC机制](https://wujingchao.github.io/2016/01/04/art-of-android-development-notes-ipc/)

https://gank.io/post/5e52801241bd447e3b978092