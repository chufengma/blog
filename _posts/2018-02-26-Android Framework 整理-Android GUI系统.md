
### Android activity启动之后ui的绘制
1. ActivityThread启动Activity
2. Activity实例化之后 初始化Window，获取WindowManager，创建DectorView
3. ActivityThread执行ActivityResumed, 并调用WindowManager.addView，此时创建viewrootimpl，添加窗口，add dectorview，开始绘制ui

                 
### windowmanager addview过程分析：
这是一个应用程序进程向wms发起添加窗口操作并执行绘制的过程：
1. WindowManager.addView，此过程创建 ViewRootImpl，并在内部setview，向wms申请添加窗口，分配surface，此时才完成正在添加窗口的操作，随后立即发生第一次绘制
2. ViewRootImpl是Android控件系统的大中枢，负责和WMS交互，控件树布局测量绘制，负责事件的接受和分发。
3. viewrootimpl的第一次绘制：先对窗口预测量，然后根据预测量的结果向wms请求窗口尺寸，然后根据wms约定的窗口尺寸做最终测量，测量之后开始布局控件树布局，最后绘制控件树。


### View 绘制的来龙去脉 - 什么时候渲染，掉帧的原理：

1. 调用 VIewRoot.performTraversals  将会通知View开始重新计算布局，重新渲染。内部通过向Choreographer发送渲染请求，Choreographer将会在每一次帧绘制周期内响应渲染流程。极短时间的performTraversals将只会触发 VIewRoot.performTraversals 一次
2. Choreographer 接受时间帧来回调所有接口使其允许View渲染，一般是60hz(1秒60帧，一帧约15ms)，所有的渲染请求在它提供的回调里面执行，在执行的时候将判断从开始到回调的时间，如果时间长了将跳过该帧的渲染
3. 当一个动画操作的绘制超过一帧的时间（16ms）则会出现掉帧现象。
4. View绘制的来龙去脉-测量,布局,绘制
5. 父布局将子布局的LayoutParams+父布局本身的约束 组装成 MeasureSpec 传递给子View，使其实现响应的测量，所有子布局测量好了之后父布局需要调用setMeasureDimention来设置自己的测量结果		
6. View回执的来龙去脉-滑动：VelocityTracker，OverScroller
7. View绘制的来龙去脉- Surface, WIndow, SurfacceFlinger,Layer


