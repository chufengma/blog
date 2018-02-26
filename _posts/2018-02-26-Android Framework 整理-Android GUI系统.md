### View 绘制的来龙去脉 - 什么时候渲染，掉帧的原理：

1. 调用 VIewRoot.performTraversals  将会通知View开始重新计算布局，重新渲染。内部通过向Choreographer发送渲染请求，Choreographer将会在每一次帧绘制周期内响应渲染流程。极短时间的performTraversals将只会触发 VIewRoot.performTraversals 一次
2. Choreographer 接受时间帧来回调所有接口使其允许View渲染，一般是60hz(1秒60帧，一帧约15ms)，所有的渲染请求在它提供的回调里面执行，在执行的时候将判断从开始到回调的时间，如果时间长了将跳过该帧的渲染
3. 当一个动画操作的绘制超过一帧的时间（16ms）则会出现掉帧现象。
4. View绘制的来龙去脉-测量,布局,绘制
5. 父布局将子布局的LayoutParams+父布局本身的约束 组装成 MeasureSpec 传递给子View，使其实现响应的测量，所有子布局测量好了之后父布局需要调用setMeasureDimention来设置自己的测量结果		
6. View回执的来龙去脉-滑动：VelocityTracker，OverScroller
7. View绘制的来龙去脉- Surface, WIndow, SurfacceFlinger,Layer

                 
