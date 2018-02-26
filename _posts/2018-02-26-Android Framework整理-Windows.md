### WindowManager 对window的管理

windowmanager管理android系统的window，window是一个显示界面的抽象，每一个Window至少对应一个Surface，每个Surface 对应一个View的树，windowManager管理若干Surface，在显示的时候将要显示的区域合成并通过SurfaceFlinger 提交到显示设备以显示。

#### 创建一个Window的过程：
1. 客户端调用WindowManager.addView方法创建一个Window 
2. WindowManagerGlobal创建一个ViewRootImpl
3. ViewRootImpl内置创建一个Surface，并创建View树
4. ViewRootImpl 调用 addToDiaplay显示

![windows抽象](http://onefengma.com/blog/images/ink.png)
