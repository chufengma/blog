### Android开机启动流程

1. 启动Init进程：1>挂载设备  2>初始化  3>启动ZygoteInit 进程

2. Zygote进程： 1> 通过AppRuntime开启Zygote进程
			         2> 开启Java虚拟机，注册JNI。
	     		     3> 调用Java的ZygoteInit.main ，ZygoteJava：预加载+创建Socket；
			         4> 启动SystemServer进程
			         6> 通过socket等待客户端请求 ( ActivityManagerSerivce 与Zygote通信创建新的应用程序进程 )

 3. SystemServer进程：
			        1> 启动Binder线程池，这样就可以与其他进程进行通信。 
			        2> 创建SystemServiceManager用于对系统的服务进行创建、启动和生命周期管理。 
			        3> 启动各种系统服务。


### Binder机制

1. Binder是一个Android提供的一个Linux设备，是一种新的IPC方式
2. Binder通常包括：Service, Client, Binder，client与service通过binder设备进行跨进程通信
3. Android Framework内部大量服务使用Binder机制
  4. 特点：1> 内存只拷贝一次，效率高  2> 可添加UID/PID更安全  3>面向对象的实现方式，操作方便


### Android 应用启动过程

1. ActivityManagerService启动之后 根据Intent 启动Launcher
2. 点击Launcher任意一个Icon即调用Activity.startActivityForResult启动Activity
3. 调用 Instrumentation.execStartActivity  —> ActivityManagerNative.getDefault().startActivity:  
	   Instrumentation是管理Activity生命周期的仪表盘
	   ActivityManagerNative是该进程与ActivityManagerService通信的Proxy端
4. ActivityManagerService 启动Activity。
     1> 如果Activity所在进程不存在则请求Zygote进程fork子进程，改子进程入口是    ActivityThread.man。	
     2> 如果Activity进程存在，则处理回退栈等具体逻辑（待补充）
5. ActivityThread.main 所做的事情
6. 如果进程存在，启动Actvity的逻辑
