# SurfaceStack
Surface堆叠展示

#### 问题说明：
音视频开发中，我们会使用SurfaceView进行视频画面的展示。当场景中有多个SurfaceView的时候，会出现展示内容的堆叠覆盖，导致我们本想看到的内容被遮挡。

#### 一、SurfaceView 简介
SurfaceView是View的子类，它内嵌了一个专门用于绘制的Surface，你可以控制这个Surface的格式和尺寸，Surfaceview控制这个Surface的绘制位置。Surface是纵深排序(Z-ordered)的，说明它总在自己所在窗口的后面。SurfaceView提供了一个可见区域，只有在这个可见区域内的surface内容才可见。surface的排版显示受到视图层级关系的影响，它的兄弟视图结点会在顶端显示。这意味者 surface的内容会被它的兄弟视图遮挡，这一特性可以用来放置遮盖物(overlays)(例如，文本和按钮等控件)。


#### 二、解决方案
在SurfaceView onAttachedToWindow之前调用如下设置：

surfaceView.setZOrderMediaOverlay(true);
在onViewAttachedToWindow的时候，调用如下设置：
```
surfaceView.getHolder().setFormat(PixelFormat.TRANSLUCENT);
```

#### 三、核心API方法描述
1). public void setZOrderMediaOverla(boolean isMediaOverlay)
Added in API level 5
```
Control whether the surface view's surface is placed on top of another regular surface view in the window 
(but still behind the window itself). This is typically used to place overlays on top of an underlying media surface view.
Note that this must be set before the surface view's containing window is attached to the window manager.
Calling this overrides any previous call to setZOrderOnTop(boolean).
```
控制这个surfaceView是否被放在另一个普通的surfaceView上面（但是仍然在窗口之后）。这个函数通常被用来将覆盖层至于一个多媒体层上面。
这个函数必须在窗口被添加到窗口管理器之前设置。
调用这个函数会使之前调用的setZOrderOnTop(boolean)无效。
 
 
2).surfaceView.getHolder().setFormat(PixelFormat.TRANSLUCENT);
 
此设置的意义是将布局中不展示内容的地方设置为透明，这样两个SurfaceView的内容就能按照我们的需要进行展示了。
