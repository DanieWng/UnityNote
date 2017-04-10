#UnityAction, delegate, event使用


*	UnityAction

	Button脚本的`onClick`中设置监听方法`AddListener`中的参数即是**UnityAction**类型， 之前我在设置事件函数的方法主要分为两种：
	
	1. 无参数时，直接在脚本中添加
		
		```
		btn.OnClick.AddListener(this.MethodA);
		
		btn.OnClick.AddListener(()=>{
			// add my code
		});
		```
		
		在脚本中添加代理的方法。
	
	2. 有参数时，在编辑器中拖动添加
	
	**但是**在研究了下UnityAction的原理后发现还有另外一种方法。
		
		```
		btn.OnClick.AddListener(delegate{
			this.MethodB(Params1);
		});
		```
	
*	delegate