#Unity - Time

![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/3962099-3080b40dfde0f472.png?raw=true)

> [Unity Time类详解](http://www.jianshu.com/p/eb99f97a920a)

* #####Time.time

	**time**是指程序开始运行时开始计时的数值

* #####Time.timeScale

	**time scale**这个用来做加速和暂停。默认是1。
	
	当为1时: 指的是时间的速率等同于真实时间。所以当设置为0.5时，就可以理解为放缓为真实时间的2倍的意思。
	
	当为**0**时:
	1. `Update`和`LateUpdate`是不会停止刷新的，`FixedUpdate`会停止。
	2. **delta time**会变为0，所有用的这个变量的物体都会停止，例如移动或旋转。

	>**注意事项:**
	>
	>1. 当在**Coroutine**中使用了`new WaitForSeconds()`时， 因为**delta time**的指也会发生改变，所以延迟时间也会和真实时间不符。例如当**time scale=0**，那么时间并不会增加。所以如果需要不受**time scale**影响的话，需要使用`new WaitForSecondsRealtime(1)`

	>2. 使用**DoTween**插件的时候，如果想要Action不受影响，需要设置`tweener.SetUpdate(true)`
	

* #####Time.deltaTime

	**delta time**是距离上一次调用`Update`或`FixedUpdate`所用的时间。
	
	一般在移动或着转向的物体操作中，用这个就可以转为以秒来计算。
	
	```
	void Update()
	{
		// 每秒移动10单位
		float x = Time.deltaTime * 10f;
		transform.Translate(x, 0, 0);
	}
	``` 

* #####Time.fixedDeltaTime

	**fixed delta time**是指`FixedUpdate`调用的时间间隔的。单位是秒。
