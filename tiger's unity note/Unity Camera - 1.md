#Unity Camera - 1

## Description

>A Camera is a device through which the player views the world.
>
>A screen space point is defined in pixels. The bottom-left of the screen is >(0,0); the right-top is (pixelWidth,pixelHeight). The z position is in world >units from the Camera.
>
>A viewport space point is normalized and relative to the Camera. The bottom->left of the Camera is (0,0); the top-right is (1,1). The z position is in >world units from the Camera.
>
>A world space point is defined in global coordinates (for example, >Transform.position).

_unity圣典_: <http://www.ceeger.com/Components/class-Camera.html>

## 重要概念
*	aspect 视窗

	

* 	size

	1. 正交投影
		
		2d游戏可以使用平行投影的camera，这种camera需要设置size (orthographicSize)，size的含义为屏幕高度的一半，不过单位不是像素而是unit坐标，即通过pixels to units换算的坐标。例如：屏幕高度为640，pixels to units为100的情况下，orthographic size为640/2/100 = 3.2 
	
		>在Orthographic模式下，视截体尺寸可如下计算：
		>视截体高度= camera.size
		>视截体宽度= 视截体高度*(screenWidth/screenHeight)*(camera.viewportRect.W/		>camera.viewportRect.H)
		
	2. 透视投影
		
		Field of View(FOV)
		
		相机的视野，沿着本地Y轴测量的角度。
		
	3. viewport
		
		（0， 1） （1， 1）
		
		（0， 0） （1， 0）
		
		左下角为（0， 0）， 右上角为（1， 1）
		

	