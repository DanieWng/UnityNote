#Unity 开发时的疑问总结 - 1

*	在Button.cs文件发现了`onClick`的声明方式和c#中的delegate有很大区别。

	```c sharp
		[Serializable]
       public class ButtonClickedEvent : UnityEvent {}
	
		[Serializable]
	    public class ButtonClickedEvent : UnityEvent {}
	
	    // Event delegates triggered on click.
	    [FormerlySerializedAs("onClick")]
	    [SerializeField]
	    private ButtonClickedEvent m_OnClick = new ButtonClickedEvent();
	    
	    public ButtonClickedEvent onClick
	    {
	        get { return m_OnClick; }
	        set { m_OnClick = value; }
	    }
	    
	     private void Press()
        {
            if (!IsActive() || !IsInteractable())
                return;

            m_OnClick.Invoke();
        }
  	```
  	
  	在这段代码中可以发现它是继承了UnityEvent类实现的代理委托， 而且调用是通过`Invoke()`方法。 这里有几个还不太理解的内容是:
  	
  	*	`[FormerlySerializedAs("onClick")]`的意义是什么？ 测试发现它会在inspector面板内添加这样一块区域
  	
  		![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/Jietu20170322-170025.jpg?raw=true)
  		
  	*	`UnityEvent`与c#基本的delegate, event有什么区别？ 哪个更加有效率？
	
	